---
title: Covid 19 - Dashboard
content_img_path: /images/covid19-1-1200x689.jpg
home_spotlights:
  enabled: true
  home_img:
    data_position: top center
    path: /images/covid19-1-1200x689.jpg
  weight: -1
  excerpt: Covid 19 Dashboard
layout: page
---


```python
# Load the Pandas libraries with alias 'pd' 
import pandas as pd 
```


```python
# Load data from https://github.com/CSSEGISandData/COVID-19 
# Novel Coronavirus (COVID-19) Cases, provided by JHU CSSE https://systems.jhu.edu/research/publ…

df_world_covid_19_data = pd.read_csv("https://raw.githubusercontent.com/CSSEGISandData/COVID-19/master/csse_covid_19_data/csse_covid_19_time_series/time_series_19-covid-Confirmed.csv")

# delete unused columns 
df_world_covid_19_data.drop(df_world_covid_19_data.columns[[2,3]], axis = 1, inplace = True) 

# fix some country names
df_world_covid_19_data['Country/Region'] = df_world_covid_19_data['Country/Region'].replace("Congo (Brazzaville)", "DR Congo")
df_world_covid_19_data['Country/Region'] = df_world_covid_19_data['Country/Region'].replace("Congo (Kinshasa)", "Republic of the Congo")
df_world_covid_19_data['Country/Region'] = df_world_covid_19_data['Country/Region'].replace("Cote d'Ivoire", "Ivory Coast")
df_world_covid_19_data['Country/Region'] = df_world_covid_19_data['Country/Region'].replace("Holy See", "Vatican City")
df_world_covid_19_data['Country/Region'] = df_world_covid_19_data['Country/Region'].replace("The Bahamas", "Bahamas")
df_world_covid_19_data['Country/Region'] = df_world_covid_19_data['Country/Region'].replace("US", "United States")
df_world_covid_19_data['Country/Region'] = df_world_covid_19_data['Country/Region'].replace("Taiwan*", "Taiwan")
df_world_covid_19_data['Country/Region'] = df_world_covid_19_data['Country/Region'].replace("Korea, South", "South Korea")

# Group & Sum by Country
df_world_covid_19_data = df_world_covid_19_data.groupby(df_world_covid_19_data['Country/Region']).sum()

# SORT LAST VALUE 
df_world_covid_19_data = df_world_covid_19_data.sort_values(by=df_world_covid_19_data.columns[-1], ascending=[False])

# DEBUG
# df_world_covid_19_data.head(5)
```


```python
# Load data from https://github.com/mledoze/countries
# World countries in JSON, CSV, XML and Yaml. Any help is welcome! https://git.io/countries

df_world_contry_code = pd.read_csv("https://raw.githubusercontent.com/mledoze/countries/master/dist/countries.csv")

# create a common index 
df_world_contry_code["Country/Region"] = df_world_contry_code["name"].str.split(",", n = 1, expand = True)[0] 

# DEBUG
#df_world_contry_code.head(5)

```


```python
# Load data from https://github.com/samayo/country-json
# A simple data of the world by country each in JSON format. https://data.world/samayo

df_world_contry_population = pd.read_json("https://raw.githubusercontent.com/samayo/country-json/master/src/country-by-population.json")
# create a common index 
df_world_contry_population.rename(columns={'country':'Country/Region'}, inplace=True)

# DEBUG
#df_world_contry_population.head(5)
```


```python
# Now merge dataframes df_world_covid_19_data & df_world_contry_code
df_merge1 = pd.merge(df_world_covid_19_data, df_world_contry_code, how='left', on=['Country/Region'])
df_world_covid = pd.merge(df_merge1, df_world_contry_population, how='left', on=['Country/Region'])

# DEBUG
#df_world_covid.head(5)
```


```python
# looking for countries not found  
#filtered_df = df_world_covid[df_world_covid['region'].isnull()]
#filtered_df.head(1000)

# 3 unused country 
```


```python
# NOW ADD SOME CALCS
df_world_covid_all = df_world_covid

# PERCENTS OF SICKED ON POPULATIONS
df_world_covid_all['percent'] = df_world_covid_all['3/17/20']/df_world_covid_all['population']*100
df_world_covid_all = df_world_covid_all.sort_values(by=df_world_covid_all.columns[-1], ascending=[False])

```


```python
# top country 
df_world_covid_all = df_world_covid_all.sort_values(by=df_world_covid_all.columns[55], ascending=[False])
df_top_country = df_world_covid_all[['Country/Region', '3/17/20', 'population', 'percent']].copy()
df_top_country.head(10)


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
      <th>Country/Region</th>
      <th>3/17/20</th>
      <th>population</th>
      <th>percent</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>China</td>
      <td>81058</td>
      <td>1.409517e+09</td>
      <td>0.005751</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Italy</td>
      <td>31506</td>
      <td>5.935990e+07</td>
      <td>0.053076</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Iran</td>
      <td>16169</td>
      <td>8.116279e+07</td>
      <td>0.019922</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Spain</td>
      <td>11748</td>
      <td>4.635432e+07</td>
      <td>0.025344</td>
    </tr>
    <tr>
      <th>5</th>
      <td>South Korea</td>
      <td>8320</td>
      <td>5.098221e+07</td>
      <td>0.016319</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Germany</td>
      <td>9257</td>
      <td>8.211422e+07</td>
      <td>0.011273</td>
    </tr>
    <tr>
      <th>6</th>
      <td>France</td>
      <td>7699</td>
      <td>6.497955e+07</td>
      <td>0.011848</td>
    </tr>
    <tr>
      <th>7</th>
      <td>United States</td>
      <td>6421</td>
      <td>3.244595e+08</td>
      <td>0.001979</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Switzerland</td>
      <td>2700</td>
      <td>8.476005e+06</td>
      <td>0.031855</td>
    </tr>
    <tr>
      <th>9</th>
      <td>United Kingdom</td>
      <td>1960</td>
      <td>6.618158e+07</td>
      <td>0.002962</td>
    </tr>
  </tbody>
</table>
</div>




```python
import yaml
import pickle

with open('df_top_country.yml', 'w') as file:
    yaml.dump({'simple': df_top_country.to_dict(orient='records')}, file, default_flow_style=False)
diction=df_top_country.to_dict(orient='records')
f = open("file.txt","wb")
pickle.dump(diction,f)
f.close()
```


```python
#
df_percentile_by_country = df_world_covid_all[['Country/Region', 'percent']].copy()
df_percentile_by_country.head(10)
```


```python


```


```python
df_region = df_world_covid.groupby(df_world_covid['region']).sum()
df_region = df_region.drop(["ccn3", "independent", "landlocked"], axis=1)

