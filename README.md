
# Heroes of Pymoli Data Analysis
--------------------------------

##Males make up a disproportionate number of total players, over 80%.
##Most players are 24 or younger, over 80%.
##The most popular item is not the most profitable.



```python
import pandas as pd    
pd.options.display.float_format = '${:,.2f}'.format

#reading json file
file = 'purchase_data.json'
df=pd.read_json(file)
df.head(10)
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
      <td>$3.37</td>
      <td>Aelalis34</td>
    </tr>
    <tr>
      <th>1</th>
      <td>21</td>
      <td>Male</td>
      <td>119</td>
      <td>Stormbringer, Dark Blade of Ending Misery</td>
      <td>$2.32</td>
      <td>Eolo46</td>
    </tr>
    <tr>
      <th>2</th>
      <td>34</td>
      <td>Male</td>
      <td>174</td>
      <td>Primitive Blade</td>
      <td>$2.46</td>
      <td>Assastnya25</td>
    </tr>
    <tr>
      <th>3</th>
      <td>21</td>
      <td>Male</td>
      <td>92</td>
      <td>Final Critic</td>
      <td>$1.36</td>
      <td>Pheusrical25</td>
    </tr>
    <tr>
      <th>4</th>
      <td>23</td>
      <td>Male</td>
      <td>63</td>
      <td>Stormfury Mace</td>
      <td>$1.27</td>
      <td>Aela59</td>
    </tr>
    <tr>
      <th>5</th>
      <td>20</td>
      <td>Male</td>
      <td>10</td>
      <td>Sleepwalker</td>
      <td>$1.73</td>
      <td>Tanimnya91</td>
    </tr>
    <tr>
      <th>6</th>
      <td>20</td>
      <td>Male</td>
      <td>153</td>
      <td>Mercenary Sabre</td>
      <td>$4.57</td>
      <td>Undjaskla97</td>
    </tr>
    <tr>
      <th>7</th>
      <td>29</td>
      <td>Female</td>
      <td>169</td>
      <td>Interrogator, Blood Blade of the Queen</td>
      <td>$3.32</td>
      <td>Iathenudil29</td>
    </tr>
    <tr>
      <th>8</th>
      <td>25</td>
      <td>Male</td>
      <td>118</td>
      <td>Ghost Reaver, Longsword of Magic</td>
      <td>$2.77</td>
      <td>Sondenasta63</td>
    </tr>
    <tr>
      <th>9</th>
      <td>31</td>
      <td>Male</td>
      <td>99</td>
      <td>Expiration, Warscythe Of Lost Worlds</td>
      <td>$4.53</td>
      <td>Hilaerin92</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Player Count
player_count = df.groupby(['SN']).size()
player_count.count()
print(player_count.count())

#output data frame
df_tot_players = pd.DataFrame({
    "Total Players": [player_count.count()]
})
df_tot_players
```

    573
    




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
#Purchasing Analysis (Total)**
#Number of Unique Items
pd.options.display.float_format = '${:,.2f}'.format

group_by_unique_items = df.groupby(['Item Name']).size()

group_by_unique_items.count()

#Total Number of Purchases
len(df.index)

#output data frame
df_purchasing_analysis = pd.DataFrame({
    "Number of Unique Items": [group_by_unique_items.count()],
    "Average Purchase Price": df["Price"].mean(),
    "Total Number of Purchases": len(df.index),
    "Total Revenue": (df['Price'].sum())
})
df_purchasing_analysis
df_purchasing_analysis[["Number of Unique Items","Average Purchase Price","Total Number of Purchases","Total Revenue"]]
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
      <th>Number of Unique Items</th>
      <th>Average Purchase Price</th>
      <th>Total Number of Purchases</th>
      <th>Total Revenue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>179</td>
      <td>$2.93</td>
      <td>780</td>
      <td>$2,286.33</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Gender Demographics
pd.options.display.float_format = '{:,.2f}'.format

#group_by_sn = df.loc["Female"].groupby(['Gender','SN']).size().reset_index()
only_females = df.loc[df["Gender"] == "Female", :].groupby('SN').size()

#count of each gender
female_count=len(df.loc[df["Gender"] == "Female", :].groupby('SN').size())
male_count=len(df.loc[df["Gender"] == "Male", :].groupby('SN').size())
other_count=len(df.loc[df["Gender"] == "Other / Non-Disclosed", :].groupby('SN').size())

