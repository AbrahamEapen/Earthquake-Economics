

```python
# Dependencies
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt 
import requests
from census import Census
from us import states

```


```python
##################################
# Get Test Population
##################################



ziplst=[96130,89406,95501,93117,82520,92247,73013,95422,73020,95540,75964,93555,93561,92284,98807,75652,89433,93212,90275,93436,96150]

ctr=1
for zip in ziplst:   
    yr=2012            
    while yr<=2013:
        try:
            c = Census("85ac64b6b5a9c0901b00329d1ef41f0c53ccfc98", year=yr)
            census_data = c.acs5.get(("NAME",
                                              "B01003_001E", 
                                              "B01002_001E",
                                              "B19013_001E",
                                              "B19301_001E",
                                              "B17001_002E",
                                              "B23025_002E",
                                              "B23025_005E",
                                              "B25077_001E",
                                              "B25064_001E",
                                              "C24010_009E",
                                              "C24010_016E",
                                              "C24010_019E",
                                              "C24010_022E",
                                              "C24010_023E",
                                              "C24010_025E",
                                              "C24010_030E",
                                              "C24010_032E",
                                              "C24010_045E",
                                              "C24010_052E",
                                              "C24010_055E",
                                              "C24010_058E",
                                              "C24010_059E",
                                              "C24010_061E",
                                              "C24010_066E",
                                              "C24010_068E",                         
                                     ), {'for': 'zip code tabulation area:'+str(zip)})
            # Convert to DataFrame
            census_pd = pd.DataFrame(census_data)
            census_pd["Year"]=yr
            # Column Reordering
            census_pd = census_pd.rename(columns={"B01003_001E": "Population", 
                                                  "B01002_001E": "Median Age",
                                                  "B19013_001E": "Household Income",
                                                  "B19301_001E": "Per Capita Income",
                                                  "B17001_002E": "Poverty Count",
                                                  "B23025_002E": "Employment Count",
                                                  "B23025_005E": "Unemployment Count",
                                                  "B25077_001E": "Median Home Value",
                                                  "B25064_001E": "Median Gross Rent",                                      
                                                  "C24010_009E": "Emp Male Arch Engnr",
                                                  "C24010_016E": "Emp Male Health Prac",
                                                  "C24010_019E": "Emp Male Service Occ",
                                                  "C24010_022E": "Emp Male Fire Prev",
                                                  "C24010_023E": "Emp Male Law Enfrcmnt",
                                                  "C24010_025E": "Emp Male BldgGrnd Cleaning",
                                                  "C24010_030E": "Emp Male Ntrl Rsrces Const",
                                                  "C24010_032E": "Emp Male Const Extrctn",                                      
                                                  "C24010_045E": "Emp FMale Arch Engnr",
                                                  "C24010_052E": "Emp FMale Health Prac",
                                                  "C24010_055E": "Emp FMale Service Occ",
                                                  "C24010_058E": "Emp FMale Fire Prev",
                                                  "C24010_059E": "Emp FMale Law Enfrcmnt",
                                                  "C24010_061E": "Emp FMale BldgGrnd Cleaning",
                                                  "C24010_066E": "Emp FMale Ntrl Rsrces Const",
                                                  "C24010_068E": "Emp FMale Const Extrctn",                                                                          
                                                  "NAME": "Name",                                       
                                                  "zip code tabulation area": "Zip",
                                                  "Year":"Year"})

            census_pd["Emp Arch Engnr"]=census_pd["Emp Male Arch Engnr"]+census_pd["Emp FMale Arch Engnr"]
            census_pd["Emp Health Prac"]=census_pd["Emp Male Health Prac"]+census_pd["Emp FMale Health Prac"]
            census_pd["Emp Service Occ"]=census_pd["Emp Male Service Occ"]+census_pd["Emp FMale Service Occ"]
            census_pd["Emp Fire Prev"]=census_pd["Emp Male Fire Prev"]+census_pd["Emp FMale Fire Prev"]
            census_pd["Emp Law Enfrcmnt"]=census_pd["Emp Male Law Enfrcmnt"]+census_pd["Emp FMale Law Enfrcmnt"]
            census_pd["Emp BldgGrnd Cleaning"]=census_pd["Emp Male BldgGrnd Cleaning"]+census_pd["Emp FMale BldgGrnd Cleaning"]
            census_pd["Emp Ntrl Rsrces Const"]=census_pd["Emp Male Ntrl Rsrces Const"]+census_pd["Emp FMale Ntrl Rsrces Const"]
            census_pd["Emp Const Extrctn"]=census_pd["Emp Male Const Extrctn"]+census_pd["Emp FMale Const Extrctn"]

            # Add in Poverty Rate (Poverty Count / Population)
            census_pd["Poverty Rate"] = 100 * census_pd["Poverty Count"].astype(int) / census_pd["Population"].astype(int)

            # Add in Employment Rate (Employment Count / Population)
            census_pd["Unemployment Rate"] = 100 * census_pd["Unemployment Count"].astype(int) / census_pd["Population"].astype(int)

            # Final DataFrame
            census_pd = census_pd[["Year",                       
                                   "Name", 
                                   "Zip",
                                   "Population", 
                                   "Median Age", 
                                   "Household Income",
                                   "Per Capita Income", 
                                   "Poverty Count", 
                                   "Poverty Rate",
                                   "Employment Count", 
                                   "Unemployment Rate",
                                   "Median Home Value",
                                   "Median Gross Rent",
                                   "Emp Arch Engnr",
                                   "Emp Health Prac",
                                   "Emp Service Occ",
                                   "Emp Fire Prev",
                                   "Emp Law Enfrcmnt",
                                   "Emp BldgGrnd Cleaning",
                                   "Emp Ntrl Rsrces Const",
                                   "Emp Const Extrctn"
                                   ]]                 

            if ctr==1:
                print("Creating from year "+str(yr)+"...")
                census_pd_test=census_pd
            else:
                print("appending year "+str(yr)+"...")
                census_pd_test=census_pd_test.append(census_pd)
            ctr+=1
        except:
            print("No Data")
        yr+=1
    
```

    Creating from year 2012...
    appending year 2013...
    appending year 2012...
    appending year 2013...
    appending year 2012...
    appending year 2013...
    appending year 2012...
    appending year 2013...
    appending year 2012...
    appending year 2013...
    No Data
    No Data
    appending year 2012...
    appending year 2013...
    appending year 2012...
    appending year 2013...
    appending year 2012...
    appending year 2013...
    appending year 2012...
    appending year 2013...
    appending year 2012...
    appending year 2013...
    appending year 2012...
    appending year 2013...
    appending year 2012...
    appending year 2013...
    appending year 2012...
    appending year 2013...
    No Data
    No Data
    appending year 2012...
    appending year 2013...
    appending year 2012...
    appending year 2013...
    appending year 2012...
    appending year 2013...
    appending year 2012...
    appending year 2013...
    appending year 2012...
    appending year 2013...
    appending year 2012...
    appending year 2013...
    


