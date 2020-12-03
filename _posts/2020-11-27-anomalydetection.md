---
title: "Ewaso - Anomaly Detection"
date: 2020-11-27
tags: [Timeseries, KMeans, Python, project ewaso, DeKUT-DSAIL, clusters, anomalies]
header:
    image: "assets/img/kmeasb/km8.jpg"
excerpt: "Project Ewaso - water-level timeseries data  - Anomaly Detection using Kmeans"
mathjax: true
classes: wide
---
## Introduction
- ### Anomaly detection for Time Series Data
Sensor prototypes deployed under potentially harsh weather conditions for tasks like environmental forecasting are prone to breakage and damage. This translates to the high probability of erroneous readings or data corruption during transmission, which in turn brings up the problem of ensuring quality of the data collected by the sensor. Since WSNs (Wireless Sensor Networks) have to operate continuously and therefore generate very large volumes of data every day, data quality process has to be automated, scalable and fast enough to be applicable to streaming data. The most common approach to ensure quality of data is Anomaly Detection. It consists of automatic detection of erroneous reading or anomalous behaviour. (Giannoni et al., 2018)   
- ### KMeans
KMeans is a Clustering based – Unsupervised machine learning technique used to identify clusters of data objects in a dataset. Among many clustering methods, KMeans is one of the oldest and most approachable. These trait makes KMeans implementation in Python easy and straightforward.
- ### Time Series data
By utilizing the sleep mode libraries/setting in many IoT devices data is, data is collected at equal time intervals hence the data end up having a natural temporal order. **Definition**: AN IoT time series dataset is a series of data points indexed in time order. The Project Ewaso prototypes deployed along river Ewaso Nyiro in Kenya, are collecting water-level data at 30 minutes intervals as shown in the figure below.

{% include figure image_path="assets/img/kmeasb/km1.PNG" alt="*Fig 1: Timeseries data (**indexed by Time**)*" caption="*Fig 1: Timeseries data (**indexed by Time**)*" %}

## Anomaly detection on Project Ewaso data
In most cases the quantities being monitored by wireless sensor networks are not dependant on time. Therefore clustering is one dimensional. Figure below shows a plot of all water level data points collected during the month of July 2020. According to the plot, most of the accurate data points are between the 3 – 3.2 Meter range, hence, the data points will be in one cluster. 

{% include figure image_path="assets/img/kmeasb/km2.PNG" alt="*Fig 2: KMeans clustering*" caption="*Fig 2: KMeans clustering*" %}
- ### Step one: Import the python modules. 
```python
    import pandas as pd
    from matplotlib import pyplot as plt
    from pandas import DataFrame, Series
    from pandas.io.json import json_normalize
    from datetime import datetime, timedelta
    import numpy as np 
    import pytz
    import tzlocal
    from sklearn.cluster import KMeans
```
- ### Reading the data.
```python
    df = pd.read_csv('water_level_data.csv')
    df[['time']] = df[['time']].apply(pd.to_datetime)
```
- ### select the month of July (or any other month).
```python
    df_index = df.set_index('time')
    df_index1 = df_index['2020-07-01':'2020-07-31']
    df_index2 = df_index1.reset_index()
```
{% include figure image_path="assets/img/kmeasb/km1.PNG" alt="*Fig 3: water level data in meters*" caption="*Fig 3: Water level data in meters*" %}{: .full}

- ### `Silhouette_score` (to find the optimal number of clusters to use in  KMeans clustering).
Silhouette_score = This is a better measure to decide the number of clusters to be formulated from data.
```python
    from sklearn.cluster import KMeans
    from sklearn.metrics import silhouette_samples, silhouette_score
    water_level = np.array(df_index2['Data']).reshape(-1, 1)
    kmax = 10
    for k in range(2, kmax+1):# dissimilarity would not be defined for a single cluster, thus, minimum number of clusters should be 2
        kmeans = KMeans(n_clusters= k).fit(water_level)
        labels = kmeans.labels_
        silhouette_avg = silhouette_score(water_level, labels, metric = 'euclidean') # euclidean distance from the centre
        print("For k_clusters =", k, "The average silhouette_score is :", silhouette_avg)
        sample_silhouette_values = silhouette_samples(water_level,labels)
        # select the k_clusters with the highest silhoutte_score == Hence k = 5
```
{% include figure image_path="assets/img/kmeasb/km3.PNG" alt="*Fig 4: Silhouette_score*" caption="*Fig 4: Silhouette_score*" %}{: .full}

- ### KMeans Clustering
```python
    num_of_clusters = 5
    water_level_pred2 = KMeans(n_clusters = num_of_clusters, random_state = 123).fit_predict(water_level)
    plt.figure()
    for indx in range (num_of_clusters):
        water_level_clusters = water_level[water_level_pred2 == indx]
        plt.hist(water_level_clusters, 20)
        plt.title('KMeans clustering (legend = 5 clusters with the Cluster Means) of WATER LEVEL data collected from 01 July 2020 - 31 July 2020', fontsize=18, weight = 'bold')
        plt.xlabel('Clusters', fontsize=18, weight = 'bold')
        plt.ylabel('WATER LEVEL IN METERS', fontsize=18, weight = 'bold')
        plt.xticks(fontsize=16, weight = 'bold')
        plt.yticks(fontsize=16, weight = 'bold')
        plt.legend(water_level_pred.cluster_centers_, loc="upper left")
        plt.savefig('histo1.jpg', dpi=500, orientation='portrait', bbox_inches='tight', facecolor='w',edgecolor='b',)
        print(len(water_level_clusters), water_level_clusters.mean())
```
{% include figure image_path="assets/img/kmeasb/km7.PNG" alt="*Fig 5: CLuster dominance*" caption="*Fig 5: CLuster dominance*" %}

- ### Select the dominant cluster and produce a pandas dataframe (with the majority KMeans index)
```python
    water_level_pred2 = list(water_level_pred)
    majority_index = (max(set(water_level_pred2), key = water_level_pred2.count))
    kmeans_index = water_level_pred2
    df_index2['kmeans_index'] = kmeans_index
    df_majority1 = df_index2.drop(df_plota[df_plota.kmeans_index != majority_index].index)
    df_majority2 = df_majority1.drop(['kmeans_index'], axis = 1)
    df_majority2
```
- ### You can plot the data on (df_majority) or get the mean water level.
```python
    df_majority3 = df_majority2.groupby(df_majority2.time.dt.date).mean()
    df_mean_waterlevel = df_majority3.reset_index() # plot a bar representing the mean for each day of the month.
```
{% include figure image_path="assets/img/kmeasb/km6.PNG" alt="*Fig 6: daily mean*" caption="*Fig 6: daily mean*" %}

## Conclusion
**NB** : The cluster with the highest number of data points is assumed to be the desired cluster since the number of accurate data points collected by an IoT device is higher than the number of anomalous datapoints.

## Referenced works
- Giannoni, Federico & Mancini, Marco & Marinelli, Federico. (2018). Anomaly Detection Models for IoT Time Series Data.
- How to Determine the Optimal K for K-Means? -  by Khyati Mahendru (https://medium.com/analytics-vidhya/how-to-determine-the-optimal-k-for-k-means-708505d204eb).