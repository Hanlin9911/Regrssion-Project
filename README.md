# Regression Project
The goal of this project is to create and test several regression models to predict rental prices for units in across Canada in 2024.
The training dataset is obtained from [rentfaster.ca](https://www.rentfaster.ca/?utm_source=OOH&utm_medium=sign&utm_campaign=ca)
The work is roughly splitted into four steps: EDA and Data Preparation, Model exploration and creation, Final model creation, and Testing

## EDA and Data Preparation
In this step, duplicates are dropped, missing data are dropped or filled by imputed values.

## Model Creation
In this step, only multiple regrssion is used (Lasso and Ridge). A total of 8 models are explored. Each model is tested for 10 times and the average performance (MAE, MSE, RMSE, R2) are compared. 

### Model 1
All features except for `longitude`, `latitude` and `province`, from cleaned dataset are used in the model. 

### Model 1 Modified
Outliers from `type` are dropped. Log transformation of `sq_feet_num` is performed. There is a decrease in performance metrics. However this was not caught at the first place. Transformations in this step will unfortunately be carried in the later models. 

### Model 2 Interaction
Three extra columns are built: `bedroom/sqft`, `bathrooms/sqft`, `price/sqft`. Extreme outliers from `price/sqft` are removed (Those who are 3 sigmas away from mean of `price/sqft`). Note that there might be an issue here, as the mean of the full dataset is used rather than the mean of the training data. 
There is an improvement in performance compared with previous models but it can be raised from the overfitting issue. 

### Model 3 Postcode
Since psotcode is an importnat feature to predict rental price, the `latitude` and `longitude` is matched with their postcode with the application of `geopy`. Then the postcode are grouped based on the average `sqft/price` to create `postcode_index`. There is also a similar overfitting issue as seen in model2 as the average of the fulldataset is used. 
There is a slight improvement in terms of model performance.

## Final Model Creation
A final model is created based on model3, the Ridge model. It is trained by the entire dataset. 
I just caught another issue... Note that in final model training the hyperparamter is tuned instead of using the parameter that are found in tuning. 

## Testing
3 properties are foound from [rentfaster.ca](https://www.rentfaster.ca/?utm_source=OOH&utm_medium=sign&utm_campaign=ca). The features are organized similarly to the original data given.
