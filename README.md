
### You must include a written description of three observable trends based on the data.

1. Rural rides have a higher variance on the fare, which makes sense as I took a taxi from small town USA to 2nd smallest town USA for 128$ in 2012 (Which would be an outlier in this dataset).

2. Fares/ Drivers/ Rides are all similar in proportion. Although correlation does not imply causation, in this case, it seems that the number of riders is highly correlated with the number of drivers.

3. Urban areas seem most dense in terms of number of rides per city, fares, and driver count.



```python
# Import Dependencies
import matplotlib.pyplot as plt
import pandas as pd
import numpy as np
import os
import matplotlib.cm as cm
```


```python
#read the files and make them into a df
csv1 = os.path.join('raw_data','ride_data.csv')
csv2 = os.path.join('raw_data','city_data.csv')
ride_df = pd.read_csv(csv1)
city_df = pd.read_csv(csv2)
```


```python
#Merge the two DF based on city
merge_df = pd.merge(ride_df, city_df, on="city")
merge_df.head()

```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>city</th>
      <th>date</th>
      <th>fare</th>
      <th>ride_id</th>
      <th>driver_count</th>
      <th>type</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Sarabury</td>
      <td>2016-01-16 13:49:27</td>
      <td>38.35</td>
      <td>5403689035038</td>
      <td>46</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Sarabury</td>
      <td>2016-07-23 07:42:44</td>
      <td>21.76</td>
      <td>7546681945283</td>
      <td>46</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Sarabury</td>
      <td>2016-04-02 04:32:25</td>
      <td>38.03</td>
      <td>4932495851866</td>
      <td>46</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Sarabury</td>
      <td>2016-06-23 05:03:41</td>
      <td>26.82</td>
      <td>6711035373406</td>
      <td>46</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Sarabury</td>
      <td>2016-09-30 12:48:34</td>
      <td>30.30</td>
      <td>6388737278232</td>
      <td>46</td>
      <td>Urban</td>
    </tr>
  </tbody>
</table>
</div>




```python
testmerge = merge_df.groupby('city')
testmerge2 = testmerge['ride_id'].count()
# testmerge2.sort_values('ride_id', ascending=False).head()
testmerge3 = pd.DataFrame(testmerge2)
testmerge3.sort_values('ride_id', ascending = False).head()

```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>ride_id</th>
    </tr>
    <tr>
      <th>city</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Port Johnstad</th>
      <td>34</td>
    </tr>
    <tr>
      <th>Swansonbury</th>
      <td>34</td>
    </tr>
    <tr>
      <th>South Louis</th>
      <td>32</td>
    </tr>
    <tr>
      <th>Port James</th>
      <td>32</td>
    </tr>
    <tr>
      <th>Alvarezhaven</th>
      <td>31</td>
    </tr>
  </tbody>
</table>
</div>




```python
#average $ per ride per city
meandf = merge_df[["city", "fare"]]
groupmean = meandf.groupby('city')
# meancitydf = groupmean.mean()
# meancitydf.head()
```


```python
#Total number of rides per city
ridespcity = merge_df.groupby("city")["ride_id"].count()
# ridedf = pd.DataFrame(ridespcity)
# ridedf.head()
```


```python
#Total drivers per city
driverspcity = merge_df.groupby("city")["driver_count"].max()
# tddf = pd.DataFrame(driverspcity)
# tddf.head()
```


```python
#Avg fare per city
avgpcity = merge_df.groupby("city")["fare"].mean()

# avgdf = pd.DataFrame(avgpcity)
# avgdf.head()

```


```python

typedf = merge_df.groupby("city")["type"].max()
# typedf2 = typedf.drop_duplicates(subset = ['city'])
# typedf2.head()

```


```python
plotdf = pd.concat([typedf, avgpcity, ridespcity, driverspcity], axis=1).reset_index()
plotdf.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>city</th>
      <th>type</th>
      <th>fare</th>
      <th>ride_id</th>
      <th>driver_count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Alvarezhaven</td>
      <td>Urban</td>
      <td>23.928710</td>
      <td>31</td>
      <td>21</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Alyssaberg</td>
      <td>Urban</td>
      <td>20.609615</td>
      <td>26</td>
      <td>67</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Anitamouth</td>
      <td>Suburban</td>
      <td>37.315556</td>
      <td>9</td>
      <td>16</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Antoniomouth</td>
      <td>Urban</td>
      <td>23.625000</td>
      <td>22</td>
      <td>21</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Aprilchester</td>
      <td>Urban</td>
      <td>21.981579</td>
      <td>19</td>
      <td>49</td>
    </tr>
  </tbody>
</table>
</div>




```python
x = plotdf['ride_id']
y = plotdf['fare']
colors = {'Urban':'Gold', 'Suburban':'lightskyblue', 'Rural':'lightcoral'}
colorpick = plotdf['type'].apply(lambda x: colors[x])
plotdf['colorpick'] = pd.Series(colorpick, index=plotdf.index)
plotdf.head()

