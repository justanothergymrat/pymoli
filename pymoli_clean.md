

```python
#######################################
##                         Observations
#######################################
```


```python
# pertaining to purchase_data.json:

# 1. Vast  majority of players are male

# 2. "Other"/"Non-Disclosed" gender has higher average & normalized purchased

# 3. Majority of purchases were made by ages 25-29
```


```python
#######################################
##                              imports
#######################################
```


```python
import pandas as pd
import numpy as np
import os

```


```python
data_file = os.path.join("resources","purchase_data.json")
```


```python
#######################################
##                         Player count
#######################################
```


```python
purchase_pd = pd.read_json(data_file)
# purchase_pd.columns
total_transactions = purchase_pd['Price'].count()
# total_transactions
total_players = len(purchase_pd['SN'].value_counts())
# total_players

#make it look like your example on the homework PDF
total_players_df = pd.DataFrame(
    {"Total Players" : [total_players]},
     index = [0])

total_players_df
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
      <th>Total Players</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>573</td>
    </tr>
  </tbody>
</table>
</div>




```python
#######################################
##           Purchasing Analysis(total)
#######################################
```


```python
number_of_unique_items = len(purchase_pd['Item Name'].unique())
# number_of_unique_items
average_purchase_price = purchase_pd['Price'].mean()
# average_purchase_price
total_transactions = purchase_pd['Price'].count()
# total_transactions
total_revenue = purchase_pd['Price'].sum()
# total_revenue

#make it look like your example on the homework PDF
purchasing_analysis_df = pd.DataFrame(
    {"Number of Unique Items" : [number_of_unique_items],
     "Average Price": [average_purchase_price],
     "Number of Purchases": [total_transactions],
     "Total Revenue": [total_revenue]
    },
     index = [0])

purchasing_analysis_df["Average Price"] = purchasing_analysis_df["Average Price"].map("$ {:,.2f}".format)
purchasing_analysis_df["Total Revenue"] = purchasing_analysis_df["Total Revenue"].map("$ {:,.2f}".format)

purchasing_analysis_df
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
      <th>Average Price</th>
      <th>Number of Purchases</th>
      <th>Number of Unique Items</th>
      <th>Total Revenue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>$ 2.93</td>
      <td>780</td>
      <td>179</td>
      <td>$ 2,286.33</td>
    </tr>
  </tbody>
</table>
</div>




```python
#######################################
##                  Gender Demographics
#######################################
```


```python
number_of_players2 = purchase_pd.groupby(['Gender', 'SN']).count()
# number_of_players2
total_number_of_players = len(number_of_players2)
# total_number_of_players
gender_breakdown = number_of_players2.unstack('Gender')
# gender_breakdown
gender_counts_male = gender_breakdown['Age']['Male'].count()
# gender_counts_male
gender_counts_female = gender_breakdown['Age']['Female'].count()
# gender_counts_female
gender_counts_other = gender_breakdown['Age']['Other / Non-Disclosed'].count()
# gender_counts_other
gender_percent_male =  (gender_counts_male / total_number_of_players)*100
# gender_percent_male
gender_percent_female =  (gender_counts_female / total_number_of_players)*100
# gender_percent_female
gender_percent_other =  (gender_counts_other / total_number_of_players)*100
# gender_percent_other

#make it look like your example on the homework PDF
gender_demographics_df = pd.DataFrame(
    {"Percentage of Players" : [gender_percent_male, gender_percent_female, gender_percent_other],
     "Total Count": [gender_counts_male, gender_counts_female, gender_counts_other],
    },
     index = ["Male", "Female", "Other / Non-Disclosed"])

gender_demographics_df["Percentage of Players"] = gender_demographics_df["Percentage of Players"].map("{0:.02f}%".format)
# gender_demographics_df["Total Revenue"] = gender_demographics_df["Total Revenue"].map("{0:.02f}%".format)


gender_demographics_df
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
      <th>Percentage of Players</th>
      <th>Total Count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Male</th>
      <td>81.15%</td>
      <td>465</td>
    </tr>
    <tr>
      <th>Female</th>
      <td>17.45%</td>
      <td>100</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>1.40%</td>
      <td>8</td>
    </tr>
  </tbody>
</table>
</div>




```python
#######################################
##         Purchasing Analysis (gender)
#######################################
```


```python
gender_purchase2 = purchase_pd.groupby(['Gender', 'SN']).count()
# print(gender_purchase2)
gender_purchase_count_female = gender_purchase2['Item Name']['Female'].sum()
# gender_purchase_count_female
gender_purchase_count_male = gender_purchase2['Item Name']['Male'].sum()
# gender_purchase_count_male
gender_purchase_count_other = gender_purchase2['Item Name']['Other / Non-Disclosed'].sum()
# gender_purchase_count_other

