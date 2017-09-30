

```python
import csv
import matplotlib.pyplot as plt
import requests as req
import pandas as pd
import numpy as np
```


```python
from citipy import citipy


seismic_pd = pd.read_csv("./Resources/seismic_data2011_2015.csv")


for index, row in seismic_pd.iterrows():
        lat=row["lng"]
        lng=row["lat"]
        
        city = citipy.nearest_city(lat,lng)             
        city_name=city.city_name
        country_cd=city.country_code        
        #print(row["place"]+"  "+str(lat)+"  "+str(lng)+"  "+city_name)        
        seismic_pd.loc[index, 'Nrst City']=city_name
        seismic_pd.loc[index, 'Nrst Country']=country_cd





```


```python
# Magnitude	Earthquake Effects	Estimated Number
# Each Year
# 2.5 or less	Usually not felt, but can be recorded by seismograph.	900,000
# 2.5 to 5.4	Often felt, but only causes minor damage.	30,000
# 5.5 to 6.0	Slight damage to buildings and other structures.	500
# 6.1 to 6.9	May cause a lot of damage in very populated areas.	100
# 7.0 to 7.9	Major earthquake. Serious damage.	20
# 8.0 or greater	Great earthquake. Can totally destroy communities near the epicenter.	One every 5 to 10 years

#Earthquake Magnitude Classes
# Class     Magnitude
# Great     8 or more
# Major     7 - 7.9
# Strong    6 - 6.9
# Moderate  5 - 5.9
# Light     4 - 4.9
# Minor     3 -3.9

seismic_pd.head()
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
      <th>Unnamed: 0</th>
      <th>alert</th>
      <th>cd_by_month</th>
      <th>cd_by_year</th>
      <th>cdi</th>
      <th>code</th>
      <th>converted_date</th>
      <th>coordinates_3</th>
      <th>detail</th>
      <th>dmin</th>
      <th>...</th>
      <th>time</th>
      <th>title</th>
      <th>tsunami</th>
      <th>type</th>
      <th>types</th>
      <th>tz</th>
      <th>updated</th>
      <th>url</th>
      <th>Nrst City</th>
      <th>Nrst Country</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>97242</td>
      <td>NaN</td>
      <td>12-2014</td>
      <td>2014</td>
      <td>NaN</td>
      <td>c000tg05</td>
      <td>12-31-2014</td>
      <td>0.000</td>
      <td>https://earthquake.usgs.gov/fdsnws/event/1/que...</td>
      <td>1.016</td>
      <td>...</td>
      <td>1420064599410</td>
      <td>M 3.3 Mining Explosion - Wyoming</td>
      <td>0</td>
      <td>mining explosion</td>
      <td>,origin,phase-data,</td>
      <td>NaN</td>
      <td>1426559911040</td>
      <td>https://earthquake.usgs.gov/earthquakes/eventp...</td>
      <td>gillette</td>
      <td>us</td>
    </tr>
    <tr>
      <th>1</th>
      <td>97243</td>
      <td>NaN</td>
      <td>12-2014</td>
      <td>2014</td>
      <td>2.0</td>
      <td>c000tb1r</td>
      <td>12-31-2014</td>
      <td>2.510</td>
      <td>https://earthquake.usgs.gov/fdsnws/event/1/que...</td>
      <td>1.094</td>
      <td>...</td>
      <td>1420063581010</td>
      <td>M 2.8 - 24km N of Snyder, Texas</td>
      <td>0</td>
      <td>earthquake</td>
      <td>,cap,dyfi,general-link,geoserve,nearby-cities,...</td>
      <td>-360.0</td>
      <td>1426559911040</td>
      <td>https://earthquake.usgs.gov/earthquakes/eventp...</td>
      <td>snyder</td>
      <td>us</td>
    </tr>
    <tr>
      <th>2</th>
      <td>97244</td>
      <td>NaN</td>
      <td>12-2014</td>
      <td>2014</td>
      <td>2.0</td>
      <td>c000tazd</td>
      <td>12-31-2014</td>
      <td>5.000</td>
      <td>https://earthquake.usgs.gov/fdsnws/event/1/que...</td>
      <td>1.058</td>
      <td>...</td>
      <td>1420050699990</td>
      <td>M 3.2 - 26km NNE of Snyder, Texas</td>
      <td>0</td>
      <td>earthquake</td>
      <td>,cap,dyfi,general-link,geoserve,impact-text,ne...</td>
      <td>-360.0</td>
      <td>1426559909040</td>
      <td>https://earthquake.usgs.gov/earthquakes/eventp...</td>
      <td>snyder</td>
      <td>us</td>
    </tr>
    <tr>
      <th>3</th>
      <td>97245</td>
      <td>NaN</td>
      <td>12-2014</td>
      <td>2014</td>
      <td>1.0</td>
      <td>c000tdcv</td>
      <td>12-31-2014</td>
      <td>6.158</td>
      <td>https://earthquake.usgs.gov/fdsnws/event/1/que...</td>
      <td>NaN</td>
      <td>...</td>
      <td>1420035103400</td>
      <td>M 2.6 - 5km NNE of Medford, Oklahoma</td>
      <td>0</td>
      <td>earthquake</td>
      <td>,dyfi,general-link,geoserve,nearby-cities,orig...</td>
      <td>-360.0</td>
      <td>1426559906040</td>
      <td>https://earthquake.usgs.gov/earthquakes/eventp...</td>
      <td>enid</td>
      <td>us</td>
    </tr>
    <tr>
      <th>4</th>
      <td>97246</td>
      <td>NaN</td>
      <td>12-2014</td>
      <td>2014</td>
      <td>1.0</td>
      <td>c000tdc2</td>
      <td>12-31-2014</td>
      <td>5.096</td>
      <td>https://earthquake.usgs.gov/fdsnws/event/1/que...</td>
      <td>NaN</td>
      <td>...</td>
      <td>1420029323400</td>
      <td>M 2.6 - 18km SE of Medford, Oklahoma</td>
      <td>0</td>
      <td>earthquake</td>
      <td>,dyfi,general-link,geoserve,nearby-cities,orig...</td>
      <td>-360.0</td>
      <td>1426559905040</td>
      <td>https://earthquake.usgs.gov/earthquakes/eventp...</td>
      <td>enid</td>
      <td>us</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 36 columns</p>
</div>




```python
seismic_pd.to_csv("./Resources/seismic_pd.csv")
```


```python
seismic_max_pd=seismic_pd.groupby(["cd_by_year","Nrst City"],as_index=False).max()

seismic2013_max_pd=seismic_max_pd.loc[(seismic_max_pd["cd_by_year"]==2013) & (seismic_max_pd["mag"]>=1) & (seismic_max_pd["Nrst Country"]>="us")]