```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>city</th>
      <th>type</th>
      <th>fare</th>
      <th>ride_id</th>
      <th>driver_count</th>
      <th>colorpick</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Alvarezhaven</td>
      <td>Urban</td>
      <td>23.928710</td>
      <td>31</td>
      <td>21</td>
      <td>Gold</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Alyssaberg</td>
      <td>Urban</td>
      <td>20.609615</td>
      <td>26</td>
      <td>67</td>
      <td>Gold</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Anitamouth</td>
      <td>Suburban</td>
      <td>37.315556</td>
      <td>9</td>
      <td>16</td>
      <td>lightskyblue</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Antoniomouth</td>
      <td>Urban</td>
      <td>23.625000</td>
      <td>22</td>
      <td>21</td>
      <td>Gold</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Aprilchester</td>
      <td>Urban</td>
      <td>21.981579</td>
      <td>19</td>
      <td>49</td>
      <td>Gold</td>
    </tr>
  </tbody>
</table>
</div>




```python
plt.figure(figsize = (10,10))

plt.title("Pyber Ridesharing Data 2016")
plt.xlabel("Total Number of Rides (Per City)")
plt.ylabel("Average Fare ($)")



# for i in colors:
        
#             plt.scatter(x,y, s=plotdf['driver_count']*5,  c=plotdf['type'].apply(lambda x: colors[x]), alpha = .7, edgecolors = 'black', label = i)
            

# plt.legend()
# plt.scatter(x,y, s=plotdf['driver_count']*5,  c=plotdf['type'].apply(lambda x: colors[x]), alpha = .7, edgecolors = 'black')
plt.scatter(x,y, s=plotdf['driver_count']*5,  c="Gold", alpha = .7, edgecolors = 'black', label = "Urban")
plt.scatter(x,y, s=plotdf['driver_count']*5,  c="lightskyblue", alpha = .7, edgecolors = 'black', label = "Suburban")
plt.scatter(x,y, s=plotdf['driver_count']*5,  c="lightcoral", alpha = .7, edgecolors = 'black', label = "Rural")

plt.legend()
plt.scatter(x,y, s=plotdf['driver_count']*5,  c=plotdf['type'].apply(lambda x: colors[x]), alpha = .7, edgecolors = 'black')


        



plt.show()
```


![png](plotHW_files/plotHW_12_0.png)



```python
#total fares by city type
faresum = merge_df.groupby("type")
faredf = faresum["fare"].sum()
farepie = pd.DataFrame(faredf).reset_index()
farepie

```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>type</th>
      <th>fare</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Rural</td>
      <td>4255.09</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Suburban</td>
      <td>19317.88</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Urban</td>
      <td>40078.34</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Labels for the sections of our pie chart
labels = farepie["type"]

# The values of each section of the pie chart
sizes = farepie["fare"]

# The colors of each section of the pie chart
colors = ["Gold", "lightskyblue", "lightcoral"]

# Tells matplotlib to seperate the "Python" section from the others
explode = (0, 0, 0.1)
```


```python
# Tell matplotlib to create a bar chart based upon the above data
plt.pie(sizes, explode=explode, labels=labels, colors=colors,
        autopct="%1.1f%%", shadow=True, startangle=140)
```




    ([<matplotlib.patches.Wedge at 0x1d6c747c550>,
      <matplotlib.patches.Wedge at 0x1d6c747cf98>,
      <matplotlib.patches.Wedge at 0x1d6c7484a58>],
     [Text(-0.97154,0.515859,'Rural'),
      Text(-0.858531,-0.687695,'Suburban'),
      Text(1.0724,0.538475,'Urban')],
     [Text(-0.529931,0.281378,'6.7%'),
      Text(-0.46829,-0.375106,'30.3%'),
      Text(0.625568,0.31411,'63.0%')])




```python
plt.title("% of Total Fares by City Type")

plt.savefig("PyPies.png")
plt.show()
```


![png](plotHW_files/plotHW_16_0.png)



```python
sizes2 = ridepie["ride_id"]
```


```python
#total rides by city type
ridesum = merge_df.groupby("type")
ridecdf = faresum["ride_id"].count()
ridepie = pd.DataFrame(ridecdf).reset_index()
ridepie

# Tell matplotlib to create a bar chart based upon the above data
plt.pie(sizes2, explode=explode, labels=labels, colors=colors,
        autopct="%1.1f%%", shadow=True, startangle=140)

plt.title("% of Total rides by City Type")

plt.savefig("PyPies2.png")
plt.show()

```


![png](plotHW_files/plotHW_18_0.png)



```python
#total drivers by city type
drivecmax = merge_df.groupby("type")
drivecdf = drivecmax["driver_count"].max()
drivepie = pd.DataFrame(drivecdf).reset_index()
drivepie
sizes3 = drivepie["driver_count"]
# Tell matplotlib to create a bar chart based upon the above data
plt.pie(sizes3, explode=explode, labels=labels, colors=colors,
        autopct="%1.1f%%", shadow=True, startangle=140)

plt.title("% of Total Drivers by City Type")

plt.savefig("PyPies2.png")
plt.show()
```


![png](plotHW_files/plotHW_19_0.png)

