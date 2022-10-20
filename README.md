This is a repository for group 9785-17's ICT Capstone Project on Electricity Demand Forecasting with Machine Learning at the University of Canberra. This repository contains only the final deliverables for the project. 

For viewing past versions, please request access to private reposoitory. 

# Key Team Members
1. Viriya Duch Sam | Lead Data Scientist & Scrum Master
      
      Bachelor of Software Engineering
      
2. Kim Khanh Tran | Machine Learning Engineer
      
      Bachelor of Software Engineering
3. Su Hyun Kim | Data Engineer
      
      Bachelor of Software Engineering
      
4. Paola Abrogueña | Data Analyst
      
      Bachelor of Business Informatics

# Project Description
As the world has undergone many industrial revolutions and tremendous development, electricity consumption has risen globally over the years to support its industrial growth, increase of automation, and household needs. It is apparent that access to energy resources is imperative for nations’ economic growth. 

For the energy industry, accurate electricity demand forecasting serves as crucial basis for making informed investment decisions regarding power system planning and operation to determine required resources for economically generating power to the consumers with minimal interruption and risks. Especially after the commencement of the COVID-19 pandemic, which has skewed energy demand trends, there is an urgent need for understanding of future electricity consumption in order to maintain the equilibrium of energy supply and demand in a rapidly changing economy.

Group 9785-17 proposes two machine learning-based algorithms and implementation for short-term load forecasting of 1-day ahead electricity demand. The two models are a Random Forest regressor with application of Recursive Features Elimination (RF-RFE) and a Long Short-term Memory neural network (LSTM). Both models were written in python codes and executed primarily on Jupyter Notebook. Historical demand data from the state of Victoria (from 01 Jan 2015 to 16 Oct 2020) was used in the training and testing of both systems; however, we believe that with slight modifications and re-training, the systems can also be utilised in other Australian electricity markets as well.  The systems may be utilised by entities with interest in balancing the supply and demand of electricity, such as market suppliers and regulatory bodies. 

# How to Use
1. **Datasets**:
All datasets used in the project can be found in the **data.zip** file, which contains 3 microsoft excel files for the original data set, clean data, and data with engineered features. 

2. **Jupyter Notebook Reports**:
Four .ipynb files have been provided as project documentation, wherein the first report is the EDAReport.ipynb for initial exploratory data analysis, followed by FeatureEngineering.ipynb, and RF-RFE.ipynb and LSTM.ipynb for modelling tasks. 

Both RF-RFE and LSTM models have been saved as pickle (.pkl) and json (.json and .h5 for weights) files respectively which can be loaded for future usage. These files can be found in **models.zip**. 

# Report on Results
## Model 1: RF-RFE Regressor
In the case of our electricity demand forecasting project, which is a time-series problem, we used a Random Forest Regressor, where we applied Randomised Search Cross-Validation technique to determine the most optimal parameters for our model. After determining the most optimal parameters for our RF, we proceeded with the study and selection importance of features using a Recursive Features Elimination algorithm. 

Recursive feature elimination (RFE) is a wrapper feature selection method that fits a model and removes the weakest features until the specified number of features is reached. RFE works by searching for a subset of features by starting with all features in the training dataset and successfully removing features until the desired number remains. In this case, we git the optimised Random Decision Forest Algorithm, where RFE then ranks features by importance, discards the least important features, and re-fits the model. This process is repeated until a specified number of features remains. We had a total of 55 original and engineered features, and we decided to include only the top 25% (around 14 features) most important features. 

During different runs, we noticed that the selected top features vary slightly, depending on the calculated most optimal parameters of the RF regressor—this is an inherent characteristic of a wrapper method, where the feature selection process is based on a specific machine learning algorithm to fit on the given dataset. 
We refit our training data back into our optimised model, and we found that data with only selected important features yielded better MAPE score in comparison to including all features. 

In our original train set, we had not accommodated dates after the commencement of COVID-19; therefore, the COVID feature was shown to have very little importance. Thus, in the next part, we re-split our data to accommodate more covid dates into training, while maintaining the original train-test ratio. Results showed that our accuracy improved when accommodating more covid data into our new train sets—both the ones where all features were selected and where only the top 25% were selected. 
Overall, the summary of the RF-RFE regressor were between **3.24-3.15 percent** MAPE for all features and about **0.73-0.95 percent** MAPE for top 25% features.

## Model 2: LSTM Neural Network 
In past papers, the LSTM model has been used over traditional regression analysis models to solve the problems of nonlinearity and data interdependence in regression analysis. LSTM is a type of traditional recurrent neural network (RNN) which ensures the continuous transmission of data through internal multi-loop and constant weight adjustment via backpropagation technique (Tan, 2021). 

In our project, we are dealing with a multivariate time series problem, which has more than one time-dependent variable (i.e., demand is autocorrelated). Each variable depends not only on its past values but also has some dependency on other variables, and thus can be used for forecasting future values. In our project, we convert our demand time series in a supervised learning dataset, where we specify 1-day lag observation input, and predicts 1-day observation as output. Unlike in our previous RF model, we chose to include all features regardless of importance, since we assume that LSTM can better deal with more complex dataset. Likewise, all [engineered] lag features, particularly 7, 14, and 28-day lags, are retained as feature vectors for our LSTM network. Our LSTM model consists of 100 layers, with a dropout layer set 0.1, and 1 dense layer, in addition with customised early stopping call-backs, to prevent overfitting issues. 

In assessing our model performance, we chose the Mean Squared Error for validation loss (vs. training). The results of the LSTM model performance were not as good as the RF-RFE model. The MAPE score for the LSTM model was between **5.53 to 6.54 percent** on test and validation sets respectively. 

### Reference
Tan, F. (2021). Regression analysis and prediction using LSTM model and machine learning methods. Journal of Physics: Conference Series, 1982(1), p.012013. doi:10.1088/1742-6596/1982/1/012013.
