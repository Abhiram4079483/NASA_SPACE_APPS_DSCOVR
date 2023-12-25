# NASA_SPACE_APPS_DSCOVR
### Nasa-Space Apps Challenge DSCOVR - 2023
## Background
Geomagnetic storms pose significant risks to modern technologies, especially GPS satellite systems and electrical power grids. These storms result from solar winds reaching Earth, causing deformation of the magnetic field and particle showers at the magnetic poles. Despite efforts, predicting these storms remains challenging due to uncertain travel times of solar material, ranging from two to four days or potentially missing Earth entirely.

DSCOVR, stationed at Lagrange point 1 a million miles from Earth, monitors plasma from a unique vantage point. It measures potential geomagnetic storm activity hours before reaching Earth, offering an early warning system.

NOAA employs solar wind measurements (density, temperature, speed, and magnetic field) to run simulations of Earth's magnetic field and atmosphere. Using these simulations, NOAA predicts the occurrence and intensity of geomagnetic storms, measured on the Planetary K-index (Kp) scale.

Originally planned for five years, DSCOVR is now in its eighth year. While the instrument measuring solar wind's magnetic field remains robust, the sensor for density, temperature, and speed is less sensitive, experiencing unpredictable faults. Real-time detection of these faults is challenging, especially during storm events. Compounded by the Sun's nearing peak activity, storms are more frequent than ever in DSCOVR's extended mission.

## Objective
Using the raw data from DSCOVR—faults and all—to predict geomagnetic storms on Earth.At present, we detect things using the well-calibrated level 2 data, but it's a slow process. We're encouraged the exploration of other methods to make this detection faster and more efficient and immune to any erroneous readings.

## Approach
### Gathering Data
We used the resources available on the Problem Statement page to gather raw PlasMAG data from DSCOVR. The data can be found [here](https://www.spaceappschallenge.org/develop-the-oracle-of-dscovr-experimental-data-repository/) on an yearly basis.
To predict the solar storms we decided on using Kp values, which quantifies disturbances in the horizontal component of Earth's magnetic field with an integer in the range 0–9 with 1 being calm and 5 or more indicating a geomagnetic storm.It is derived from the maximum fluctuations of horizontal components observed on a magnetometer during a three-hour interval.The data for the Kp values can be found [here](https://www.gfz-potsdam.de/en/section/geomagnetism/data-products-services/geomagnetic-kp-index). 
Using this we found the Kp and Ap values since 1932. We scraped this data and filtered out the required portions using Python. We converted this to csv which can be found in the drive attached at the end.
### Scrapped Approaches
Prior to finding the Kp values of the data, we assumed a gaussian approach for kp values. Kp values are generally obtained for every 3 hours. We tried to use gaussian approach assuming a peak near solar storm. A lorentzian fitting method would also work. A kp value of 5 or higher is generally considered a solar storm. However these mehtods cannot be relied for future prediction. 
### Pre-Processing The Data
We firstly loaded the data as required and observed that a lot of values were NaN, especially at the extremity columns of the dataset.We also noticed that the kp values are determined every 3 hours and 3 minutes, which is equivalent to 183 minutes. Our strategy is to use the entire set of 183 data points to predict the kp value for that specific time period. This approach helps mitigate the impact of potential errors in the data due to sensitivity loss. By utilizing the complete set of data points, any errors in individual data points are balanced out by the overall window, allowing the model to better ignore the anomalies and improve its learning process.

In this process not all windows might have enough information, and interpolation could lead to distortion of data. Thus we discussed and settled on a threshold- If there are more than 8 columns who have more than 2 NaN values then we drop the window.


We experimented with different interpolation methods, including polynomial and linear techniques. Ultimately, we opted for a combination of the Window Mean and Gaussian Noise for each column because we can avoid the model learning something wrong.This filtered and interpolated data is saved as csv in the Filtered_Interpolation.csv in the drive attached at the end.
### Generating train and test datasets
In the merged CSV file, we extracted the Kp values, which were attached to the beginning of each window. We then divided the dataset into respective windows. However, we observed that the time difference between Kp values is not consistently 183 for all windows. Towards the end of the dataset, particularly where the time difference is around 159 instead of 183, we addressed this by randomly reusing some of the data points within the same window to ensure proper padding.Using this we generated X and y for our model.

After rechecking for any errors, we converted them to the neccessary form (numpy array or pandas df) accordingly.We then used MinMaxScaler to scale the X dataset. We then split the dataset for train and test respectively.
### Model 
We opted to use a RNN network with LSTM layers to capture the time series nature of data.
Initially we opted to use a regression based approach to predict Kp values of the window.
We used Root Mean Squared Error Loss and Adam's Optimizer.We train the model with 250 epochs and recieved rmse of 0.1647. However the results were not good enough as there are a lot of false negatives. This is because the data consists of lot of datapoints which are below 5 thus the model being more prone to predicting a value less than 5.
On later trials we tried to use Binary classification on the presence of solar storm.Here too we have a huge discrepancy with the number of samples for solar storm occuring, so we implemented a custom loss function which extends BinaryCrossEntropy by adding a weight(>1) to the false Negative predictions.This avoids the model from thinking it is better to predict anything as less than 5.

The custom loss function formula is:

$``loss=-((1-y_{true})*log(1-y_{pred})+ c*y_{true}*log(y_{pred})) \:\:\:\:\:\:  Where \:\: c>=1``$


### Predicting 
After training the model we used the X_test to predict the existence of solar storm.In case of regression we chose 5 to be the threshold as notified by various sources.

## Future Scope
- The learning of the model gets saturated a lot which indicates that the model may not be complex enough to understand the underlying patterns in the data.
- Also the loss function needs some additional experimentation to get the optimal c value.
- Additionally other methods such as synthetic data generation can be explored to resolve the bias issue.

## Using/Running the Project
- Clone our project using command 
`git clone 'repo-url'`
- Install the data required from the drive(contains csv files)
- Run the blocks of code starting with a `--RUN--` comment
- Blocks which can be run but are unnecesary will be marked by `--Choice--` comment
### CSV FILES THAT CONTAIN THE DATA THAT WE USED FOR THIS CHALLENGE ARE UPLOADED HERE - [https://drive.google.com/drive/folders/1UyHzZlMd01r27uyxbUwXNydsIxT0F1fN?usp=sharing]