gender_purchase = purchase_pd.groupby(['Gender', 'SN']).sum()
# gender_purchase.head()
total_purchase_female = gender_purchase['Price']['Female'].sum()
# total_purchase_female                      
total_purchase_male = gender_purchase['Price']['Male'].sum()
# total_purchase_male
total_purchase_other = gender_purchase['Price']['Other / Non-Disclosed'].sum()
# total_purchase_other

gender_purchase_count = purchase_pd.groupby(['Gender']).count()
# gender_purchase_count
gender_purchase_totals_mean = purchase_pd.groupby(['Gender']).mean()
# gender_purchase_totals_mean.filter(regex='Price')
gender_purchase_totals_sum = purchase_pd.groupby(['Gender']).sum()
# gender_purchase_totals_sum.filter(regex='Price')

#make it look like your example on the homework PDF
gender_purchase_count["Average Purchase Price"] = [
    gender_purchase_totals_mean['Price']['Female'],
    gender_purchase_totals_mean['Price']['Male'],
    gender_purchase_totals_mean['Price']['Other / Non-Disclosed']
]
# added in average purchase row 
gender_purchase_count["Total Purchase Value"] = [
    gender_purchase_totals_sum['Price']['Female'],
    gender_purchase_totals_sum['Price']['Male'],
    gender_purchase_totals_sum['Price']['Other / Non-Disclosed']
]
# added in total purchase row
gender_purchase_count["Normalized Total (purchase total per gender / player count per gender)"] = [
    gender_purchase_totals_sum['Price']['Female'] / int(gender_breakdown['Age']['Female'].count()),
    gender_purchase_totals_sum['Price']['Male'] / int(gender_breakdown['Age']['Male'].count()),
    gender_purchase_totals_sum['Price']['Other / Non-Disclosed'] / int(gender_breakdown['Age']['Other / Non-Disclosed'].count())]
# added in 'normalized totals' as per carlos in slack.
gender_purchase_count["Average Purchase Price"] = gender_purchase_count["Average Purchase Price"].map("$ {:,.2f}".format)
gender_purchase_count["Total Purchase Value"] = gender_purchase_count["Total Purchase Value"].map("$ {:,.2f}".format)
gender_purchase_count["Normalized Total (purchase total per gender / player count per gender)"] = gender_purchase_count["Normalized Total (purchase total per gender / player count per gender)"].map("$ {:,.2f}".format)
gender_purchase_count = gender_purchase_count.rename(columns = {'SN':'Purchase Count'})
gender_purchase_count.drop(['Age', 'Item ID', 'Item Name', 'Price'], axis=1, inplace=True)
gender_purchase_count
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
      <th>Purchase Count</th>
      <th>Average Purchase Price</th>
      <th>Total Purchase Value</th>
      <th>Normalized Total (purchase total per gender / player count per gender)</th>
    </tr>
    <tr>
      <th>Gender</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Female</th>
      <td>136</td>
      <td>$ 2.82</td>
      <td>$ 382.91</td>
      <td>$ 3.83</td>
    </tr>
    <tr>
      <th>Male</th>
      <td>633</td>
      <td>$ 2.95</td>
      <td>$ 1,867.68</td>
      <td>$ 4.02</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>11</td>
      <td>$ 3.25</td>
      <td>$ 35.74</td>
      <td>$ 4.47</td>
    </tr>
  </tbody>
</table>
</div>




```python
#######################################
##                     Age Demographics
#######################################
```