#Percentage and Count of Male Players
male_players_percentage=len(df.loc[df["Gender"] == "Male", :].groupby('SN').size())/player_count.count()*100

#Percentage and Count of Female Players
female_players_percentage=len(df.loc[df["Gender"] == "Female", :].groupby('SN').size())/player_count.count()*100

#Percentage and Count of Other / Non-Disclosed
other_players_percentage=len(df.loc[df["Gender"] == "Other / Non-Disclosed", :].groupby('SN').size())/player_count.count()*100

df_gender_dem = pd.DataFrame({'Gender': ["Female","Male","Other / Non-Disclosed"], 
                   'Percentage of Players': [female_players_percentage,male_players_percentage,other_players_percentage],
                   'Total Count': [female_count,male_count,other_count]
                             }).set_index('Gender')
df_gender_dem
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
      <th>Gender</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Female</th>
      <td>17.45</td>
      <td>100</td>
    </tr>
    <tr>
      <th>Male</th>
      <td>81.15</td>
      <td>465</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>1.40</td>
      <td>8</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Purchasing Analysis (Gender) 
pd.options.display.float_format = '${:,.2f}'.format

#The below each broken by gender

#Purchase Count
df["Gender"].value_counts()

#Average Purchase Price
g1=df.groupby(['Gender']).mean()
g1.Price

#Total Purchase Value
g2=df.groupby(['Gender']).sum()
g2.Price

#Normalized Totals
g3=df.groupby(['Gender']).sum()/player_count.count()
g3.Price


df_purchase_analysis_gen = pd.DataFrame({
                   'Purchase Count': df["Gender"].value_counts(),
                   'Average Purchase Price': g1.Price,
                   'Total Purchase Value': g2.Price,
                   'Normalized Totals': g3.Price
                             })
df_purchase_analysis_gen.index.name = 'Gender'

df_purchase_analysis_gen
df_purchase_analysis_gen[["Purchase Count","Average Purchase Price","Total Purchase Value","Normalized Totals"]]
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
      <td>$2.82</td>
      <td>$382.91</td>
      <td>$0.67</td>
    </tr>
    <tr>
      <th>Male</th>
      <td>633</td>
      <td>$2.95</td>
      <td>$1,867.68</td>
      <td>$3.26</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>11</td>
      <td>$3.25</td>
      <td>$35.74</td>
      <td>$0.06</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Age Demographics
pd.options.display.float_format = '{:,.2f}'.format

df_age=pd.DataFrame({'count' : df.groupby( [ "SN", "Age"] ).size()}).reset_index()


#The below each broken into bins of 4 years (i.e. <10, 10-14, 15-19, etc.) 
# Create the bins in which Data will be held
# Bins are <10, 10-14, 15-19, 20-24, 25-29, 30-34



bins = [0, 10, 15, 20, 25, 30, 35, 39, 43]

# Create the names for the four bins
group_names = ['<10', '10-14', '15-19', '20-24', '25-29', '30-34', '35-39', '40-44']

# Cut Age and place the players into bins
pd.cut(df_age["Age"], bins, labels=group_names)

df_age["Age Group"] = pd.cut(df_age["Age"], bins, labels=group_names)
df.head()

#count by bin
count_by_bin=df_age.groupby(['Age Group']).size()
#count_by_bin

count_by_bin_percent=df_age.groupby(['Age Group']).size()/player_count.count()*100
#count_by_bin_percent
df_age_demo = pd.DataFrame({ 
                   'Percentage of Players': count_by_bin_percent,
                   'Total Count': count_by_bin
                             })
df_age_demo
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
      <th>Age Group</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;10</th>
      <td>3.84</td>
      <td>22</td>
    </tr>
    <tr>
      <th>10-14</th>
      <td>9.42</td>
      <td>54</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>24.26</td>
      <td>139</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>40.84</td>
      <td>234</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>9.08</td>
      <td>52</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>7.68</td>
      <td>44</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>2.97</td>
      <td>17</td>
    </tr>
    <tr>
      <th>40-44</th>
      <td>1.75</td>
      <td>10</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Purchasing Analysis (Age)
pd.options.display.float_format = '${:,.2f}'.format

