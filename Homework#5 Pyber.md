

```python
# Dependencies
from matplotlib import pyplot as plt
from scipy import stats
import numpy as np
import pandas as pd
```


```python
city_data = pd.read_csv("city_data.csv")
ride_data = pd.read_csv("ride_data.csv")

```


```python
avg_ride_data = ride_data.groupby("city").agg({'fare':'mean'})
avg_ride_data  = avg_ride_data.round(2)
avg_ride_data.head(5)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>fare</th>
    </tr>
    <tr>
      <th>city</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Alvarezhaven</th>
      <td>23.93</td>
    </tr>
    <tr>
      <th>Alyssaberg</th>
      <td>20.61</td>
    </tr>
    <tr>
      <th>Anitamouth</th>
      <td>37.32</td>
    </tr>
    <tr>
      <th>Antoniomouth</th>
      <td>23.62</td>
    </tr>
    <tr>
      <th>Aprilchester</th>
      <td>21.98</td>
    </tr>
  </tbody>
</table>
</div>




```python
avg_ride_data  = avg_ride_data .rename(
    columns={"fare": "Avg Fare"})
```


```python
avg_ride_data2 = ride_data.groupby("city").agg({'fare':'count'})
avg_ride_data3  = avg_ride_data2.rename(
    columns={"fare": "Total # of Fare's"})
avg_ride_data3 .head(5)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Total # of Fare's</th>
    </tr>
    <tr>
      <th>city</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Alvarezhaven</th>
      <td>31</td>
    </tr>
    <tr>
      <th>Alyssaberg</th>
      <td>26</td>
    </tr>
    <tr>
      <th>Anitamouth</th>
      <td>9</td>
    </tr>
    <tr>
      <th>Antoniomouth</th>
      <td>22</td>
    </tr>
    <tr>
      <th>Aprilchester</th>
      <td>19</td>
    </tr>
  </tbody>
</table>
</div>




```python
avg_ride_data = avg_ride_data.reset_index()
avg_ride_data3 = avg_ride_data3.reset_index()
merged_ride = avg_ride_data.merge(avg_ride_data3, on="city")
merged_ride.head(5)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>city</th>
      <th>Avg Fare</th>
      <th>Total # of Fare's</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Alvarezhaven</td>
      <td>23.93</td>
      <td>31</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Alyssaberg</td>
      <td>20.61</td>
      <td>26</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Anitamouth</td>
      <td>37.32</td>
      <td>9</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Antoniomouth</td>
      <td>23.62</td>
      <td>22</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Aprilchester</td>
      <td>21.98</td>
      <td>19</td>
    </tr>
  </tbody>
</table>
</div>




```python
city_data = city_data.reset_index()
merged_ride = merged_ride.reset_index()
data_final = city_data.merge(merged_ride, on="city")

data_final.head(5)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>index_x</th>
      <th>city</th>
      <th>driver_count</th>
      <th>type</th>
      <th>index_y</th>
      <th>Avg Fare</th>
      <th>Total # of Fare's</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>Kelseyland</td>
      <td>63</td>
      <td>Urban</td>
      <td>33</td>
      <td>21.81</td>
      <td>28</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>Nguyenbury</td>
      <td>8</td>
      <td>Urban</td>
      <td>63</td>
      <td>25.90</td>
      <td>26</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>East Douglas</td>
      <td>12</td>
      <td>Urban</td>
      <td>14</td>
      <td>26.17</td>
      <td>22</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>West Dawnfurt</td>
      <td>34</td>
      <td>Urban</td>
      <td>110</td>
      <td>22.33</td>
      <td>29</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>Rodriguezburgh</td>
      <td>52</td>
      <td>Urban</td>
      <td>80</td>
      <td>21.33</td>
      <td>23</td>
    </tr>
  </tbody>
</table>
</div>




```python
data_final_urban = data_final.loc[data_final["type"] == "Urban", :]
data_final_suburban = data_final.loc[data_final["type"] == "Suburban", :]
data_final_rural = data_final.loc[data_final["type"] == "Rural", :]
```


```python
x = data_final_urban["Total # of Fare's"]
y = data_final_urban["Avg Fare"]
count = data_final_urban["driver_count"]

x2 = data_final_suburban["Total # of Fare's"]
y2 = data_final_suburban["Avg Fare"]
count2 = data_final_suburban["driver_count"]

x3 = data_final_rural["Total # of Fare's"]
y3 = data_final_rural["Avg Fare"]
count3 = data_final_rural["driver_count"]

plt.scatter(x, y,count, linewidths=1, color='b', alpha=0.75, label="Urban")
plt.scatter(x2, y2,count, linewidths=1, color='r', alpha=0.75, label="Suburban")
plt.scatter(x3, y3,count, linewidths=1, color='g', alpha=0.75, label="Rural")

plt.legend()

plt.xlim(0, 40)
plt.ylim(15, 55)

plt.title ('Pyber Ride Sharing Data 2016')
plt.xlabel('Total Number Rides per City')
plt.ylabel('Average Fare $')
plt.grid()
plt.figtext(0.7, 1, '*Note dot size corresponds to number of drivers in city', horizontalalignment='right')

plt.savefig("PyberRideShareData.png")
plt.show()

```


![png](output_8_0.png)



```python
#% of Fares by City Type
total = x.sum()
total2 = x2.sum()
total3 = x3.sum()

types1 = ["Urban", "Suburban", "Rural"]
fare_totals = [total, total2, total3]
colors = ["yellowgreen", "red", "blue"]
#explode = (0.0, 0.2, 0, 0)
```


```python
plt.title("% of Total Fares by City Type")
plt.pie(fare_totals, labels=types1, colors=colors,
        autopct="%1.1f%%", shadow=True, startangle=90)
plt.axis("equal")
plt.show()
```


![png](output_10_0.png)



```python
#% of Rides by City Type
totala = x.count()
totalb = x2.count()
totalc = x3.count()

types1 = ["Urban", "Suburban", "Rural"]
fare_totals2 = [totala, totalb, totalc]
colors = ["yellow", "blue", "orange"]
#explode = (0.0, 0.2, 0, 0)
```


```python
plt.title("% of Total Rides by City Type")
plt.pie(fare_totals2, labels=types1, colors=colors,
        autopct="%1.1f%%", shadow=True, startangle=90)
plt.axis("equal")
plt.show()
```


![png](output_12_0.png)



```python
#% of Total Drivers by City Type

drivers = count.sum()
drivers2 = count2.sum()
drivers3 = count3.sum()

types1 = ["Urban", "Suburban", "Rural"]
drivers_total = [drivers, drivers2, drivers3]
colors = ["red", "blue", "green"]
#explode = (0.0, 0.2, 0, 0)
```


```python
plt.title("% of Total Drivers by City Type")
plt.pie(drivers_total, labels=types1, colors=colors,
        autopct="%1.1f%%", shadow=True, startangle=90)
plt.axis("equal")
plt.show()
```


![png](output_14_0.png)

