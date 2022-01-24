Get my notebook: https://colab.research.google.com/drive/1E0tz6UMO4IlMncLwhe9cD9LWj2vKKVHw?usp=sharing

Kaggle username: Nina Reiser

Test_SalePrice_Predictions.csv: https://raw.githubusercontent.com/nlreiser/MSDS-Machine-Learning/main/Datasets/hp_submission%20(3).csv


I.	Introduction: Research Question and Problem for Management

The house prices dataset consists of 80 attributes for 1460 different homes in Ames, Iowa. The purpose of this study is to determine how these attributes affect sale prices of homes. Analysis of the house price data provides information and insight into what specifically affects the cost of a home and can be used to make predictions about the cost of other homes for which price is not available. Real estate firms and builders can use this information to predict and set costs of homes for future buyers.  


II.	Analysis of Dependent Variable (Sale Price)

An initial plot of the dependent variable SalePrice shows that this data does not follow normal distribution, with a skewness of 1.88 and kurtosis of 6.54 (Figure 1). The mean SalePrice is $180,921 and the most expensive home in the training data is $755,000. The SalePrice data was transformed to log scale and plotted. As it can be seen in Figure 2, this log transformation fixed the issue of skewness with the SalePrice data, with a new skewness of 0.12 and kurtosis 0.81. 
                       
![image](https://user-images.githubusercontent.com/97359451/149686692-b8764eee-1309-4d73-8603-bc2ed09d3acb.png)

Figure 1. SalePrice Normal Distribution Plot		      

            
![image](https://user-images.githubusercontent.com/97359451/149686774-2a3cf6f7-f34a-4777-9ec6-ee66488e4fb3.png)

Figure 2. SalePrice Log Normal Distribution Plot


III. Feature Creation

Some of the features that home-buyers may find important were split up unusually. For example, there was no total square footage or total bathrooms. I created new features to combine all of the square footage variables into TotalSF and all of the bathroom variables into TotalBaths. By viewing the heatmaps (Figures 3 & $), these new features had higher correlations with SalePrice. Note: since the bathrooms ended up a string, a heatmap of the enumerated values was used instead. The string values were later encoded. One other feature created was QualxSF which was the OverallQual multiplied by the TotalSF, which ended up resulting in a high correlation with SalePrice. This is likely because those two features are some of the main things people look for in a house: overall quality of the home and its size. They are high determining factors of sale price of a home.

![image](https://user-images.githubusercontent.com/97359451/149687290-4d16fa10-eb6a-4040-bb57-098643e5664a.png)

Figure 3. Heatmap of SalePrice vs New TotalBaths Feature and Separated Bath Features

![image](https://user-images.githubusercontent.com/97359451/149687314-b3cec591-0306-42f2-b5a7-614c6c688d29.png)

Figure 4. Heatmap of SalePrice vs New TotalSF Feature and Separated Square Footage Features

The outdoor features were also combined into a Deck_PorchSF feature, but this provided no strong correlation with SalePrice.


IV.	Analysis of Independent Variables 

After creating new features and dropping the unneeded variables, there were 21 numerical features and 40 categorical features for analysis. Fence, alley, miscfeature, and poolqc contained more than 50% null values, so those variables were dropped. After review of the data description, it was determined that the null values were due to properties that were missing features, such as having no pool or no garage. Nulls were changed to the median value for quantitative variables and ‘Missing’ for qualitative variables so they could still be included in further analysis. Variables that may heavily impact the sale price of the home include neighborhood, overall quality of the home (exterior, kitchen, etc.), having a basement or a garage, and being one or two-story. Total square footage is also highly correlated to sale prices of the homes. 


V.	Correlations and Potential Predictors of SalePrice

In order to determine which variables have the most influence on SalePrice without being confounding variables, a spearman correlation bar plot was created (Figure 5). The top independent variables affecting SalePrice include OverallQual, YearBuilt, YearRemodAdd, TotRmsAbvGrd, GarageType, GarageCars,
TotalBaths, TotalSF, QualxSF, ExterQual_E, BsmtQual_E, KitchenQual_E. Neighborhood was also added as a top feature since prior analysis of the data suggested high correlation, using a different type of encoding of categorical variables. Spearman correlation can detect relationships between variables even if they are nonlinear. Heatmaps were next plotted to determine the relationships between variables. Build year tended to be the same for garages and houses, and basements typically have the same square footage as the first floor. Garage area is correlated with number of cars that fit in the garage. 

![image](https://user-images.githubusercontent.com/97359451/150704932-736a0540-7a45-4c17-beaa-109576e67d02.png)

Figure 5. Spearman correlation map of independent variables and SalePrice.


VI. Models

The following models were tested with the train data set split at 80/20: Linear Regression, Decision Tree, Random Forest, Lasso, Ridge, and ElasticNet. The models were fit to 80% of the training dataset and then used on the 'test' 20% remaining of the training dataset to predict SalePrice. MinMax scaler was used to scale the data before fitting. Once the best model was chosen, it was fit to 100% of the training data and then applied to the real test dataset for predictions that would be put into Kaggle. 5 cross validations were used for each of these models. ElasticNet was chosen as the most suitable model for this dataset and GridSearchCV was used to optimize hyperparameters (see Figure 6). The final ElasticNet model had an alpha=0.0001, an l1_ratio=0.0, and max_iterations=10. The transformed test data was put into the model and SalePrice values were predicted.


![image](https://user-images.githubusercontent.com/97359451/150706131-bf1a42cb-543f-4124-a1ce-1117d81d366e.png)

Figure 6. Predicted_Log versus Actual SalePrice_Log Values of Training Data.


VII.	Conclusions

After EDA of the dataset, the factors to include in the machine learning model are those that have the greatest impact on sale 
price of the home, those being OverallQual, YearBuilt, YearRemodAdd, TotRmsAbvGrd, GarageType, GarageCars,
TotalBaths, TotalSF, QualxSF, ExterQual_E, BsmtQual_E, KitchenQual_E, and Neighborhood. The data was scaled to keep consistency across variables with MinMax scaler. After testing several models, ElasticNet performed the best on the training data (using train_test_split). After optimization of hyperparameters, the resulting ElasticNet model outputted SalePrice predictions that had a score of 0.17175 on the Kaggle House Prices Competition (see below). This score was substantially better than the Random Forest model score of 0.39634. For management, it is suggested to use the ElasticNet model to make predictions of sale prices for future investment and selling opportunities.

![image](https://user-images.githubusercontent.com/97359451/150704796-fa7ac1ab-6b20-4675-a5d2-0610bba63559.png)