```python
#set it up
age = purchase_pd.groupby(['SN', 'Age']).var()
age_breakdown = age.drop(['Item ID','Price'], axis=1).reset_index("Age")
# age_breakdown.head()
#get bins prepped
bins = [0, 9, 14, 19, 20, 29, 34, 39, 100]
group_names = ['<10', '10-14', '15-19', '20-24', '25-29','30-34','35-39','40+']
# Cut into the bins
age_breakdown["Age range"] = pd.cut(age_breakdown["Age"], bins, labels=group_names)
age_breakdown.head()
age_population = len(age_breakdown)
#make it look like your example on the homework PDF
age_chart = age_breakdown.groupby(['Age range']).count()
age_chart["Percentage of Players"] = [
    age_chart['Age']['<10'] / age_population * 100,
    age_chart['Age']['10-14'] / age_population * 100,
    age_chart['Age']['15-19'] / age_population * 100,
    age_chart['Age']['20-24'] / age_population * 100,
    age_chart['Age']['25-29'] / age_population * 100,
    age_chart['Age']['30-34'] / age_population * 100,
    age_chart['Age']['35-39'] / age_population * 100,
    age_chart['Age']['40+'] / age_population * 100
    
]
age_chart["Percentage of Players"] = age_chart["Percentage of Players"].map("{0:.02f}%".format)
age_chart = age_chart.rename(columns = {'Age':'Total Count'})
cols = age_chart.columns.tolist()
cols = cols[-1:] + cols[:-1]
age_chart = age_chart[cols]
age_chart
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
      <th>Percentage of Players</th>
      <th>Total Count</th>
    </tr>
    <tr>
      <th>Age range</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;10</th>
      <td>3.32%</td>
      <td>19</td>
    </tr>
    <tr>
      <th>10-14</th>
      <td>4.01%</td>
      <td>23</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>17.45%</td>
      <td>100</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>12.74%</td>
      <td>73</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>47.64%</td>
      <td>273</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>8.20%</td>
      <td>47</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>4.71%</td>
      <td>27</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>1.92%</td>
      <td>11</td>
    </tr>
  </tbody>
</table>
</div>




```python
#######################################
##            Purchasing Analysis (Age)
#######################################
```


```python
pa_age = purchase_pd
pa_age["Age range"] = pd.cut(purchase_pd['Age'], bins, labels=group_names)
pa_age_groups = pa_age.groupby("Age range")
pa_age_counts = pa_age_groups.count()
pa_age_sum = pa_age_groups.sum()
pa_age_avg = pa_age_groups.mean()
# pa_age_counts['Age']
pa_age_normalized_total = (pa_age_sum['Price'] / pa_age_counts['Age'])
# print(pa_age_normalized_total)

#make it look like your example on the homework PDF
pa_age_chart = pd.concat([pa_age_counts['Age'],pa_age_avg['Price']], axis=1)
pa_age_chart = pa_age_chart.rename(columns = {'Age':'Purchase Count', 'Price':'Average Purchase Price'})
pa_age_chart = pd.concat([pa_age_chart,pa_age_sum['Price']], axis=1)
pa_age_chart = pa_age_chart.rename(columns = {'Price':'Total Purchase Value'})
pa_age_chart = pd.concat([pa_age_chart,pa_age_normalized_total], axis=1)
pa_age_chart = pa_age_chart.rename(columns = {0:'Normalized Totals'})
pa_age_chart["Average Purchase Price"] = pa_age_chart["Average Purchase Price"].map("${0:.02f}".format)
pa_age_chart["Total Purchase Value"] = pa_age_chart["Total Purchase Value"].map("${0:.02f}".format)
pa_age_chart["Normalized Totals"] = pa_age_chart["Normalized Totals"].map("${0:.02f}".format)
pa_age_chart

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
      <th>Purchase Count</th>
      <th>Average Purchase Price</th>
      <th>Total Purchase Value</th>
      <th>Normalized Totals</th>
    </tr>
    <tr>
      <th>Age range</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;10</th>
      <td>28</td>
      <td>$2.98</td>
      <td>$83.46</td>
      <td>$2.98</td>
    </tr>
    <tr>
      <th>10-14</th>
      <td>35</td>
      <td>$2.77</td>
      <td>$96.95</td>
      <td>$2.77</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>133</td>
      <td>$2.91</td>
      <td>$386.42</td>
      <td>$2.91</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>98</td>
      <td>$2.88</td>
      <td>$282.68</td>
      <td>$2.88</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>363</td>
      <td>$2.94</td>
      <td>$1066.42</td>
      <td>$2.94</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>64</td>
      <td>$3.08</td>
      <td>$197.25</td>
      <td>$3.08</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>42</td>
      <td>$2.84</td>
      <td>$119.40</td>
      <td>$2.84</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>17</td>
      <td>$3.16</td>
      <td>$53.75</td>
      <td>$3.16</td>
    </tr>
  </tbody>