# SORT LAST VALUE 
df_region = df_region.sort_values(by=df_region.columns[-3], ascending=[False])
pd.options.display.float_format = '{:,.6f}'.format
df_region['percent'] = df_region['3/16/20']/df_region['population']
df_region.describe()  
# df_region.head(15)
df_region.head(15)
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
      <th>1/22/20</th>
      <th>1/23/20</th>
      <th>1/24/20</th>
      <th>1/25/20</th>
      <th>1/26/20</th>
      <th>1/27/20</th>
      <th>1/28/20</th>
      <th>1/29/20</th>
      <th>1/30/20</th>
      <th>1/31/20</th>
      <th>2/1/20</th>
      <th>2/2/20</th>
      <th>2/3/20</th>
      <th>2/4/20</th>
      <th>2/5/20</th>
      <th>2/6/20</th>
      <th>2/7/20</th>
      <th>2/8/20</th>
      <th>2/9/20</th>
      <th>2/10/20</th>
      <th>2/11/20</th>
      <th>2/12/20</th>
      <th>2/13/20</th>
      <th>2/14/20</th>
      <th>2/15/20</th>
      <th>2/16/20</th>
      <th>2/17/20</th>
      <th>2/18/20</th>
      <th>2/19/20</th>
      <th>2/20/20</th>
      <th>2/21/20</th>
      <th>2/22/20</th>
      <th>2/23/20</th>
      <th>2/24/20</th>
      <th>2/25/20</th>
      <th>2/26/20</th>
      <th>2/27/20</th>
      <th>2/28/20</th>
      <th>2/29/20</th>
      <th>3/1/20</th>
      <th>3/2/20</th>
      <th>3/3/20</th>
      <th>3/4/20</th>
      <th>3/5/20</th>
      <th>3/6/20</th>
      <th>3/7/20</th>
      <th>3/8/20</th>
      <th>3/9/20</th>
      <th>3/10/20</th>
      <th>3/11/20</th>
      <th>3/12/20</th>
      <th>3/13/20</th>
      <th>3/14/20</th>
      <th>3/15/20</th>
      <th>3/16/20</th>
      <th>3/17/20</th>
      <th>area</th>
      <th>population</th>
      <th>percent</th>
    </tr>
    <tr>
      <th>region</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Americas</th>
      <td>1</td>
      <td>1</td>
      <td>2</td>
      <td>2</td>
      <td>6</td>
      <td>6</td>
      <td>7</td>
      <td>7</td>
      <td>7</td>
      <td>11</td>
      <td>12</td>
      <td>12</td>
      <td>15</td>
      <td>15</td>
      <td>16</td>
      <td>16</td>
      <td>18</td>
      <td>18</td>
      <td>18</td>
      <td>18</td>
      <td>19</td>
      <td>19</td>
      <td>20</td>
      <td>20</td>
      <td>20</td>
      <td>20</td>
      <td>21</td>
      <td>21</td>
      <td>21</td>
      <td>21</td>
      <td>24</td>
      <td>24</td>
      <td>24</td>
      <td>61</td>
      <td>62</td>
      <td>69</td>
      <td>72</td>
      <td>76</td>
      <td>94</td>
      <td>112</td>
      <td>139</td>
      <td>165</td>
      <td>204</td>
      <td>282</td>
      <td>354</td>
      <td>507</td>
      <td>663</td>
      <td>752</td>
      <td>1153</td>
      <td>1553</td>
      <td>1983</td>
      <td>2752</td>
      <td>3413</td>
      <td>4329</td>
      <td>5887</td>
      <td>8095</td>
      <td>41,765,118.000000</td>
      <td>977,667,371.000000</td>
      <td>0.000006</td>
    </tr>
    <tr>
      <th>Asia</th>
      <td>554</td>
      <td>652</td>
      <td>937</td>
      <td>1429</td>
      <td>2105</td>
      <td>2912</td>
      <td>5558</td>
      <td>6143</td>
      <td>8208</td>
      <td>9889</td>
      <td>11991</td>
      <td>16738</td>
      <td>19827</td>
      <td>23836</td>
      <td>27578</td>
      <td>30759</td>
      <td>34266</td>
      <td>36990</td>
      <td>40015</td>
      <td>42551</td>
      <td>44588</td>
      <td>44966</td>
      <td>60112</td>
      <td>66585</td>
      <td>68662</td>
      <td>70786</td>
      <td>72720</td>
      <td>74510</td>
      <td>74934</td>
      <td>75479</td>
      <td>76081</td>
      <td>77792</td>
      <td>78028</td>
      <td>78516</td>
      <td>79255</td>
      <td>80056</td>
      <td>81147</td>
      <td>82218</td>
      <td>83717</td>
      <td>85317</td>
      <td>86693</td>
      <td>88559</td>
      <td>89793</td>
      <td>91070</td>
      <td>93110</td>
      <td>94848</td>
      <td>96069</td>
      <td>96939</td>
      <td>98134</td>
      <td>99909</td>
      <td>101205</td>
      <td>103047</td>
      <td>104956</td>
      <td>106893</td>
      <td>108477</td>
      <td>110571</td>
      <td>29,537,698.000000</td>
      <td>4,291,576,407.000000</td>
      <td>0.000025</td>
    </tr>
    <tr>
      <th>Europe</th>
      <td>0</td>
      <td>0</td>
      <td>2</td>
      <td>3</td>
      <td>3</td>
      <td>4</td>
      <td>8</td>
      <td>10</td>
      <td>10</td>
      <td>18</td>
      <td>23</td>
      <td>25</td>
      <td>27</td>
      <td>28</td>
      <td>28</td>
      <td>28</td>
      <td>31</td>
      <td>36</td>
      <td>38</td>
      <td>43</td>
      <td>45</td>
      <td>46</td>
      <td>46</td>
      <td>46</td>
      <td>47</td>
      <td>47</td>
      <td>47</td>
      <td>47</td>
      <td>47</td>
      <td>47</td>
      <td>64</td>
      <td>106</td>
      <td>199</td>
      <td>277</td>
      <td>381</td>
      <td>541</td>
      <td>805</td>
      <td>1094</td>
      <td>1466</td>
      <td>2203</td>
      <td>2730</td>
      <td>3359</td>
      <td>4341</td>
      <td>5742</td>
      <td>7517</td>
      <td>9659</td>
      <td>12203</td>
      <td>14987</td>
      <td>18393</td>
      <td>23456</td>
      <td>24192</td>
      <td>38316</td>
      <td>46522</td>
      <td>54913</td>
      <td>65685</td>
      <td>76835</td>
      <td>23,019,347.460000</td>
      <td>578,832,399.000000</td>
      <td>0.000113</td>
    </tr>
    <tr>
      <th>Africa</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>3</td>
      <td>3</td>
      <td>4</td>
      <td>8</td>
      <td>11</td>
      <td>21</td>
      <td>24</td>
      <td>43</td>
      <td>43</td>
      <td>83</td>
      <td>91</td>
      <td>104</td>
      <td>118</td>
      <td>134</td>
      <td>177</td>
      <td>254</td>
      <td>313</td>
      <td>401</td>
      <td>484</td>
      <td>18,831,425.000000</td>
      <td>876,847,190.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>Oceania</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>4</td>
      <td>5</td>
      <td>5</td>
      <td>6</td>
      <td>9</td>
      <td>9</td>
      <td>12</td>
      <td>12</td>
      <td>12</td>
      <td>13</td>
      <td>13</td>
      <td>14</td>
      <td>15</td>
      <td>15</td>
      <td>15</td>
      <td>15</td>
      <td>15</td>
      <td>15</td>
      <td>15</td>
      <td>15</td>
      <td>15</td>
      <td>15</td>
      <td>15</td>
      <td>15</td>
      <td>15</td>
      <td>15</td>
      <td>19</td>
      <td>22</td>
      <td>22</td>
      <td>22</td>
      <td>22</td>
      <td>22</td>
      <td>23</td>
      <td>24</td>
      <td>26</td>
      <td>28</td>
      <td>31</td>
      <td>40</td>
      <td>55</td>
      <td>58</td>
      <td>64</td>
      <td>68</td>
      <td>81</td>
      <td>96</td>
      <td>112</td>
      <td>133</td>
      <td>133</td>
      <td>205</td>
      <td>256</td>
      <td>305</td>
      <td>385</td>
      <td>464</td>
      <td>7,962,491.000000</td>
      <td>29,156,379.000000</td>
      <td>0.000013</td>
    </tr>
  </tbody>
</table>
</div>




