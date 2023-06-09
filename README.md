# Linear-Regression

Linear Regression Code

# Import the libaries
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
from  sklearn import datasets, linear_model
from sklearn import preprocessing
from sklearn.model_selection import train_test_split
from sklearn.feature_selection import RFE
# Import Dataset
Regression = pd.read_csv('Belfast_City_Centre_2022.csv')
Regression = pd.DataFrame(Regression)
Regression.tail(6)
​
Date	Ozone	Status	NO2	Status.1	PM10	Status.2	PM2.5	Status.3
263	24/12/2022	42	P µg/m³	27	P µg/m³	11	P µg/m³ (FIDAS)	7	P µg/m³ (Ref.eq)
264	25/12/2022	58	P µg/m³	14	P µg/m³	9	P µg/m³ (FIDAS)	5	P µg/m³ (Ref.eq)
265	26/12/2022	64	P µg/m³	10	P µg/m³	7	P µg/m³ (FIDAS)	5	P µg/m³ (Ref.eq)
266	27/12/2022	40	P µg/m³	30	P µg/m³	12	P µg/m³ (FIDAS)	8	P µg/m³ (Ref.eq)
267	30/12/2022	57	P µg/m³	23	P µg/m³	6	P µg/m³ (FIDAS)	4	P µg/m³ (Ref.eq)
268	31/12/2022	26	P µg/m³	41	P µg/m³	12	P µg/m³ (FIDAS)	9	P µg/m³ (Ref.eq)
Regression.dropna()
Date	Ozone	Status	NO2	Status.1	PM10	Status.2	PM2.5	Status.3
0	01/01/2022	60	V µg/m³	9	V µg/m³	18	V µg/m³ (FIDAS)	10	V µg/m³ (Ref.eq)
1	02/01/2022	59	V µg/m³	12	V µg/m³	13	V µg/m³ (FIDAS)	7	V µg/m³ (Ref.eq)
2	03/01/2022	61	V µg/m³	12	V µg/m³	12	V µg/m³ (FIDAS)	6	V µg/m³ (Ref.eq)
3	04/01/2022	56	V µg/m³	15	V µg/m³	10	V µg/m³ (FIDAS)	5	V µg/m³ (Ref.eq)
4	05/01/2022	35	V µg/m³	34	V µg/m³	13	V µg/m³ (FIDAS)	8	V µg/m³ (Ref.eq)
...	...	...	...	...	...	...	...	...	...
264	25/12/2022	58	P µg/m³	14	P µg/m³	9	P µg/m³ (FIDAS)	5	P µg/m³ (Ref.eq)
265	26/12/2022	64	P µg/m³	10	P µg/m³	7	P µg/m³ (FIDAS)	5	P µg/m³ (Ref.eq)
266	27/12/2022	40	P µg/m³	30	P µg/m³	12	P µg/m³ (FIDAS)	8	P µg/m³ (Ref.eq)
267	30/12/2022	57	P µg/m³	23	P µg/m³	6	P µg/m³ (FIDAS)	4	P µg/m³ (Ref.eq)
268	31/12/2022	26	P µg/m³	41	P µg/m³	12	P µg/m³ (FIDAS)	9	P µg/m³ (Ref.eq)
269 rows × 9 columns

# Plotting Histogram of depenedent varaible (NO2) to see distribution
plt.figure(figsize = (10, 5))
Regression['NO2'].plot(kind ="hist")
<AxesSubplot:ylabel='Frequency'>

# Defining the dependent variable NO2 (y) and independent variable Ozone (x)
x = Regression["Ozone"]
y = Regression["NO2"]
Regression.dropna()
# Using Pearson Correlation to plot all correlations (positive and Negative) between all pollutants
plt.figure(figsize=(9,8))
Regression.dropna()
cor = Regression.corr()
sns.heatmap(cor, annot=True, cmap=plt.cm.RdYlGn)
Regression.dropna()
plt.show
<function matplotlib.pyplot.show(close=None, block=None)>

Regression = Regression.drop(["Date","Status", "Status.1", "Status.2", "Status.3", "NO2", "PM10", "PM2.5"], axis=1)
Regression.head(5)
Ozone
0	60
1	59
2	61
3	56
4	35
# Determine correlations between dependent & independent variable (NO2 & Ozone)
Regression.dropna()
print(Regression[["Ozone"]].corr())
       Ozone
Ozone    1.0
# Creating test & train variables using test_train_split from sklearn library
x_train, x_test, y_train, y_test = train_test_split(Regression, y, test_size=0.20) # test size = 20% of data, train is 80%
Regression.dropna()
print(x_train.shape, y_train.shape) # 292 members in train (80%)
print(x_test.shape, y_test.shape) # 73 members in train (20%)
(215, 1) (215,)
(54, 1) (54,)
# Creating the linear regression model
lrm = linear_model.LinearRegression()
lrm_model = lrm.fit(x_train, y_train)
lrm_predictions = lrm.predict(x_test)
​
lrm_predictions[0:54]
array([29.07117735, 11.3434047 , 27.96319156, 27.96319156, 19.65329813,
       16.88333365, 14.11336918, 14.66736207, 23.5312484 , 34.6111063 ,
       15.22135497, 19.65329813, 17.99131944, 24.63923419, 29.07117735,
       32.94912762, 25.19322708, 12.45139049, 12.45139049,  7.46545443,
       32.39513472, 32.94912762, 13.00538338, 15.22135497, 30.17916314,
       22.9772555 , 14.11336918, -2.50641768, 22.42326261, 17.99131944,
       16.32934076, 14.66736207, 14.66736207, 15.22135497, 31.84114183,
       29.62517024, 13.00538338, 19.65329813, 19.09930523, 21.86926971,
       10.7894118 , 23.5312484 , 25.19322708, 21.86926971, 24.08524129,
       11.89739759, 44.58297842, 13.55937628, 15.22135497, 34.05711341,
       22.9772555 , 21.86926971, 22.42326261, 27.40919866])
plt.figure(figsize=(12,8))
plt.scatter(y_test, lrm_predictions)
plt.title("Plot of preddicted Vs Actual Values off NO2 Belfast City Centre 2022 ")
plt.xlabel("Actual Values")
plt.ylabel('Predicted Values')
​
​
Text(0, 0.5, 'Predicted Values')

print('R2:', lrm_model.score(x_test, y_test))
​
R2: 0.5149043349069473
# assume y_test and lrm_predictions are the actual and predicted values of the response variable, respectively
slope, intercept = np.polyfit(y_test, lrm_predictions, 1)  # fit a linear regression line to the data
x_values = np.array([np.min(y_test), np.max(y_test)])  # create a new array of x-values
y_values = intercept + slope * x_values  # compute the corresponding y-values
plt.figure(figsize=(14,9))
plt.scatter(y_test, lrm_predictions)
plt.plot(x_values, y_values, color='r')  # add the linear regression line to the plot
plt.title("Plot of Predicted Vs Actual Values of NO2(Belfast City Centre 2022)", fontsize=20)
plt.xlabel("Actual Values", fontsize=14)
plt.ylabel("Predicted Values", fontsize=14)
plt.show()
