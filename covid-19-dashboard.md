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
# Load data from https://github.com/pcm-dpc/COVID-19 
# COVID-19 Italia - Monitoraggio situazione
df_italy_covid_19_data = pd.read_json("https://raw.githubusercontent.com/pcm-dpc/COVID-19/master/dati-json/dpc-covid19-ita-andamento-nazionale.json")
```


```python
# set data format
df_italy_covid_19_data['data'] = pd.to_datetime(df_italy_covid_19_data['data']).dt.date
df_italy_covid_19_data.head()
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
      <th>data</th>
      <th>deceduti</th>
      <th>dimessi_guariti</th>
      <th>isolamento_domiciliare</th>
      <th>nuovi_attualmente_positivi</th>
      <th>ricoverati_con_sintomi</th>
      <th>stato</th>
      <th>tamponi</th>
      <th>terapia_intensiva</th>
      <th>totale_attualmente_positivi</th>
      <th>totale_casi</th>
      <th>totale_ospedalizzati</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2020-02-24</td>
      <td>7</td>
      <td>1</td>
      <td>94</td>
      <td>221</td>
      <td>101</td>
      <td>ITA</td>
      <td>4324</td>
      <td>26</td>
      <td>221</td>
      <td>229</td>
      <td>127</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2020-02-25</td>
      <td>10</td>
      <td>1</td>
      <td>162</td>
      <td>90</td>
      <td>114</td>
      <td>ITA</td>
      <td>8623</td>
      <td>35</td>
      <td>311</td>
      <td>322</td>
      <td>150</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2020-02-26</td>
      <td>12</td>
      <td>3</td>
      <td>221</td>
      <td>74</td>
      <td>128</td>
      <td>ITA</td>
      <td>9587</td>
      <td>36</td>
      <td>385</td>
      <td>400</td>
      <td>164</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2020-02-27</td>
      <td>17</td>
      <td>45</td>
      <td>284</td>
      <td>203</td>
      <td>248</td>
      <td>ITA</td>
      <td>12014</td>
      <td>56</td>
      <td>588</td>
      <td>650</td>
      <td>304</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2020-02-28</td>
      <td>21</td>
      <td>46</td>
      <td>412</td>
      <td>233</td>
      <td>345</td>
      <td>ITA</td>
      <td>15695</td>
      <td>64</td>
      <td>821</td>
      <td>888</td>
      <td>409</td>
    </tr>
  </tbody>
</table>
</div>




```python
txt = "italia_attualmente_positivi_e_nuovi_2\n\r"
txt = txt+"  type: area\n\r"
txt = txt+"  datetime: true\n\r"
txt = txt+"  sparkline: true\n\r"
txt = txt+"  categories: [1991,1992,1993,1994,1995,1996,1997,1998,1999]\n\r"
txt = txt+"  series:\n\r"
txt = txt+"    - name: sales\n\r"
txt = txt+"      data: [30,40,35,50,49,60,70,91,125]\n\r"

print(txt)
```

    italia_attualmente_positivi_e_nuovi_2
      type: area
      datetime: true
      sparkline: true
      categories: [1991,1992,1993,1994,1995,1996,1997,1998,1999]
      series:
        - name: sales
          data: [30,40,35,50,49,60,70,91,125]
    



```python
g_df = df_italy_covid_19_data['data']
df_list = df_italy_covid_19_data['data'].values.tolist()


x = df_italy_covid_19_data.to_string(header=False,
                  index=False).split('\n')
vals = [','.join(ele.split()) for ele in x]
print(df_list)
```

    [datetime.date(2020, 2, 24), datetime.date(2020, 2, 25), datetime.date(2020, 2, 26), datetime.date(2020, 2, 27), datetime.date(2020, 2, 28), datetime.date(2020, 2, 29), datetime.date(2020, 3, 1), datetime.date(2020, 3, 2), datetime.date(2020, 3, 3), datetime.date(2020, 3, 4), datetime.date(2020, 3, 5), datetime.date(2020, 3, 6), datetime.date(2020, 3, 7), datetime.date(2020, 3, 8), datetime.date(2020, 3, 9), datetime.date(2020, 3, 10), datetime.date(2020, 3, 11), datetime.date(2020, 3, 12), datetime.date(2020, 3, 13), datetime.date(2020, 3, 14), datetime.date(2020, 3, 15), datetime.date(2020, 3, 16), datetime.date(2020, 3, 17)]



```python

```