```python
df_subregion = df_world_covid.groupby(df_world_covid['subregion']).sum()
df_subregion = df_subregion.drop(["ccn3", "independent", "landlocked"], axis=1)

# SORT LAST VALUE 
df_subregion = df_subregion.sort_values(by=df_subregion .columns[-3], ascending=[False])


pd.set_option('display.max_columns', None)
df_subregion.head()

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
      <th>1/22/20</th>
      <th>1/23/20</th>
      <th>1/24/20</th>
      <th>1/25/20</th>
      <th>1/26/20</th>
      <th>1/27/20</th>
      <th>1/28/20</th>
      <th>1/29/20</th>
      <th>1/30/20</th>
      <th>1/31/20</th>
      <th>2/1/20</th>
      <th>2/2/20</th>
      <th>2/3/20</th>
      <th>2/4/20</th>
      <th>2/5/20</th>
      <th>2/6/20</th>
      <th>2/7/20</th>
      <th>2/8/20</th>
      <th>2/9/20</th>
      <th>2/10/20</th>
      <th>2/11/20</th>
      <th>2/12/20</th>
      <th>2/13/20</th>
      <th>2/14/20</th>
      <th>2/15/20</th>
      <th>2/16/20</th>
      <th>2/17/20</th>
      <th>2/18/20</th>
      <th>2/19/20</th>
      <th>2/20/20</th>
      <th>2/21/20</th>
      <th>2/22/20</th>
      <th>2/23/20</th>
      <th>2/24/20</th>
      <th>2/25/20</th>
      <th>2/26/20</th>
      <th>2/27/20</th>
      <th>2/28/20</th>
      <th>2/29/20</th>
      <th>3/1/20</th>
      <th>3/2/20</th>
      <th>3/3/20</th>
      <th>3/4/20</th>
      <th>3/5/20</th>
      <th>3/6/20</th>
      <th>3/7/20</th>
      <th>3/8/20</th>
      <th>3/9/20</th>
      <th>3/10/20</th>
      <th>3/11/20</th>
      <th>3/12/20</th>
      <th>3/13/20</th>
      <th>3/14/20</th>
      <th>3/15/20</th>
      <th>3/16/20</th>
      <th>3/17/20</th>
      <th>area</th>
      <th>population</th>
      <th>percent</th>
    </tr>
    <tr>
      <th>subregion</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>North America</th>
      <td>1</td>
      <td>1</td>
      <td>2</td>
      <td>2</td>
      <td>6</td>
      <td>6</td>
      <td>7</td>
      <td>7</td>
      <td>7</td>
      <td>11</td>
      <td>12</td>
      <td>12</td>
      <td>15</td>
      <td>15</td>
      <td>16</td>
      <td>16</td>
      <td>18</td>
      <td>18</td>
      <td>18</td>
      <td>18</td>
      <td>19</td>
      <td>19</td>
      <td>20</td>
      <td>20</td>
      <td>20</td>
      <td>20</td>
      <td>21</td>
      <td>21</td>
      <td>21</td>
      <td>21</td>
      <td>24</td>
      <td>24</td>
      <td>24</td>
      <td>61</td>
      <td>62</td>
      <td>68</td>
      <td>71</td>
      <td>75</td>
      <td>92</td>
      <td>103</td>
      <td>130</td>
      <td>153</td>
      <td>187</td>
      <td>259</td>
      <td>317</td>
      <td>462</td>
      <td>589</td>
      <td>667</td>
      <td>1045</td>
      <td>1397</td>
      <td>1792</td>
      <td>2384</td>
      <td>2951</td>
      <td>3792</td>
      <td>5101</td>
      <td>6982</td>
      <td>23,487,741.000000</td>
      <td>490,303,418.000000</td>
      <td>0.005118</td>
    </tr>
    <tr>
      <th>Eastern Europe</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>3</td>
      <td>3</td>
      <td>6</td>
      <td>6</td>
      <td>9</td>
      <td>10</td>
      <td>13</td>
      <td>25</td>
      <td>32</td>
      <td>54</td>
      <td>57</td>
      <td>93</td>
      <td>102</td>
      <td>137</td>
      <td>226</td>
      <td>262</td>
      <td>435</td>
      <td>613</td>
      <td>730</td>
      <td>915</td>
      <td>1177</td>
      <td>18,797,189.000000</td>
      <td>133,578,196.000000</td>
      <td>0.008071</td>
    </tr>
    <tr>
      <th>South America</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>2</td>
      <td>8</td>
      <td>8</td>
      <td>11</td>
      <td>16</td>
      <td>22</td>
      <td>34</td>
      <td>40</td>
      <td>62</td>
      <td>69</td>
      <td>91</td>
      <td>124</td>
      <td>143</td>
      <td>293</td>
      <td>358</td>
      <td>421</td>
      <td>637</td>
      <td>920</td>
      <td>17,737,675.000000</td>
      <td>424,107,976.000000</td>
      <td>0.004517</td>
    </tr>
    <tr>
      <th>Eastern Asia</th>
      <td>552</td>
      <td>646</td>
      <td>927</td>
      <td>1413</td>
      <td>2086</td>
      <td>2890</td>
      <td>5528</td>
      <td>6106</td>
      <td>8165</td>
      <td>9838</td>
      <td>11933</td>
      <td>16675</td>
      <td>19761</td>
      <td>23756</td>
      <td>27492</td>
      <td>30671</td>
      <td>34175</td>
      <td>36880</td>
      <td>39898</td>
      <td>42425</td>
      <td>44458</td>
      <td>44833</td>
      <td>59969</td>
      <td>66433</td>
      <td>68502</td>
      <td>70621</td>
      <td>72552</td>
      <td>74338</td>
      <td>74757</td>
      <td>75299</td>
      <td>75885</td>
      <td>77582</td>
      <td>77799</td>
      <td>78263</td>
      <td>78932</td>
      <td>79648</td>
      <td>80612</td>
      <td>81527</td>
      <td>82786</td>
      <td>83964</td>
      <td>84786</td>
      <td>85782</td>
      <td>86380</td>
      <td>87029</td>
      <td>87748</td>
      <td>88317</td>
      <td>88684</td>
      <td>88894</td>
      <td>89029</td>
      <td>89364</td>
      <td>89490</td>
      <td>89676</td>
      <td>89890</td>
      <td>90064</td>
      <td>90162</td>
      <td>90338</td>
      <td>11,785,404.000000</td>
      <td>1,591,059,706.000000</td>
      <td>0.022921</td>
    </tr>
    <tr>
      <th>Australia and New Zealand</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>4</td>
      <td>5</td>
      <td>5</td>
      <td>6</td>
      <td>9</td>
      <td>9</td>
      <td>12</td>
      <td>12</td>
      <td>12</td>
      <td>13</td>
      <td>13</td>
      <td>14</td>
      <td>15</td>
      <td>15</td>
      <td>15</td>
      <td>15</td>
      <td>15</td>
      <td>15</td>
      <td>15</td>
      <td>15</td>
      <td>15</td>
      <td>15</td>
      <td>15</td>
      <td>15</td>
      <td>15</td>
      <td>15</td>
      <td>19</td>
      <td>22</td>
      <td>22</td>
      <td>22</td>
      <td>22</td>
      <td>22</td>
      <td>23</td>
      <td>24</td>
      <td>26</td>
      <td>28</td>
      <td>31</td>
      <td>40</td>
      <td>55</td>
      <td>58</td>
      <td>64</td>
      <td>68</td>
      <td>81</td>
      <td>96</td>
      <td>112</td>
      <td>133</td>
      <td>133</td>
      <td>205</td>
      <td>256</td>
      <td>305</td>
      <td>385</td>
      <td>464</td>
      <td>7,962,491.000000</td>
      <td>29,156,379.000000</td>
      <td>0.002104</td>
    </tr>
  </tbody>
</table>
</div>




```python

```

```python
# Load the Pandas libraries with alias 'pd' 
import pandas as pd 
```


```python
# Load data from https://github.com/CSSEGISandData/COVID-19 
# Novel Coronavirus (COVID-19) Cases, provided by JHU CSSE https://systems.jhu.edu/research/publ…

df_world_covid_19_data = pd.read_csv("https://raw.githubusercontent.com/CSSEGISandData/COVID-19/master/csse_covid_19_data/csse_covid_19_time_series/time_series_19-covid-Confirmed.csv")

# delete unused columns 
df_world_covid_19_data.drop(df_world_covid_19_data.columns[[2,3]], axis = 1, inplace = True) 

# fix some country names
df_world_covid_19_data['Country/Region'] = df_world_covid_19_data['Country/Region'].replace("Congo (Brazzaville)", "DR Congo")
df_world_covid_19_data['Country/Region'] = df_world_covid_19_data['Country/Region'].replace("Congo (Kinshasa)", "Republic of the Congo")
df_world_covid_19_data['Country/Region'] = df_world_covid_19_data['Country/Region'].replace("Cote d'Ivoire", "Ivory Coast")
df_world_covid_19_data['Country/Region'] = df_world_covid_19_data['Country/Region'].replace("Holy See", "Vatican City")
df_world_covid_19_data['Country/Region'] = df_world_covid_19_data['Country/Region'].replace("The Bahamas", "Bahamas")
df_world_covid_19_data['Country/Region'] = df_world_covid_19_data['Country/Region'].replace("US", "United States")
df_world_covid_19_data['Country/Region'] = df_world_covid_19_data['Country/Region'].replace("Taiwan*", "Taiwan")
df_world_covid_19_data['Country/Region'] = df_world_covid_19_data['Country/Region'].replace("Korea, South", "South Korea")

# Group & Sum by Country
df_world_covid_19_data = df_world_covid_19_data.groupby(df_world_covid_19_data['Country/Region']).sum()

# SORT LAST VALUE 
df_world_covid_19_data = df_world_covid_19_data.sort_values(by=df_world_covid_19_data.columns[-1], ascending=[False])

# DEBUG
# df_world_covid_19_data.head(5)
```


```python
# Load data from https://github.com/mledoze/countries
# World countries in JSON, CSV, XML and Yaml. Any help is welcome! https://git.io/countries

df_world_contry_code = pd.read_csv("https://raw.githubusercontent.com/mledoze/countries/master/dist/countries.csv")

# create a common index 
df_world_contry_code["Country/Region"] = df_world_contry_code["name"].str.split(",", n = 1, expand = True)[0] 

# DEBUG
#df_world_contry_code.head(5)