```python
census_pd_test2012=census_pd_test.loc[census_pd_test["Year"]==2012]
census_pd_test2013=census_pd_test.loc[census_pd_test["Year"]==2013]
```


```python
census_pd_testmrg=pd.merge(census_pd_test2013, census_pd_test2012, on="Zip")
census_pd_testmrg
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
      <th>Year_x</th>
      <th>Name_x</th>
      <th>Zip</th>
      <th>Population_x</th>
      <th>Median Age_x</th>
      <th>Household Income_x</th>
      <th>Per Capita Income_x</th>
      <th>Poverty Count_x</th>
      <th>Poverty Rate_x</th>
      <th>Employment Count_x</th>
      <th>...</th>
      <th>Median Home Value_y</th>
      <th>Median Gross Rent_y</th>
      <th>Emp Arch Engnr_y</th>
      <th>Emp Health Prac_y</th>
      <th>Emp Service Occ_y</th>
      <th>Emp Fire Prev_y</th>
      <th>Emp Law Enfrcmnt_y</th>
      <th>Emp BldgGrnd Cleaning_y</th>
      <th>Emp Ntrl Rsrces Const_y</th>
      <th>Emp Const Extrctn_y</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2013</td>
      <td>ZCTA5 96130</td>
      <td>96130</td>
      <td>21963.0</td>
      <td>35.0</td>
      <td>54372.0</td>
      <td>18621.0</td>
      <td>2847.0</td>
      <td>12.962710</td>
      <td>6710.0</td>
      <td>...</td>
      <td>184500.0</td>
      <td>877.0</td>
      <td>55.0</td>
      <td>377.0</td>
      <td>2254.0</td>
      <td>97.0</td>
      <td>1166.0</td>
      <td>136.0</td>
      <td>472.0</td>
      <td>181.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2013</td>
      <td>ZCTA5 89406</td>
      <td>89406</td>
      <td>24572.0</td>
      <td>39.2</td>
      <td>49830.0</td>
      <td>24716.0</td>
      <td>3619.0</td>
      <td>14.728146</td>
      <td>11912.0</td>
      <td>...</td>
      <td>160100.0</td>
      <td>844.0</td>
      <td>95.0</td>
      <td>378.0</td>
      <td>2199.0</td>
      <td>145.0</td>
      <td>119.0</td>
      <td>538.0</td>
      <td>1133.0</td>
      <td>470.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2013</td>
      <td>ZCTA5 95501</td>
      <td>95501</td>
      <td>23704.0</td>
      <td>37.0</td>
      <td>37600.0</td>
      <td>21122.0</td>
      <td>4968.0</td>
      <td>20.958488</td>
      <td>12342.0</td>
      <td>...</td>
      <td>270700.0</td>
      <td>793.0</td>
      <td>147.0</td>
      <td>332.0</td>
      <td>3071.0</td>
      <td>216.0</td>
      <td>141.0</td>
      <td>669.0</td>
      <td>1232.0</td>
      <td>863.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2013</td>
      <td>ZCTA5 93117</td>
      <td>93117</td>
      <td>54251.0</td>
      <td>21.9</td>
      <td>56669.0</td>
      <td>22722.0</td>
      <td>12866.0</td>
      <td>23.715692</td>
      <td>29618.0</td>
      <td>...</td>
      <td>666200.0</td>
      <td>1445.0</td>
      <td>698.0</td>
      <td>1256.0</td>
      <td>6288.0</td>
      <td>489.0</td>
      <td>72.0</td>
      <td>1384.0</td>
      <td>1313.0</td>
      <td>695.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2013</td>
      <td>ZCTA5 82520</td>
      <td>82520</td>
      <td>13778.0</td>
      <td>39.3</td>
      <td>56554.0</td>
      <td>24971.0</td>
      <td>1549.0</td>
      <td>11.242561</td>
      <td>7138.0</td>
      <td>...</td>
      <td>205100.0</td>
      <td>632.0</td>
      <td>73.0</td>
      <td>431.0</td>
      <td>1411.0</td>
      <td>92.0</td>
      <td>27.0</td>
      <td>436.0</td>
      <td>819.0</td>
      <td>559.0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>2013</td>
      <td>ZCTA5 73013</td>
      <td>73013</td>
      <td>46457.0</td>
      <td>34.7</td>
      <td>75741.0</td>
      <td>38470.0</td>
      <td>3761.0</td>
      <td>8.095658</td>
      <td>24288.0</td>
      <td>...</td>
      <td>185000.0</td>
      <td>997.0</td>
      <td>629.0</td>
      <td>2075.0</td>
      <td>2567.0</td>
      <td>235.0</td>
      <td>111.0</td>
      <td>280.0</td>
      <td>1161.0</td>
      <td>652.0</td>
    </tr>
    <tr>
      <th>6</th>
      <td>2013</td>
      <td>ZCTA5 95422</td>
      <td>95422</td>
      <td>15427.0</td>
      <td>41.6</td>
      <td>25460.0</td>
      <td>16552.0</td>
      <td>5484.0</td>
      <td>35.548065</td>
      <td>6350.0</td>
      <td>...</td>
      <td>104300.0</td>
      <td>750.0</td>
      <td>25.0</td>
      <td>184.0</td>
      <td>1361.0</td>
      <td>15.0</td>
      <td>87.0</td>
      <td>155.0</td>
      <td>948.0</td>
      <td>399.0</td>
    </tr>
    <tr>
      <th>7</th>
      <td>2013</td>
      <td>ZCTA5 73020</td>
      <td>73020</td>
      <td>21499.0</td>
      <td>40.1</td>
      <td>67088.0</td>
      <td>29985.0</td>
      <td>2051.0</td>
      <td>9.539979</td>
      <td>10611.0</td>
      <td>...</td>
      <td>150300.0</td>
      <td>784.0</td>
      <td>206.0</td>
      <td>596.0</td>
      <td>1295.0</td>
      <td>187.0</td>
      <td>109.0</td>
      <td>199.0</td>
      <td>1305.0</td>
      <td>743.0</td>
    </tr>
    <tr>
      <th>8</th>
      <td>2013</td>
      <td>ZCTA5 95540</td>
      <td>95540</td>
      <td>12877.0</td>
      <td>38.7</td>
      <td>40751.0</td>
      <td>21707.0</td>
      <td>2543.0</td>
      <td>19.748389</td>
      <td>5336.0</td>
      <td>...</td>
      <td>292900.0</td>
      <td>784.0</td>
      <td>32.0</td>
      <td>200.0</td>
      <td>1160.0</td>
      <td>35.0</td>
      <td>149.0</td>
      <td>398.0</td>
      <td>606.0</td>
      <td>215.0</td>
    </tr>
    <tr>
      <th>9</th>
      <td>2013</td>
      <td>ZCTA5 75964</td>
      <td>75964</td>
      <td>19648.0</td>
      <td>32.8</td>
      <td>35827.0</td>
      <td>17313.0</td>
      <td>6085.0</td>
      <td>30.970073</td>
      <td>8920.0</td>
      <td>...</td>
      <td>78200.0</td>
      <td>677.0</td>
      <td>52.0</td>
      <td>451.0</td>
      <td>1316.0</td>
      <td>25.0</td>
      <td>87.0</td>
      <td>346.0</td>
      <td>1243.0</td>
      <td>729.0</td>
    </tr>
    <tr>
      <th>10</th>
      <td>2013</td>
      <td>ZCTA5 93555</td>
      <td>93555</td>
      <td>32376.0</td>
      <td>36.4</td>
      <td>61221.0</td>
      <td>28697.0</td>
      <td>4515.0</td>
      <td>13.945515</td>
      <td>15835.0</td>
      <td>...</td>
      <td>185200.0</td>
      <td>800.0</td>
      <td>1244.0</td>
      <td>533.0</td>
      <td>2049.0</td>
      <td>165.0</td>
      <td>195.0</td>
      <td>460.0</td>
      <td>1543.0</td>
      <td>589.0</td>
    </tr>
    <tr>
      <th>11</th>
      <td>2013</td>
      <td>ZCTA5 93561</td>
      <td>93561</td>
      <td>34851.0</td>
      <td>39.3</td>
      <td>57433.0</td>
      <td>24708.0</td>
      <td>3665.0</td>
      <td>10.516198</td>
      <td>13276.0</td>
      <td>...</td>
      <td>229300.0</td>
      <td>902.0</td>
      <td>380.0</td>
      <td>620.0</td>
      <td>3305.0</td>
      <td>168.0</td>
      <td>965.0</td>
      <td>618.0</td>
      <td>1514.0</td>
      <td>718.0</td>
    </tr>
    <tr>
      <th>12</th>
      <td>2013</td>
      <td>ZCTA5 92284</td>
      <td>92284</td>
      <td>24951.0</td>
      <td>38.1</td>
      <td>41398.0</td>
      <td>20534.0</td>
      <td>4835.0</td>
      <td>19.377981</td>
      <td>10451.0</td>
      <td>...</td>
      <td>156200.0</td>
      <td>878.0</td>
      <td>68.0</td>
      <td>628.0</td>
      <td>1552.0</td>
      <td>59.0</td>
      <td>114.0</td>
      <td>400.0</td>
      <td>1495.0</td>
      <td>893.0</td>
    </tr>
    <tr>
      <th>13</th>
      <td>2013</td>
      <td>ZCTA5 75652</td>
      <td>75652</td>
      <td>16185.0</td>
      <td>39.4</td>
      <td>42778.0</td>
      <td>19036.0</td>
      <td>2058.0</td>
      <td>12.715477</td>
      <td>5802.0</td>
      <td>...</td>
      <td>91600.0</td>
      <td>621.0</td>
      <td>40.0</td>
      <td>201.0</td>
      <td>1309.0</td>
      <td>81.0</td>
      <td>170.0</td>
      <td>226.0</td>
      <td>802.0</td>
      <td>545.0</td>
    </tr>
    <tr>
      <th>14</th>
      <td>2013</td>
      <td>ZCTA5 89433</td>
      <td>89433</td>
      <td>19630.0</td>
      <td>34.2</td>
      <td>41639.0</td>
      <td>17272.0</td>
      <td>4454.0</td>
      <td>22.689761</td>
      <td>9702.0</td>
      <td>...</td>
      <td>116300.0</td>
      <td>967.0</td>
      <td>89.0</td>
      <td>169.0</td>
      <td>2057.0</td>
      <td>196.0</td>
      <td>34.0</td>
      <td>671.0</td>
      <td>845.0</td>
      <td>585.0</td>
    </tr>
    <tr>
      <th>15</th>
      <td>2013</td>
      <td>ZCTA5 93212</td>
      <td>93212</td>
      <td>25618.0</td>
      <td>36.1</td>
      <td>33756.0</td>
      <td>8700.0</td>
      <td>3973.0</td>
      <td>15.508627</td>
      <td>5738.0</td>
      <td>...</td>
      <td>137400.0</td>
      <td>812.0</td>
      <td>12.0</td>
      <td>109.0</td>
      <td>842.0</td>
      <td>23.0</td>
      <td>132.0</td>
      <td>141.0</td>
      <td>1491.0</td>
      <td>269.0</td>
    </tr>
    <tr>
      <th>16</th>
      <td>2013</td>
      <td>ZCTA5 90275</td>
      <td>90275</td>
      <td>42051.0</td>
      <td>49.0</td>
      <td>118860.0</td>
      <td>57705.0</td>
      <td>1884.0</td>
      <td>4.480274</td>
      <td>18969.0</td>
      <td>...</td>
      <td>974900.0</td>
      <td>2001.0</td>
      <td>1047.0</td>
      <td>1853.0</td>
      <td>1036.0</td>
      <td>211.0</td>
      <td>92.0</td>
      <td>59.0</td>
      <td>326.0</td>
      <td>131.0</td>
    </tr>
    <tr>
      <th>17</th>
      <td>2013</td>
      <td>ZCTA5 93436</td>
      <td>93436</td>
      <td>54433.0</td>
      <td>34.0</td>
      <td>52032.0</td>
      <td>22926.0</td>
      <td>10073.0</td>
      <td>18.505318</td>
      <td>24231.0</td>
      <td>...</td>
      <td>277200.0</td>
      <td>995.0</td>
      <td>613.0</td>
      <td>908.0</td>
      <td>5504.0</td>
      <td>527.0</td>
      <td>368.0</td>
      <td>1397.0</td>
      <td>3044.0</td>
      <td>1095.0</td>
    </tr>
    <tr>
      <th>18</th>
      <td>2013</td>
      <td>ZCTA5 96150</td>
      <td>96150</td>
      <td>28686.0</td>
      <td>40.0</td>
      <td>46859.0</td>
      <td>25479.0</td>
      <td>4443.0</td>
      <td>15.488392</td>
      <td>16574.0</td>
      <td>...</td>
      <td>364100.0</td>
      <td>969.0</td>
      <td>188.0</td>
      <td>513.0</td>
      <td>5299.0</td>
      <td>472.0</td>
      <td>188.0</td>
      <td>1460.0</td>
      <td>1493.0</td>
      <td>867.0</td>
    </tr>
  </tbody>
</table>
<p>19 rows × 41 columns</p>
</div>




```python
#census_pd_testmrg=census_pd_testmrg.loc[(census_pd_testmrg["Population_x"]>=1100) & (census_pd_testmrg["Population_x"]<=35000)]
#census_pd_testmrg
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
      <th>Year_x</th>
      <th>Name_x</th>
      <th>Zip</th>
      <th>Population_x</th>
      <th>Median Age_x</th>
      <th>Household Income_x</th>
      <th>Per Capita Income_x</th>
      <th>Poverty Count_x</th>
      <th>Poverty Rate_x</th>
      <th>Employment Count_x</th>
      <th>...</th>
      <th>Median Home Value_y</th>
      <th>Median Gross Rent_y</th>
      <th>Emp Arch Engnr_y</th>
      <th>Emp Health Prac_y</th>
      <th>Emp Service Occ_y</th>
      <th>Emp Fire Prev_y</th>
      <th>Emp Law Enfrcmnt_y</th>
      <th>Emp BldgGrnd Cleaning_y</th>
      <th>Emp Ntrl Rsrces Const_y</th>
      <th>Emp Const Extrctn_y</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2013</td>
      <td>ZCTA5 96130</td>
      <td>96130</td>
      <td>21963.0</td>
      <td>35.0</td>
      <td>54372.0</td>
      <td>18621.0</td>
      <td>2847.0</td>
      <td>12.962710</td>
      <td>6710.0</td>
      <td>...</td>
      <td>184500.0</td>
      <td>877.0</td>
      <td>55.0</td>
      <td>377.0</td>
      <td>2254.0</td>
      <td>97.0</td>
      <td>1166.0</td>
      <td>136.0</td>
      <td>472.0</td>
      <td>181.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2013</td>
      <td>ZCTA5 89406</td>
      <td>89406</td>
      <td>24572.0</td>
      <td>39.2</td>
      <td>49830.0</td>
      <td>24716.0</td>
      <td>3619.0</td>
      <td>14.728146</td>
      <td>11912.0</td>
      <td>...</td>
      <td>160100.0</td>
      <td>844.0</td>
      <td>95.0</td>
      <td>378.0</td>
      <td>2199.0</td>
      <td>145.0</td>
      <td>119.0</td>
      <td>538.0</td>
      <td>1133.0</td>
      <td>470.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2013</td>
      <td>ZCTA5 95501</td>
      <td>95501</td>
      <td>23704.0</td>
      <td>37.0</td>
      <td>37600.0</td>
      <td>21122.0</td>
      <td>4968.0</td>
      <td>20.958488</td>
      <td>12342.0</td>
      <td>...</td>
      <td>270700.0</td>
      <td>793.0</td>
      <td>147.0</td>
      <td>332.0</td>
      <td>3071.0</td>
      <td>216.0</td>
      <td>141.0</td>
      <td>669.0</td>
      <td>1232.0</td>
      <td>863.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2013</td>
      <td>ZCTA5 82520</td>
      <td>82520</td>
      <td>13778.0</td>
      <td>39.3</td>
      <td>56554.0</td>
      <td>24971.0</td>
      <td>1549.0</td>
      <td>11.242561</td>
      <td>7138.0</td>
      <td>...</td>
      <td>205100.0</td>
      <td>632.0</td>
      <td>73.0</td>
      <td>431.0</td>
      <td>1411.0</td>
      <td>92.0</td>
      <td>27.0</td>
      <td>436.0</td>
      <td>819.0</td>
      <td>559.0</td>
    </tr>
    <tr>
      <th>6</th>
      <td>2013</td>
      <td>ZCTA5 95422</td>
      <td>95422</td>
      <td>15427.0</td>
      <td>41.6</td>
      <td>25460.0</td>
      <td>16552.0</td>
      <td>5484.0</td>
      <td>35.548065</td>
      <td>6350.0</td>
      <td>...</td>
      <td>104300.0</td>
      <td>750.0</td>
      <td>25.0</td>
      <td>184.0</td>
      <td>1361.0</td>
      <td>15.0</td>
      <td>87.0</td>
      <td>155.0</td>
      <td>948.0</td>
      <td>399.0</td>
    </tr>
    <tr>
      <th>7</th>
      <td>2013</td>
      <td>ZCTA5 73020</td>
      <td>73020</td>
      <td>21499.0</td>
      <td>40.1</td>
      <td>67088.0</td>
      <td>29985.0</td>
      <td>2051.0</td>
      <td>9.539979</td>
      <td>10611.0</td>
      <td>...</td>
      <td>150300.0</td>
      <td>784.0</td>
      <td>206.0</td>
      <td>596.0</td>
      <td>1295.0</td>
      <td>187.0</td>
      <td>109.0</td>
      <td>199.0</td>
      <td>1305.0</td>
      <td>743.0</td>
    </tr>
    <tr>
      <th>8</th>
      <td>2013</td>
      <td>ZCTA5 95540</td>
      <td>95540</td>
      <td>12877.0</td>
      <td>38.7</td>
      <td>40751.0</td>
      <td>21707.0</td>
      <td>2543.0</td>
      <td>19.748389</td>
      <td>5336.0</td>
      <td>...</td>
      <td>292900.0</td>
      <td>784.0</td>
      <td>32.0</td>
      <td>200.0</td>
      <td>1160.0</td>
      <td>35.0</td>
      <td>149.0</td>
      <td>398.0</td>
      <td>606.0</td>
      <td>215.0</td>
    </tr>
    <tr>
      <th>9</th>
      <td>2013</td>
      <td>ZCTA5 75964</td>
      <td>75964</td>
      <td>19648.0</td>
      <td>32.8</td>
      <td>35827.0</td>
      <td>17313.0</td>
      <td>6085.0</td>
      <td>30.970073</td>
      <td>8920.0</td>
      <td>...</td>
      <td>78200.0</td>
      <td>677.0</td>
      <td>52.0</td>
      <td>451.0</td>
      <td>1316.0</td>
      <td>25.0</td>
      <td>87.0</td>
      <td>346.0</td>
      <td>1243.0</td>
      <td>729.0</td>
    </tr>
    <tr>
      <th>10</th>
      <td>2013</td>
      <td>ZCTA5 93555</td>
      <td>93555</td>
      <td>32376.0</td>
      <td>36.4</td>
      <td>61221.0</td>
      <td>28697.0</td>
      <td>4515.0</td>
      <td>13.945515</td>
      <td>15835.0</td>
      <td>...</td>
      <td>185200.0</td>
      <td>800.0</td>
      <td>1244.0</td>
      <td>533.0</td>
      <td>2049.0</td>
      <td>165.0</td>
      <td>195.0</td>
      <td>460.0</td>
      <td>1543.0</td>
      <td>589.0</td>
    </tr>
    <tr>
      <th>11</th>
      <td>2013</td>
      <td>ZCTA5 93561</td>
      <td>93561</td>
      <td>34851.0</td>
      <td>39.3</td>
      <td>57433.0</td>
      <td>24708.0</td>
      <td>3665.0</td>
      <td>10.516198</td>
      <td>13276.0</td>
      <td>...</td>
      <td>229300.0</td>
      <td>902.0</td>
      <td>380.0</td>
      <td>620.0</td>
      <td>3305.0</td>
      <td>168.0</td>
      <td>965.0</td>
      <td>618.0</td>
      <td>1514.0</td>
      <td>718.0</td>
    </tr>
    <tr>
      <th>12</th>
      <td>2013</td>
      <td>ZCTA5 92284</td>
      <td>92284</td>
      <td>24951.0</td>
      <td>38.1</td>
      <td>41398.0</td>
      <td>20534.0</td>
      <td>4835.0</td>
      <td>19.377981</td>
      <td>10451.0</td>
      <td>...</td>
      <td>156200.0</td>
      <td>878.0</td>
      <td>68.0</td>
      <td>628.0</td>
      <td>1552.0</td>
      <td>59.0</td>
      <td>114.0</td>
      <td>400.0</td>
      <td>1495.0</td>
      <td>893.0</td>
    </tr>
    <tr>
      <th>13</th>
      <td>2013</td>
      <td>ZCTA5 75652</td>
      <td>75652</td>
      <td>16185.0</td>
      <td>39.4</td>
      <td>42778.0</td>
      <td>19036.0</td>
      <td>2058.0</td>
      <td>12.715477</td>
      <td>5802.0</td>
      <td>...</td>
      <td>91600.0</td>
      <td>621.0</td>
      <td>40.0</td>
      <td>201.0</td>
      <td>1309.0</td>
      <td>81.0</td>
      <td>170.0</td>
      <td>226.0</td>
      <td>802.0</td>
      <td>545.0</td>
    </tr>
    <tr>
      <th>14</th>
      <td>2013</td>
      <td>ZCTA5 89433</td>
      <td>89433</td>
      <td>19630.0</td>
      <td>34.2</td>
      <td>41639.0</td>
      <td>17272.0</td>
      <td>4454.0</td>
      <td>22.689761</td>
      <td>9702.0</td>
      <td>...</td>
      <td>116300.0</td>
      <td>967.0</td>
      <td>89.0</td>
      <td>169.0</td>
      <td>2057.0</td>
      <td>196.0</td>
      <td>34.0</td>
      <td>671.0</td>
      <td>845.0</td>
      <td>585.0</td>
    </tr>
    <tr>
      <th>15</th>
      <td>2013</td>
      <td>ZCTA5 93212</td>
      <td>93212</td>
      <td>25618.0</td>
      <td>36.1</td>
      <td>33756.0</td>
      <td>8700.0</td>
      <td>3973.0</td>
      <td>15.508627</td>
      <td>5738.0</td>
      <td>...</td>
      <td>137400.0</td>
      <td>812.0</td>
      <td>12.0</td>
      <td>109.0</td>
      <td>842.0</td>
      <td>23.0</td>
      <td>132.0</td>
      <td>141.0</td>
      <td>1491.0</td>
      <td>269.0</td>
    </tr>
    <tr>
      <th>18</th>
      <td>2013</td>
      <td>ZCTA5 96150</td>
      <td>96150</td>
      <td>28686.0</td>
      <td>40.0</td>
      <td>46859.0</td>
      <td>25479.0</td>
      <td>4443.0</td>
      <td>15.488392</td>
      <td>16574.0</td>
      <td>...</td>
      <td>364100.0</td>
      <td>969.0</td>
      <td>188.0</td>
      <td>513.0</td>
      <td>5299.0</td>
      <td>472.0</td>
      <td>188.0</td>
      <td>1460.0</td>
      <td>1493.0</td>
      <td>867.0</td>
    </tr>
  </tbody>
