

```python
# Import Dependencies
import pandas as pd
import numpy as np
```


```python
# Load in file
file = "Homework4.csv"
```


```python
file_pd = pd.read_csv(file)
file_pd.head()
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
      <th>SN</th>
      <th>Age</th>
      <th>Gender</th>
      <th>Item ID</th>
      <th>Item Name</th>
      <th>Price</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Aelalis34</td>
      <td>38</td>
      <td>Male</td>
      <td>165</td>
      <td>Bone Crushing Silver Skewer</td>
      <td>3.37</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Eolo46</td>
      <td>21</td>
      <td>Male</td>
      <td>119</td>
      <td>Stormbringer, Dark Blade of Ending Misery</td>
      <td>2.32</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Assastnya25</td>
      <td>34</td>
      <td>Male</td>
      <td>174</td>
      <td>Primitive Blade</td>
      <td>2.46</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Pheusrical25</td>
      <td>21</td>
      <td>Male</td>
      <td>92</td>
      <td>Final Critic</td>
      <td>1.36</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Aela59</td>
      <td>23</td>
      <td>Male</td>
      <td>63</td>
      <td>Stormfury Mace</td>
      <td>1.27</td>
    </tr>
  </tbody>
</table>
</div>




```python
file_pd.columns
```




    Index(['SN', 'Age', 'Gender', 'Item ID', 'Item Name', 'Price'], dtype='object')




```python
 #Player Count
Total_Players = len(file_pd["SN"].unique())
print(Total_Players)
```

    573



```python
#Purchasing Analysis (Total) = Total Items Purchased
file_pd["Item ID"].count()
```




    780




```python
#Purchasing Analysis (Total) = Total Revenue
total_rev = file_pd["Price"].sum()
print(total_rev)
```

    2286.33



```python
 #Purchasing Analysis (Total) = Calculate the number of unique items.
Unique_Items = len(file_pd["Item Name"].unique())
print(Unique_Items)
```

    179



```python
#Purchasing Analysis (Total) = Total Items Purchased
file_pd["Price"].mean()
```




    2.931192307692303




```python
#Gender Demographics = Gender Counts
file_pd['Gender'].value_counts()
```




    Male                     633
    Female                   136
    Other / Non-Disclosed     11
    Name: Gender, dtype: int64




```python
#Gender Demographics = Percentage Breakdown
file_pd['Gender'].value_counts()/file_pd["Gender"].count()

```




    Male                     0.811538
    Female                   0.174359
    Other / Non-Disclosed    0.014103
    Name: Gender, dtype: float64




```python
 # Purchasing Analysis (Gender) - The below each broken by gender:
#Purchase Count
#Average Purchase Price
#Total Purchase Value
#Normalized Totals


#Male Data
male_df = file_pd.loc[file_pd["Gender"] == "Male", :]
male_df

total_male_purchases = male_df["Item ID"].count()
average_male_purchase_price = male_df["Price"].mean()
total_male_purchase_value = male_df["Price"].sum()

print(total_male_purchases)
print(average_male_purchase_price)
print(total_male_purchase_value)
```

    633
    2.9505213270142154
    1867.68



```python
#Female Data
female_df = file_pd.loc[file_pd["Gender"] == "Female", :]
female_df

total_female_purchases = female_df["Item ID"].count()
average_female_purchase_price = female_df["Price"].mean()
total_female_purchase_value = female_df["Price"].sum()

print(total_female_purchases)
print(average_female_purchase_price)
print(total_female_purchase_value)
```

    136
    2.815514705882352
    382.90999999999997



```python
#Other / Non-Disclosed

other_df = file_pd.loc[file_pd["Gender"] == "Other / Non-Disclosed", :]
other_df

total_other_purchases = other_df["Item ID"].count()
average_other_purchase_price = other_df["Price"].mean()
total_other_purchase_value = other_df["Price"].sum()