```


```python
# Load data from https://github.com/samayo/country-json
# A simple data of the world by country each in JSON format. https://data.world/samayo

df_world_contry_population = pd.read_json("https://raw.githubusercontent.com/samayo/country-json/master/src/country-by-population.json")
# create a common index 
df_world_contry_population.rename(columns={'country':'Country/Region'}, inplace=True)

# DEBUG
#df_world_contry_population.head(5)
```


```python
# Now merge dataframes df_world_covid_19_data & df_world_contry_code
df_merge1 = pd.merge(df_world_covid_19_data, df_world_contry_code, how='left', on=['Country/Region'])
df_world_covid = pd.merge(df_merge1, df_world_contry_population, how='left', on=['Country/Region'])

# DEBUG
#df_world_covid.head(5)
```


```python
# looking for countries not found  
#filtered_df = df_world_covid[df_world_covid['region'].isnull()]
#filtered_df.head(1000)

# 3 unused country 
```


```python
# NOW ADD SOME CALCS
df_world_covid_all = df_world_covid

# PERCENTS OF SICKED ON POPULATIONS
df_world_covid_all['percent'] = df_world_covid_all['3/17/20']/df_world_covid_all['population']*100
df_world_covid_all = df_world_covid_all.sort_values(by=df_world_covid_all.columns[-1], ascending=[False])

```


```python
# top country 
df_world_covid_all = df_world_covid_all.sort_values(by=df_world_covid_all.columns[55], ascending=[False])
df_top_country = df_world_covid_all[['Country/Region', '3/17/20', 'population', 'percent']].copy()
df_top_country.head(10)


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
      <th>Country/Region</th>
      <th>3/17/20</th>
      <th>population</th>
      <th>percent</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>China</td>
      <td>81058</td>
      <td>1.409517e+09</td>
      <td>0.005751</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Italy</td>
      <td>31506</td>
      <td>5.935990e+07</td>
      <td>0.053076</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Iran</td>
      <td>16169</td>
      <td>8.116279e+07</td>
      <td>0.019922</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Spain</td>
      <td>11748</td>
      <td>4.635432e+07</td>
      <td>0.025344</td>
    </tr>
    <tr>
      <th>5</th>
      <td>South Korea</td>
      <td>8320</td>
      <td>5.098221e+07</td>
      <td>0.016319</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Germany</td>
      <td>9257</td>
      <td>8.211422e+07</td>
      <td>0.011273</td>
    </tr>
    <tr>
      <th>6</th>
      <td>France</td>
      <td>7699</td>
      <td>6.497955e+07</td>
      <td>0.011848</td>
    </tr>
    <tr>
      <th>7</th>
      <td>United States</td>
      <td>6421</td>
      <td>3.244595e+08</td>
      <td>0.001979</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Switzerland</td>
      <td>2700</td>
      <td>8.476005e+06</td>
      <td>0.031855</td>
    </tr>
    <tr>
      <th>9</th>
      <td>United Kingdom</td>
      <td>1960</td>
      <td>6.618158e+07</td>
      <td>0.002962</td>
    </tr>
  </tbody>
</table>
</div>




```python
import yaml
import pickle

with open('df_top_country.yml', 'w') as file:
    yaml.dump({'simple': df_top_country.to_dict(orient='records')}, file, default_flow_style=False)
diction=df_top_country.to_dict(orient='records')
f = open("file.txt","wb")
pickle.dump(diction,f)
f.close()
```


```python
#
df_percentile_by_country = df_world_covid_all[['Country/Region', 'percent']].copy()
df_percentile_by_country.head(10)
```


```python


```


```python
df_region = df_world_covid.groupby(df_world_covid['region']).sum()
df_region = df_region.drop(["ccn3", "independent", "landlocked"], axis=1)

# SORT LAST VALUE 
df_region = df_region.sort_values(by=df_region.columns[-3], ascending=[False])
pd.options.display.float_format = '{:,.6f}'.format
df_region['percent'] = df_region['3/16/20']/df_region['population']
df_region.describe()  
# df_region.head(15)
df_region.head(15)
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
      <th>1/22/20</th>
      <th>1/23/20</th>
      <th>1/24/20</th>
      <th>1/25/20</th>
      <th>1/26/20</th>
      <th>1/27/20</th>
      <th>1/28/20</th>
      <th>1/29/20</th>
      <th>1/30/20</th>
      <th>1/31/20</th>
      <th>2/1/20</th>
      <th>2/2/20</th>
      <th>2/3/20</th>
      <th>2/4/20</th>
      <th>2/5/20</th>
      <th>2/6/20</th>
      <th>2/7/20</th>
      <th>2/8/20</th>
      <th>2/9/20</th>
      <th>2/10/20</th>
      <th>2/11/20</th>
      <th>2/12/20</th>
      <th>2/13/20</th>
      <th>2/14/20</th>
      <th>2/15/20</th>
      <th>2/16/20</th>
      <th>2/17/20</th>
      <th>2/18/20</th>
      <th>2/19/20</th>
      <th>2/20/20</th>
      <th>2/21/20</th>
      <th>2/22/20</th>
      <th>2/23/20</th>
      <th>2/24/20</th>
      <th>2/25/20</th>
      <th>2/26/20</th>
      <th>2/27/20</th>
      <th>2/28/20</th>
      <th>2/29/20</th>
      <th>3/1/20</th>
      <th>3/2/20</th>
      <th>3/3/20</th>
      <th>3/4/20</th>
      <th>3/5/20</th>
      <th>3/6/20</th>
      <th>3/7/20</th>
      <th>3/8/20</th>
      <th>3/9/20</th>
      <th>3/10/20</th>
      <th>3/11/20</th>
      <th>3/12/20</th>
      <th>3/13/20</th>
      <th>3/14/20</th>
      <th>3/15/20</th>
      <th>3/16/20</th>
      <th>3/17/20</th>
      <th>area</th>
      <th>population</th>
      <th>percent</th>
    </tr>
    <tr>
      <th>region</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Americas</th>
      <td>1</td>
      <td>1</td>
      <td>2</td>
      <td>2</td>
      <td>6</td>
      <td>6</td>
      <td>7</td>
      <td>7</td>
      <td>7</td>
      <td>11</td>
      <td>12</td>
      <td>12</td>
      <td>15</td>
      <td>15</td>
      <td>16</td>
      <td>16</td>
      <td>18</td>
      <td>18</td>
      <td>18</td>
      <td>18</td>
      <td>19</td>
      <td>19</td>
      <td>20</td>
      <td>20</td>
      <td>20</td>
      <td>20</td>
      <td>21</td>
      <td>21</td>
      <td>21</td>
      <td>21</td>
      <td>24</td>
      <td>24</td>
      <td>24</td>
      <td>61</td>
      <td>62</td>
      <td>69</td>
      <td>72</td>
      <td>76</td>
      <td>94</td>
      <td>112</td>
      <td>139</td>
      <td>165</td>
      <td>204</td>
      <td>282</td>
      <td>354</td>
      <td>507</td>
      <td>663</td>
      <td>752</td>
      <td>1153</td>
      <td>1553</td>
      <td>1983</td>
      <td>2752</td>
      <td>3413</td>
      <td>4329</td>
      <td>5887</td>
      <td>8095</td>
      <td>41,765,118.000000</td>
      <td>977,667,371.000000</td>
      <td>0.000006</td>
    </tr>
    <tr>
      <th>Asia</th>
      <td>554</td>
      <td>652</td>
      <td>937</td>
      <td>1429</td>
      <td>2105</td>
      <td>2912</td>
      <td>5558</td>
      <td>6143</td>
      <td>8208</td>
      <td>9889</td>
      <td>11991</td>
      <td>16738</td>
      <td>19827</td>
      <td>23836</td>
      <td>27578</td>
      <td>30759</td>
      <td>34266</td>
      <td>36990</td>
      <td>40015</td>
      <td>42551</td>
      <td>44588</td>
      <td>44966</td>
      <td>60112</td>
      <td>66585</td>
      <td>68662</td>
      <td>70786</td>
      <td>72720</td>
      <td>74510</td>
      <td>74934</td>
      <td>75479</td>
      <td>76081</td>
      <td>77792</td>
      <td>78028</td>
      <td>78516</td>
      <td>79255</td>
      <td>80056</td>
      <td>81147</td>
      <td>82218</td>
      <td>83717</td>
      <td>85317</td>
      <td>86693</td>
      <td>88559</td>
      <td>89793</td>
      <td>91070</td>
      <td>93110</td>
      <td>94848</td>
      <td>96069</td>
      <td>96939</td>
      <td>98134</td>
      <td>99909</td>
      <td>101205</td>
      <td>103047</td>
      <td>104956</td>
      <td>106893</td>
      <td>108477</td>
      <td>110571</td>
      <td>29,537,698.000000</td>
      <td>4,291,576,407.000000</td>
      <td>0.000025</td>
    </tr>
    <tr>
      <th>Europe</th>
      <td>0</td>
      <td>0</td>
      <td>2</td>
      <td>3</td>
      <td>3</td>
      <td>4</td>
      <td>8</td>
      <td>10</td>
      <td>10</td>
      <td>18</td>
      <td>23</td>
      <td>25</td>
      <td>27</td>
      <td>28</td>
      <td>28</td>
      <td>28</td>
      <td>31</td>
      <td>36</td>
      <td>38</td>
      <td>43</td>
      <td>45</td>
      <td>46</td>
      <td>46</td>
      <td>46</td>
      <td>47</td>
      <td>47</td>
      <td>47</td>
      <td>47</td>
      <td>47</td>
      <td>47</td>
      <td>64</td>
      <td>106</td>
      <td>199</td>
      <td>277</td>
      <td>381</td>
      <td>541</td>
      <td>805</td>
      <td>1094</td>
      <td>1466</td>
      <td>2203</td>
      <td>2730</td>
      <td>3359</td>
      <td>4341</td>
      <td>5742</td>
      <td>7517</td>
      <td>9659</td>
      <td>12203</td>
      <td>14987</td>
      <td>18393</td>
      <td>23456</td>
      <td>24192</td>
      <td>38316</td>
      <td>46522</td>
      <td>54913</td>
      <td>65685</td>
      <td>76835</td>
      <td>23,019,347.460000</td>
      <td>578,832,399.000000</td>
      <td>0.000113</td>
    </tr>
    <tr>
      <th>Africa</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>3</td>
      <td>3</td>
      <td>4</td>
      <td>8</td>
      <td>11</td>
      <td>21</td>
      <td>24</td>
      <td>43</td>
      <td>43</td>
      <td>83</td>
      <td>91</td>
      <td>104</td>
      <td>118</td>
      <td>134</td>
      <td>177</td>
      <td>254</td>
      <td>313</td>
      <td>401</td>
      <td>484</td>
      <td>18,831,425.000000</td>
      <td>876,847,190.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>Oceania</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>4</td>
      <td>5</td>
      <td>5</td>
      <td>6</td>
      <td>9</td>
      <td>9</td>
      <td>12</td>
      <td>12</td>
      <td>12</td>
      <td>13</td>
      <td>13</td>
      <td>14</td>
      <td>15</td>
      <td>15</td>
      <td>15</td>
      <td>15</td>
      <td>15</td>
      <td>15</td>
      <td>15</td>
      <td>15</td>
      <td>15</td>
      <td>15</td>
      <td>15</td>
      <td>15</td>
      <td>15</td>
      <td>15</td>
      <td>19</td>
      <td>22</td>
      <td>22</td>
      <td>22</td>
      <td>22</td>
      <td>22</td>
      <td>23</td>
      <td>24</td>
      <td>26</td>
      <td>28</td>
      <td>31</td>
      <td>40</td>
      <td>55</td>
      <td>58</td>
      <td>64</td>
      <td>68</td>
      <td>81</td>
      <td>96</td>
      <td>112</td>
      <td>133</td>
      <td>133</td>
      <td>205</td>
      <td>256</td>
      <td>305</td>
      <td>385</td>
      <td>464</td>
      <td>7,962,491.000000</td>
      <td>29,156,379.000000</td>
      <td>0.000013</td>
    </tr>
  </tbody>
</table>
</div>




```python
df_subregion = df_world_covid.groupby(df_world_covid['subregion']).sum()
df_subregion = df_subregion.drop(["ccn3", "independent", "landlocked"], axis=1)

