# LightGBM

`LigthGBM` 是 `boosting` 集合模型中的新进成员，由微软提供，它和 `XGBoost` 一样是对 `GBDT` 的高效实现，原理上它和GBDT及XGBoost类似，都采用损失函数的负梯度作为当前决策树的残差近似值，去拟合新的决策树。


LightGBM在很多方面会比XGBoost表现的更为优秀。它有以下优势：

* 更快的训练效率
* 低内存使用
* 更高的准确率
* 支持并行化学习
* 可处理大规模数据
* 支持直接使用category特征
* LightGBM比XGBoost快将近10倍，内存占用率大约为XGBoost的1/6，并且准确率也有提升。


## 定义模型

    def model(features, test_features, encoding = 'ohe', n_folds = 5):
        
        """Train and test a light gradient boosting model using
        cross validation. 
        
        Parameters
        --------
            features (pd.DataFrame): 
                dataframe of training features to use 
                for training a model. Must include the TARGET column.
            test_features (pd.DataFrame): 
                dataframe of testing features to use
                for making predictions with the model. 
            encoding (str, default = 'ohe'): 
                method for encoding categorical variables. Either 'ohe' for one-hot encoding or 'le' for integer label encoding
                n_folds (int, default = 5): number of folds to use for cross validation
            
        Return
        --------
            submission (pd.DataFrame): 
                dataframe with `SK_ID_CURR` and `TARGET` probabilities
                predicted by the model.
            feature_importances (pd.DataFrame): 
                dataframe with the feature importances from the model.
            valid_metrics (pd.DataFrame): 
                dataframe with training and validation metrics (ROC AUC) for each fold and overall.
            
        """
        
        # 提取 ID
        train_ids = features['SK_ID_CURR']
        test_ids = test_features['SK_ID_CURR']
        
        # 提取标签进行培训
        labels = features['TARGET']
        
        # 删除 ID 和 目标
        features = features.drop(columns = ['SK_ID_CURR', 'TARGET'])
        test_features = test_features.drop(columns = ['SK_ID_CURR'])
        
        
        # One Hot Encoding
        if encoding == 'ohe':
            features = pd.get_dummies(features)
            test_features = pd.get_dummies(test_features)
            
            # Align the dataframes by the columns
            features, test_features = features.align(test_features, join = 'inner', axis = 1)
            
            # No categorical indices to record
            cat_indices = 'auto'
        
        # Integer label encoding
        elif encoding == 'le':
            
            # Create a label encoder
            label_encoder = LabelEncoder()
            
            # List for storing categorical indices
            cat_indices = []
            
            # Iterate through each column
            for i, col in enumerate(features):
                if features[col].dtype == 'object':
                    # Map the categorical features to integers
                    features[col] = label_encoder.fit_transform(np.array(features[col].astype(str)).reshape((-1,)))
                    test_features[col] = label_encoder.transform(np.array(test_features[col].astype(str)).reshape((-1,)))

                    # Record the categorical indices
                    cat_indices.append(i)
        
        # Catch error if label encoding scheme is not valid
        else:
            raise ValueError("Encoding must be either 'ohe' or 'le'")
            
        print('Training Data Shape: ', features.shape)
        print('Testing Data Shape: ', test_features.shape)
        
        # Extract feature names
        feature_names = list(features.columns)
        
        # Convert to np arrays
        features = np.array(features)
        test_features = np.array(test_features)
        
        # Create the kfold object
        k_fold = KFold(n_splits = n_folds, shuffle = False, random_state = 50)
        
        # Empty array for feature importances
        feature_importance_values = np.zeros(len(feature_names))
        
        # Empty array for test predictions
        test_predictions = np.zeros(test_features.shape[0])
        
        # Empty array for out of fold validation predictions
        out_of_fold = np.zeros(features.shape[0])
        
        # Lists for recording validation and training scores
        valid_scores = []
        train_scores = []
        
        # Iterate through each fold
        for train_indices, valid_indices in k_fold.split(features):
            
            # Training data for the fold
            train_features, train_labels = features[train_indices], labels[train_indices]
            # Validation data for the fold
            valid_features, valid_labels = features[valid_indices], labels[valid_indices]
            
            # Create the model
            model = lgb.LGBMClassifier(n_estimators=10000, objective = 'binary', 
                                    class_weight = 'balanced', learning_rate = 0.05, 
                                    reg_alpha = 0.1, reg_lambda = 0.1, 
                                    subsample = 0.8, n_jobs = -1, random_state = 50)
            
            # Train the model
            model.fit(train_features, train_labels, eval_metric = 'auc',
                    eval_set = [(valid_features, valid_labels), (train_features, train_labels)],
                    eval_names = ['valid', 'train'], categorical_feature = cat_indices,
                    early_stopping_rounds = 100, verbose = 200)
            
            # Record the best iteration
            best_iteration = model.best_iteration_
            
            # Record the feature importances
            feature_importance_values += model.feature_importances_ / k_fold.n_splits 
            
            # Make predictions
            test_predictions += model.predict_proba(test_features, num_iteration = best_iteration)[:, 1] / k_fold.n_splits
            
            # Record the out of fold predictions
            out_of_fold[valid_indices] = model.predict_proba(valid_features, num_iteration = best_iteration)[:, 1]
            
            # Record the best score
            valid_score = model.best_score_['valid']['auc']
            train_score = model.best_score_['train']['auc']
            
            valid_scores.append(valid_score)
            train_scores.append(train_score)
            
            # Clean up memory
            gc.enable()
            del model, train_features, valid_features
            gc.collect()
            
        # Make the submission dataframe
        submission = pd.DataFrame({'SK_ID_CURR': test_ids, 'TARGET': test_predictions})
        
        # Make the feature importance dataframe
        feature_importances = pd.DataFrame({'feature': feature_names, 'importance': feature_importance_values})
        
        # Overall validation score
        valid_auc = roc_auc_score(labels, out_of_fold)
        
        # Add the overall scores to the metrics
        valid_scores.append(valid_auc)
        train_scores.append(np.mean(train_scores))
        
        # Needed for creating dataframe of validation scores
        fold_names = list(range(n_folds))
        fold_names.append('overall')
        
        # Dataframe of validation scores
        metrics = pd.DataFrame({'fold': fold_names,
                                'train': train_scores,
                                'valid': valid_scores}) 
        
        return submission, feature_importances, metrics


## 评估方法

和 `xgb` 一样，lgb也可以使用 `gini` 系数来对模型进行评估


`1、定义基尼系数：`

    def gini(y, pred):
        g = np.asarray(np.c_[y, pred, np.arange(len(y)) ], dtype=np.float)
        g = g[np.lexsort((g[:,2], -1*g[:,1]))]
        gs = g[:,0].cumsum().sum() / g[:,0].sum()
        gs -= (len(y) + 1) / 2.
        return gs / len(y)

`2、定义 lgb gini 系数：`

    def gini_lgb(preds, dtrain):
        y = list(dtrain.get_label())
        score = gini(y, preds) / gini(y, y)
        return 'gini', score, True

`3、调用：`

    # lgb.train 中的 feval 参数可以用于指定评估方法，这里我们使用自定义的 gini_lgb 使用gini系数来进行评估
    lgb_model = lgb.train(params, lgb.Dataset(X_train, label=y_train), nrounds, 
                  lgb.Dataset(X_eval, label=y_eval), verbose_eval=100, 
                  feval=gini_lgb, early_stopping_rounds=100)







## 详细参考地址

[机器学习算法之 LightGBM](https://www.biaodianfu.com/lightgbm.html)