print(total_other_purchases)
print(average_other_purchase_price)
print(average_other_purchase_price)
```

    11
    3.2490909090909086
    3.2490909090909086



```python
male_data_normalized = pd.DataFrame({
      "Purchase Count":[total_male_purchases],
      "Average Purchase Price":[average_male_purchase_price],
      "Total Purchase Value":[total_male_purchase_value],
})
male_data_normalized.head()
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
      <th>Average Purchase Price</th>
      <th>Purchase Count</th>
      <th>Total Purchase Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2.950521</td>
      <td>633</td>
      <td>1867.68</td>
    </tr>
  </tbody>
</table>
</div>




```python
female_data_normalized = pd.DataFrame({
      "Purchase Count":[total_female_purchases],
      "Average Purchase Price":[average_female_purchase_price],
      "Total Purchase Value":[total_female_purchase_value],
})
female_data_normalized.head()
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
      <th>Average Purchase Price</th>
      <th>Purchase Count</th>
      <th>Total Purchase Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2.815515</td>
      <td>136</td>
      <td>382.91</td>
    </tr>
  </tbody>
</table>
</div>




```python
other_data_normalized = pd.DataFrame({
      "Purchase Count":[total_other_purchases],
      "Average Purchase Price":[average_other_purchase_price],
      "Total Purchase Value":[total_other_purchase_value],
})
other_data_normalized.head()
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
      <th>Average Purchase Price</th>
      <th>Purchase Count</th>
      <th>Total Purchase Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>3.249091</td>
      <td>11</td>
      <td>35.74</td>
    </tr>
  </tbody>
</table>
</div>




```python
net_data_normalized = pd.DataFrame({
      "Purchase Count":[total_male_purchases, total_female_purchases, total_other_purchases],
      "Average Purchase Price":[average_male_purchase_price, average_female_purchase_price, average_other_purchase_price],
      "Total Purchase Value":[total_male_purchase_value, total_female_purchase_value, total_other_purchase_value],
})
net_data_normalized.head()
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
      <th>Average Purchase Price</th>
      <th>Purchase Count</th>
      <th>Total Purchase Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2.950521</td>
      <td>633</td>
      <td>1867.68</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2.815515</td>
      <td>136</td>
      <td>382.91</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3.249091</td>
      <td>11</td>
      <td>35.74</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Age Demographics

#The below each broken into bins of 4 years (i.e. <10, 10-14, 15-19, etc.) 

#Purchase Count
#Average Purchase Price
#Total Purchase Value
#Normalized Totals

# Create the bins in which Data will be held
bins = [0, 10, 14, 18, 22, 26, 30, 34, 38, 42, 46, 50, 100]

group_labels = ["0 - 10", "10 - 14", "14 - 18", "18 - 22", "22 - 26", "26 - 30",
                "30 - 34", "34 - 38", "38 - 42", "42 - 46", "46 - 50", "50 - 100",]
```


```python
# Slice the data and place it into bins
pd.cut(file_pd["Age"], bins, labels=group_labels).head()
```




    0    34 - 38
    1    18 - 22
    2    30 - 34
    3    18 - 22
    4    22 - 26
    Name: Age, dtype: category
    Categories (12, object): [0 - 10 < 10 - 14 < 14 - 18 < 18 - 22 ... 38 - 42 < 42 - 46 < 46 - 50 < 50 - 100]