# SORT LAST VALUE 
df_subregion = df_subregion.sort_values(by=df_subregion .columns[-3], ascending=[False])


pd.set_option('display.max_columns', None)
df_subregion.head()

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
      <th>1/22/20</th>
      <th>1/23/20</th>
      <th>1/24/20</th>
      <th>1/25/20</th>
      <th>1/26/20</th>
      <th>1/27/20</th>
      <th>1/28/20</th>
      <th>1/29/20</th>
      <th>1/30/20</th>
      <th>1/31/20</th>
      <th>2/1/20</th>
      <th>2/2/20</th>
      <th>2/3/20</th>
      <th>2/4/20</th>
      <th>2/5/20</th>
      <th>2/6/20</th>
      <th>2/7/20</th>
      <th>2/8/20</th>
      <th>2/9/20</th>
      <th>2/10/20</th>
      <th>2/11/20</th>
      <th>2/12/20</th>
      <th>2/13/20</th>
      <th>2/14/20</th>
      <th>2/15/20</th>
      <th>2/16/20</th>
      <th>2/17/20</th>
      <th>2/18/20</th>
      <th>2/19/20</th>
      <th>2/20/20</th>
      <th>2/21/20</th>
      <th>2/22/20</th>
      <th>2/23/20</th>
      <th>2/24/20</th>
      <th>2/25/20</th>
      <th>2/26/20</th>
      <th>2/27/20</th>
      <th>2/28/20</th>
      <th>2/29/20</th>
      <th>3/1/20</th>
      <th>3/2/20</th>
      <th>3/3/20</th>
      <th>3/4/20</th>
      <th>3/5/20</th>
      <th>3/6/20</th>
      <th>3/7/20</th>
      <th>3/8/20</th>
      <th>3/9/20</th>
      <th>3/10/20</th>
      <th>3/11/20</th>
      <th>3/12/20</th>
      <th>3/13/20</th>
      <th>3/14/20</th>
      <th>3/15/20</th>
      <th>3/16/20</th>
      <th>3/17/20</th>
      <th>area</th>
      <th>population</th>
      <th>percent</th>
    </tr>
    <tr>
      <th>subregion</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>North America</th>
      <td>1</td>
      <td>1</td>
      <td>2</td>
      <td>2</td>
      <td>6</td>
      <td>6</td>
      <td>7</td>
      <td>7</td>
      <td>7</td>
      <td>11</td>
      <td>12</td>
      <td>12</td>
      <td>15</td>
      <td>15</td>
      <td>16</td>
      <td>16</td>
      <td>18</td>
      <td>18</td>
      <td>18</td>
      <td>18</td>
      <td>19</td>
      <td>19</td>
      <td>20</td>
      <td>20</td>
      <td>20</td>
      <td>20</td>
      <td>21</td>
      <td>21</td>
      <td>21</td>
      <td>21</td>
      <td>24</td>
      <td>24</td>
      <td>24</td>
      <td>61</td>
      <td>62</td>
      <td>68</td>
      <td>71</td>
      <td>75</td>
      <td>92</td>
      <td>103</td>
      <td>130</td>
      <td>153</td>
      <td>187</td>
      <td>259</td>
      <td>317</td>
      <td>462</td>
      <td>589</td>
      <td>667</td>
      <td>1045</td>
      <td>1397</td>
      <td>1792</td>
      <td>2384</td>
      <td>2951</td>
      <td>3792</td>
      <td>5101</td>
      <td>6982</td>
      <td>23,487,741.000000</td>
      <td>490,303,418.000000</td>
      <td>0.005118</td>
    </tr>
    <tr>
      <th>Eastern Europe</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>3</td>
      <td>3</td>
      <td>6</td>
      <td>6</td>
      <td>9</td>
      <td>10</td>
      <td>13</td>
      <td>25</td>
      <td>32</td>
      <td>54</td>
      <td>57</td>
      <td>93</td>
      <td>102</td>
      <td>137</td>
      <td>226</td>
      <td>262</td>
      <td>435</td>
      <td>613</td>
      <td>730</td>
      <td>915</td>
      <td>1177</td>
      <td>18,797,189.000000</td>
      <td>133,578,196.000000</td>
      <td>0.008071</td>
    </tr>
    <tr>
      <th>South America</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>2</td>
      <td>8</td>
      <td>8</td>
      <td>11</td>
      <td>16</td>
      <td>22</td>
      <td>34</td>
      <td>40</td>
      <td>62</td>
      <td>69</td>
      <td>91</td>
      <td>124</td>
      <td>143</td>
      <td>293</td>
      <td>358</td>
      <td>421</td>
      <td>637</td>
      <td>920</td>
      <td>17,737,675.000000</td>
      <td>424,107,976.000000</td>
      <td>0.004517</td>
    </tr>
    <tr>
      <th>Eastern Asia</th>
      <td>552</td>
      <td>646</td>
      <td>927</td>
      <td>1413</td>
      <td>2086</td>
      <td>2890</td>
      <td>5528</td>
      <td>6106</td>
      <td>8165</td>
      <td>9838</td>
      <td>11933</td>
      <td>16675</td>
      <td>19761</td>
      <td>23756</td>
      <td>27492</td>
      <td>30671</td>
      <td>34175</td>
      <td>36880</td>
      <td>39898</td>
      <td>42425</td>
      <td>44458</td>
      <td>44833</td>
      <td>59969</td>
      <td>66433</td>
      <td>68502</td>
      <td>70621</td>
      <td>72552</td>
      <td>74338</td>
      <td>74757</td>
      <td>75299</td>
      <td>75885</td>
      <td>77582</td>
      <td>77799</td>
      <td>78263</td>
      <td>78932</td>
      <td>79648</td>
      <td>80612</td>
      <td>81527</td>
      <td>82786</td>
      <td>83964</td>
      <td>84786</td>
      <td>85782</td>
      <td>86380</td>
      <td>87029</td>
      <td>87748</td>
      <td>88317</td>
      <td>88684</td>
      <td>88894</td>
      <td>89029</td>
      <td>89364</td>
      <td>89490</td>
      <td>89676</td>
      <td>89890</td>
      <td>90064</td>
      <td>90162</td>
      <td>90338</td>
      <td>11,785,404.000000</td>
      <td>1,591,059,706.000000</td>
      <td>0.022921</td>
    </tr>
    <tr>
      <th>Australia and New Zealand</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>4</td>
      <td>5</td>
      <td>5</td>
      <td>6</td>
      <td>9</td>
      <td>9</td>
      <td>12</td>
      <td>12</td>
      <td>12</td>
      <td>13</td>
      <td>13</td>
      <td>14</td>
      <td>15</td>
      <td>15</td>
      <td>15</td>
      <td>15</td>
      <td>15</td>
      <td>15</td>
      <td>15</td>
      <td>15</td>
      <td>15</td>
      <td>15</td>
      <td>15</td>
      <td>15</td>
      <td>15</td>
      <td>15</td>
      <td>19</td>
      <td>22</td>
      <td>22</td>
      <td>22</td>
      <td>22</td>
      <td>22</td>
      <td>23</td>
      <td>24</td>
      <td>26</td>
      <td>28</td>
      <td>31</td>
      <td>40</td>
      <td>55</td>
      <td>58</td>
      <td>64</td>
      <td>68</td>
      <td>81</td>
      <td>96</td>
      <td>112</td>
      <td>133</td>
      <td>133</td>
      <td>205</td>
      <td>256</td>
      <td>305</td>
      <td>385</td>
      <td>464</td>
      <td>7,962,491.000000</td>
      <td>29,156,379.000000</td>
      <td>0.002104</td>
    </tr>
  </tbody>
