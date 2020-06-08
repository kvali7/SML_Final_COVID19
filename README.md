# Relating number of COVID19 cases in the US with social-distancing measures

link to the Project's Repo:
https://github.com/kvali7/SML_Final_COVID19

### Authors: Kourosh Vali, Begum Kasap

### Discription

This is the Repository for the Statistical Machine Learning (STA 208) course Project in which we will be focusing on the COVID19 data and we can relate to the social distancing data.

### Abstract

This project aims to find the relation between the COVID19 reported anonymized cases with the Social Distancing data. This is achievable by taking a look at the unlabled transportation data of the people in a city and see how we can train a model to learn the trend of the number of cases reported in that specific city. 

For this Project, we Will be focusing on the Joh Hopkins University COVID19 data available at https://github.com/CSSEGISandData/COVID-19 as well as the SafeGraph Social Distancing data available at https://www.safegraph.com/covid-19-data-consortium. We have found a relevant research at the University of Texas [1] in which they take a look at Death rate and how the trend flattens with the shelter in place order. Their model could be a very helpful example to our project. The paper is regressing on the death cases, however we are regressing on the confirmed cases. Also, in the reference they are employing the negative binomial loss whereas we are using the poisson loss.

### Preparation

 We are going to create the daily and weekly training and test dataset from the John Hopkins University COVID-19 cases and SafeGraph social distancing data. In order to do so, we have to place the "csse_covid_19_data" downloaded from the above CSSEGISandData repo to the "/data/COVID-19/csse_covid_19_data/csse_covid_19_daily_reports/" folder. Also, we need to download the SafeGraph social distancing dataset and place it in the /data folder. Only the percentage staying at home data computed from Safegraph data is downloaded using Superset website from Rill Data. Finally, the census 2019 population data should be placed in the /data folder as we require the population info in the input data. All the data necessary to execute the notebooks has been uploaded to /data folder. Note that in this project, we have looked at the COVID-19 cases until the May 24th. We can execute the "/code/combine_jhu_data.py" python file (by Prof. James Sharpnack) to create the daily.df data file from the COVID-19 cases dataset at the /data folder. However, the daily.df data is already provided in this repo and the execution of this code is not necessary. 

### Preprocessing 

Now, To create the input data for this prediction task, the file at "/notebooks/preprocessing_data.ipynb" can be executed which is developed to create meaningful data from Large John Hopkins University COVID-19 cases and SafeGraph GPS tracking datasets which will create two preprocessed and meaningful datasets for the two Prediction Tasks. The daily changes of COVID 19 cases, as well as the Percentage of people in each state that are at home, and the population of the state, alongside the Incident Rate (which is the number of confirmed cases per 100000 people) as the labels are stored at "/data/X_daily_...csv" and "/data/y_daily_...csv" files. 

In addition, the weekly changes in Incident rate from the last week are tracked and the resulting file is stored at "/data/X_weekly_change_...csv" and "/data/y_weekly_change_...csv" data files to include element of history. The weekly datasets have additional features such as change in incident rate and the incident rate of the previous week. Since we don't have a large dataset for each state, we applied feature engineering and computed two additional features in order to improve our Model.

More info on the steps of preprocessing is available inside the aforementioned notebook file. All the datasets are exported by preprocessing_data.ipynb under /data as .csv files so that they can be used in later notebooks which will regress on the training data and predict for the test data. All the .csv files are available in the Repo. 

### Regression Task

In this project, there are two Prediction Tasks that we tried to address. The First Prediction task is to predict daily incident rate using features such as States, Population of States, and number of elapsed days since incident rate crossed the 0.5 threshold and percentage of people staying home. We have the states as one of the variables because different states have incorporated different methods for dealing with the pandemic. File located at "/notebooks/Models_Task1.ipynb" is the notebook file that implements this first task. It contains all the blocks for importing and polishing the preprocessed data under /data folder. The first prediction task is performed using different models such as: SVM, Random Forest, Poisson Regression and Multi-Layer Perceptron. Parameters for each model are optimized using the training dataset and a grid of different parameters. The results of predicted daily cases are plotted against the actual daily cases for the State of California, to check if the models can capture the trend in daily cases. Unfortunately, results showed that neither SVM, Random Forest or Generalized Linear Models with Poisson Class could predict the trend in daily cases. Several Multi-layer perceptron models are trained to find the best number of hidden layers, activation function and units per hidden layer. The models are trained and optimized using Adam optimizer and Poisson loss. Poison loss is mostly useful for Count data, like the data that we have here, so we use Poisson loss. The best model is chosen based on the Mean-Absoulute-Error and is tested on the test data. The test results using the best model having one hidden layer with sigmoid activation function and with ReLU activation function on the output layer, showed that Multi Layer Perceptron outperformed SVM, Random Forest and Poisson regression. This can be clearly seen by looking at the trend in daily cases for the state of California plot in the "/notebooks/Models_task1_ipynb" python notebook for each Method as well as the Mean-Absolute-Error for the whole dataset.

The second prediction task uses the weekly dataset in order to predict the top 20 states which will see the most change in their incident rate over a period of 2 weeks between May10 - May24. The model that performs this task can be found under "/notebooks/Models_Task2.ipynb". We trained several MLPs with different activation functions and different number of hidden layers. All the models are optimized using the Adam optimizer and Poisson Loss function. The best model is chosen based on the mean-absolute-error metric resulting from the training dataset. The best model, with 2 hidden layers with relu activation function and sigmoid activation on the output layer, is tested on 2-week long dataset and finally the top 20 states that are predicted to have the highest change in incident rate are outputted. The Top state (Minnesota) that has the highest change in incident rate is predicted correctly. Furthermore, the test results showed that overall 15 out of 20 states were predicted correctly. The order by change in incident rate of the predicted states do not exactly match with the order of actual states, but it is promising to see that 75% of the top 20 state names were predicted correctly. 



### References
[1] S. Woody, et al. "Projections for first-wave COVID-19 deaths across the US using social-distancing measures derived from mobile phones", Not yet published https://doi.org/10.1101/2020.04.16.20068163