</table>
</div>




```python
#######################################
##                         Top Spenders
#######################################
```


```python
spenders = purchase_pd
spenders_groups = spenders.groupby("SN")
spenders_counts = spenders_groups.count()
#drop the trash
trash = spenders_counts.drop(['Age','Gender','Item ID','Item Name'], axis=1, inplace=True)
spenders_counts = spenders_counts.rename(columns={'Price':'Purchase Count'})
# spenders_counts
spenders_sum = spenders_groups.sum()
top_5_spenders = pd.concat([spenders_counts,spenders_sum], axis=1)
# top_5_spenders
#drop the trash
trash = top_5_spenders.drop(['Age','Item ID'], axis=1, inplace=True )
top_5_spenders = top_5_spenders.rename(columns={'Price':'Total Purchase Value'})
# top_5_spenders

#make it look like your example on the homework PDF
top_5_spenders = top_5_spenders.nlargest(n=5,columns='Total Purchase Value',keep='first')
top_5_spenders["Average Purchase Price"] = top_5_spenders["Total Purchase Value"]/top_5_spenders["Purchase Count"]
# top_5_spenders
cols = ["Purchase Count", "Average Purchase Price", "Total Purchase Value"]
top_5_spenders = top_5_spenders[cols]
top_5_spenders["Average Purchase Price"] = top_5_spenders["Average Purchase Price"].map("${0:.02f}".format)
top_5_spenders["Total Purchase Value"] = top_5_spenders["Total Purchase Value"].map("${0:.02f}".format)
top_5_spenders
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
      <th>Purchase Count</th>
      <th>Average Purchase Price</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>SN</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Undirrala66</th>
      <td>5</td>
      <td>$3.41</td>
      <td>$17.06</td>
    </tr>
    <tr>
      <th>Saedue76</th>
      <td>4</td>
      <td>$3.39</td>
      <td>$13.56</td>
    </tr>
    <tr>
      <th>Mindimnya67</th>
      <td>4</td>
      <td>$3.18</td>
      <td>$12.74</td>
    </tr>
    <tr>
      <th>Haellysu29</th>
      <td>3</td>
      <td>$4.24</td>
      <td>$12.73</td>
    </tr>
    <tr>
      <th>Eoda93</th>
      <td>3</td>
      <td>$3.86</td>
      <td>$11.58</td>
    </tr>
  </tbody>
</table>
</div>




```python
#######################################
##                    Most Popular Item
#######################################
```


```python
popular_items = purchase_pd
popular_items_groups = popular_items.groupby(["Item ID", "Item Name"])
popular_items_counts = popular_items_groups.count()
#drop the trash
trash = popular_items_counts.drop(['Age','Gender','SN'], axis=1, inplace=True)
popular_items_counts = popular_items_counts.rename(columns={'Price':'Purchase Count'})
# popular_items_counts
popular_items_sum = popular_items_groups.sum()
top_5_popular_items = pd.concat([popular_items_counts,popular_items_sum], axis=1)
# top_5_popular_items
#drop the trash
trash = top_5_popular_items.drop(['Age'], axis=1, inplace=True )
top_5_popular_items = top_5_popular_items.rename(columns={'Price':'Total Purchase Value'})
# top_5_popular_items