</table>
</div>




```python

```

```python
# Load the Pandas libraries with alias 'pd' 
import pandas as pd 
```


```python
# Load data from https://github.com/CSSEGISandData/COVID-19 
# Novel Coronavirus (COVID-19) Cases, provided by JHU CSSE https://systems.jhu.edu/research/publ…

df_world_covid_19_data = pd.read_csv("https://raw.githubusercontent.com/CSSEGISandData/COVID-19/master/csse_covid_19_data/csse_covid_19_time_series/time_series_19-covid-Confirmed.csv")

# delete unused columns 
df_world_covid_19_data.drop(df_world_covid_19_data.columns[[2,3]], axis = 1, inplace = True) 

# fix some country names
df_world_covid_19_data['Country/Region'] = df_world_covid_19_data['Country/Region'].replace("Congo (Brazzaville)", "DR Congo")
df_world_covid_19_data['Country/Region'] = df_world_covid_19_data['Country/Region'].replace("Congo (Kinshasa)", "Republic of the Congo")
df_world_covid_19_data['Country/Region'] = df_world_covid_19_data['Country/Region'].replace("Cote d'Ivoire", "Ivory Coast")
df_world_covid_19_data['Country/Region'] = df_world_covid_19_data['Country/Region'].replace("Holy See", "Vatican City")
df_world_covid_19_data['Country/Region'] = df_world_covid_19_data['Country/Region'].replace("The Bahamas", "Bahamas")
df_world_covid_19_data['Country/Region'] = df_world_covid_19_data['Country/Region'].replace("US", "United States")
df_world_covid_19_data['Country/Region'] = df_world_covid_19_data['Country/Region'].replace("Taiwan*", "Taiwan")
df_world_covid_19_data['Country/Region'] = df_world_covid_19_data['Country/Region'].replace("Korea, South", "South Korea")

# Group & Sum by Country
df_world_covid_19_data = df_world_covid_19_data.groupby(df_world_covid_19_data['Country/Region']).sum()

# SORT LAST VALUE 
df_world_covid_19_data = df_world_covid_19_data.sort_values(by=df_world_covid_19_data.columns[-1], ascending=[False])

# DEBUG
# df_world_covid_19_data.head(5)
```


```python
# Load data from https://github.com/mledoze/countries
# World countries in JSON, CSV, XML and Yaml. Any help is welcome! https://git.io/countries

df_world_contry_code = pd.read_csv("https://raw.githubusercontent.com/mledoze/countries/master/dist/countries.csv")

# create a common index 
df_world_contry_code["Country/Region"] = df_world_contry_code["name"].str.split(",", n = 1, expand = True)[0] 

# DEBUG
#df_world_contry_code.head(5)

```


```python
# Load data from https://github.com/samayo/country-json
# A simple data of the world by country each in JSON format. https://data.world/samayo

df_world_contry_population = pd.read_json("https://raw.githubusercontent.com/samayo/country-json/master/src/country-by-population.json")
# create a common index 
df_world_contry_population.rename(columns={'country':'Country/Region'}, inplace=True)

# DEBUG
#df_world_contry_population.head(5)
```


```python
# Now merge dataframes df_world_covid_19_data & df_world_contry_code
df_merge1 = pd.merge(df_world_covid_19_data, df_world_contry_code, how='left', on=['Country/Region'])
df_world_covid = pd.merge(df_merge1, df_world_contry_population, how='left', on=['Country/Region'])

# DEBUG
#df_world_covid.head(5)
```


```python
# looking for countries not found  
#filtered_df = df_world_covid[df_world_covid['region'].isnull()]
#filtered_df.head(1000)

# 3 unused country 
```


```python
# NOW ADD SOME CALCS
df_world_covid_all = df_world_covid

# PERCENTS OF SICKED ON POPULATIONS
df_world_covid_all['percent'] = df_world_covid_all['3/17/20']/df_world_covid_all['population']*100
df_world_covid_all = df_world_covid_all.sort_values(by=df_world_covid_all.columns[-1], ascending=[False])

```


```python
# top country 
df_world_covid_all = df_world_covid_all.sort_values(by=df_world_covid_all.columns[55], ascending=[False])
df_top_country = df_world_covid_all[['Country/Region', '3/17/20', 'population', 'percent']].copy()
df_top_country.head(10)


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
      <th>Country/Region</th>
      <th>3/17/20</th>
      <th>population</th>
      <th>percent</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>China</td>
      <td>81058</td>
      <td>1.409517e+09</td>
      <td>0.005751</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Italy</td>
      <td>31506</td>
      <td>5.935990e+07</td>
      <td>0.053076</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Iran</td>
      <td>16169</td>
      <td>8.116279e+07</td>
      <td>0.019922</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Spain</td>
      <td>11748</td>
      <td>4.635432e+07</td>
      <td>0.025344</td>
    </tr>
    <tr>
      <th>5</th>
      <td>South Korea</td>
      <td>8320</td>
      <td>5.098221e+07</td>
      <td>0.016319</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Germany</td>
      <td>9257</td>
      <td>8.211422e+07</td>
      <td>0.011273</td>
    </tr>
    <tr>
      <th>6</th>
      <td>France</td>
      <td>7699</td>
      <td>6.497955e+07</td>
      <td>0.011848</td>
    </tr>
    <tr>
      <th>7</th>
      <td>United States</td>
      <td>6421</td>
      <td>3.244595e+08</td>
      <td>0.001979</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Switzerland</td>
      <td>2700</td>
      <td>8.476005e+06</td>
      <td>0.031855</td>
    </tr>
    <tr>
      <th>9</th>
      <td>United Kingdom</td>
      <td>1960</td>
      <td>6.618158e+07</td>
      <td>0.002962</td>
    </tr>
  </tbody>
</table>
</div>




```python
import yaml
import pickle

