# Relating number of COVID19 cases in the US with social-distancing measures

### Authors: Kourosh Vali, Begum Kasap

### Discription

This is the Repository for the Statistical Machine Learning (STA 208) course Project in which we will be focusing on the COVID19 data and we can relate to the social distancing data.

### Abstract

This project aims to find the relation between the COVID19 reported anonymized cases with the Social Distancing data. This is achievable by taking a look at the unlabled transportation data of the people in a city and see how we can train a model to learn the trend of the number of cases reported in that specific city. 

For this Project, we Will be focusing on the Joh Hopkins University COVID19 data available at https://github.com/CSSEGISandData/COVID-19 as well as the SafeGraph Social Distancing data available at https://www.safegraph.com/covid-19-data-consortium. We have found a relevant research at the University of Texas [1] in which they take a look at Death rate and how the trend flattens with the shelter in place order. Their model could be a very helpful example to our project.

### Instructions

To execute the project, the file located at /notebooks/Models.ipynb is the start point. It contains all the Blocks for polishing the preprocessed data which was previously dumped to /data to a dataset that can be used in later blocks which will regress on the training data and predict for the test data.  

To create the input data for this prediction task, the file at /notebooks/preprocessing_data.ipynb can be executed which is developed to create meaningful data from Large John Hopkins University COVID-19 cases and SafeGraph GPS tracking datasets. More info on the steps of preprocessing is available inside the aforementioned notebook file.

### References
[1] S. Woody, et al. "Projections for first-wave COVID-19 deaths across the US using social-distancing measures derived from mobile phones", Not yet published https://doi.org/10.1101/2020.04.16.20068163
