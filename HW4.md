

```python
# DEPENDENCIES
import pandas as pd
import json
```


```python
# Creating the file path
filename = '/Users/Jenny/Desktop/NEW/Heroes-of-Pymoli/purchase_data.json'

# Opening the file
game_data_df = pd.read_json(filename, orient='columns')
game_data_df.head()
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
      <th>Age</th>
      <th>Gender</th>
      <th>Item ID</th>
      <th>Item Name</th>
      <th>Price</th>
      <th>SN</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>38</td>
      <td>Male</td>
      <td>165</td>
      <td>Bone Crushing Silver Skewer</td>
      <td>3.37</td>
      <td>Aelalis34</td>
    </tr>
    <tr>
      <th>1</th>
      <td>21</td>
      <td>Male</td>
      <td>119</td>
      <td>Stormbringer, Dark Blade of Ending Misery</td>
      <td>2.32</td>
      <td>Eolo46</td>
    </tr>
    <tr>
      <th>2</th>
      <td>34</td>
      <td>Male</td>
      <td>174</td>
      <td>Primitive Blade</td>
      <td>2.46</td>
      <td>Assastnya25</td>
    </tr>
    <tr>
      <th>3</th>
      <td>21</td>
      <td>Male</td>
      <td>92</td>
      <td>Final Critic</td>
      <td>1.36</td>
      <td>Pheusrical25</td>
    </tr>
    <tr>
      <th>4</th>
      <td>23</td>
      <td>Male</td>
      <td>63</td>
      <td>Stormfury Mace</td>
      <td>1.27</td>
      <td>Aela59</td>
    </tr>
  </tbody>
</table>
</div>




```python
# PLAYER COUNT
# player_count = game_data_df['SN'].count()
players = {
    'Total # of Players': [game_data_df['SN'].count()]
}

players_count = pd.DataFrame(players)
players_count
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
      <th>Total # of Players</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>780</td>
    </tr>
  </tbody>
</table>
</div>




```python
# PURCHASING ANALYSIS (TOTAL)
total_unique_items = game_data_df['Item Name'].count()
total_avg_price = game_data_df['Price'].mean()
total_purchases = game_data_df['Price'].count()
total_revenue = game_data_df['Price'].sum()

purchase_analysis = {
    '# of Unique Items': [game_data_df['Item Name'].count()],
    'Average Price': [game_data_df['Price'].mean()],
    '# of Purchases': [game_data_df['Price'].count()],
    'Total Revenue': [game_data_df['Price'].sum()]
}

purchase_analysis_df = pd.DataFrame(analysis)
purchase_analysis_df
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
      <th># of Purchases</th>
      <th># of Unique Items</th>
      <th>Average Price</th>
      <th>Total Revenue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>780</td>
      <td>780</td>
      <td>2.931192</td>
      <td>2286.33</td>
    </tr>
  </tbody>
</table>
</div>




```python
# GENDER DEMOGRAPHICS
# Calculate players by gender
players_by_gender = game_data_df['Gender'].value_counts()
players_by_gender_df = pd.DataFrame(players_by_gender)

# Calculate percent of players by gender
percent_players = (game_data_df['Gender'].value_counts() / (game_data_df['Gender'].count())) * 100

# Add 'Percent of Players' to table
players_by_gender_df['Percent of Players'] = percent_players
gender_demographics_df = pd.DataFrame(players_by_gender_df)
gender_demographics_df

# Rename 'Gender' to 'Total Count'
new_gender_demographics_df = gender_demographics_df.rename(columns={
    'Gender': 'Total Count',
})
new_gender_demographics_df
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
      <th>Total Count</th>
      <th>Percent of Players</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Male</th>
      <td>633</td>
      <td>81.153846</td>
    </tr>
    <tr>
      <th>Female</th>
      <td>136</td>
      <td>17.435897</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>11</td>
      <td>1.410256</td>
    </tr>
  </tbody>
</table>
</div>