with open('df_top_country.yml', 'w') as file:
    yaml.dump({'simple': df_top_country.to_dict(orient='records')}, file, default_flow_style=False)
diction=df_top_country.to_dict(orient='records')
f = open("file.txt","wb")
pickle.dump(diction,f)
f.close()
```


```python
#
df_percentile_by_country = df_world_covid_all[['Country/Region', 'percent']].copy()
df_percentile_by_country.head(10)
```


```python


```


```python
df_region = df_world_covid.groupby(df_world_covid['region']).sum()
df_region = df_region.drop(["ccn3", "independent", "landlocked"], axis=1)

# SORT LAST VALUE 
df_region = df_region.sort_values(by=df_region.columns[-3], ascending=[False])
pd.options.display.float_format = '{:,.6f}'.format
df_region['percent'] = df_region['3/16/20']/df_region['population']
df_region.describe()  
# df_region.head(15)
df_region.head(15)
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
      <th>1/22/20</th>
      <th>1/23/20</th>
      <th>1/24/20</th>
      <th>1/25/20</th>
      <th>1/26/20</th>
      <th>1/27/20</th>
      <th>1/28/20</th>
      <th>1/29/20</th>
      <th>1/30/20</th>
      <th>1/31/20</th>
      <th>2/1/20</th>
      <th>2/2/20</th>
      <th>2/3/20</th>
      <th>2/4/20</th>
      <th>2/5/20</th>
      <th>2/6/20</th>
      <th>2/7/20</th>
      <th>2/8/20</th>
      <th>2/9/20</th>
      <th>2/10/20</th>
      <th>2/11/20</th>
      <th>2/12/20</th>
      <th>2/13/20</th>
      <th>2/14/20</th>
      <th>2/15/20</th>
      <th>2/16/20</th>
      <th>2/17/20</th>
      <th>2/18/20</th>
      <th>2/19/20</th>
      <th>2/20/20</th>
      <th>2/21/20</th>
      <th>2/22/20</th>
      <th>2/23/20</th>
      <th>2/24/20</th>
      <th>2/25/20</th>
      <th>2/26/20</th>
      <th>2/27/20</th>
      <th>2/28/20</th>
      <th>2/29/20</th>
      <th>3/1/20</th>
      <th>3/2/20</th>
      <th>3/3/20</th>
      <th>3/4/20</th>
      <th>3/5/20</th>
      <th>3/6/20</th>
      <th>3/7/20</th>
      <th>3/8/20</th>
      <th>3/9/20</th>
      <th>3/10/20</th>
      <th>3/11/20</th>
      <th>3/12/20</th>
      <th>3/13/20</th>
      <th>3/14/20</th>
      <th>3/15/20</th>
      <th>3/16/20</th>
      <th>3/17/20</th>
      <th>area</th>
      <th>population</th>
      <th>percent</th>
    </tr>
    <tr>
      <th>region</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Americas</th>
      <td>1</td>
      <td>1</td>
      <td>2</td>
      <td>2</td>
      <td>6</td>
      <td>6</td>
      <td>7</td>
      <td>7</td>
      <td>7</td>
      <td>11</td>
      <td>12</td>
      <td>12</td>
      <td>15</td>
      <td>15</td>
      <td>16</td>
      <td>16</td>
      <td>18</td>
      <td>18</td>
      <td>18</td>
      <td>18</td>
      <td>19</td>
      <td>19</td>
      <td>20</td>
      <td>20</td>
      <td>20</td>
      <td>20</td>
      <td>21</td>
      <td>21</td>
      <td>21</td>
      <td>21</td>
      <td>24</td>
      <td>24</td>
      <td>24</td>
      <td>61</td>
      <td>62</td>
      <td>69</td>
      <td>72</td>
      <td>76</td>
      <td>94</td>
      <td>112</td>
      <td>139</td>
      <td>165</td>
      <td>204</td>
      <td>282</td>
      <td>354</td>
      <td>507</td>
      <td>663</td>
      <td>752</td>
      <td>1153</td>
      <td>1553</td>
      <td>1983</td>
      <td>2752</td>
      <td>3413</td>
      <td>4329</td>
      <td>5887</td>
      <td>8095</td>
      <td>41,765,118.000000</td>
      <td>977,667,371.000000</td>
      <td>0.000006</td>
    </tr>
    <tr>
      <th>Asia</th>
      <td>554</td>
      <td>652</td>
      <td>937</td>
      <td>1429</td>
      <td>2105</td>
      <td>2912</td>
      <td>5558</td>
      <td>6143</td>
      <td>8208</td>
      <td>9889</td>
      <td>11991</td>
      <td>16738</td>
      <td>19827</td>
      <td>23836</td>
      <td>27578</td>
      <td>30759</td>
      <td>34266</td>
      <td>36990</td>
      <td>40015</td>
      <td>42551</td>
      <td>44588</td>
      <td>44966</td>
      <td>60112</td>
      <td>66585</td>
      <td>68662</td>
      <td>70786</td>
      <td>72720</td>
      <td>74510</td>
      <td>74934</td>
      <td>75479</td>
      <td>76081</td>
      <td>77792</td>
      <td>78028</td>
      <td>78516</td>
      <td>79255</td>
      <td>80056</td>
      <td>81147</td>
      <td>82218</td>
      <td>83717</td>
      <td>85317</td>
      <td>86693</td>
      <td>88559</td>
      <td>89793</td>
      <td>91070</td>
      <td>93110</td>
      <td>94848</td>
      <td>96069</td>
      <td>96939</td>
      <td>98134</td>
      <td>99909</td>
      <td>101205</td>
      <td>103047</td>
      <td>104956</td>
      <td>106893</td>
      <td>108477</td>
      <td>110571</td>
      <td>29,537,698.000000</td>
      <td>4,291,576,407.000000</td>
      <td>0.000025</td>
    </tr>
    <tr>
      <th>Europe</th>
      <td>0</td>
      <td>0</td>
      <td>2</td>
      <td>3</td>
      <td>3</td>
      <td>4</td>
      <td>8</td>
      <td>10</td>
      <td>10</td>
      <td>18</td>
      <td>23</td>
      <td>25</td>
      <td>27</td>
      <td>28</td>
      <td>28</td>
      <td>28</td>
      <td>31</td>
      <td>36</td>
      <td>38</td>
      <td>43</td>
      <td>45</td>
      <td>46</td>
      <td>46</td>
      <td>46</td>
      <td>47</td>
      <td>47</td>
      <td>47</td>
      <td>47</td>
      <td>47</td>
      <td>47</td>
      <td>64</td>
      <td>106</td>
      <td>199</td>
      <td>277</td>
      <td>381</td>
      <td>541</td>
      <td>805</td>
      <td>1094</td>
      <td>1466</td>
      <td>2203</td>
      <td>2730</td>
      <td>3359</td>
      <td>4341</td>
      <td>5742</td>
      <td>7517</td>
      <td>9659</td>
      <td>12203</td>
      <td>14987</td>
      <td>18393</td>
      <td>23456</td>
      <td>24192</td>
      <td>38316</td>
      <td>46522</td>
      <td>54913</td>
      <td>65685</td>
      <td>76835</td>
      <td>23,019,347.460000</td>
      <td>578,832,399.000000</td>
      <td>0.000113</td>
    </tr>
    <tr>
      <th>Africa</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>3</td>
      <td>3</td>
      <td>4</td>
      <td>8</td>
      <td>11</td>
      <td>21</td>
      <td>24</td>
      <td>43</td>
      <td>43</td>
      <td>83</td>
      <td>91</td>
      <td>104</td>
      <td>118</td>
      <td>134</td>
      <td>177</td>
      <td>254</td>
      <td>313</td>
      <td>401</td>
      <td>484</td>
      <td>18,831,425.000000</td>
      <td>876,847,190.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>Oceania</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>4</td>
      <td>5</td>
      <td>5</td>
      <td>6</td>
      <td>9</td>
      <td>9</td>
      <td>12</td>
      <td>12</td>
      <td>12</td>
      <td>13</td>
      <td>13</td>
      <td>14</td>
      <td>15</td>
      <td>15</td>
      <td>15</td>
      <td>15</td>
      <td>15</td>
      <td>15</td>
      <td>15</td>
      <td>15</td>
      <td>15</td>
      <td>15</td>
      <td>15</td>
      <td>15</td>
      <td>15</td>
      <td>15</td>
      <td>19</td>
      <td>22</td>
      <td>22</td>
      <td>22</td>
      <td>22</td>
      <td>22</td>
      <td>23</td>
      <td>24</td>
      <td>26</td>
      <td>28</td>
      <td>31</td>
      <td>40</td>
      <td>55</td>
      <td>58</td>
      <td>64</td>
      <td>68</td>
      <td>81</td>
      <td>96</td>
      <td>112</td>
      <td>133</td>
      <td>133</td>
      <td>205</td>
      <td>256</td>
      <td>305</td>
      <td>385</td>
      <td>464</td>
      <td>7,962,491.000000</td>
      <td>29,156,379.000000</td>
      <td>0.000013</td>
    </tr>
  </tbody>