seismic2013_max_pd
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
      <th>cd_by_year</th>
      <th>Nrst City</th>
      <th>Nrst Country</th>
      <th>Unnamed: 0</th>
      <th>cd_by_month</th>
      <th>cdi</th>
      <th>code</th>
      <th>converted_date</th>
      <th>coordinates_3</th>
      <th>detail</th>
      <th>...</th>
      <th>sources</th>
      <th>status</th>
      <th>time</th>
      <th>title</th>
      <th>tsunami</th>
      <th>type</th>
      <th>types</th>
      <th>tz</th>
      <th>updated</th>
      <th>url</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>610</th>
      <td>2013</td>
      <td>ada</td>
      <td>us</td>
      <td>102596</td>
      <td>06-2013</td>
      <td>NaN</td>
      <td>p000k155</td>
      <td>06-07-2013</td>
      <td>16.700</td>
      <td>https://earthquake.usgs.gov/fdsnws/event/1/que...</td>
      <td>...</td>
      <td>,us,</td>
      <td>reviewed</td>
      <td>1370587948500</td>
      <td>M 3.0 - Oklahoma</td>
      <td>0</td>
      <td>earthquake</td>
      <td>,impact-text,origin,phase-data,</td>
      <td>NaN</td>
      <td>1415325062162</td>
      <td>https://earthquake.usgs.gov/earthquakes/eventp...</td>
    </tr>
    <tr>
      <th>612</th>
      <td>2013</td>
      <td>alamo</td>
      <td>us</td>
      <td>102243</td>
      <td>08-2013</td>
      <td>NaN</td>
      <td>72054261</td>
      <td>08-22-2013</td>
      <td>8.264</td>
      <td>https://earthquake.usgs.gov/fdsnws/event/1/que...</td>
      <td>...</td>
      <td>,nc,</td>
      <td>reviewed</td>
      <td>1377166700660</td>
      <td>M 2.8 - 1km WSW of Alamo, California</td>
      <td>0</td>
      <td>earthquake</td>
      <td>,cap,geoserve,nearby-cities,origin,phase-data,...</td>
      <td>-420.0</td>
      <td>1485941850918</td>
      <td>https://earthquake.usgs.gov/earthquakes/eventp...</td>
    </tr>
    <tr>
      <th>613</th>
      <td>2013</td>
      <td>alpine</td>
      <td>us</td>
      <td>103348</td>
      <td>12-2013</td>
      <td>3.8</td>
      <td>15352745</td>
      <td>12-28-2013</td>
      <td>10.225</td>
      <td>https://earthquake.usgs.gov/fdsnws/event/1/que...</td>
      <td>...</td>
      <td>,ci,us,</td>
      <td>reviewed</td>
      <td>1388254478540</td>
      <td>M 3.4 - 22km SW of Ocotillo Wells, CA</td>
      <td>0</td>
      <td>earthquake</td>
      <td>,dyfi,eq-location-map,general-link,geoserve,hi...</td>
      <td>-420.0</td>
      <td>1457755576449</td>
      <td>https://earthquake.usgs.gov/earthquakes/eventp...</td>
    </tr>
    <tr>
      <th>614</th>
      <td>2013</td>
      <td>altamont</td>
      <td>us</td>
      <td>102878</td>
      <td>05-2013</td>
      <td>NaN</td>
      <td>60524282</td>
      <td>05-03-2013</td>
      <td>0.139</td>
      <td>https://earthquake.usgs.gov/fdsnws/event/1/que...</td>
      <td>...</td>
      <td>,uw,us,</td>
      <td>reviewed</td>
      <td>1367533668170</td>
      <td>M 2.7 - 12km SE of Lakeview, Oregon</td>
      <td>0</td>
      <td>earthquake</td>
      <td>,cap,general-link,geoserve,impact-text,nearby-...</td>
      <td>-420.0</td>
      <td>1469214613090</td>
      <td>https://earthquake.usgs.gov/earthquakes/eventp...</td>
    </tr>
    <tr>
      <th>615</th>
      <td>2013</td>
      <td>american canyon</td>
      <td>us</td>
      <td>102353</td>
      <td>08-2013</td>
      <td>3.8</td>
      <td>72041146</td>
      <td>08-01-2013</td>
      <td>10.825</td>
      <td>https://earthquake.usgs.gov/fdsnws/event/1/que...</td>
      <td>...</td>
      <td>,nc,us,</td>
      <td>reviewed</td>
      <td>1375336368710</td>
      <td>M 2.7 - 2km S of Green Valley, California</td>
      <td>0</td>
      <td>earthquake</td>
      <td>,cap,dyfi,focal-mechanism,geoserve,nearby-citi...</td>
      <td>-420.0</td>
      <td>1485931180100</td>
      <td>https://earthquake.usgs.gov/earthquakes/eventp...</td>
    </tr>
    <tr>
      <th>617</th>
      <td>2013</td>
      <td>anaconda</td>
      <td>us</td>
      <td>102138</td>
      <td>09-2013</td>
      <td>NaN</td>
      <td>13083265</td>
      <td>09-10-2013</td>
      <td>11.000</td>
      <td>https://earthquake.usgs.gov/fdsnws/event/1/que...</td>
      <td>...</td>
      <td>,mb,us,</td>
      <td>AUTOMATIC</td>
      <td>1378822962400</td>
      <td>M 2.5 - 10km NNW of Philipsburg, Montana</td>
      <td>0</td>
      <td>earthquake</td>
      <td>,general-link,general-link,geoserve,nearby-cit...</td>
      <td>-360.0</td>
      <td>1384882636000</td>
      <td>https://earthquake.usgs.gov/earthquakes/eventp...</td>
    </tr>
    <tr>
      <th>618</th>
      <td>2013</td>
      <td>anacortes</td>
      <td>us</td>
      <td>103318</td>
      <td>01-2013</td>
      <td>3.8</td>
      <td>60497387</td>
      <td>01-25-2013</td>
      <td>23.415</td>
      <td>https://earthquake.usgs.gov/fdsnws/event/1/que...</td>
      <td>...</td>
      <td>,uw,us,</td>
      <td>reviewed</td>
      <td>1359105116260</td>
      <td>M 3.4 - San Juan Islands region, Washington</td>
      <td>0</td>
      <td>earthquake</td>
      <td>,cap,dyfi,impact-text,origin,phase-data,shakemap,</td>
      <td>NaN</td>
      <td>1491099077046</td>
      <td>https://earthquake.usgs.gov/earthquakes/eventp...</td>
    </tr>
    <tr>
      <th>619</th>
      <td>2013</td>
      <td>anderson</td>
      <td>us</td>
      <td>103313</td>
      <td>12-2013</td>
      <td>2.4</td>
      <td>72119100</td>
      <td>12-11-2013</td>
      <td>29.862</td>
      <td>https://earthquake.usgs.gov/fdsnws/event/1/que...</td>
      <td>...</td>
      <td>,nc,us,</td>
      <td>reviewed</td>
      <td>1386767960610</td>
      <td>M 2.8 - 1km SSW of Palo Cedro, California</td>
      <td>0</td>
      <td>earthquake</td>
      <td>,focal-mechanism,nearby-cities,origin,phase-data,</td>
      <td>-420.0</td>
      <td>1486004507830</td>
      <td>https://earthquake.usgs.gov/earthquakes/eventp...</td>
    </tr>
    <tr>
      <th>620</th>
      <td>2013</td>
      <td>arcata</td>
      <td>us</td>
      <td>103409</td>
      <td>10-2013</td>
      <td>3.4</td>
      <td>72099131</td>
      <td>10-31-2013</td>
      <td>40.061</td>
      <td>https://earthquake.usgs.gov/fdsnws/event/1/que...</td>
      <td>...</td>
      <td>,nc,us,</td>
      <td>reviewed</td>
      <td>1383243857870</td>
      <td>M 3.8 - 49km NNE of Willow Creek, California</td>
      <td>0</td>
      <td>earthquake</td>
      <td>,focal-mechanism,nearby-cities,origin,phase-data,</td>
      <td>-420.0</td>
      <td>1489791416794</td>
      <td>https://earthquake.usgs.gov/earthquakes/eventp...</td>
    </tr>
    <tr>
      <th>621</th>
      <td>2013</td>
      <td>ardmore</td>
      <td>us</td>
      <td>103428</td>
      <td>09-2013</td>
      <td>5.5</td>
      <td>p000jxq4</td>
      <td>09-23-2013</td>
      <td>15.200</td>
      <td>https://earthquake.usgs.gov/fdsnws/event/1/que...</td>
      <td>...</td>
      <td>,us,us,</td>
      <td>reviewed</td>
      <td>1379944585540</td>
      <td>M 3.5 - 13km ENE of Dickson, Oklahoma</td>
      <td>0</td>
      <td>earthquake</td>
      <td>,general-link,general-link,geoserve,nearby-cit...</td>
      <td>-300.0</td>
      <td>1500061462100</td>
      <td>https://earthquake.usgs.gov/earthquakes/eventp...</td>
    </tr>
    <tr>
      <th>622</th>
      <td>2013</td>
      <td>arvin</td>
      <td>us</td>
      <td>103325</td>
      <td>08-2013</td>
      <td>3.1</td>
      <td>15277225</td>
      <td>08-03-2013</td>
      <td>14.474</td>
      <td>https://earthquake.usgs.gov/fdsnws/event/1/que...</td>
      <td>...</td>
      <td>,ci,us,</td>
      <td>reviewed</td>
      <td>1375522602170</td>
      <td>M 2.8 - 10km WSW of Grapevine, CA</td>
      <td>0</td>
      <td>earthquake</td>
      <td>,cap,impact-text,nearby-cities,origin,phase-data,</td>
      <td>-420.0</td>
      <td>1457733784885</td>
      <td>https://earthquake.usgs.gov/earthquakes/eventp...</td>
    </tr>
    <tr>
      <th>623</th>
      <td>2013</td>
      <td>ashland</td>
      <td>us</td>
      <td>103090</td>
      <td>03-2013</td>
      <td>NaN</td>
      <td>60512661</td>
      <td>03-26-2013</td>
      <td>14.394</td>
      <td>https://earthquake.usgs.gov/fdsnws/event/1/que...</td>
      <td>...</td>
      <td>,uw,us,</td>
      <td>reviewed</td>
      <td>1364244929860</td>
      <td>M 3.1 - 33km E of Shady Cove, Oregon</td>
      <td>0</td>
      <td>earthquake</td>
      <td>,cap,general-link,geoserve,impact-text,nearby-...</td>
      <td>-420.0</td>
      <td>1469214497110</td>
      <td>https://earthquake.usgs.gov/earthquakes/eventp...</td>
    </tr>
    <tr>
      <th>624</th>
      <td>2013</td>
      <td>atascadero</td>
      <td>us</td>
      <td>103085</td>
      <td>07-2013</td>
      <td>2.4</td>
      <td>72035200</td>
      <td>07-28-2013</td>
      <td>6.013</td>
      <td>https://earthquake.usgs.gov/fdsnws/event/1/que...</td>
      <td>...</td>
      <td>,nc,us,ci,</td>
      <td>reviewed</td>
      <td>1374970710360</td>
      <td>M 2.8 - 12km WNW of Templeton, California</td>
      <td>0</td>
      <td>earthquake</td>
      <td>,cap,dyfi,focal-mechanism,geoserve,impact-text...</td>
      <td>-420.0</td>
      <td>1485928839380</td>
      <td>https://earthquake.usgs.gov/earthquakes/eventp...</td>
    </tr>
    <tr>
      <th>625</th>
      <td>2013</td>
      <td>athens</td>
      <td>us</td>
      <td>102475</td>
      <td>11-2013</td>
      <td>4.6</td>
      <td>b000l2y3</td>
      <td>11-20-2013</td>
      <td>23.210</td>
      <td>https://earthquake.usgs.gov/fdsnws/event/1/que...</td>
      <td>...</td>
      <td>,us,ld,</td>
      <td>reviewed</td>
      <td>1384970379820</td>
      <td>M 3.5 - 2km ESE of Nelsonville, Ohio</td>
      <td>0</td>
      <td>earthquake</td>
      <td>,cap,dyfi,general-link,geoserve,nearby-cities,...</td>
      <td>-240.0</td>
      <td>1464297938510</td>
      <td>https://earthquake.usgs.gov/earthquakes/eventp...</td>
    </tr>
    <tr>
      <th>626</th>
      <td>2013</td>
      <td>avenal</td>
      <td>us</td>
      <td>103359</td>
      <td>12-2013</td>
      <td>4.1</td>
      <td>72131076</td>
      <td>12-27-2013</td>
      <td>24.466</td>
      <td>https://earthquake.usgs.gov/fdsnws/event/1/que...</td>
      <td>...</td>
      <td>,us,nc,ci,</td>
      <td>reviewed</td>
      <td>1388103924620</td>
      <td>M 3.3 - 9km NW of Avenal, California</td>
      <td>0</td>
      <td>earthquake</td>
      <td>,focal-mechanism,geoserve,nearby-cities,origin...</td>
      <td>-420.0</td>
      <td>1486012376170</td>
      <td>https://earthquake.usgs.gov/earthquakes/eventp...</td>
    </tr>
    <tr>
      <th>627</th>
      <td>2013</td>
      <td>azle</td>
      <td>us</td>
      <td>101784</td>
      <td>12-2013</td>
      <td>4.9</td>
      <td>c000lqat</td>
      <td>12-23-2013</td>
      <td>6.390</td>
      <td>https://earthquake.usgs.gov/fdsnws/event/1/que...</td>
      <td>...</td>
      <td>,us,</td>
      <td>reviewed</td>
      <td>1387804294040</td>
      <td>M 3.6 - 3km WNW of Azle, Texas</td>
      <td>0</td>
      <td>earthquake</td>
      <td>,cap,general-link,general-link,geoserve,nearby...</td>
      <td>-360.0</td>
      <td>1498086770357</td>
      <td>https://earthquake.usgs.gov/earthquakes/eventp...</td>
    </tr>
    <tr>
      <th>628</th>
      <td>2013</td>
      <td>baker city</td>
      <td>us</td>
      <td>101786</td>
      <td>12-2013</td>
      <td>NaN</td>
      <td>60647017</td>
      <td>12-10-2013</td>
      <td>4.146</td>
      <td>https://earthquake.usgs.gov/fdsnws/event/1/que...</td>
      <td>...</td>
      <td>,uw,us,</td>
      <td>reviewed</td>
      <td>1386713342080</td>
      <td>M 2.7 - 30km ESE of Baker City, Oregon</td>
      <td>0</td>
      <td>explosion</td>
      <td>,cap,general-link,geoserve,nearby-cities,origi...</td>
      <td>-480.0</td>
      <td>1469215253310</td>
      <td>https://earthquake.usgs.gov/earthquakes/eventp...</td>
    </tr>
    <tr>
      <th>629</th>
      <td>2013</td>
      <td>banning</td>
      <td>us</td>
      <td>103215</td>
      <td>07-2013</td>
      <td>3.1</td>
      <td>15343017</td>
      <td>07-22-2013</td>
      <td>17.275</td>
      <td>https://earthquake.usgs.gov/fdsnws/event/1/que...</td>
      <td>...</td>
      <td>,ci,us,</td>
      <td>reviewed</td>
      <td>1374458388050</td>
      <td>M 3.2 - 6km ENE of Cabazon, CA</td>
      <td>0</td>
      <td>earthquake</td>
      <td>,dyfi,general-link,geoserve,impact-text,nearby...</td>
      <td>-420.0</td>
      <td>1457760708466</td>
      <td>https://earthquake.usgs.gov/earthquakes/eventp...</td>
    </tr>
    <tr>
      <th>630</th>
      <td>2013</td>
      <td>barstow</td>
      <td>us</td>
      <td>103203</td>
      <td>12-2013</td>
      <td>3.1</td>
      <td>15443001</td>
      <td>12-11-2013</td>
      <td>6.127</td>
      <td>https://earthquake.usgs.gov/fdsnws/event/1/que...</td>
      <td>...</td>
      <td>,ci,us,</td>
      <td>reviewed</td>
      <td>1386772735450</td>
      <td>M 3.3 - 18km SW of Fort Irwin, CA</td>
      <td>0</td>
      <td>earthquake</td>
      <td>,general-link,geoserve,impact-text,nearby-citi...</td>
      <td>-420.0</td>
      <td>1495739025630</td>
      <td>https://earthquake.usgs.gov/earthquakes/eventp...</td>
    </tr>
    <tr>
      <th>631</th>
      <td>2013</td>
      <td>batesville</td>
      <td>us</td>
      <td>102782</td>
      <td>05-2013</td>
      <td>3.3</td>
      <td>609899</td>
      <td>05-21-2013</td>
      <td>15.470</td>
      <td>https://earthquake.usgs.gov/fdsnws/event/1/que...</td>
      <td>...</td>
      <td>,nm,nm,us,</td>
      <td>reviewed</td>
      <td>1369128486770</td>
      <td>M 2.9 - 22km E of Cave City, Arkansas</td>
      <td>0</td>
      <td>earthquake</td>
      <td>,associate,cap,dyfi,general-link,geoserve,impa...</td>
      <td>-360.0</td>
      <td>1460054350320</td>
      <td>https://earthquake.usgs.gov/earthquakes/eventp...</td>
    </tr>
    <tr>
      <th>632</th>
      <td>2013</td>
      <td>bay point</td>
      <td>us</td>
      <td>102500</td>
      <td>06-2013</td>
      <td>3.1</td>
      <td>72015700</td>
      <td>06-26-2013</td>
      <td>21.485</td>
      <td>https://earthquake.usgs.gov/fdsnws/event/1/que...</td>
      <td>...</td>
      <td>,nc,us,</td>
      <td>reviewed</td>
      <td>1372221800290</td>
      <td>M 2.6 - 13km SE of Suisun, California</td>
      <td>0</td>
      <td>earthquake</td>
      <td>,cap,dyfi,focal-mechanism,geoserve,impact-text...</td>
      <td>-420.0</td>
      <td>1485913015780</td>
      <td>https://earthquake.usgs.gov/earthquakes/eventp...</td>
    </tr>
    <tr>
      <th>633</th>
      <td>2013</td>
      <td>beaumont</td>
      <td>us</td>
      <td>103118</td>
      <td>03-2013</td>
      <td>3.1</td>
      <td>15305937</td>
      <td>03-15-2013</td>
      <td>10.536</td>
      <td>https://earthquake.usgs.gov/fdsnws/event/1/que...</td>
      <td>...</td>
      <td>,ci,us,</td>
      <td>reviewed</td>
      <td>1363355942610</td>
      <td>M 2.9 - 8km SSW of Calimesa, CA</td>
      <td>0</td>
      <td>earthquake</td>
      <td>,cap,dyfi,general-link,geoserve,impact-text,ne...</td>
      <td>-420.0</td>
      <td>1457667518887</td>
      <td>https://earthquake.usgs.gov/earthquakes/eventp...</td>
    </tr>
    <tr>
      <th>634</th>
      <td>2013</td>
      <td>belgrade</td>
      <td>us</td>
      <td>102823</td>
      <td>06-2013</td>
      <td>NaN</td>
      <td>13758562</td>
      <td>06-26-2013</td>
      <td>12.600</td>
      <td>https://earthquake.usgs.gov/fdsnws/event/1/que...</td>
      <td>...</td>
      <td>,mb,us,mb,</td>
      <td>AUTOMATIC</td>
      <td>1372281040700</td>
      <td>M 2.7 - 34km SSW of Three Forks, Montana</td>
      <td>0</td>
      <td>earthquake</td>
      <td>,general-link,general-link,geoserve,impact-tex...</td>
      <td>-360.0</td>
      <td>1415325067623</td>
      <td>https://earthquake.usgs.gov/earthquakes/eventp...</td>
    </tr>
    <tr>
      <th>635</th>
      <td>2013</td>
      <td>belle fourche</td>
      <td>us</td>
      <td>102365</td>
      <td>07-2013</td>
      <td>NaN</td>
      <td>13418334</td>
      <td>07-29-2013</td>
      <td>7.900</td>
      <td>https://earthquake.usgs.gov/fdsnws/event/1/que...</td>
      <td>...</td>
      <td>,mb,</td>
      <td>AUTOMATIC</td>
      <td>1375089241600</td>
      <td>M 2.6 - 18km SE of Ekalaka, Montana</td>
      <td>0</td>
      <td>earthquake</td>
      <td>,general-link,general-link,geoserve,nearby-cit...</td>
      <td>-360.0</td>
      <td>1375201137647</td>
      <td>https://earthquake.usgs.gov/earthquakes/eventp...</td>
    </tr>
    <tr>
      <th>636</th>
      <td>2013</td>
      <td>bend</td>
      <td>us</td>
      <td>102892</td>
      <td>12-2013</td>
      <td>NaN</td>
      <td>60660966</td>
      <td>12-25-2013</td>
      <td>7.014</td>
      <td>https://earthquake.usgs.gov/fdsnws/event/1/que...</td>
      <td>...</td>
      <td>,uw,us,</td>
      <td>reviewed</td>
      <td>1387937230400</td>
      <td>M 2.7 - 45km ENE of Silver Lake, Oregon</td>
      <td>0</td>
      <td>explosion</td>
      <td>,cap,general-link,geoserve,nearby-cities,origi...</td>
      <td>-420.0</td>
      <td>1469215291220</td>
      <td>https://earthquake.usgs.gov/earthquakes/eventp...</td>
    </tr>
    <tr>
      <th>637</th>
      <td>2013</td>
      <td>berkeley</td>
      <td>us</td>
      <td>101979</td>
      <td>10-2013</td>
      <td>3.8</td>
      <td>72087836</td>
      <td>10-15-2013</td>
      <td>6.915</td>
      <td>https://earthquake.usgs.gov/fdsnws/event/1/que...</td>
      <td>...</td>
      <td>,nc,us,ci,</td>
      <td>reviewed</td>
      <td>1381828512170</td>
      <td>M 3.2 - 3km ENE of Berkeley, California</td>
      <td>0</td>
      <td>earthquake</td>
      <td>,dyfi,focal-mechanism,geoserve,nearby-cities,o...</td>
      <td>-420.0</td>
      <td>1485971134520</td>
      <td>https://earthquake.usgs.gov/earthquakes/eventp...</td>
    </tr>
    <tr>
      <th>638</th>
      <td>2013</td>
      <td>billings</td>
      <td>us</td>
      <td>102776</td>
      <td>05-2013</td>
      <td>NaN</td>
      <td>p000k0vm</td>
      <td>05-21-2013</td>
      <td>7.300</td>
      <td>https://earthquake.usgs.gov/fdsnws/event/1/que...</td>
      <td>...</td>
      <td>,mb,us,</td>
      <td>reviewed</td>
      <td>1369148074000</td>
      <td>M 2.8 - 57km NNW of Hysham, Montana</td>
      <td>0</td>
      <td>earthquake</td>
      <td>,general-link,general-link,geoserve,impact-tex...</td>
      <td>-360.0</td>
      <td>1415325052846</td>
      <td>https://earthquake.usgs.gov/earthquakes/eventp...</td>
    </tr>
    <tr>
      <th>639</th>
      <td>2013</td>
      <td>blytheville</td>
      <td>us</td>
      <td>103199</td>
      <td>08-2013</td>
      <td>3.4</td>
      <td>610011</td>
      <td>08-10-2013</td>
      <td>16.280</td>
      <td>https://earthquake.usgs.gov/fdsnws/event/1/que...</td>
      <td>...</td>
      <td>,nm,us,nm,</td>
      <td>reviewed</td>
      <td>1376095264910</td>
      <td>M 2.8 - 3km SSE of Steele, Missouri</td>
      <td>0</td>
      <td>earthquake</td>
      <td>,associate,dyfi,eq-location-map,general-link,g...</td>
      <td>-300.0</td>
      <td>1460055057980</td>
      <td>https://earthquake.usgs.gov/earthquakes/eventp...</td>
    </tr>
    <tr>
      <th>640</th>
      <td>2013</td>
      <td>boone</td>
      <td>us</td>
      <td>102221</td>
      <td>08-2013</td>
      <td>4.8</td>
      <td>610210</td>
      <td>08-25-2013</td>
      <td>9.390</td>
      <td>https://earthquake.usgs.gov/fdsnws/event/1/que...</td>
      <td>...</td>
      <td>,se,se,us,</td>
      <td>reviewed</td>
      <td>1377460240320</td>
      <td>M 2.9 - 3km NNE of Blowing Rock, North Carolina</td>
      <td>0</td>
      <td>earthquake</td>
      <td>,associate,cap,dyfi,general-link,geoserve,near...</td>
      <td>-240.0</td>
      <td>1460056324830</td>
      <td>https://earthquake.usgs.gov/earthquakes/eventp...</td>
    </tr>
    <tr>
      <th>641</th>
      <td>2013</td>
      <td>boulder city</td>
      <td>us</td>
      <td>101439</td>
      <td>12-2013</td>
      <td>2.8</td>
      <td>00432623</td>
      <td>12-30-2013</td>
      <td>6.000</td>
      <td>https://earthquake.usgs.gov/fdsnws/event/1/que...</td>
      <td>...</td>
      <td>,nn,ci,us,</td>
      <td>reviewed</td>
      <td>1388410714387</td>
      <td>M 2.9 - 7km SSE of Boulder City, Nevada</td>
      <td>0</td>
      <td>earthquake</td>
      <td>,cap,dyfi,general-link,geoserve,nearby-cities,...</td>
      <td>-480.0</td>
      <td>1497295322343</td>
      <td>https://earthquake.usgs.gov/earthquakes/eventp...</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>899</th>
      <td>2013</td>
      <td>steamboat springs</td>
      <td>us</td>
      <td>101852</td>
      <td>10-2013</td>
      <td>NaN</td>
      <td>c000kshb</td>
      <td>10-31-2013</td>
      <td>1.000</td>
      <td>https://earthquake.usgs.gov/fdsnws/event/1/que...</td>
      <td>...</td>
      <td>,us,</td>
      <td>reviewed</td>
      <td>1383232746200</td>
      <td>M 2.8 - 17km WSW of Steamboat Springs, Colorado</td>
      <td>0</td>
      <td>earthquake</td>
      <td>,general-link,general-link,geoserve,nearby-cit...</td>
      <td>-360.0</td>
      <td>1389348219000</td>
      <td>https://earthquake.usgs.gov/earthquakes/eventp...</td>
    </tr>
    <tr>
      <th>900</th>
      <td>2013</td>
      <td>stillwater</td>
      <td>us</td>
      <td>102779</td>
      <td>12-2013</td>
      <td>4.3</td>
      <td>p000k1bn</td>
      <td>12-31-2013</td>
      <td>6.100</td>
      <td>https://earthquake.usgs.gov/fdsnws/event/1/que...</td>
      <td>...</td>
      <td>,us,</td>
      <td>reviewed</td>
      <td>1388459634070</td>
      <td>M 3.6 - 11km NE of Stillwater, Oklahoma</td>
      <td>0</td>
      <td>earthquake</td>
      <td>,impact-text,origin,phase-data,</td>
      <td>-300.0</td>
      <td>1498515148677</td>
      <td>https://earthquake.usgs.gov/earthquakes/eventp...</td>
    </tr>
    <tr>
      <th>901</th>
      <td>2013</td>
      <td>suisun city</td>
      <td>us</td>
      <td>103315</td>
      <td>01-2013</td>
      <td>3.4</td>
      <td>71928651</td>
      <td>01-25-2013</td>
      <td>8.011</td>
      <td>https://earthquake.usgs.gov/fdsnws/event/1/que...</td>
      <td>...</td>
      <td>,nc,us,</td>
      <td>reviewed</td>
      <td>1359135538710</td>
      <td>M 2.9 - Northern California</td>
      <td>0</td>
      <td>earthquake</td>
      <td>,cap,dyfi,focal-mechanism,impact-text,nearby-c...</td>
      <td>NaN</td>
      <td>1485815505640</td>
      <td>https://earthquake.usgs.gov/earthquakes/eventp...</td>
    </tr>
    <tr>
      <th>903</th>
      <td>2013</td>
      <td>summerville</td>
      <td>us</td>
      <td>102079</td>
      <td>09-2013</td>
      <td>2.7</td>
      <td>610220</td>
      <td>09-20-2013</td>
      <td>11.440</td>
      <td>https://earthquake.usgs.gov/fdsnws/event/1/que...</td>
      <td>...</td>
      <td>,se,se,us,</td>
      <td>reviewed</td>
      <td>1379618051170</td>
      <td>M 2.5 - 8km WSW of Summerville, South Carolina</td>
      <td>0</td>
      <td>earthquake</td>
      <td>,associate,cap,dyfi,general-link,geoserve,near...</td>
      <td>-240.0</td>
      <td>1460056387900</td>
      <td>https://earthquake.usgs.gov/earthquakes/eventp...</td>
    </tr>
    <tr>
      <th>904</th>
      <td>2013</td>
      <td>sun valley</td>
      <td>us</td>
      <td>102452</td>
      <td>08-2013</td>
      <td>4.4</td>
      <td>00421303</td>
      <td>08-27-2013</td>
      <td>15.700</td>
      <td>https://earthquake.usgs.gov/fdsnws/event/1/que...</td>
      <td>...</td>
      <td>,nn,nc,us,ci,</td>
      <td>reviewed</td>
      <td>1377564703827</td>
      <td>M 4.2 - 3km NE of Spanish Springs, Nevada</td>
      <td>0</td>
      <td>earthquake</td>
      <td>,cap,focal-mechanism,general-link,geoserve,imp...</td>
      <td>-420.0</td>
      <td>1500523827143</td>
      <td>https://earthquake.usgs.gov/earthquakes/eventp...</td>
    </tr>
    <tr>
      <th>905</th>
      <td>2013</td>
      <td>sunrise manor</td>
      <td>us</td>
      <td>103378</td>
      <td>12-2013</td>
      <td>2.5</td>
      <td>00432746</td>
      <td>12-31-2013</td>
      <td>12.100</td>
      <td>https://earthquake.usgs.gov/fdsnws/event/1/que...</td>
      <td>...</td>
      <td>,us,nn,</td>
      <td>reviewed</td>
      <td>1388484603632</td>
      <td>M 3.8 - 39km N of Alamo, Nevada</td>
      <td>0</td>
      <td>earthquake</td>
      <td>,origin,phase-data,</td>
      <td>-420.0</td>
      <td>1497295326046</td>
      <td>https://earthquake.usgs.gov/earthquakes/eventp...</td>
    </tr>
    <tr>
      <th>906</th>
      <td>2013</td>
      <td>susanville</td>
      <td>us</td>
      <td>103279</td>
      <td>12-2013</td>
      <td>7.2</td>
      <td>72129046</td>
      <td>12-23-2013</td>
      <td>15.057</td>
      <td>https://earthquake.usgs.gov/fdsnws/event/1/que...</td>
      <td>...</td>
      <td>,us,us,nc,nn,</td>
      <td>reviewed</td>
      <td>1387803229760</td>
      <td>M 5.7 - 10km WNW of Greenville, California</td>
      <td>1</td>
      <td>earthquake</td>
      <td>,nearby-cities,origin,phase-data,scitech-link,</td>
      <td>-420.0</td>
      <td>1497295173528</td>
      <td>https://earthquake.usgs.gov/earthquakes/eventp...</td>
    </tr>
    <tr>
      <th>908</th>
      <td>2013</td>
      <td>sycamore</td>
      <td>us</td>
      <td>102580</td>
      <td>06-2013</td>
      <td>NaN</td>
      <td>2013rgc2</td>
      <td>06-10-2013</td>
      <td>5.000</td>
      <td>https://earthquake.usgs.gov/fdsnws/event/1/que...</td>
      <td>...</td>
      <td>,us,</td>
      <td>reviewed</td>
      <td>1370867379500</td>
      <td>M 2.6 - 11km WNW of Village of Campton Hills, ...</td>
      <td>0</td>
      <td>earthquake</td>
      <td>,general-link,general-link,geoserve,impact-tex...</td>
      <td>-300.0</td>
      <td>1415325063489</td>
      <td>https://earthquake.usgs.gov/earthquakes/eventp...</td>
    </tr>
    <tr>
      <th>909</th>
      <td>2013</td>
      <td>taft</td>
      <td>us</td>
      <td>103351</td>
      <td>07-2013</td>
      <td>3.6</td>
      <td>15343601</td>
      <td>07-05-2013</td>
      <td>12.358</td>
      <td>https://earthquake.usgs.gov/fdsnws/event/1/que...</td>
      <td>...</td>
      <td>,ci,us,</td>
      <td>reviewed</td>
      <td>1373010077450</td>
      <td>M 3.3 - 31km WSW of Pine Mountain Club, CA</td>
      <td>0</td>
      <td>earthquake</td>
      <td>,dyfi,eq-location-map,general-link,geoserve,hi...</td>
      <td>-420.0</td>
      <td>1457726978097</td>
      <td>https://earthquake.usgs.gov/earthquakes/eventp...</td>
    </tr>
    <tr>
      <th>911</th>
      <td>2013</td>
      <td>tehachapi</td>
      <td>us</td>
      <td>103368</td>
      <td>12-2013</td>
      <td>4.7</td>
      <td>15443329</td>
      <td>12-13-2013</td>
      <td>10.061</td>
      <td>https://earthquake.usgs.gov/fdsnws/event/1/que...</td>
      <td>...</td>
      <td>,ci,us,</td>
      <td>reviewed</td>
      <td>1386855618540</td>
      <td>M 4.3 - 22km ESE of Bodfish, CA</td>
      <td>1</td>
      <td>earthquake</td>
      <td>,dyfi,impact-text,nearby-cities,origin,phase-d...</td>
      <td>-420.0</td>
      <td>1457761768428</td>
      <td>https://earthquake.usgs.gov/earthquakes/eventp...</td>
    </tr>
    <tr>
      <th>912</th>
      <td>2013</td>
      <td>temecula</td>
      <td>us</td>
      <td>103295</td>
      <td>08-2013</td>
      <td>4.8</td>
      <td>15395521</td>
      <td>08-22-2013</td>
      <td>6.748</td>
      <td>https://earthquake.usgs.gov/fdsnws/event/1/que...</td>
      <td>...</td>
      <td>,ci,us,</td>
      <td>reviewed</td>
      <td>1377178163270</td>
      <td>M 3.6 - 8km N of Pala, CA</td>
      <td>0</td>
      <td>earthquake</td>
      <td>,cap,dyfi,impact-text,nearby-cities,origin,pha...</td>
      <td>-420.0</td>
      <td>1457761274954</td>
      <td>https://earthquake.usgs.gov/earthquakes/eventp...</td>
    </tr>
    <tr>
      <th>914</th>
      <td>2013</td>
      <td>trinidad</td>
      <td>us</td>
      <td>103412</td>
      <td>12-2013</td>
      <td>3.0</td>
      <td>p000k1dw</td>
      <td>12-10-2013</td>
      <td>7.460</td>
      <td>https://earthquake.usgs.gov/fdsnws/event/1/que...</td>
      <td>...</td>
      <td>,us,</td>
      <td>reviewed</td>
      <td>1386646372580</td>
      <td>M 3.6 - 39km W of Raton, New Mexico</td>
      <td>0</td>
      <td>earthquake</td>
      <td>,impact-text,origin,phase-data,</td>
      <td>-360.0</td>
      <td>1438881051355</td>
      <td>https://earthquake.usgs.gov/earthquakes/eventp...</td>
    </tr>
    <tr>
      <th>915</th>
      <td>2013</td>
      <td>truckee</td>
      <td>us</td>
      <td>102264</td>
      <td>12-2013</td>
      <td>3.4</td>
      <td>72112115</td>
      <td>12-07-2013</td>
      <td>7.200</td>
      <td>https://earthquake.usgs.gov/fdsnws/event/1/que...</td>
      <td>...</td>
      <td>,us,nn,nc,</td>
      <td>reviewed</td>
      <td>1386361218020</td>
      <td>M 3.0 - 10km NNW of Incline Village, Nevada</td>
      <td>0</td>
      <td>earthquake</td>
      <td>,focal-mechanism,general-link,geoserve,nearby-...</td>
      <td>-420.0</td>
      <td>1497295253710</td>
      <td>https://earthquake.usgs.gov/earthquakes/eventp...</td>
    </tr>
    <tr>
      <th>916</th>
      <td>2013</td>
      <td>ukiah</td>
      <td>us</td>
      <td>103389</td>
      <td>12-2013</td>
      <td>4.1</td>
      <td>72117945</td>
      <td>12-09-2013</td>
      <td>10.198</td>
      <td>https://earthquake.usgs.gov/fdsnws/event/1/que...</td>
      <td>...</td>
      <td>,nc,us,</td>
      <td>reviewed</td>
      <td>1386572067490</td>
      <td>M 3.5 - Northern California</td>
      <td>0</td>
      <td>earthquake</td>
      <td>,geoserve,nearby-cities,origin,phase-data,scit...</td>
      <td>-420.0</td>
      <td>1489790415327</td>
      <td>https://earthquake.usgs.gov/earthquakes/eventp...</td>
    </tr>
    <tr>
      <th>917</th>
      <td>2013</td>
      <td>university place</td>
      <td>us</td>
      <td>103024</td>
      <td>08-2013</td>
      <td>5.0</td>
      <td>60575952</td>
      <td>08-21-2013</td>
      <td>26.164</td>
      <td>https://earthquake.usgs.gov/fdsnws/event/1/que...</td>
      <td>...</td>
      <td>,uw,us,</td>
      <td>reviewed</td>
      <td>1377024090280</td>
      <td>M 3.6 - 2km NNE of Key Center, Washington</td>
      <td>0</td>
      <td>earthquake</td>
      <td>,cap,dyfi,geoserve,impact-text,nearby-cities,o...</td>
      <td>-420.0</td>
      <td>1491100284530</td>
      <td>https://earthquake.usgs.gov/earthquakes/eventp...</td>
    </tr>
    <tr>
      <th>919</th>
      <td>2013</td>
      <td>victorville</td>
      <td>us</td>
      <td>102226</td>
      <td>08-2013</td>
      <td>3.2</td>
      <td>15396745</td>
      <td>08-25-2013</td>
      <td>6.689</td>
      <td>https://earthquake.usgs.gov/fdsnws/event/1/que...</td>
      <td>...</td>
      <td>,ci,us,</td>
      <td>reviewed</td>
      <td>1377432021860</td>
      <td>M 2.9 - 19km NNE of Victorville, CA</td>
      <td>0</td>
      <td>earthquake</td>
      <td>,cap,dyfi,general-link,geoserve,nearby-cities,...</td>
      <td>-420.0</td>
      <td>1457694250291</td>
      <td>https://earthquake.usgs.gov/earthquakes/eventp...</td>
    </tr>
    <tr>
      <th>920</th>
      <td>2013</td>
      <td>washington</td>
      <td>us</td>
      <td>102638</td>
      <td>06-2013</td>
      <td>3.4</td>
      <td>60025772</td>
      <td>06-01-2013</td>
      <td>6.600</td>
      <td>https://earthquake.usgs.gov/fdsnws/event/1/que...</td>
      <td>...</td>
      <td>,uu,us,ci,</td>
      <td>REVIEWED</td>
      <td>1370043716100</td>
      <td>M 2.9 - 9km ENE of Washington, Utah</td>
      <td>0</td>
      <td>earthquake</td>
      <td>,cap,general-link,general-link,geoserve,impact...</td>
      <td>-360.0</td>
      <td>1457687610931</td>
      <td>https://earthquake.usgs.gov/earthquakes/eventp...</td>
    </tr>
    <tr>
      <th>921</th>
      <td>2013</td>
      <td>washougal</td>
      <td>us</td>
      <td>102784</td>
      <td>08-2013</td>
      <td>NaN</td>
      <td>60572321</td>
      <td>08-14-2013</td>
      <td>6.207</td>
      <td>https://earthquake.usgs.gov/fdsnws/event/1/que...</td>
      <td>...</td>
      <td>,uw,us,</td>
      <td>reviewed</td>
      <td>1376501729520</td>
      <td>M 2.7 - 32km NE of Amboy, Washington</td>
      <td>0</td>
      <td>earthquake</td>
      <td>,cap,general-link,geoserve,nearby-cities,origi...</td>
      <td>-420.0</td>
      <td>1469214904180</td>
      <td>https://earthquake.usgs.gov/earthquakes/eventp...</td>
    </tr>
    <tr>
      <th>922</th>
      <td>2013</td>
      <td>waxahachie</td>
      <td>us</td>
      <td>103186</td>
      <td>02-2013</td>
      <td>2.9</td>
      <td>c000fckx</td>
      <td>02-25-2013</td>
      <td>5.000</td>
      <td>https://earthquake.usgs.gov/fdsnws/event/1/que...</td>
      <td>...</td>
      <td>,us,</td>
      <td>reviewed</td>
      <td>1361740069700</td>
      <td>M 2.7 - 4km S of Midlothian, Texas</td>
      <td>0</td>
      <td>earthquake</td>
      <td>,dyfi,eq-location-map,general-link,general-lin...</td>
      <td>-360.0</td>
      <td>1422591638044</td>
      <td>https://earthquake.usgs.gov/earthquakes/eventp...</td>
    </tr>
    <tr>
      <th>923</th>
      <td>2013</td>
      <td>waynesville</td>
      <td>us</td>
      <td>102607</td>
      <td>06-2013</td>
      <td>3.4</td>
      <td>610189</td>
      <td>06-06-2013</td>
      <td>6.960</td>
      <td>https://earthquake.usgs.gov/fdsnws/event/1/que...</td>
      <td>...</td>
      <td>,se,us,se,</td>
      <td>reviewed</td>
      <td>1370542890300</td>
      <td>M 2.5 - 12km WSW of Cullowhee, North Carolina</td>
      <td>0</td>
      <td>earthquake</td>
      <td>,associate,cap,dyfi,general-link,geoserve,impa...</td>
      <td>-240.0</td>
      <td>1460056191840</td>
      <td>https://earthquake.usgs.gov/earthquakes/eventp...</td>
    </tr>
    <tr>
      <th>924</th>
      <td>2013</td>
      <td>weiser</td>
      <td>us</td>
      <td>102351</td>
      <td>08-2013</td>
      <td>2.7</td>
      <td>b000itvl</td>
      <td>08-02-2013</td>
      <td>1.000</td>
      <td>https://earthquake.usgs.gov/fdsnws/event/1/que...</td>
      <td>...</td>
      <td>,us,uw,</td>
      <td>reviewed</td>
      <td>1375407147160</td>
      <td>M 3.1 - 12km NE of Council, Idaho</td>
      <td>0</td>
      <td>earthquake</td>
      <td>,cap,dyfi,general-link,geoserve,impact-text,ne...</td>
      <td>-360.0</td>
      <td>1469214856960</td>
      <td>https://earthquake.usgs.gov/earthquakes/eventp...</td>
    </tr>
    <tr>
      <th>925</th>
      <td>2013</td>
      <td>wenatchee</td>
      <td>us</td>
      <td>102492</td>
      <td>10-2013</td>
      <td>5.2</td>
      <td>60619906</td>
      <td>10-29-2013</td>
      <td>8.198</td>
      <td>https://earthquake.usgs.gov/fdsnws/event/1/que...</td>
      <td>...</td>
      <td>,uw,us,</td>
      <td>reviewed</td>
      <td>1383045854840</td>
      <td>M 4.3 - 25km N of Leavenworth, Washington</td>
      <td>0</td>
      <td>earthquake</td>
      <td>,cap,dyfi,geoserve,nearby-cities,origin,phase-...</td>
      <td>-420.0</td>
      <td>1500342124325</td>
      <td>https://earthquake.usgs.gov/earthquakes/eventp...</td>
    </tr>
    <tr>
      <th>926</th>
      <td>2013</td>
      <td>west plains</td>
      <td>us</td>
      <td>102176</td>
      <td>09-2013</td>
      <td>NaN</td>
      <td>610031</td>
      <td>09-02-2013</td>
      <td>9.350</td>
      <td>https://earthquake.usgs.gov/fdsnws/event/1/que...</td>
      <td>...</td>
      <td>,nm,nm,us,</td>
      <td>reviewed</td>
      <td>1378149250470</td>
      <td>M 2.5 - 10km WSW of Thayer, Missouri</td>
      <td>0</td>
      <td>earthquake</td>
      <td>,associate,cap,general-link,geoserve,nearby-ci...</td>
      <td>-300.0</td>
      <td>1460055186540</td>
      <td>https://earthquake.usgs.gov/earthquakes/eventp...</td>
    </tr>
    <tr>
      <th>927</th>
      <td>2013</td>
      <td>west wendover</td>
      <td>us</td>
      <td>103214</td>
      <td>04-2013</td>
      <td>NaN</td>
      <td>00410558</td>
      <td>04-27-2013</td>
      <td>14.500</td>
      <td>https://earthquake.usgs.gov/fdsnws/event/1/que...</td>
      <td>...</td>
      <td>,nn,</td>
      <td>reviewed</td>
      <td>1367012881067</td>
      <td>M 2.5 - Nevada</td>
      <td>0</td>
      <td>earthquake</td>
      <td>,origin,phase-data,</td>
      <td>NaN</td>
      <td>1497294430171</td>
      <td>https://earthquake.usgs.gov/earthquakes/eventp...</td>
    </tr>
    <tr>
      <th>929</th>
      <td>2013</td>
      <td>whitefish</td>
      <td>us</td>
      <td>103061</td>
      <td>03-2013</td>
      <td>NaN</td>
      <td>13705190</td>
      <td>03-28-2013</td>
      <td>13.800</td>
      <td>https://earthquake.usgs.gov/fdsnws/event/1/que...</td>
      <td>...</td>
      <td>,mb,us,</td>
      <td>AUTOMATIC</td>
      <td>1364461305800</td>
      <td>M 2.5 - 6km E of Whitefish, Montana</td>
      <td>0</td>
      <td>earthquake</td>
      <td>,general-link,general-link,geoserve,impact-tex...</td>
      <td>-360.0</td>
      <td>1415325024084</td>
      <td>https://earthquake.usgs.gov/earthquakes/eventp...</td>
    </tr>
    <tr>
      <th>930</th>
      <td>2013</td>
      <td>winnemucca</td>
      <td>us</td>
      <td>103197</td>
      <td>10-2013</td>
      <td>2.6</td>
      <td>p000k0xm</td>
      <td>10-11-2013</td>
      <td>12.500</td>
      <td>https://earthquake.usgs.gov/fdsnws/event/1/que...</td>
      <td>...</td>
      <td>,nn,us,</td>
      <td>reviewed</td>
      <td>1381481885682</td>
      <td>M 3.5 - 34km NE of Winnemucca, Nevada</td>
      <td>0</td>
      <td>earthquake</td>
      <td>,general-link,general-link,geoserve,impact-tex...</td>
      <td>-420.0</td>
      <td>1497295069461</td>
      <td>https://earthquake.usgs.gov/earthquakes/eventp...</td>
    </tr>
    <tr>
      <th>931</th>
      <td>2013</td>
      <td>winslow</td>
      <td>us</td>
      <td>102290</td>
      <td>08-2013</td>
      <td>NaN</td>
      <td>b000j1qa</td>
      <td>08-12-2013</td>
      <td>0.000</td>
      <td>https://earthquake.usgs.gov/fdsnws/event/1/que...</td>
      <td>...</td>
      <td>,us,</td>
      <td>reviewed</td>
      <td>1376344721450</td>
      <td>M 2.8 Mining Explosion - 32km SSW of Kayenta, ...</td>
      <td>0</td>
      <td>mining explosion</td>
      <td>,general-link,general-link,geoserve,nearby-cit...</td>
      <td>-360.0</td>
      <td>1381613559000</td>
      <td>https://earthquake.usgs.gov/earthquakes/eventp...</td>
    </tr>
    <tr>
      <th>932</th>
      <td>2013</td>
      <td>yakima</td>
      <td>us</td>
      <td>102164</td>
      <td>09-2013</td>
      <td>NaN</td>
      <td>60585032</td>
      <td>09-05-2013</td>
      <td>9.459</td>
      <td>https://earthquake.usgs.gov/fdsnws/event/1/que...</td>
      <td>...</td>
      <td>,uw,us,</td>
      <td>reviewed</td>
      <td>1378378441580</td>
      <td>M 2.8 - 2km W of Selah, Washington</td>
      <td>0</td>
      <td>earthquake</td>
      <td>,cap,general-link,geoserve,nearby-cities,origi...</td>
      <td>-420.0</td>
      <td>1469214997210</td>
      <td>https://earthquake.usgs.gov/earthquakes/eventp...</td>
    </tr>
    <tr>
      <th>933</th>
      <td>2013</td>
      <td>yucaipa</td>
      <td>us</td>
      <td>102368</td>
      <td>11-2013</td>
      <td>3.0</td>
      <td>11387578</td>
      <td>11-05-2013</td>
      <td>5.745</td>
      <td>https://earthquake.usgs.gov/fdsnws/event/1/que...</td>
      <td>...</td>
      <td>,ci,us,</td>
      <td>reviewed</td>
      <td>1383604183400</td>
      <td>M 3.0 - 9km E of Running Springs, CA</td>
      <td>0</td>
      <td>earthquake</td>
      <td>,dyfi,general-link,geoserve,nearby-cities,orig...</td>
      <td>-420.0</td>
      <td>1457742057117</td>
      <td>https://earthquake.usgs.gov/earthquakes/eventp...</td>
    </tr>
    <tr>
      <th>934</th>
      <td>2013</td>
      <td>yucca valley</td>
      <td>us</td>
      <td>103019</td>
      <td>11-2013</td>
      <td>3.4</td>
      <td>15353001</td>
      <td>11-02-2013</td>
      <td>8.326</td>
      <td>https://earthquake.usgs.gov/fdsnws/event/1/que...</td>
      <td>...</td>
      <td>,ci,us,</td>
      <td>reviewed</td>
      <td>1383416590830</td>
      <td>M 4.3 - 12km W of Ludlow, CA</td>
      <td>0</td>
      <td>earthquake</td>
      <td>,cap,general-link,geoserve,nearby-cities,origi...</td>
      <td>-420.0</td>
      <td>1505334611270</td>
      <td>https://earthquake.usgs.gov/earthquakes/eventp...</td>
    </tr>
  </tbody>