#make it look like your example on the homework PDF
top_5_popular_items = top_5_popular_items.nlargest(n=5,columns='Purchase Count',keep='first')
top_5_popular_items["Item Price"] = top_5_popular_items["Total Purchase Value"]/top_5_popular_items["Purchase Count"]
# top_5_popular_items
cols = ["Purchase Count", "Item Price", "Total Purchase Value"]
top_5_popular_items = top_5_popular_items[cols]
# top_5_popular_items
top_5_popular_items["Item Price"] = top_5_popular_items["Item Price"].map("${0:.02f}".format)
top_5_popular_items["Total Purchase Value"] = top_5_popular_items["Total Purchase Value"].map("${0:.02f}".format)
top_5_popular_items
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
      <th></th>
      <th>Purchase Count</th>
      <th>Item Price</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>Item ID</th>
      <th>Item Name</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>39</th>
      <th>Betrayal, Whisper of Grieving Widows</th>
      <td>11</td>
      <td>$2.35</td>
      <td>$25.85</td>
    </tr>
    <tr>
      <th>84</th>
      <th>Arcane Gem</th>
      <td>11</td>
      <td>$2.23</td>
      <td>$24.53</td>
    </tr>
    <tr>
      <th>13</th>
      <th>Serenity</th>
      <td>9</td>
      <td>$1.49</td>
      <td>$13.41</td>
    </tr>
    <tr>
      <th>31</th>
      <th>Trickster</th>
      <td>9</td>
      <td>$2.07</td>
      <td>$18.63</td>
    </tr>
    <tr>
      <th>34</th>
      <th>Retribution Axe</th>
      <td>9</td>
      <td>$4.14</td>
      <td>$37.26</td>
    </tr>
  </tbody>
</table>
</div>




```python
#######################################
##                Most Profitable Items
#######################################
```


```python
popular_items = purchase_pd
popular_items_groups = popular_items.groupby(["Item ID", "Item Name"])
popular_items_counts = popular_items_groups.count()
#drop the trash
trash = popular_items_counts.drop(['Age','Gender','SN'], axis=1, inplace=True)
popular_items_counts = popular_items_counts.rename(columns={'Price':'Purchase Count'})
# popular_items_counts
popular_items_sum = popular_items_groups.sum()
top_5_popular_items = pd.concat([popular_items_counts,popular_items_sum], axis=1)
# top_5_popular_items
#drop the trash
trash = top_5_popular_items.drop(['Age'], axis=1, inplace=True )
top_5_popular_items = top_5_popular_items.rename(columns={'Price':'Total Purchase Value'})
# top_5_popular_items

#make it look like your example on the homework PDF
top_5_popular_items = top_5_popular_items.nlargest(n=5,columns='Total Purchase Value',keep='first')
top_5_popular_items["Item Price"] = top_5_popular_items["Total Purchase Value"]/top_5_popular_items["Purchase Count"]
# top_5_popular_items
cols = ["Purchase Count", "Item Price", "Total Purchase Value"]
top_5_popular_items = top_5_popular_items[cols]
# top_5_popular_items
top_5_popular_items["Item Price"] = top_5_popular_items["Item Price"].map("${0:.02f}".format)
top_5_popular_items["Total Purchase Value"] = top_5_popular_items["Total Purchase Value"].map("${0:.02f}".format)
top_5_popular_items
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
      <th></th>
      <th>Purchase Count</th>
      <th>Item Price</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>Item ID</th>
      <th>Item Name</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>34</th>
      <th>Retribution Axe</th>
      <td>9</td>
      <td>$4.14</td>
      <td>$37.26</td>
    </tr>
    <tr>
      <th>115</th>
      <th>Spectral Diamond Doomblade</th>
      <td>7</td>
      <td>$4.25</td>
      <td>$29.75</td>
    </tr>
    <tr>
      <th>32</th>
      <th>Orenmir</th>
      <td>6</td>
      <td>$4.95</td>
      <td>$29.70</td>
    </tr>
    <tr>
      <th>103</th>
      <th>Singed Scalpel</th>
      <td>6</td>
      <td>$4.87</td>
      <td>$29.22</td>
    </tr>
    <tr>
      <th>107</th>
      <th>Splitter, Foe Of Subtlety</th>
      <td>8</td>
      <td>$3.61</td>
      <td>$28.88</td>
    </tr>
  </tbody>
</table>
</div>