</table>
</div>




```python
df_subregion = df_world_covid.groupby(df_world_covid['subregion']).sum()
df_subregion = df_subregion.drop(["ccn3", "independent", "landlocked"], axis=1)

# SORT LAST VALUE 
df_subregion = df_subregion.sort_values(by=df_subregion .columns[-3], ascending=[False])


pd.set_option('display.max_columns', None)
df_subregion.head()

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
      <th>1/22/20</th>
      <th>1/23/20</th>
      <th>1/24/20</th>
      <th>1/25/20</th>
      <th>1/26/20</th>
      <th>1/27/20</th>
      <th>1/28/20</th>
      <th>1/29/20</th>
      <th>1/30/20</th>
      <th>1/31/20</th>
      <th>2/1/20</th>
      <th>2/2/20</th>
      <th>2/3/20</th>
      <th>2/4/20</th>
      <th>2/5/20</th>
      <th>2/6/20</th>
      <th>2/7/20</th>
      <th>2/8/20</th>
      <th>2/9/20</th>
      <th>2/10/20</th>
      <th>2/11/20</th>
      <th>2/12/20</th>
      <th>2/13/20</th>
      <th>2/14/20</th>
      <th>2/15/20</th>
      <th>2/16/20</th>
      <th>2/17/20</th>
      <th>2/18/20</th>
      <th>2/19/20</th>
      <th>2/20/20</th>
      <th>2/21/20</th>
      <th>2/22/20</th>
      <th>2/23/20</th>
      <th>2/24/20</th>
      <th>2/25/20</th>
      <th>2/26/20</th>
      <th>2/27/20</th>
      <th>2/28/20</th>
      <th>2/29/20</th>
      <th>3/1/20</th>
      <th>3/2/20</th>
      <th>3/3/20</th>
      <th>3/4/20</th>
      <th>3/5/20</th>
      <th>3/6/20</th>
      <th>3/7/20</th>
      <th>3/8/20</th>
      <th>3/9/20</th>
      <th>3/10/20</th>
      <th>3/11/20</th>
      <th>3/12/20</th>
      <th>3/13/20</th>
      <th>3/14/20</th>
      <th>3/15/20</th>
      <th>3/16/20</th>
      <th>3/17/20</th>
      <th>area</th>
      <th>population</th>
      <th>percent</th>
    </tr>
    <tr>
      <th>subregion</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>North America</th>
      <td>1</td>
      <td>1</td>
      <td>2</td>
      <td>2</td>
      <td>6</td>
      <td>6</td>
      <td>7</td>
      <td>7</td>
      <td>7</td>
      <td>11</td>
      <td>12</td>
      <td>12</td>
      <td>15</td>
      <td>15</td>
      <td>16</td>
      <td>16</td>
      <td>18</td>
      <td>18</td>
      <td>18</td>
      <td>18</td>
      <td>19</td>
      <td>19</td>
      <td>20</td>
      <td>20</td>
      <td>20</td>
      <td>20</td>
      <td>21</td>
      <td>21</td>
      <td>21</td>
      <td>21</td>
      <td>24</td>
      <td>24</td>
      <td>24</td>
      <td>61</td>
      <td>62</td>
      <td>68</td>
      <td>71</td>
      <td>75</td>
      <td>92</td>
      <td>103</td>
      <td>130</td>
      <td>153</td>
      <td>187</td>
      <td>259</td>
      <td>317</td>
      <td>462</td>
      <td>589</td>
      <td>667</td>
      <td>1045</td>
      <td>1397</td>
      <td>1792</td>
      <td>2384</td>
      <td>2951</td>
      <td>3792</td>
      <td>5101</td>
      <td>6982</td>
      <td>23,487,741.000000</td>
      <td>490,303,418.000000</td>
      <td>0.005118</td>
    </tr>
    <tr>
      <th>Eastern Europe</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>3</td>
      <td>3</td>
      <td>6</td>
      <td>6</td>
      <td>9</td>
      <td>10</td>
      <td>13</td>
      <td>25</td>
      <td>32</td>
      <td>54</td>
      <td>57</td>
      <td>93</td>
      <td>102</td>
      <td>137</td>
      <td>226</td>
      <td>262</td>
      <td>435</td>
      <td>613</td>
      <td>730</td>
      <td>915</td>
      <td>1177</td>
      <td>18,797,189.000000</td>
      <td>133,578,196.000000</td>
      <td>0.008071</td>
    </tr>
    <tr>
      <th>South America</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>2</td>
      <td>8</td>
      <td>8</td>
      <td>11</td>
      <td>16</td>
      <td>22</td>
      <td>34</td>
      <td>40</td>
      <td>62</td>
      <td>69</td>
      <td>91</td>
      <td>124</td>
      <td>143</td>
      <td>293</td>
      <td>358</td>
      <td>421</td>
      <td>637</td>
      <td>920</td>
      <td>17,737,675.000000</td>
      <td>424,107,976.000000</td>
      <td>0.004517</td>
    </tr>
    <tr>
      <th>Eastern Asia</th>
      <td>552</td>
      <td>646</td>
      <td>927</td>
      <td>1413</td>
      <td>2086</td>
      <td>2890</td>
      <td>5528</td>
      <td>6106</td>
      <td>8165</td>
      <td>9838</td>
      <td>11933</td>
      <td>16675</td>
      <td>19761</td>
      <td>23756</td>
      <td>27492</td>
      <td>30671</td>
      <td>34175</td>
      <td>36880</td>
      <td>39898</td>
      <td>42425</td>
      <td>44458</td>
      <td>44833</td>
      <td>59969</td>
      <td>66433</td>
      <td>68502</td>
      <td>70621</td>
      <td>72552</td>
      <td>74338</td>
      <td>74757</td>
      <td>75299</td>
      <td>75885</td>
      <td>77582</td>
      <td>77799</td>
      <td>78263</td>
      <td>78932</td>
      <td>79648</td>
      <td>80612</td>
      <td>81527</td>
      <td>82786</td>
      <td>83964</td>
      <td>84786</td>
      <td>85782</td>
      <td>86380</td>
      <td>87029</td>
      <td>87748</td>
      <td>88317</td>
      <td>88684</td>
      <td>88894</td>
      <td>89029</td>
      <td>89364</td>
      <td>89490</td>
      <td>89676</td>
      <td>89890</td>
      <td>90064</td>
      <td>90162</td>
      <td>90338</td>
      <td>11,785,404.000000</td>
      <td>1,591,059,706.000000</td>
      <td>0.022921</td>
    </tr>
    <tr>
      <th>Australia and New Zealand</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>4</td>
      <td>5</td>
      <td>5</td>
      <td>6</td>
      <td>9</td>
      <td>9</td>
      <td>12</td>
      <td>12</td>
      <td>12</td>
      <td>13</td>
      <td>13</td>
      <td>14</td>
      <td>15</td>
      <td>15</td>
      <td>15</td>
      <td>15</td>
      <td>15</td>
      <td>15</td>
      <td>15</td>
      <td>15</td>
      <td>15</td>
      <td>15</td>
      <td>15</td>
      <td>15</td>
      <td>15</td>
      <td>15</td>
      <td>19</td>
      <td>22</td>
      <td>22</td>
      <td>22</td>
      <td>22</td>
      <td>22</td>
      <td>23</td>
      <td>24</td>
      <td>26</td>
      <td>28</td>
      <td>31</td>
      <td>40</td>
      <td>55</td>
      <td>58</td>
      <td>64</td>
      <td>68</td>
      <td>81</td>
      <td>96</td>
      <td>112</td>
      <td>133</td>
      <td>133</td>
      <td>205</td>
      <td>256</td>
      <td>305</td>
      <td>385</td>
      <td>464</td>
      <td>7,962,491.000000</td>
      <td>29,156,379.000000</td>
      <td>0.002104</td>
    </tr>
  </tbody>
</table>
</div>




```python

```