</table>
<p>15 rows × 41 columns</p>
</div>




```python
census_pd_testsample=census_pd_testmrg

```


```python
census_pd_testsample
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
      <th>Year_x</th>
      <th>Name_x</th>
      <th>Zip</th>
      <th>Population_x</th>
      <th>Median Age_x</th>
      <th>Household Income_x</th>
      <th>Per Capita Income_x</th>
      <th>Poverty Count_x</th>
      <th>Poverty Rate_x</th>
      <th>Employment Count_x</th>
      <th>...</th>
      <th>Median Home Value_y</th>
      <th>Median Gross Rent_y</th>
      <th>Emp Arch Engnr_y</th>
      <th>Emp Health Prac_y</th>
      <th>Emp Service Occ_y</th>
      <th>Emp Fire Prev_y</th>
      <th>Emp Law Enfrcmnt_y</th>
      <th>Emp BldgGrnd Cleaning_y</th>
      <th>Emp Ntrl Rsrces Const_y</th>
      <th>Emp Const Extrctn_y</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2013</td>
      <td>ZCTA5 96130</td>
      <td>96130</td>
      <td>21963.0</td>
      <td>35.0</td>
      <td>54372.0</td>
      <td>18621.0</td>
      <td>2847.0</td>
      <td>12.962710</td>
      <td>6710.0</td>
      <td>...</td>
      <td>184500.0</td>
      <td>877.0</td>
      <td>55.0</td>
      <td>377.0</td>
      <td>2254.0</td>
      <td>97.0</td>
      <td>1166.0</td>
      <td>136.0</td>
      <td>472.0</td>
      <td>181.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2013</td>
      <td>ZCTA5 89406</td>
      <td>89406</td>
      <td>24572.0</td>
      <td>39.2</td>
      <td>49830.0</td>
      <td>24716.0</td>
      <td>3619.0</td>
      <td>14.728146</td>
      <td>11912.0</td>
      <td>...</td>
      <td>160100.0</td>
      <td>844.0</td>
      <td>95.0</td>
      <td>378.0</td>
      <td>2199.0</td>
      <td>145.0</td>
      <td>119.0</td>
      <td>538.0</td>
      <td>1133.0</td>
      <td>470.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2013</td>
      <td>ZCTA5 95501</td>
      <td>95501</td>
      <td>23704.0</td>
      <td>37.0</td>
      <td>37600.0</td>
      <td>21122.0</td>
      <td>4968.0</td>
      <td>20.958488</td>
      <td>12342.0</td>
      <td>...</td>
      <td>270700.0</td>
      <td>793.0</td>
      <td>147.0</td>
      <td>332.0</td>
      <td>3071.0</td>
      <td>216.0</td>
      <td>141.0</td>
      <td>669.0</td>
      <td>1232.0</td>
      <td>863.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2013</td>
      <td>ZCTA5 93117</td>
      <td>93117</td>
      <td>54251.0</td>
      <td>21.9</td>
      <td>56669.0</td>
      <td>22722.0</td>
      <td>12866.0</td>
      <td>23.715692</td>
      <td>29618.0</td>
      <td>...</td>
      <td>666200.0</td>
      <td>1445.0</td>
      <td>698.0</td>
      <td>1256.0</td>
      <td>6288.0</td>
      <td>489.0</td>
      <td>72.0</td>
      <td>1384.0</td>
      <td>1313.0</td>
      <td>695.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2013</td>
      <td>ZCTA5 82520</td>
      <td>82520</td>
      <td>13778.0</td>
      <td>39.3</td>
      <td>56554.0</td>
      <td>24971.0</td>
      <td>1549.0</td>
      <td>11.242561</td>
      <td>7138.0</td>
      <td>...</td>
      <td>205100.0</td>
      <td>632.0</td>
      <td>73.0</td>
      <td>431.0</td>
      <td>1411.0</td>
      <td>92.0</td>
      <td>27.0</td>
      <td>436.0</td>
      <td>819.0</td>
      <td>559.0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>2013</td>
      <td>ZCTA5 73013</td>
      <td>73013</td>
      <td>46457.0</td>
      <td>34.7</td>
      <td>75741.0</td>
      <td>38470.0</td>
      <td>3761.0</td>
      <td>8.095658</td>
      <td>24288.0</td>
      <td>...</td>
      <td>185000.0</td>
      <td>997.0</td>
      <td>629.0</td>
      <td>2075.0</td>
      <td>2567.0</td>
      <td>235.0</td>
      <td>111.0</td>
      <td>280.0</td>
      <td>1161.0</td>
      <td>652.0</td>
    </tr>
    <tr>
      <th>6</th>
      <td>2013</td>
      <td>ZCTA5 95422</td>
      <td>95422</td>
      <td>15427.0</td>
      <td>41.6</td>
      <td>25460.0</td>
      <td>16552.0</td>
      <td>5484.0</td>
      <td>35.548065</td>
      <td>6350.0</td>
      <td>...</td>
      <td>104300.0</td>
      <td>750.0</td>
      <td>25.0</td>
      <td>184.0</td>
      <td>1361.0</td>
      <td>15.0</td>
      <td>87.0</td>
      <td>155.0</td>
      <td>948.0</td>
      <td>399.0</td>
    </tr>
    <tr>
      <th>7</th>
      <td>2013</td>
      <td>ZCTA5 73020</td>
      <td>73020</td>
      <td>21499.0</td>
      <td>40.1</td>
      <td>67088.0</td>
      <td>29985.0</td>
      <td>2051.0</td>
      <td>9.539979</td>
      <td>10611.0</td>
      <td>...</td>
      <td>150300.0</td>
      <td>784.0</td>
      <td>206.0</td>
      <td>596.0</td>
      <td>1295.0</td>
      <td>187.0</td>
      <td>109.0</td>
      <td>199.0</td>
      <td>1305.0</td>
      <td>743.0</td>
    </tr>
    <tr>
      <th>8</th>
      <td>2013</td>
      <td>ZCTA5 95540</td>
      <td>95540</td>
      <td>12877.0</td>
      <td>38.7</td>
      <td>40751.0</td>
      <td>21707.0</td>
      <td>2543.0</td>
      <td>19.748389</td>
      <td>5336.0</td>
      <td>...</td>
      <td>292900.0</td>
      <td>784.0</td>
      <td>32.0</td>
      <td>200.0</td>
      <td>1160.0</td>
      <td>35.0</td>
      <td>149.0</td>
      <td>398.0</td>
      <td>606.0</td>
      <td>215.0</td>
    </tr>
    <tr>
      <th>9</th>
      <td>2013</td>
      <td>ZCTA5 75964</td>
      <td>75964</td>
      <td>19648.0</td>
      <td>32.8</td>
      <td>35827.0</td>
      <td>17313.0</td>
      <td>6085.0</td>
      <td>30.970073</td>
      <td>8920.0</td>
      <td>...</td>
      <td>78200.0</td>
      <td>677.0</td>
      <td>52.0</td>
      <td>451.0</td>
      <td>1316.0</td>
      <td>25.0</td>
      <td>87.0</td>
      <td>346.0</td>
      <td>1243.0</td>
      <td>729.0</td>
    </tr>
    <tr>
      <th>10</th>
      <td>2013</td>
      <td>ZCTA5 93555</td>
      <td>93555</td>
      <td>32376.0</td>
      <td>36.4</td>
      <td>61221.0</td>
      <td>28697.0</td>
      <td>4515.0</td>
      <td>13.945515</td>
      <td>15835.0</td>
      <td>...</td>
      <td>185200.0</td>
      <td>800.0</td>
      <td>1244.0</td>
      <td>533.0</td>
      <td>2049.0</td>
      <td>165.0</td>
      <td>195.0</td>
      <td>460.0</td>
      <td>1543.0</td>
      <td>589.0</td>
    </tr>
    <tr>
      <th>11</th>
      <td>2013</td>
      <td>ZCTA5 93561</td>
      <td>93561</td>
      <td>34851.0</td>
      <td>39.3</td>
      <td>57433.0</td>
      <td>24708.0</td>
      <td>3665.0</td>
      <td>10.516198</td>
      <td>13276.0</td>
      <td>...</td>
      <td>229300.0</td>
      <td>902.0</td>
      <td>380.0</td>
      <td>620.0</td>
      <td>3305.0</td>
      <td>168.0</td>
      <td>965.0</td>
      <td>618.0</td>
      <td>1514.0</td>
      <td>718.0</td>
    </tr>
    <tr>
      <th>12</th>
      <td>2013</td>
      <td>ZCTA5 92284</td>
      <td>92284</td>
      <td>24951.0</td>
      <td>38.1</td>
      <td>41398.0</td>
      <td>20534.0</td>
      <td>4835.0</td>
      <td>19.377981</td>
      <td>10451.0</td>
      <td>...</td>
      <td>156200.0</td>
      <td>878.0</td>
      <td>68.0</td>
      <td>628.0</td>
      <td>1552.0</td>
      <td>59.0</td>
      <td>114.0</td>
      <td>400.0</td>
      <td>1495.0</td>
      <td>893.0</td>
    </tr>
    <tr>
      <th>13</th>
      <td>2013</td>
      <td>ZCTA5 75652</td>
      <td>75652</td>
      <td>16185.0</td>
      <td>39.4</td>
      <td>42778.0</td>
      <td>19036.0</td>
      <td>2058.0</td>
      <td>12.715477</td>
      <td>5802.0</td>
      <td>...</td>
      <td>91600.0</td>
      <td>621.0</td>
      <td>40.0</td>
      <td>201.0</td>
      <td>1309.0</td>
      <td>81.0</td>
      <td>170.0</td>
      <td>226.0</td>
      <td>802.0</td>
      <td>545.0</td>
    </tr>
    <tr>
      <th>14</th>
      <td>2013</td>
      <td>ZCTA5 89433</td>
      <td>89433</td>
      <td>19630.0</td>
      <td>34.2</td>
      <td>41639.0</td>
      <td>17272.0</td>
      <td>4454.0</td>
      <td>22.689761</td>
      <td>9702.0</td>
      <td>...</td>
      <td>116300.0</td>
      <td>967.0</td>
      <td>89.0</td>
      <td>169.0</td>
      <td>2057.0</td>
      <td>196.0</td>
      <td>34.0</td>
      <td>671.0</td>
      <td>845.0</td>
      <td>585.0</td>
    </tr>
    <tr>
      <th>15</th>
      <td>2013</td>
      <td>ZCTA5 93212</td>
      <td>93212</td>
      <td>25618.0</td>
      <td>36.1</td>
      <td>33756.0</td>
      <td>8700.0</td>
      <td>3973.0</td>
      <td>15.508627</td>
      <td>5738.0</td>
      <td>...</td>
      <td>137400.0</td>
      <td>812.0</td>
      <td>12.0</td>
      <td>109.0</td>
      <td>842.0</td>
      <td>23.0</td>
      <td>132.0</td>
      <td>141.0</td>
      <td>1491.0</td>
      <td>269.0</td>
    </tr>
    <tr>
      <th>16</th>
      <td>2013</td>
      <td>ZCTA5 90275</td>
      <td>90275</td>
      <td>42051.0</td>
      <td>49.0</td>
      <td>118860.0</td>
      <td>57705.0</td>
      <td>1884.0</td>
      <td>4.480274</td>
      <td>18969.0</td>
      <td>...</td>
      <td>974900.0</td>
      <td>2001.0</td>
      <td>1047.0</td>
      <td>1853.0</td>
      <td>1036.0</td>
      <td>211.0</td>
      <td>92.0</td>
      <td>59.0</td>
      <td>326.0</td>
      <td>131.0</td>
    </tr>
    <tr>
      <th>17</th>
      <td>2013</td>
      <td>ZCTA5 93436</td>
      <td>93436</td>
      <td>54433.0</td>
      <td>34.0</td>
      <td>52032.0</td>
      <td>22926.0</td>
      <td>10073.0</td>
      <td>18.505318</td>
      <td>24231.0</td>
      <td>...</td>
      <td>277200.0</td>
      <td>995.0</td>
      <td>613.0</td>
      <td>908.0</td>
      <td>5504.0</td>
      <td>527.0</td>
      <td>368.0</td>
      <td>1397.0</td>
      <td>3044.0</td>
      <td>1095.0</td>
    </tr>
    <tr>
      <th>18</th>
      <td>2013</td>
      <td>ZCTA5 96150</td>
      <td>96150</td>
      <td>28686.0</td>
      <td>40.0</td>
      <td>46859.0</td>
      <td>25479.0</td>
      <td>4443.0</td>
      <td>15.488392</td>
      <td>16574.0</td>
      <td>...</td>
      <td>364100.0</td>
      <td>969.0</td>
      <td>188.0</td>
      <td>513.0</td>
      <td>5299.0</td>
      <td>472.0</td>
      <td>188.0</td>
      <td>1460.0</td>
      <td>1493.0</td>
      <td>867.0</td>
    </tr>
  </tbody>
</table>
<p>19 rows × 41 columns</p>
</div>




```python
for index,row in census_pd_testsample.iterrows():
    pcchg=(row["Per Capita Income_x"]-row["Per Capita Income_y"])/row["Per Capita Income_y"]
    census_pd_testsample.loc[index,"PCI Change"]=pcchg
    