</table>
<p>268 rows × 35 columns</p>
</div>




```python

seismic2013_max_pd.to_csv("./Resources/seismic2013_allmag.csv")
```


```python
seismic_max_pd=seismic_pd.groupby(["cd_by_year","Nrst City"],as_index=False).max()

seismic2013_max_pd=seismic_max_pd.loc[(seismic_max_pd["cd_by_year"]==2013) & (seismic_max_pd["mag"]>=4) & (seismic_max_pd["Nrst Country"]>="us")]

seismic2013_max_pd


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
      <th>cd_by_year</th>
      <th>Nrst City</th>
      <th>Nrst Country</th>
      <th>Unnamed: 0</th>
      <th>cd_by_month</th>
      <th>cdi</th>
      <th>code</th>
      <th>converted_date</th>
      <th>coordinates_3</th>
      <th>detail</th>
      <th>...</th>
      <th>sources</th>
      <th>status</th>
      <th>time</th>
      <th>title</th>
      <th>tsunami</th>
      <th>type</th>
      <th>types</th>
      <th>tz</th>
      <th>updated</th>
      <th>url</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>668</th>
      <td>2013</td>
      <td>choctaw</td>
      <td>us</td>
      <td>103397</td>
      <td>12-2013</td>
      <td>5.8</td>
      <td>p000k1un</td>
      <td>12-29-2013</td>
      <td>13.500</td>
      <td>https://earthquake.usgs.gov/fdsnws/event/1/que...</td>
      <td>...</td>
      <td>,us,us,</td>
      <td>reviewed</td>
      <td>1388316854900</td>
      <td>M 4.4 - 12km ENE of Luther, Oklahoma</td>
      <td>0</td>
      <td>earthquake</td>
      <td>,impact-text,origin,phase-data,</td>
      <td>-300.0</td>
      <td>1500061424477</td>
      <td>https://earthquake.usgs.gov/earthquakes/eventp...</td>
    </tr>
    <tr>
      <th>670</th>
      <td>2013</td>
      <td>clearlake</td>
      <td>us</td>
      <td>103422</td>
      <td>12-2013</td>
      <td>5.5</td>
      <td>72129966</td>
      <td>12-25-2013</td>
      <td>11.837</td>
      <td>https://earthquake.usgs.gov/fdsnws/event/1/que...</td>
      <td>...</td>
      <td>,nc,us,ci,</td>
      <td>reviewed</td>
      <td>1387951171180</td>
      <td>M 4.4 - 5km WSW of Cobb, California</td>
      <td>1</td>
      <td>earthquake</td>
      <td>,nearby-cities,origin,phase-data,scitech-link,</td>
      <td>-420.0</td>
      <td>1489791598734</td>
      <td>https://earthquake.usgs.gov/earthquakes/eventp...</td>
    </tr>
    <tr>
      <th>682</th>
      <td>2013</td>
      <td>corcoran</td>
      <td>us</td>
      <td>101554</td>
      <td>12-2013</td>
      <td>4.1</td>
      <td>72119970</td>
      <td>12-14-2013</td>
      <td>21.746</td>
      <td>https://earthquake.usgs.gov/fdsnws/event/1/que...</td>
      <td>...</td>
      <td>,nc,ci,us,</td>
      <td>reviewed</td>
      <td>1386920997470</td>
      <td>M 4.1 - 23km NNW of Lost Hills, California</td>
      <td>0</td>
      <td>earthquake</td>
      <td>,cap,dyfi,focal-mechanism,geoserve,losspager,m...</td>
      <td>-480.0</td>
      <td>1489791643161</td>
      <td>https://earthquake.usgs.gov/earthquakes/eventp...</td>
    </tr>
    <tr>
      <th>703</th>
      <td>2013</td>
      <td>edmond</td>
      <td>us</td>
      <td>102995</td>
      <td>12-2013</td>
      <td>5.6</td>
      <td>c000lx5r</td>
      <td>12-28-2013</td>
      <td>9.010</td>
      <td>https://earthquake.usgs.gov/fdsnws/event/1/que...</td>
      <td>...</td>
      <td>,us,</td>
      <td>reviewed</td>
      <td>1388182777100</td>
      <td>M 4.5 - 9km ESE of Edmond, Oklahoma</td>
      <td>0</td>
      <td>earthquake</td>
      <td>,general-link,general-link,geoserve,nearby-cit...</td>
      <td>-300.0</td>
      <td>1501730325721</td>
      <td>https://earthquake.usgs.gov/earthquakes/eventp...</td>
    </tr>
    <tr>
      <th>716</th>
      <td>2013</td>
      <td>eureka</td>
      <td>us</td>
      <td>103338</td>
      <td>12-2013</td>
      <td>4.8</td>
      <td>72125816</td>
      <td>12-17-2013</td>
      <td>29.504</td>
      <td>https://earthquake.usgs.gov/fdsnws/event/1/que...</td>
      <td>...</td>
      <td>,nc,us,</td>
      <td>reviewed</td>
      <td>1387286881840</td>
      <td>M 4.9 - 53km WNW of Eureka, California</td>
      <td>1</td>
      <td>earthquake</td>
      <td>,geoserve,nearby-cities,origin,phase-data,scit...</td>
      <td>-420.0</td>
      <td>1489791685025</td>
      <td>https://earthquake.usgs.gov/earthquakes/eventp...</td>
    </tr>
    <tr>
      <th>719</th>
      <td>2013</td>
      <td>fallon</td>
      <td>us</td>
      <td>103357</td>
      <td>10-2013</td>
      <td>3.8</td>
      <td>11250138</td>
      <td>10-17-2013</td>
      <td>13.500</td>
      <td>https://earthquake.usgs.gov/fdsnws/event/1/que...</td>
      <td>...</td>
      <td>,us,nn,ci,nn,</td>
      <td>reviewed</td>
      <td>1381976163479</td>
      <td>M 5.1 - 72km W of Tonopah, Nevada</td>
      <td>0</td>
      <td>earthquake</td>
      <td>,origin,phase-data,</td>
      <td>-420.0</td>
      <td>1500523607182</td>
      <td>https://earthquake.usgs.gov/earthquakes/eventp...</td>
    </tr>
    <tr>
      <th>726</th>
      <td>2013</td>
      <td>fortuna</td>
      <td>us</td>
      <td>103411</td>
      <td>12-2013</td>
      <td>4.2</td>
      <td>72129596</td>
      <td>12-24-2013</td>
      <td>35.439</td>
      <td>https://earthquake.usgs.gov/fdsnws/event/1/que...</td>
      <td>...</td>
      <td>,us,nc,</td>
      <td>reviewed</td>
      <td>1387880929840</td>
      <td>M 4.3 - 59km W of Ferndale, California</td>
      <td>1</td>
      <td>earthquake</td>
      <td>,nearby-cities,origin,phase-data,</td>
      <td>-420.0</td>
      <td>1489791717962</td>
      <td>https://earthquake.usgs.gov/earthquakes/eventp...</td>
    </tr>
    <tr>
      <th>752</th>
      <td>2013</td>
      <td>henderson</td>
      <td>us</td>
      <td>102175</td>
      <td>09-2013</td>
      <td>5.3</td>
      <td>b000jfey</td>
      <td>09-02-2013</td>
      <td>4.750</td>
      <td>https://earthquake.usgs.gov/fdsnws/event/1/que...</td>
      <td>...</td>
      <td>,us,</td>
      <td>reviewed</td>
      <td>1378158758940</td>
      <td>M 4.2 - 14km WNW of Timpson, Texas</td>
      <td>0</td>
      <td>earthquake</td>
      <td>,cap,dyfi,general-link,general-link,geoserve,l...</td>
      <td>-300.0</td>
      <td>1422458814269</td>
      <td>https://earthquake.usgs.gov/earthquakes/eventp...</td>
    </tr>
    <tr>
      <th>760</th>
      <td>2013</td>
      <td>isla vista</td>
      <td>us</td>
      <td>102655</td>
      <td>10-2013</td>
      <td>4.9</td>
      <td>15355073</td>
      <td>10-06-2013</td>
      <td>10.050</td>
      <td>https://earthquake.usgs.gov/fdsnws/event/1/que...</td>
      <td>...</td>
      <td>,ci,us,</td>
      <td>reviewed</td>
      <td>1381051253000</td>
      <td>M 4.8 - 6km W of Isla Vista, CA</td>
      <td>1</td>
      <td>earthquake</td>
      <td>,cap,general-link,geoserve,nearby-cities,origi...</td>
      <td>-420.0</td>
      <td>1492672613462</td>
      <td>https://earthquake.usgs.gov/earthquakes/eventp...</td>
    </tr>
    <tr>
      <th>769</th>
      <td>2013</td>
      <td>la quinta</td>
      <td>us</td>
      <td>103421</td>
      <td>12-2013</td>
      <td>4.8</td>
      <td>15340873</td>
      <td>12-31-2013</td>
      <td>13.378</td>
      <td>https://earthquake.usgs.gov/fdsnws/event/1/que...</td>
      <td>...</td>
      <td>,us,ci,</td>
      <td>reviewed</td>
      <td>1388481668640</td>
      <td>M 4.7 - 21km ESE of Anza, CA</td>
      <td>1</td>
      <td>earthquake</td>
      <td>,general-link,geoserve,nearby-cities,origin,ph...</td>
      <td>-420.0</td>
      <td>1502821201930</td>
      <td>https://earthquake.usgs.gov/earthquakes/eventp...</td>
    </tr>
    <tr>
      <th>772</th>
      <td>2013</td>
      <td>lander</td>
      <td>us</td>
      <td>102924</td>
      <td>09-2013</td>
      <td>4.3</td>
      <td>b000jx5i</td>
      <td>09-21-2013</td>
      <td>76.200</td>
      <td>https://earthquake.usgs.gov/fdsnws/event/1/que...</td>
      <td>...</td>
      <td>,us,</td>
      <td>reviewed</td>
      <td>1379776534090</td>
      <td>M 4.8 - 20km W of Fort Washakie, Wyoming</td>
      <td>0</td>
      <td>earthquake</td>
      <td>,general-link,general-link,geoserve,nearby-cit...</td>
      <td>-360.0</td>
      <td>1422448443940</td>
      <td>https://earthquake.usgs.gov/earthquakes/eventp...</td>
    </tr>
    <tr>
      <th>780</th>
      <td>2013</td>
      <td>lompoc</td>
      <td>us</td>
      <td>102341</td>
      <td>08-2013</td>
      <td>3.1</td>
      <td>15396097</td>
      <td>08-24-2013</td>
      <td>6.357</td>
      <td>https://earthquake.usgs.gov/fdsnws/event/1/que...</td>
      <td>...</td>
      <td>,ci,us,</td>
      <td>reviewed</td>
      <td>1377306990990</td>
      <td>M 4.1 - 45km WSW of Vandenberg Air Force Base, CA</td>
      <td>1</td>
      <td>earthquake</td>
      <td>,cap,general-link,geoserve,nearby-cities,origi...</td>
      <td>-420.0</td>
      <td>1457734899137</td>
      <td>https://earthquake.usgs.gov/earthquakes/eventp...</td>
    </tr>
    <tr>
      <th>806</th>
      <td>2013</td>
      <td>nacogdoches</td>
      <td>us</td>
      <td>103320</td>
      <td>09-2013</td>
      <td>6.3</td>
      <td>p000k11q</td>
      <td>09-03-2013</td>
      <td>5.000</td>
      <td>https://earthquake.usgs.gov/fdsnws/event/1/que...</td>
      <td>...</td>
      <td>,us,us,</td>
      <td>reviewed</td>
      <td>1378165875460</td>
      <td>M 4.3 - 3km WNW of Timpson, Texas</td>
      <td>0</td>
      <td>earthquake</td>
      <td>,impact-text,origin,phase-data,</td>
      <td>-300.0</td>
      <td>1427221297905</td>
      <td>https://earthquake.usgs.gov/earthquakes/eventp...</td>
    </tr>
    <tr>
      <th>849</th>
      <td>2013</td>
      <td>rancho palos verdes</td>
      <td>us</td>
      <td>103362</td>
      <td>11-2013</td>
      <td>4.2</td>
      <td>15356353</td>
      <td>11-21-2013</td>
      <td>7.668</td>
      <td>https://earthquake.usgs.gov/fdsnws/event/1/que...</td>
      <td>...</td>
      <td>,us,ci,us,</td>
      <td>reviewed</td>
      <td>1385039041670</td>
      <td>M 4.1 - 10km S of Rancho Palos Verdes, CA</td>
      <td>1</td>
      <td>earthquake</td>
      <td>,nearby-cities,origin,phase-data,</td>
      <td>-420.0</td>
      <td>1458149638765</td>
      <td>https://earthquake.usgs.gov/earthquakes/eventp...</td>
    </tr>
    <tr>
      <th>858</th>
      <td>2013</td>
      <td>ridgecrest</td>
      <td>us</td>
      <td>103429</td>
      <td>12-2013</td>
      <td>4.6</td>
      <td>15447537</td>
      <td>12-27-2013</td>
      <td>12.300</td>
      <td>https://earthquake.usgs.gov/fdsnws/event/1/que...</td>
      <td>...</td>
      <td>,nn,us,ci,</td>
      <td>reviewed</td>
      <td>1388166936230</td>
      <td>M 4.3 - 15km NW of Coso Junction, CA</td>
      <td>1</td>
      <td>earthquake</td>
      <td>,dyfi,nearby-cities,origin,phase-data,</td>
      <td>-420.0</td>
      <td>1497295139089</td>
      <td>https://earthquake.usgs.gov/earthquakes/eventp...</td>
    </tr>
    <tr>
      <th>898</th>
      <td>2013</td>
      <td>south lake tahoe</td>
      <td>us</td>
      <td>103367</td>
      <td>05-2013</td>
      <td>4.2</td>
      <td>71984666</td>
      <td>05-03-2013</td>
      <td>11.200</td>
      <td>https://earthquake.usgs.gov/fdsnws/event/1/que...</td>
      <td>...</td>
      <td>,nn,us,ci,</td>
      <td>reviewed</td>
      <td>1367584339130</td>
      <td>M 4.0 - 34km SW of Smith Valley, Nevada</td>
      <td>0</td>
      <td>earthquake</td>
      <td>,focal-mechanism,geoserve,nearby-cities,origin...</td>
      <td>-420.0</td>
      <td>1497294448151</td>
      <td>https://earthquake.usgs.gov/earthquakes/eventp...</td>
    </tr>
    <tr>
      <th>904</th>
      <td>2013</td>
      <td>sun valley</td>
      <td>us</td>
      <td>102452</td>
      <td>08-2013</td>
      <td>4.4</td>
      <td>00421303</td>
      <td>08-27-2013</td>
      <td>15.700</td>
      <td>https://earthquake.usgs.gov/fdsnws/event/1/que...</td>
      <td>...</td>
      <td>,nn,nc,us,ci,</td>
      <td>reviewed</td>
      <td>1377564703827</td>
      <td>M 4.2 - 3km NE of Spanish Springs, Nevada</td>
      <td>0</td>
      <td>earthquake</td>
      <td>,cap,focal-mechanism,general-link,geoserve,imp...</td>
      <td>-420.0</td>
      <td>1500523827143</td>
      <td>https://earthquake.usgs.gov/earthquakes/eventp...</td>
    </tr>
    <tr>
      <th>906</th>
      <td>2013</td>
      <td>susanville</td>
      <td>us</td>
      <td>103279</td>
      <td>12-2013</td>
      <td>7.2</td>
      <td>72129046</td>
      <td>12-23-2013</td>
      <td>15.057</td>
      <td>https://earthquake.usgs.gov/fdsnws/event/1/que...</td>
      <td>...</td>
      <td>,us,us,nc,nn,</td>
      <td>reviewed</td>
      <td>1387803229760</td>
      <td>M 5.7 - 10km WNW of Greenville, California</td>
      <td>1</td>
      <td>earthquake</td>
      <td>,nearby-cities,origin,phase-data,scitech-link,</td>
      <td>-420.0</td>
      <td>1497295173528</td>
      <td>https://earthquake.usgs.gov/earthquakes/eventp...</td>
    </tr>
    <tr>
      <th>911</th>
      <td>2013</td>
      <td>tehachapi</td>
      <td>us</td>
      <td>103368</td>
      <td>12-2013</td>
      <td>4.7</td>
      <td>15443329</td>
      <td>12-13-2013</td>
      <td>10.061</td>
      <td>https://earthquake.usgs.gov/fdsnws/event/1/que...</td>
      <td>...</td>
      <td>,ci,us,</td>
      <td>reviewed</td>
      <td>1386855618540</td>
      <td>M 4.3 - 22km ESE of Bodfish, CA</td>
      <td>1</td>
      <td>earthquake</td>
      <td>,dyfi,impact-text,nearby-cities,origin,phase-d...</td>
      <td>-420.0</td>
      <td>1457761768428</td>
      <td>https://earthquake.usgs.gov/earthquakes/eventp...</td>
    </tr>
    <tr>
      <th>925</th>
      <td>2013</td>
      <td>wenatchee</td>
      <td>us</td>
      <td>102492</td>
      <td>10-2013</td>
      <td>5.2</td>
      <td>60619906</td>
      <td>10-29-2013</td>
      <td>8.198</td>
      <td>https://earthquake.usgs.gov/fdsnws/event/1/que...</td>
      <td>...</td>
      <td>,uw,us,</td>
      <td>reviewed</td>
      <td>1383045854840</td>
      <td>M 4.3 - 25km N of Leavenworth, Washington</td>
      <td>0</td>
      <td>earthquake</td>
      <td>,cap,dyfi,geoserve,nearby-cities,origin,phase-...</td>
      <td>-420.0</td>
      <td>1500342124325</td>
      <td>https://earthquake.usgs.gov/earthquakes/eventp...</td>
    </tr>
    <tr>
      <th>934</th>
      <td>2013</td>
      <td>yucca valley</td>
      <td>us</td>
      <td>103019</td>
      <td>11-2013</td>
      <td>3.4</td>
      <td>15353001</td>
      <td>11-02-2013</td>
      <td>8.326</td>
      <td>https://earthquake.usgs.gov/fdsnws/event/1/que...</td>
      <td>...</td>
      <td>,ci,us,</td>
      <td>reviewed</td>
      <td>1383416590830</td>
      <td>M 4.3 - 12km W of Ludlow, CA</td>
      <td>0</td>
      <td>earthquake</td>
      <td>,cap,general-link,geoserve,nearby-cities,origi...</td>
      <td>-420.0</td>
      <td>1505334611270</td>
      <td>https://earthquake.usgs.gov/earthquakes/eventp...</td>
    </tr>
  </tbody>
</table>
<p>21 rows × 35 columns</p>
</div>




```python
seismic2013_max_pd.to_csv("./Resources/seismic2013_ge4mag.csv")
```


```python

```
