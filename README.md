Get my notebook: https://colab.research.google.com/drive/1Cl_clxLCq7hF6cqAPcFcypXUWuXLQj2-?usp=sharing

Kaggle username: Nina Reiser

Test_SalePrice_Predictions.csv: https://github.com/nlreiser/MSDS-Machine-Learning/blob/main/Old%20Versions/hp_submission4.csv


I.	Introduction: Research Question and Problem for Management

The house prices dataset consists of 80 attributes for 1460 different homes in Ames, Iowa. The purpose of this study is to determine how these attributes affect sale prices of homes. Analysis of the house price data provides information and insight into what specifically affects the cost of a home and can be used to make predictions about the cost of other homes for which price is not available. Real estate firms and builders can use this information to predict and set costs of homes for future buyers.  


II.	Analysis of Dependent Variable (Sale Price)

An initial plot of the dependent variable SalePrice shows that this data does not follow normal distribution, with a skewness of 1.88 and kurtosis of 6.54 (Figure 1). The mean SalePrice is $180,921 and the most expensive home in the training data is $755,000. The SalePrice data was transformed to log scale and plotted. As it can be seen in Figure 2, this log transformation fixed the issue of skewness with the SalePrice data, with a new skewness of 0.12 and kurtosis 0.81. 
                       
![image](https://user-images.githubusercontent.com/97359451/149686692-b8764eee-1309-4d73-8603-bc2ed09d3acb.png)

Figure 1. SalePrice Normal Distribution Plot		      

            
![image](https://user-images.githubusercontent.com/97359451/149686774-2a3cf6f7-f34a-4777-9ec6-ee66488e4fb3.png)

Figure 2. SalePrice Log Normal Distribution Plot


III. Feature Creation

Some of the features that home-buyers may find important were split up unusually. For example, there was no total square footage or total bathrooms. I created new features to combine all of the square footage variables into TotalSF and all of the bathroom variables into TotalBaths. By viewing the heatmaps (Figures 3 & $), these new features had higher correlations with SalePrice. Note: since the bathrooms ended up a string, a heatmap of the enumerated values was used instead. The string values were later encoded.

![image](https://user-images.githubusercontent.com/97359451/149687290-4d16fa10-eb6a-4040-bb57-098643e5664a.png)

Figure 3. Heatmap of SalePrice vs New TotalBaths Feature and Separated Bath Features

![image](https://user-images.githubusercontent.com/97359451/149687314-b3cec591-0306-42f2-b5a7-614c6c688d29.png)

Figure 4. Heatmap of SalePrice vs New TotalSF Feature and Separated Square Footage Features

The outdoor features were also combined into a Deck_PorchSF feature, but this provided no strong correlation with SalePrice.


IV.	Analysis of Independent Variables 

After creating new features and dropping the unneeded variables, there were 21 numerical features and 40 categorical features for analysis. Fence, alley, miscfeature, and poolqc contained more than 50% null values, so those variables were dropped. After review of the data description, it was determined that the null values were due to properties that were missing features, such as having no pool or no garage. Nulls were changed to a value of 0 for quantitative variables and ‘Missing’ for qualitative variables so they could still be included in further analysis. Variables that may heavily impact the sale price of the home include neighborhood, overall quality of the home (exterior, kitchen, etc.), having a basement or a garage, and being one or two-story. 


V.	Correlations and Potential Predictors of SalePrice

In order to determine which variables have the most influence on SalePrice without being confounding variables, a spearman correlation bar plot was created (Figure 5). The top 10 independent variables affecting SalePrice include OverallQual, YearBuilt, YearRemodAdd, TotRmsAbvGrd, GarageCars, TotalSF, Neighborhood_E, ExterQual_E, Foundation_E, BsmtQual_E, KitchenQual_E, FireplaceQu_E, GarageType_E, GarageFinish_E, and TotalBaths_E. Spearman correlation can detect relationships between variables even if they are nonlinear. Heatmaps were next plotted to determine the relationships between variables. Build year tended to be the same for garages and houses, and basements typically have the same square footage as the first floor. Garage area is correlated with number of cars that fit in the garage. 

![image](https://user-images.githubusercontent.com/97359451/149687764-782bf282-7ec6-4ddf-aec3-0f5c46a6f30d.png)

Figure 5. Spearman correlation map of independent variables and SalePrice.


VI. Models

Linear regression, decision tree regression, and random forest regression models were fit to the training data and then used on the test data to predict SalePrice. A pipeline transformer was used to standardize the data. The linear regression model had a root mean square error (rmse) value of 0.15. Decision tree had a rmse of 0.0033 and standard deviation of 0.025. Random forest had a rmse of 0.054 and a standard deviation of 0.016. 10 cross validations were used in each of these models. Random forest was determined to be the best model for this set of data, and it led to predictions for the test dataset of house prices.


VII.	Conclusions
After EDA of the dataset, the factors to include in the machine learning model are those that have the greatest impact on sale 
price of the home, those being OverallQual, YearBuilt, YearRemodAdd, TotRmsAbvGrd, GarageCars, TotalSF, Neighborhood_E, ExterQual_E, Foundation_E, BsmtQual_E, KitchenQual_E, FireplaceQu_E, GarageType_E, GarageFinish_E, and TotalBaths_E. The data was scaled to keep consistency across variables with a pipeline transformer. The resulting predictions had a score of 0.31349 on the Kaggle House Prices Competition (see below). 

![image](https://user-images.githubusercontent.com/97359451/149688550-156d2414-d562-4cdc-8fca-51af83dbea97.png)