# Bins are <10, 10-14, 15-19, 20-24, 25-29, 30-34
bins = [0, 10, 15, 20, 25, 30, 35, 39, 43]

# Create the names for the bins
group_names = ['<10', '10-14', '15-19', '20-24', '25-29', '30-34', '35-39', '40-44']

# Cut Age and place the players into bins
pd.cut(df["Age"], bins, labels=group_names)

df["Age Group"] = pd.cut(df["Age"], bins, labels=group_names)
df.head()

#Purchase Count
purchase_count_by_age=df.groupby(['Age Group']).size()


#Average Purchase Price
age_groupby_mean = df.groupby(['Age Group']).mean()
age_groupby_mean.Price

#Total Purchase Value
age_groupby_sum = df.groupby(['Age Group']).sum()
age_groupby_sum.Price

#Normalized Totals
normalized_player_total = df.groupby(['Age Group']).sum()
normalized_player_total.Price/player_count.count()

df_purchasing_analysis_by_age = pd.DataFrame({
                   'Purchase Count': purchase_count_by_age,
                   'Average Purchase Price': age_groupby_mean.Price,
                   'Total Purchase Value': age_groupby_sum.Price,
                   'Normalized Totals': normalized_player_total.Price/player_count.count()
                             })
df_purchasing_analysis_by_age
df_purchasing_analysis_by_age[["Purchase Count","Average Purchase Price","Total Purchase Value","Normalized Totals"]]
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
      <th>Age Group</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;10</th>
      <td>32</td>
      <td>$3.02</td>
      <td>$96.62</td>
      <td>$0.17</td>
    </tr>
    <tr>
      <th>10-14</th>
      <td>78</td>
      <td>$2.87</td>
      <td>$224.15</td>
      <td>$0.39</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>184</td>
      <td>$2.87</td>
      <td>$528.74</td>
      <td>$0.92</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>305</td>
      <td>$2.96</td>
      <td>$902.61</td>
      <td>$1.58</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>76</td>
      <td>$2.89</td>
      <td>$219.82</td>
      <td>$0.38</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>58</td>
      <td>$3.07</td>
      <td>$178.26</td>
      <td>$0.31</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>30</td>
      <td>$2.75</td>
      <td>$82.38</td>
      <td>$0.14</td>
    </tr>
    <tr>
      <th>40-44</th>
      <td>16</td>
      <td>$3.19</td>
      <td>$51.03</td>
      <td>$0.09</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Top Spenders
pd.options.display.float_format = '${:,.2f}'.format

#setting up output data frame
top_spender_df = pd.DataFrame({
     "SN": df['SN'],
     "Price": df['Price'],
     "Item ID": df['Item ID']})

top_spender_df


#Identify the the top 5 spenders in the game by total purchase value, then list (in a table):


#total items bought by each user
top_spender_groupby = top_spender_df.groupby(['SN']).count()
top_spender_groupby

#total purchase price
top_spender_price_groupby1 = top_spender_df.groupby(['SN']).sum()
top_spender_price_groupby1

#getting total purchase value
top_spender_groupby["Price"] = top_spender_price_groupby1["Price"]

#average purchase price
top_spender_groupby["Average Purchase Price"] = (top_spender_groupby["Price"]/top_spender_groupby["Item ID"])
top_spender_groupby

#renaming columns to match hw
top_spender_groupby = top_spender_groupby.rename(columns={'Price': 'Total Purchase Value', 
                                                          'Item ID': 'Purchase Count',
                                                          })

top_spender_groupby

top_spender_sorted = top_spender_groupby.sort_values('Total Purchase Value', ascending=False)
top_spender_sorted

top5=top_spender_sorted.head(5)
top5

top5[["Purchase Count","Average Purchase Price","Total Purchase Value"]]
#SN
#Purchase Count
#Average Purchase Price
#Total Purchase Value
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
#Most Popular Items
pd.options.display.float_format = '${:,.2f}'.format

#setting up output data frame

most_popular_df = pd.DataFrame({
     "Item ID": df['Item ID'],
     "Item Name": df['Item Name'],
     "Price": df['Price']
  })

most_popular_df


#Identify the 5 most popular items by purchase count, then list (in a table):