```python
# PURCHASING ANALYSIS (GENDER)
# Calculate purchase count by gender
all_gender_df = game_data_df.groupby('Gender')

player_count = all_gender_df['SN'].count()
player_count_df = pd.DataFrame(player_count)

# Calculate average purchase price by gender
avg_price = all_gender_df['Price'].mean()
# avg_price_df = pd.DataFrame(avg_price)

# Calculate total purchase price by gender
total_gender_purchase = all_gender_df['Price'].sum()

# Calculate normalized totals (total per category / total count per category)
norm_total = total_gender_purchase / player_count

# Add 'Average Purchase Price' to table
player_count_df['Average Purchase Price'] = avg_price
gender_purchase_df = pd.DataFrame(player_count_df)

# Rename 'SN' column to 'Purchase Count'
new_gender_purchase_df = gender_purchase_df.rename(columns={
    'SN': 'Purchase Count',
})

# Add 'Normalized Totals' to table
new_gender_purchase_df['Total Purchase Price'] = total_gender_purchase
new_gender_purchase2_df = pd.DataFrame(new_gender_purchase_df)

# Add 'Total Purchase Price' to table
new_gender_purchase2_df['Normalized Totals'] = norm_total
total_gender_purchase_df = pd.DataFrame(new_gender_purchase2_df)
total_gender_purchase_df
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
      <th>Purchase Count</th>
      <th>Average Purchase Price</th>
      <th>Total Purchase Price</th>
      <th>Normalized Totals</th>
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
      <td>2.815515</td>
      <td>382.91</td>
      <td>2.815515</td>
    </tr>
    <tr>
      <th>Male</th>
      <td>633</td>
      <td>2.950521</td>
      <td>1867.68</td>
      <td>2.950521</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>11</td>
      <td>3.249091</td>
      <td>35.74</td>
      <td>3.249091</td>
    </tr>
  </tbody>
</table>
</div>




```python
# AGE DEMOGRAPHICS
# Create bins for different ranges of age groups
bins = [0, 10, 15, 20, 25, 30, 35, 40, 100]
group_names = ['<10', '10-14', '15-19', '20-24', '25-29', '30-34', '35-39', '40+']

# Place total number of people per age group in their respective bins
game_data_df['Total Count'] = pd.cut(game_data_df['Age'],
                                     bins, labels=group_names)
age_groups = game_data_df.groupby('Total Count')
age_groups_df = age_groups.count()

# Only show total number of people per 'Age' column in table
organized_age_df = age_groups_df[['Age']]
organized_age_df

# Calculate percent of players per age group
#percent_age_group = (organized_age_df / player_count) * 100
#percent_age_group
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
      <th>Age</th>
    </tr>
    <tr>
      <th>Total Count</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;10</th>
      <td>32</td>
    </tr>
    <tr>
      <th>10-14</th>
      <td>78</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>184</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>305</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>76</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>58</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>44</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>3</td>
    </tr>
  </tbody>
</table>
</div>




```python
# PURCHASING ANALYSIS (AGE)
# Create bins for different ranges of age groups
bins2 = [0, 10, 15, 20, 25, 30, 35, 40, 100]
group_names2 = ['<10', '10-14', '15-19', '20-24', '25-29', '30-34', '35-39', '40+']

# Place total number of people per age group in their respective bins
game_data_df['Total Count'] = pd.cut(game_data_df['Item ID'],
                                     bins2, labels=group_names2)
age_groups2 = game_data_df.groupby('Total Count')
age_groups2_df = age_groups2.count()
age_purchase = age_groups2_df['Item ID']
age_purchase

organized_age2_df = age_groups2_df[['Item ID']]
organized_age2_df

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
      <th>Item ID</th>
    </tr>
    <tr>
      <th>Total Count</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;10</th>
      <td>33</td>
    </tr>
    <tr>
      <th>10-14</th>
      <td>32</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>20</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>19</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>17</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>32</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>28</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>249</td>
    </tr>
  </tbody>
</table>
</div>




```python
# TOP SPENDERS
# Calculate 'Total Purchase Count' for each SN
count_per_SN = top_spenders.groupby('SN')['Price'].count()
count_per_SN_df = pd.DataFrame(count_per_SN)

# Calculate 'Total Purchase Price' for each SN
unique_SN_df = top_spenders.groupby('SN')['Price'].sum()

# Calculate 'Average Purchase Price'
avg_price_per_SN = top_spenders.groupby('SN')['Price'].mean()

# Add 'Average Purchase Price' to table
count_per_SN_df['Average Purchase Price'] = avg_price_per_SN

# Add 'Total Purchase Value' to table
count_per_SN_df['Total Purchase Value'] = unique_SN_df
count_per_SN_df

# Rename 'Price' to 'Purchase Count'
updated_count_per_SN_df = count_per_SN_df.rename(columns={
    'Price': 'Purchase Count',
})

# Sort 'Price' from highest to lowest
sorted_SN_df = updated_count_per_SN_df.sort_values(by=['Total Purchase Value'], ascending=False)
sorted_SN_df

# Show top 5 spenders only
sorted_SN_df.head(5)
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
      <td>3.412000</td>
      <td>17.06</td>
    </tr>
    <tr>
      <th>Saedue76</th>
      <td>4</td>
      <td>3.390000</td>
      <td>13.56</td>
    </tr>
    <tr>
      <th>Mindimnya67</th>
      <td>4</td>
      <td>3.185000</td>
      <td>12.74</td>
    </tr>
    <tr>
      <th>Haellysu29</th>
      <td>3</td>
      <td>4.243333</td>
      <td>12.73</td>
    </tr>
    <tr>
      <th>Eoda93</th>
      <td>3</td>
      <td>3.860000</td>
      <td>11.58</td>
    </tr>
  </tbody>
