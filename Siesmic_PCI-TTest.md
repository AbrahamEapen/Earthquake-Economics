

```python
import numpy as np
import pandas as pd
import scipy.stats as stats
import matplotlib.pyplot as plt
import math

file1 = './Resources/seismic_test_pop_ttest.csv'
seis_test_pop = pd.read_csv(file1)
seis_test_pop
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
      <th>Zip</th>
      <th>PCI Change</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>96130</td>
      <td>-0.033629</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>89406</td>
      <td>-0.016631</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>95501</td>
      <td>-0.062910</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>93117</td>
      <td>-0.028102</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>82520</td>
      <td>0.012283</td>
    </tr>
    <tr>
      <th>5</th>
      <td>5</td>
      <td>73013</td>
      <td>-0.020272</td>
    </tr>
    <tr>
      <th>6</th>
      <td>6</td>
      <td>95422</td>
      <td>0.018773</td>
    </tr>
    <tr>
      <th>7</th>
      <td>7</td>
      <td>73020</td>
      <td>0.037902</td>
    </tr>
    <tr>
      <th>8</th>
      <td>8</td>
      <td>95540</td>
      <td>-0.060953</td>
    </tr>
    <tr>
      <th>9</th>
      <td>9</td>
      <td>75964</td>
      <td>0.015842</td>
    </tr>
    <tr>
      <th>10</th>
      <td>10</td>
      <td>93555</td>
      <td>-0.007780</td>
    </tr>
    <tr>
      <th>11</th>
      <td>11</td>
      <td>93561</td>
      <td>0.013911</td>
    </tr>
    <tr>
      <th>12</th>
      <td>12</td>
      <td>92284</td>
      <td>-0.037318</td>
    </tr>
    <tr>
      <th>13</th>
      <td>13</td>
      <td>75652</td>
      <td>0.011746</td>
    </tr>
    <tr>
      <th>14</th>
      <td>14</td>
      <td>89433</td>
      <td>0.001798</td>
    </tr>
    <tr>
      <th>15</th>
      <td>15</td>
      <td>93212</td>
      <td>-0.033118</td>
    </tr>
    <tr>
      <th>16</th>
      <td>16</td>
      <td>90275</td>
      <td>-0.019490</td>
    </tr>
    <tr>
      <th>17</th>
      <td>17</td>
      <td>93436</td>
      <td>-0.010830</td>
    </tr>
    <tr>
      <th>18</th>
      <td>18</td>
      <td>96150</td>
      <td>0.019323</td>
    </tr>
  </tbody>
</table>
</div>




```python
file2 = './Resources/seismic_ctrl_pop_ttest.csv'
seis_ctrl_pop = pd.read_csv(file2)
seis_ctrl_pop
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
      <th>Zip</th>
      <th>PCI Change</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>9406</td>
      <td>29585</td>
      <td>-0.062074</td>
    </tr>
    <tr>
      <th>1</th>
      <td>27173</td>
      <td>78248</td>
      <td>0.007755</td>
    </tr>
    <tr>
      <th>2</th>
      <td>6844</td>
      <td>22315</td>
      <td>0.012051</td>
    </tr>
    <tr>
      <th>3</th>
      <td>16244</td>
      <td>49048</td>
      <td>-0.028741</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1048</td>
      <td>4072</td>
      <td>0.038603</td>
    </tr>
    <tr>
      <th>5</th>
      <td>6580</td>
      <td>21236</td>
      <td>0.001031</td>
    </tr>
    <tr>
      <th>6</th>
      <td>30056</td>
      <td>90049</td>
      <td>-0.045848</td>
    </tr>
    <tr>
      <th>7</th>
      <td>1069</td>
      <td>4105</td>
      <td>0.004361</td>
    </tr>
    <tr>
      <th>8</th>
      <td>27253</td>
      <td>78504</td>
      <td>-0.052511</td>
    </tr>
    <tr>
      <th>9</th>
      <td>6544</td>
      <td>21158</td>
      <td>0.031433</td>
    </tr>
    <tr>
      <th>10</th>
      <td>18152</td>
      <td>54501</td>
      <td>0.058915</td>
    </tr>
    <tr>
      <th>11</th>
      <td>27976</td>
      <td>80525</td>
      <td>0.010491</td>
    </tr>
    <tr>
      <th>12</th>
      <td>6678</td>
      <td>21704</td>
      <td>0.039551</td>
    </tr>
    <tr>
      <th>13</th>
      <td>29172</td>
      <td>85298</td>
      <td>0.006284</td>
    </tr>
    <tr>
      <th>14</th>
      <td>26958</td>
      <td>77662</td>
      <td>0.029920</td>
    </tr>
    <tr>
      <th>15</th>
      <td>1838</td>
      <td>6437</td>
      <td>0.033070</td>
    </tr>
    <tr>
      <th>16</th>
      <td>7228</td>
      <td>23601</td>
      <td>-0.031314</td>
    </tr>
    <tr>
      <th>17</th>
      <td>10164</td>
      <td>31605</td>
      <td>-0.024800</td>
    </tr>
    <tr>
      <th>18</th>
      <td>510</td>
      <td>2189</td>
      <td>-0.042112</td>
    </tr>
    <tr>
      <th>19</th>
      <td>1720</td>
      <td>6071</td>
      <td>-0.027464</td>
    </tr>
    <tr>
      <th>20</th>
      <td>16015</td>
      <td>48506</td>
      <td>0.035736</td>
    </tr>
    <tr>
      <th>21</th>
      <td>8407</td>
      <td>27263</td>
      <td>-0.029227</td>
    </tr>
    <tr>
      <th>22</th>
      <td>13161</td>
      <td>40505</td>
      <td>0.005054</td>
    </tr>
    <tr>
      <th>23</th>
      <td>30586</td>
      <td>92618</td>
      <td>0.005025</td>
    </tr>
    <tr>
      <th>24</th>
      <td>29333</td>
      <td>85748</td>
      <td>0.018742</td>
    </tr>
    <tr>
      <th>25</th>
      <td>14167</td>
      <td>44026</td>
      <td>0.016941</td>
    </tr>
    <tr>
      <th>26</th>
      <td>15260</td>
      <td>46806</td>
      <td>-0.039769</td>
    </tr>
    <tr>
      <th>27</th>
      <td>29701</td>
      <td>88007</td>
      <td>0.050732</td>
    </tr>
    <tr>
      <th>28</th>
      <td>24714</td>
      <td>71909</td>
      <td>0.035162</td>
    </tr>
    <tr>
      <th>29</th>
      <td>8954</td>
      <td>28539</td>
      <td>0.030660</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>970</th>
      <td>2013</td>
      <td>7063</td>
      <td>-0.056273</td>
    </tr>
    <tr>
      <th>971</th>
      <td>16839</td>
      <td>50208</td>
      <td>0.042233</td>
    </tr>
    <tr>
      <th>972</th>
      <td>12338</td>
      <td>38108</td>
      <td>0.071060</td>
    </tr>
    <tr>
      <th>973</th>
      <td>29174</td>
      <td>85302</td>
      <td>-0.076913</td>
    </tr>
    <tr>
      <th>974</th>
      <td>4484</td>
      <td>15238</td>
      <td>0.037528</td>
    </tr>
    <tr>
      <th>975</th>
      <td>7286</td>
      <td>23860</td>
      <td>0.024758</td>
    </tr>
    <tr>
      <th>976</th>
      <td>6246</td>
      <td>20110</td>
      <td>-0.000203</td>
    </tr>
    <tr>
      <th>977</th>
      <td>28806</td>
      <td>84040</td>
      <td>0.031373</td>
    </tr>
    <tr>
      <th>978</th>
      <td>8448</td>
      <td>27370</td>
      <td>-0.013803</td>
    </tr>
    <tr>
      <th>979</th>
      <td>12707</td>
      <td>39042</td>
      <td>0.016468</td>
    </tr>
    <tr>
      <th>980</th>
      <td>4431</td>
      <td>15122</td>
      <td>0.040496</td>
    </tr>
    <tr>
      <th>981</th>
      <td>27428</td>
      <td>78840</td>
      <td>0.018715</td>
    </tr>
    <tr>
      <th>982</th>
      <td>20551</td>
      <td>60194</td>
      <td>0.038446</td>
    </tr>
    <tr>
      <th>983</th>
      <td>9575</td>
      <td>30009</td>
      <td>-0.050570</td>
    </tr>
    <tr>
      <th>984</th>
      <td>31580</td>
      <td>95833</td>
      <td>0.017752</td>
    </tr>
    <tr>
      <th>985</th>
      <td>15223</td>
      <td>46750</td>
      <td>0.027461</td>
    </tr>
    <tr>
      <th>986</th>
      <td>9599</td>
      <td>30045</td>
      <td>-0.026055</td>
    </tr>
    <tr>
      <th>987</th>
      <td>11511</td>
      <td>35757</td>
      <td>-0.052732</td>
    </tr>
    <tr>
      <th>988</th>
      <td>30500</td>
      <td>92344</td>
      <td>-0.024223</td>
    </tr>
    <tr>
      <th>989</th>
      <td>17748</td>
      <td>53154</td>
      <td>0.013445</td>
    </tr>
    <tr>
      <th>990</th>
      <td>26760</td>
      <td>77327</td>
      <td>0.006491</td>
    </tr>
    <tr>
      <th>991</th>
      <td>30969</td>
      <td>94070</td>
      <td>-0.041032</td>
    </tr>
    <tr>
      <th>992</th>
      <td>6229</td>
      <td>20016</td>
      <td>-0.023795</td>
    </tr>
    <tr>
      <th>993</th>
      <td>16885</td>
      <td>50263</td>
      <td>0.014473</td>
    </tr>
    <tr>
      <th>994</th>
      <td>24162</td>
      <td>70126</td>
      <td>-0.036505</td>
    </tr>
    <tr>
      <th>995</th>
      <td>25983</td>
      <td>75216</td>
      <td>-0.063847</td>
    </tr>
    <tr>
      <th>996</th>
      <td>1865</td>
      <td>6482</td>
      <td>-0.016550</td>
    </tr>
    <tr>
      <th>997</th>
      <td>29975</td>
      <td>89460</td>
      <td>-0.044914</td>
    </tr>
    <tr>
      <th>998</th>
      <td>27963</td>
      <td>80503</td>
      <td>-0.000713</td>
    </tr>
    <tr>
      <th>999</th>
      <td>30859</td>
      <td>93619</td>
      <td>0.005996</td>
    </tr>
  </tbody>
</table>
<p>1000 rows × 3 columns</p>
</div>




```python
seis_test_pop["PCI Change"].mean()
```




    -0.010497626630977449




```python
seis_ctrl_pop["PCI Change"].mean()
```




    0.004651648006269979




```python
stats.ttest_1samp(a=seis_test_pop["PCI Change"], popmean=seis_ctrl_pop["PCI Change"].mean())
```




    Ttest_1sampResult(statistic=-2.3565664182867767, pvalue=0.029976357202402)




```python

```
