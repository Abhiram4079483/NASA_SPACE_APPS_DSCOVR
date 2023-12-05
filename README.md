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
### Pre-Processing The Data



CSV FILES THAT CONTAIN THE DATA THAT WE USED FOR THIS CHALLENGE ARE UPLOADED HERE - [https://drive.google.com/drive/folders/1UyHzZlMd01r27uyxbUwXNydsIxT0F1fN?usp=sharing]