</table>
</div>




```python
# MOST POPULAR ITEMS
# Calculate 'Total Purchase Count' of each item
count_per_item = top_spenders.groupby('Item ID')['Price'].count()
count_per_item_df = pd.DataFrame(count_per_item)

# Calculate 'Total Purchase Price' of each item
unique_item_df = top_spenders.groupby('Item ID')['Price'].sum()

# Add 'Item Name' to table
item_name_df = top_spenders.iloc[:, 3]
count_per_item_df['Item Name'] = item_name_df

# Add 'Item Price' to table
item_price_df = top_spenders.iloc[:, 4]
count_per_item_df['Item Price'] = item_price_df

# Add 'Total Purchase Value' to table
count_per_item_df['Total Purchase Value'] = unique_item_df
count_per_item_df

# Rename 'Price' to 'Purchase Count'
updated_count_per_item_df = count_per_item_df.rename(columns={
    'Price': 'Purchase Count',
})

# Rearrange columns
organized_items_df = updated_count_per_item_df[['Item Name', 'Purchase Count', 'Item Price', 'Total Purchase Value']]

# Sort 'Purchase Count' from highest to lowest
sorted_items_df = organized_items_df.sort_values(by=['Purchase Count'], ascending=False)

# Show top 5 popular items only
sorted_items_df.head(5)
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
      <th>Item Name</th>
      <th>Purchase Count</th>
      <th>Item Price</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>Item ID</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>39</th>
      <td>Stormfury Mace</td>
      <td>11</td>
      <td>1.27</td>
      <td>25.85</td>
    </tr>
    <tr>
      <th>84</th>
      <td>Thorn, Satchel of Dark Souls</td>
      <td>11</td>
      <td>4.51</td>
      <td>24.53</td>
    </tr>
    <tr>
      <th>31</th>
      <td>Shadow Strike, Glory of Ending Hope</td>
      <td>9</td>
      <td>1.93</td>
      <td>18.63</td>
    </tr>
    <tr>
      <th>175</th>
      <td>Retribution Axe</td>
      <td>9</td>
      <td>4.14</td>
      <td>11.16</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Piety, Guardian of Riddles</td>
      <td>9</td>
      <td>3.68</td>
      <td>13.41</td>
    </tr>
  </tbody>
</table>
</div>




```python
# MOST PROFITABLE ITEMS
# Calculate 'Total Purchase Count' of each item
count_per_item2 = top_spenders.groupby('Item ID')['Price'].count()
count_per_item2_df = pd.DataFrame(count_per_item2)

# Calculate 'Total Purchase Price' of each item
unique_item2_df = top_spenders.groupby('Item ID')['Price'].sum()

# Add 'Item Name' to table
item_name2_df = top_spenders.iloc[:, 3]
count_per_item2_df['Item Name'] = item_name2_df

# Add 'Item Price' to table
item_price2_df = top_spenders.iloc[:, 4]
count_per_item2_df['Item Price'] = item_price2_df

# Add 'Total Purchase Value' to table
count_per_item2_df['Total Purchase Value'] = unique_item2_df
count_per_item2_df

# Rename 'Price' to 'Purchase Count'
updated_count_per_item2_df = count_per_item2_df.rename(columns={
    'Price': 'Purchase Count',
})

# Rearrange columns
organized_items2_df = updated_count_per_item2_df[['Item Name', 'Purchase Count', 'Item Price', 'Total Purchase Value']]

# Sort 'Purchase Count' from highest to lowest
sorted_items2_df = organized_items2_df.sort_values(by=['Total Purchase Value'], ascending=False)

# Show top 5 popular items only
sorted_items2_df.head(5)
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
      <th>Item Name</th>
      <th>Purchase Count</th>
      <th>Item Price</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>Item ID</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>34</th>
      <td>Alpha, Reach of Ending Hope</td>
      <td>9</td>
      <td>1.55</td>
      <td>37.26</td>
    </tr>
    <tr>
      <th>115</th>
      <td>Thorn, Conqueror of the Corrupted</td>
      <td>7</td>
      <td>2.04</td>
      <td>29.75</td>
    </tr>
    <tr>
      <th>32</th>
      <td>Rage, Legacy of the Lone Victor</td>
      <td>6</td>
      <td>4.32</td>
      <td>29.70</td>
    </tr>
    <tr>
      <th>103</th>
      <td>Mercy, Katana of Dismay</td>
      <td>6</td>
      <td>4.37</td>
      <td>29.22</td>
    </tr>
    <tr>
      <th>107</th>
      <td>Spectral Diamond Doomblade</td>
      <td>8</td>
      <td>4.25</td>
      <td>28.88</td>
    </tr>
  </tbody>
</table>
</div>