```python
# Place the data series into a new column inside of the DataFrame
file_pd["View Group"] = pd.cut(file_pd["Age"], bins, labels=group_labels)
file_pd.head()
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
      <th>SN</th>
      <th>Age</th>
      <th>Gender</th>
      <th>Item ID</th>
      <th>Item Name</th>
      <th>Price</th>
      <th>View Group</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Aelalis34</td>
      <td>38</td>
      <td>Male</td>
      <td>165</td>
      <td>Bone Crushing Silver Skewer</td>
      <td>3.37</td>
      <td>34 - 38</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Eolo46</td>
      <td>21</td>
      <td>Male</td>
      <td>119</td>
      <td>Stormbringer, Dark Blade of Ending Misery</td>
      <td>2.32</td>
      <td>18 - 22</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Assastnya25</td>
      <td>34</td>
      <td>Male</td>
      <td>174</td>
      <td>Primitive Blade</td>
      <td>2.46</td>
      <td>30 - 34</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Pheusrical25</td>
      <td>21</td>
      <td>Male</td>
      <td>92</td>
      <td>Final Critic</td>
      <td>1.36</td>
      <td>18 - 22</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Aela59</td>
      <td>23</td>
      <td>Male</td>
      <td>63</td>
      <td>Stormfury Mace</td>
      <td>1.27</td>
      <td>22 - 26</td>
    </tr>
  </tbody>
</table>
</div>




```python
 # Create a GroupBy object based upon "View Group"
age_group = file_pd.groupby("View Group")

#Total Purchase Value
purchase_count_age = age_group["Item ID"].count()


average_purchase_age = age_group["Price"].mean()


total_purchase_value_age = age_group["Price"].sum()

```


```python
 # Normalized Summary of Age Demographics
age_summary_table = pd.DataFrame({"Purchase Count by Age": purchase_count_age,
                                    "Average Purchase by Age": average_purchase_age,
                                  "Total Purchase by Age": total_purchase_value_age,
                                 })
age_summary_table.head()
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
      <th>Average Purchase by Age</th>
      <th>Purchase Count by Age</th>
      <th>Total Purchase by Age</th>
    </tr>
    <tr>
      <th>View Group</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0 - 10</th>
      <td>3.019375</td>
      <td>32</td>
      <td>96.62</td>
    </tr>
    <tr>
      <th>10 - 14</th>
      <td>2.702903</td>
      <td>31</td>
      <td>83.79</td>
    </tr>
    <tr>
      <th>14 - 18</th>
      <td>2.876757</td>
      <td>111</td>
      <td>319.32</td>
    </tr>
    <tr>
      <th>18 - 22</th>
      <td>2.927273</td>
      <td>231</td>
      <td>676.20</td>
    </tr>
    <tr>
      <th>22 - 26</th>
      <td>2.937295</td>
      <td>207</td>
      <td>608.02</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Top Spenders

#Identify the the top 5 spenders in the game by total purchase value, then list (in a table):

#SN
#Purchase Count
#Average Purchase Price
#Total Purchase Value

# Grouping the DataFrame by "Assignee"
SN_group_2 = file_pd.groupby("SN").agg({'Price':'sum','Item ID': 'count'})
SN_group_3 = file_pd.groupby("SN").agg({'Price':'mean'})
SN_group_3  = SN_group_3.round(2)
```


```python
#Sorted
top_sorted = SN_group_2.sort_values(by='Price', ascending=False,) 
top_sorted2 = SN_group_3.sort_values(by='Price', ascending=False,) 
top_sorted  = top_sorted .rename(
    columns={"Price": "Total Purchase Value", "Item ID": "Purchase Count"})
top_sorted2  = top_sorted2 .rename(
    columns={"Price": "Average Purchase Price"})

top_sorted = top_sorted.reset_index()
top_sorted2 = top_sorted2.reset_index()
top_sorted = top_sorted.merge(top_sorted2, on="SN")

top_sorted.head(5)
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
      <th>SN</th>
      <th>Total Purchase Value</th>
      <th>Purchase Count</th>
      <th>Average Purchase Price</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Undirrala66</td>
      <td>17.06</td>
      <td>5</td>
      <td>3.41</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Saedue76</td>
      <td>13.56</td>
      <td>4</td>
      <td>3.39</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Mindimnya67</td>
      <td>12.74</td>
      <td>4</td>
      <td>3.18</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Haellysu29</td>
      <td>12.73</td>
      <td>3</td>
      <td>4.24</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Eoda93</td>
      <td>11.58</td>
      <td>3</td>
      <td>3.86</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Most Popular Items
