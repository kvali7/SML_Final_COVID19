# Relating number of COVID19 cases in the US with social-distancing measures

### Authors: Kourosh Vali, Begum Kasap

### Discription

This is the Repository for the Statistical Machine Learning (STA 208) course Project in which we will be focusing on the COVID19 data and we can relate to the social distancing data.

### Abstract

This project aims to find the relation between the COVID19 reported anonymized cases with the Social Distancing data. This is achievable by taking a look at the unlabled transportation data of the people in a city and see how we can train a model to learn the trend of the number of cases reported in that specific city. 

For this Project, we Will be focusing on the Joh Hopkins University COVID19 data available at https://github.com/CSSEGISandData/COVID-19 as well as the SafeGraph Social Distancing data available at https://www.safegraph.com/covid-19-data-consortium. We have found a relevant research at the University of Texas [1] in which they take a look at Death rate and how the trend flattens with the shelter in place order. Their model could be a very helpful example to our project.

### Instructions


#### Preprocessing
To create the input data for this prediction task, the file at /notebooks/preprocessing_data.ipynb can be executed which is developed to create meaningful data from Large John Hopkins University COVID-19 cases and SafeGraph GPS tracking datasets. To be more precise, we are creating the daily and weekly training and test dataset from the John Hopkins University COVID-19 cases and SafeGraph social distancing data. In order to do so, we have to place the "csse_covid_19_data" downloaded from the referenced CSSEGISandData to the "/data/COVID-19/csse_covid_19_data/csse_covid_19_daily_reports/" folder and then execute the combine_jhu_data.py (provided by Prof. James Sharpnack) to create the daily.df data file at the /data folder. Also, we need to download the SafeGraph social distancing dataset and place it in the /data folder. Only the percentage staying home per state data computed from Safegraph data is downloaded using Superset website from Rill Data. All the data necessary to execute the notebooks has been uploaded to data folder. 

Next to apply the preprocessing on the SafeGraph dataset and the daily cases dataset, head to /notebooks and execute the preprocessing_data python notebook in which we create two preprocessed and meaningful datasets for the two Prediction Tasks. The daily changes of COVID 19 cases, as well as the Percentage of people in each state that are at home are stored at /data/X_daily_.. and /data/y_daily_.. files. In addition, the weekly changes in Incident Rate (which is the label) from the last week are tracked and the resulting file is stored at /data/X_weekly_change_.. and /data/y_weekly_change_.. data files. The weekly datasets have additional features such as change in incident rate ans the incident rate the previous week. These two additional features were added to improve our results.

More info on the steps of preprocessing is available inside the aforementioned notebook file. All the datasets are exported by preprocessing_data.ipynb under /data as .csv files so that they can be used in later blocks which will regress on the training data and predict for the test data. 

#### Regression Task

In this project, there are two Prediction Tasks that we tried to address. The First Prediction task is to predict daily incident rate using features such as States, Population of States, and number of elapsed days since incident rate crossed the 0.5 threshold and percentage of people staying home. We have the states as one of the variables because different states have incorporated different methods for dealing with the pandemic. File located at /notebooks/Models_Task1.ipynb is notebook that implements this first task. It contains all the blocks for importing and polishing the preprocessed data under /data folder. The first prediction task is eprformed using different models such as: SVM, Random Forest, Poisson Regression and Multi-Layer Perceptron. Parameters for each model are optimized using the training dataset and a grid of different parameters. The results of predicted daily cases are plotted against the actual daily cases for the State of California, to check if the models can capture the trend in daily cases. Unfortunately, results showed that neither SVM, Random Forest or Poisson Regression could predict the trend in daily cases. Several Multi-layer perceptron models are trained to find the best number of hidden layers, activation function and units per hidden layer. The models are trained and optimized using Adam optimizer and Poisson loss. The best model is chosen based on the Mean-Absoulute-Error and is tested on the test data. The test results using the best MLP model showed that MLP outperformed SVM, Random Forest and Poisson regression. This can be clearly seen by looking at the trend in daily cases for the state of California plot in the Models_task2_ipynb notebook.

The second prediction task uses the weekly dataset in order to predict the top 20 states which will see the most change in their incident rate over a period of 2 weeks between May10 - May24. The model that performs this task can be found under /notebooks/Models_Task2.ipynb. The Model contains 2 hidden layers with Sigmoid Activation and Adam Optimizer together with  Poisson Loss function are used to train the model variables. Mean-Absolute-Error is used as a metric to check how well the trained model performs. The trained model is tested on 2-week long dataset and finally the top 20 states that are predicted to have the highest change in incident rate are outputted. The Top state (Minnesota) that has the highest change in incident rate is predicted correctly. Furthermore, the test results showed that overall 15 out of 20 states were predicted correctly. The order by change in incident rate of the predicted states do not exactly match with the order of actual states, but it is promising to see that 75% of the top 20 state names were predicted correctly. 



### References
[1] S. Woody, et al. "Projections for first-wave COVID-19 deaths across the US using social-distancing measures derived from mobile phones", Not yet published https://doi.org/10.1101/2020.04.16.20068163