top_items_groupby = most_popular_df.groupby(['Item Name','Item ID']).count()
top_items_groupby["Purchase Count"]=top_items_groupby["Price"]
top_items_total_purchase_price_groupby = most_popular_df.groupby(['Item Name','Item ID']).sum()
top_items_groupby["Price"]=top_items_total_purchase_price_groupby["Price"]
top_items_groupby["Item Price"]=(top_items_total_purchase_price_groupby["Price"]/top_items_groupby["Purchase Count"])


#renaming columns to match hw
top_item = top_items_groupby.rename(columns={'Price': 'Total Purchase Value' 
                                                          })
top_item.head()

top_item_sorted = top_item.sort_values('Purchase Count', ascending=False)

top5=top_item_sorted.head(5)

top5=top5.reset_index()
top5
df_top = pd.DataFrame({
                   'Item ID': top5["Item ID"],
                   'Item Name': top5["Item Name"],
                   'Purchase Count': top5["Purchase Count"],
                   'Item Price': top5["Item Price"],
                   'Total Purchase Value': top5["Total Purchase Value"]
          }).set_index('Item ID')

df_top
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
      <th>Item Price</th>
      <th>Purchase Count</th>
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
      <td>Betrayal, Whisper of Grieving Widows</td>
      <td>$2.35</td>
      <td>11</td>
      <td>$25.85</td>
    </tr>
    <tr>
      <th>84</th>
      <td>Arcane Gem</td>
      <td>$2.23</td>
      <td>11</td>
      <td>$24.53</td>
    </tr>
    <tr>
      <th>34</th>
      <td>Retribution Axe</td>
      <td>$4.14</td>
      <td>9</td>
      <td>$37.26</td>
    </tr>
    <tr>
      <th>31</th>
      <td>Trickster</td>
      <td>$2.07</td>
      <td>9</td>
      <td>$18.63</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Serenity</td>
      <td>$1.49</td>
      <td>9</td>
      <td>$13.41</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Most Profitable Items
pd.options.display.float_format = '${:,.2f}'.format

#Identify the 5 most profitable items by total purchase value, then list (in a table):


#Item ID
#Item Name
#Purchase Count
#Item Price
#Total Purchase Value

most_profit_df = pd.DataFrame({
     "Item ID": df['Item ID'],
     "Item Name": df['Item Name'],
     "Price": df['Price']
})

most_profit_df

#items by purchase value
pop_items_groupby = most_profit_df.groupby(['Item Name','Item ID']).sum()

#items by purchase count
pop_items_groupby["Purchase Count"] = most_profit_df.groupby(['Item Name','Item ID']).count()

#item price
pop_items_groupby["Item Price"] = most_profit_df.groupby(['Item Name','Item ID']).sum()/most_profit_df.groupby(['Item Name','Item ID']).count()

#renaming columns to match hw
pop_items_groupby = pop_items_groupby.rename(columns={'Price': 'Total Purchase Value' 
                                                     })
#sort by total purchase value
pop_item_sorted = pop_items_groupby.sort_values('Total Purchase Value', ascending=False)

pop5=pop_item_sorted.head(5)
pop5=pop5.reset_index()

df_pop = pd.DataFrame({
                   'Item ID': pop5["Item ID"],
                   'Item Name': pop5["Item Name"],
                   'Purchase Count': pop5["Purchase Count"],
                   'Item Price': pop5["Item Price"],
                   'Total Purchase Value': pop5["Total Purchase Value"]
                             }).set_index('Item ID')

df_pop

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
      <th>Item Price</th>
      <th>Purchase Count</th>
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
      <td>Retribution Axe</td>
      <td>$4.14</td>
      <td>9</td>
      <td>$37.26</td>
    </tr>
    <tr>
      <th>115</th>
      <td>Spectral Diamond Doomblade</td>
      <td>$4.25</td>
      <td>7</td>
      <td>$29.75</td>
    </tr>
    <tr>
      <th>32</th>
      <td>Orenmir</td>
      <td>$4.95</td>
      <td>6</td>
      <td>$29.70</td>
    </tr>
    <tr>
      <th>103</th>
      <td>Singed Scalpel</td>
      <td>$4.87</td>
      <td>6</td>
      <td>$29.22</td>
    </tr>
    <tr>
      <th>107</th>
      <td>Splitter, Foe Of Subtlety</td>
      <td>$3.61</td>
      <td>8</td>
      <td>$28.88</td>
    </tr>
  </tbody>
</table>
</div>