#Identify the 5 most popular items by purchase count, then list (in a table):
#Item ID
#Item Name
#Purchase Count (Item ID)
#Item Price
#Total Purchase Value (Price.sum)

Pop_group_1 = file_pd.groupby("Item Name").agg({'Item ID': 'count','Price':'sum'})
```


```python
Pop_group_2 = file_pd.groupby("Item Name").agg({'Price':'unique'})
```


```python
#Sorted
pop_sorted = Pop_group_1.sort_values(by='Item ID', ascending=False,) 
pop_sorted  = pop_sorted .rename(
     columns={"Item ID": "Purchase Count","Price": "Total Purchase Value"})
Pop_group_2  = Pop_group_2 .rename(
     columns={"Price": "Item Price"})

pop_sorted = pop_sorted.reset_index()
Pop_group_2 = Pop_group_2.reset_index()
pop_sorted = pop_sorted.merge(Pop_group_2, on="Item Name")

pop_sorted.head(5)
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
      <th>Item Name</th>
      <th>Purchase Count</th>
      <th>Total Purchase Value</th>
      <th>Item Price</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Final Critic</td>
      <td>14</td>
      <td>38.60</td>
      <td>[1.36, 4.62]</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Arcane Gem</td>
      <td>11</td>
      <td>24.53</td>
      <td>[2.23]</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Betrayal, Whisper of Grieving Widows</td>
      <td>11</td>
      <td>25.85</td>
      <td>[2.35]</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Stormcaller</td>
      <td>10</td>
      <td>34.65</td>
      <td>[4.15, 2.78]</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Woeful Adamantite Claymore</td>
      <td>9</td>
      <td>11.16</td>
      <td>[1.24]</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Most Profitable Items
#Identify the 5 most profitable items by total purchase value, then list (in a table):
#Item ID
#Item Name
#Purchase Count
#Item Price
#Total Purchase Value

Pro_group_1 = file_pd.groupby("Item ID").agg({'Item ID': 'count','Price':'sum'})
Pro_group_2 = file_pd.groupby("Item ID").agg({'Price':'unique','Item Name':'unique'})

```


```python
#Sorted
pro_sorted = Pro_group_1.sort_values(by='Item ID', ascending=False,) 
pro_sorted  = pro_sorted .rename(
     columns={"Item ID": "Purchase Count","Price": "Total Purchase Value"})
Pro_group_2  = Pro_group_2 .rename(
     columns={"Price": "Item Price"})

pro_sorted = pro_sorted .reset_index()
Pro_group_2 = Pro_group_2.reset_index()
pro_sorted = pro_sorted.merge(Pro_group_2, on="Item ID")

pro_sorted.head(5)
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
      <th>Item ID</th>
      <th>Purchase Count</th>
      <th>Total Purchase Value</th>
      <th>Item Price</th>
      <th>Item Name</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>39</td>
      <td>11</td>
      <td>25.85</td>
      <td>[2.35]</td>
      <td>[Betrayal, Whisper of Grieving Widows]</td>
    </tr>
    <tr>
      <th>1</th>
      <td>84</td>
      <td>11</td>
      <td>24.53</td>
      <td>[2.23]</td>
      <td>[Arcane Gem]</td>
    </tr>
    <tr>
      <th>2</th>
      <td>31</td>
      <td>9</td>
      <td>18.63</td>
      <td>[2.07]</td>
      <td>[Trickster]</td>
    </tr>
    <tr>
      <th>3</th>
      <td>175</td>
      <td>9</td>
      <td>11.16</td>
      <td>[1.24]</td>
      <td>[Woeful Adamantite Claymore]</td>
    </tr>
    <tr>
      <th>4</th>
      <td>13</td>
      <td>9</td>
      <td>13.41</td>
      <td>[1.49]</td>
      <td>[Serenity]</td>
    </tr>
  </tbody>
</table>
</div>