```


```python
census_pd_testsample.to_csv("./Resources/seismic_test_pop.csv")

    
    
```


```python
census_pd_test_ttest=census_pd_testsample[["Zip","PCI Change"]]
```


```python
census_pd_test_ttest
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
      <th>Zip</th>
      <th>PCI Change</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>96130</td>
      <td>-0.033629</td>
    </tr>
    <tr>
      <th>1</th>
      <td>89406</td>
      <td>-0.016631</td>
    </tr>
    <tr>
      <th>2</th>
      <td>95501</td>
      <td>-0.062910</td>
    </tr>
    <tr>
      <th>3</th>
      <td>93117</td>
      <td>-0.028102</td>
    </tr>
    <tr>
      <th>4</th>
      <td>82520</td>
      <td>0.012283</td>
    </tr>
    <tr>
      <th>5</th>
      <td>73013</td>
      <td>-0.020272</td>
    </tr>
    <tr>
      <th>6</th>
      <td>95422</td>
      <td>0.018773</td>
    </tr>
    <tr>
      <th>7</th>
      <td>73020</td>
      <td>0.037902</td>
    </tr>
    <tr>
      <th>8</th>
      <td>95540</td>
      <td>-0.060953</td>
    </tr>
    <tr>
      <th>9</th>
      <td>75964</td>
      <td>0.015842</td>
    </tr>
    <tr>
      <th>10</th>
      <td>93555</td>
      <td>-0.007780</td>
    </tr>
    <tr>
      <th>11</th>
      <td>93561</td>
      <td>0.013911</td>
    </tr>
    <tr>
      <th>12</th>
      <td>92284</td>
      <td>-0.037318</td>
    </tr>
    <tr>
      <th>13</th>
      <td>75652</td>
      <td>0.011746</td>
    </tr>
    <tr>
      <th>14</th>
      <td>89433</td>
      <td>0.001798</td>
    </tr>
    <tr>
      <th>15</th>
      <td>93212</td>
      <td>-0.033118</td>
    </tr>
    <tr>
      <th>16</th>
      <td>90275</td>
      <td>-0.019490</td>
    </tr>
    <tr>
      <th>17</th>
      <td>93436</td>
      <td>-0.010830</td>
    </tr>
    <tr>
      <th>18</th>
      <td>96150</td>
      <td>0.019323</td>
    </tr>
  </tbody>
</table>
</div>




```python
census_pd_test_ttest.to_csv("./Resources/seismic_test_pop_ttest.csv")
```


```python

```
