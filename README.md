# Pollution and academic performance

This is the README for Pollution and Academic Performance.

In order to replicate our analysis, please run all cells in this notebook.

The report is this notebook, with all outputs. There are also supplementary notebooks, showing the development of our project.

Our question is:

_Does the level of N02 pollution around schools in London affect the attendance and attainment of the pupils?_

Since the level of environmental pollution is at an increasing rate in the 21st century, we are keen to find if there is a correlation between the level of environmental pollution and the quality of educational performance. We selected the relevant data set of schools in London and extracted several related factors which are N02 (ug/m3 mean), attendance and average grade. We are going to compare the data to see if there is a correlation.

It is important to see the correlation between pollution and attendance. N02 has been proven to cause respiratory issues, such as asthma. Health issues will affect attendance and performance.

We did lots of research in this field, and we have discovered this has never been studied in the UK before. This was a further benefit of choosing to study this, we could contribute new information to the scientific community.


```python
import numpy as np
import matplotlib.pyplot as plt
%matplotlib inline

# Fancy plots
plt.style.use('fivethirtyeight')

# Data frame library
import pandas as pd
```

Numpy is a python library. As we imported it, we added supports for multi-dimensional and large matrices and arrays, along with large collections of high level of mathematical functions to operate on these arrays.

We used matplotlib which is a plotting library as it allows us to produce 2D graphics in various interactive environments.
We used panda, which is a software library for python, it enables us to analyse and manipulate the data.

# Literature review about 'Pollution and its effect on school absense'

1. In Janet Currie and her team-mates’ study and their literature about ‘Does pollution increase school absences’, they adopted the Texas Schools project which is a longitudinal administrative dataset of Texas’ student absenteeism. Then they aggregated the data of pollution from the Texas commission on quality of environment into 6week time blocks to combined with the administrative data of absenteeism. And they assumed that each person in different regions react similarly to the weather changes regardless of the implications of their actions for general air quality.  This assumption allows them to make relevant behavior changes uncorrelated with idiosyncratic changes in the quality of air so that they could put their focus on how the absences relate to the level of pollution. Through a set of experiments they did, they concluded that although they found a positive association between absences and level of pollution, they did not find out a monotonically increasing relationship between these two factors (Currie et al., 2009).




- Reference: Currie, J., Hanushek, E., Kahn, E., Neidell, M. and Rivkin, S. (2009). Does Pollution Increase School Absences?. Review of Economics and Statistics, [online] 91(4), pp.682-694. Available at: https://www.jstor.org/stable/25651370?seq=9#metadata_info_tab_contents [Accessed 17 Mar. 2019].



# Pollution and its effect on school absence

This project is using a data set from this links:
- https://data.gov.uk/dataset/90b02236-3a4e-4899-8bdc-7f5122df4d9e/schools-and-educational-institutions-air-quality-exposure-data-broken-down-by-parliamentary-constituency
- https://www.compare-school-performance.service.gov.uk/download-data?currentstep=datatypes&regiontype=all&la=0&downloadYear=2012-2013&datatypes=spine&datatypes=ks2&datatypes=ks4&datatypes=ks4underlying&datatypes=ks5&datatypes=pupilabsence&datatypes=Workforce&datatypes=spendperpupil&datatypes=spendperpupilgrouped

We found this by looking at the open source data on data.gov 

Now we load our data and display the columns in order to drop those not needed.


```python
schoolpolutiondata = pd.read_csv('school polution.csv')
schoolpolutiondata.columns
```




    Index(['URN', 'Local Authority name', 'Establisment name', 'Easting',
           'Northing', 'Type of Establishment', 'Phase of education',
           'NO2ug/m3 mean 2013', 'Above limit', 'Parliamentary Constituences',
           'Establishment Status', 'Close Date', 'Unnamed: 12'],
          dtype='object')




```python
schoolpolutiondata = schoolpolutiondata.drop(['Local Authority name', 'Easting', 'Northing', 'Parliamentary Constituences', 'Close Date', 'Unnamed: 12'], axis=1)
```

Reference for the drop function: https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.drop.html


```python
primary_data = schoolpolutiondata[schoolpolutiondata['Phase of education'] == 'Primary']
primary_data.head()
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
      <th>URN</th>
      <th>Establisment name</th>
      <th>Type of Establishment</th>
      <th>Phase of education</th>
      <th>NO2ug/m3 mean 2013</th>
      <th>Above limit</th>
      <th>Establishment Status</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>4</th>
      <td>101136</td>
      <td>St Mary's Bryanston Square CofE School</td>
      <td>Voluntary Aided School</td>
      <td>Primary</td>
      <td>67.0</td>
      <td>Yes</td>
      <td>Open</td>
    </tr>
    <tr>
      <th>6</th>
      <td>100351</td>
      <td>St Paul's CofE Primary School</td>
      <td>Voluntary Aided School</td>
      <td>Primary</td>
      <td>65.2</td>
      <td>Yes</td>
      <td>Open</td>
    </tr>
    <tr>
      <th>7</th>
      <td>101127</td>
      <td>St Clement Danes CofE Primary School</td>
      <td>Voluntary Aided School</td>
      <td>Primary</td>
      <td>65.0</td>
      <td>Yes</td>
      <td>Open</td>
    </tr>
    <tr>
      <th>10</th>
      <td>101140</td>
      <td>St Peter's Eaton Square CofE Primary School</td>
      <td>Voluntary Aided School</td>
      <td>Primary</td>
      <td>64.3</td>
      <td>Yes</td>
      <td>Open</td>
    </tr>
    <tr>
      <th>11</th>
      <td>100828</td>
      <td>St George's Cathedral Catholic Primary School</td>
      <td>Voluntary Aided School</td>
      <td>Primary</td>
      <td>64.3</td>
      <td>Yes</td>
      <td>Open</td>
    </tr>
  </tbody>
</table>
</div>




```python
Absences = pd.read_csv('england_abs.csv')
Absences.head()
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
      <th>LA</th>
      <th>ESTAB</th>
      <th>URN</th>
      <th>PERCTOT</th>
      <th>PPERSABS15</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>201</td>
      <td>3614</td>
      <td>100000.0</td>
      <td>3.5</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>202</td>
      <td>2000</td>
      <td>136807.0</td>
      <td>5.4</td>
      <td>SUPP</td>
    </tr>
    <tr>
      <th>2</th>
      <td>202</td>
      <td>2019</td>
      <td>100008.0</td>
      <td>5.4</td>
      <td>5.8</td>
    </tr>
    <tr>
      <th>3</th>
      <td>202</td>
      <td>2036</td>
      <td>100009.0</td>
      <td>4.1</td>
      <td>2.7</td>
    </tr>
    <tr>
      <th>4</th>
      <td>202</td>
      <td>2065</td>
      <td>100010.0</td>
      <td>5.6</td>
      <td>5.3</td>
    </tr>
  </tbody>
</table>
</div>



- This code is to make pandas open and read the csv file. 


```python
Absences_polution = pd.merge(Absences, primary_data, on='URN')
Absences_polution.head()
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
      <th>LA</th>
      <th>ESTAB</th>
      <th>URN</th>
      <th>PERCTOT</th>
      <th>PPERSABS15</th>
      <th>Establisment name</th>
      <th>Type of Establishment</th>
      <th>Phase of education</th>
      <th>NO2ug/m3 mean 2013</th>
      <th>Above limit</th>
      <th>Establishment Status</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>201</td>
      <td>3614</td>
      <td>100000.0</td>
      <td>3.5</td>
      <td>0</td>
      <td>Sir John Cass's Foundation Primary School</td>
      <td>Voluntary Aided School</td>
      <td>Primary</td>
      <td>62.6</td>
      <td>Yes</td>
      <td>Open</td>
    </tr>
    <tr>
      <th>1</th>
      <td>202</td>
      <td>2000</td>
      <td>136807.0</td>
      <td>5.4</td>
      <td>SUPP</td>
      <td>St Luke's Church of England Primary</td>
      <td>Free Schools</td>
      <td>Primary</td>
      <td>43.4</td>
      <td>Yes</td>
      <td>Open</td>
    </tr>
    <tr>
      <th>2</th>
      <td>202</td>
      <td>2019</td>
      <td>100008.0</td>
      <td>5.4</td>
      <td>5.8</td>
      <td>Argyle Primary School</td>
      <td>Community School</td>
      <td>Primary</td>
      <td>58.8</td>
      <td>Yes</td>
      <td>Open</td>
    </tr>
    <tr>
      <th>3</th>
      <td>202</td>
      <td>2036</td>
      <td>100009.0</td>
      <td>4.1</td>
      <td>2.7</td>
      <td>Beckford Primary School</td>
      <td>Community School</td>
      <td>Primary</td>
      <td>39.4</td>
      <td>No</td>
      <td>Open</td>
    </tr>
    <tr>
      <th>4</th>
      <td>202</td>
      <td>2065</td>
      <td>100010.0</td>
      <td>5.6</td>
      <td>5.3</td>
      <td>Brecknock Primary School</td>
      <td>Community School</td>
      <td>Primary</td>
      <td>43.9</td>
      <td>Yes</td>
      <td>Open</td>
    </tr>
  </tbody>
</table>
</div>



We merged the "Primary_data" which is a column of data extracted from "school pollution" data frame with the different data frame named as "Absences" by using the coding that we searched from internet sources

Reference: (https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.merge.html)

We made a data frame with only primary schools, this was because we decided to look at primary schools first, as there was an abundance of data to use. Once we had the programme written to analyze primary schools, it would be easy to adapt it for other ages.

We will define two functions needed for cleaning up the data, we chose to define functions in order to boost reusability:

How it works:
I found a function that checks if a value can be converted into a float and put it in the cell below.
Source: Eric Leschinski: https://stackoverflow.com/questions/736043/checking-if-a-string-can-be-converted-to-float-in-python

The way it does that is: it attempts to convert the variable to float by using the function float(). We expect to have the following non float values: NaN, NA and SUPP. NaN will pass throu the function and will be verified directly. The other non float values will raise "ValueError". This error is accounted for in the function and makes the function return "False".


```python
def isfloat(value):
  try:
    float(value)      #try to convert to float
    return True
  except ValueError:  #it's a word so it can't convert to float
    return False
```

Small tests:


```python
print(isfloat('12.98'))
print(isfloat('SUPP'))
print(isfloat('NaN'))
```

    True
    False
    True
    

Observe how NaN passes throu the function. This means that it will have to be taken care of seperatly.

What the function below does:
- we make a bool array(clean) of length equal to that of the column we want to sort (the array will have True at the positions of numbers and False at the positions of words and NaN values)
- we use the array to sort the data frame
- transform the strings to floats


```python
def all_floats (data_frame, colomn):
    '''
    This function returns a data frame where the original data frame's specified colomn is converted to float. Note: the function does not support any float that can not be transformed into a float directly by float().
    Parameters:
    data_frame - data frame of the column in question
    column - string - name of colomn
    Returns:
    data frame where all the values in column are floats
    '''
    clean = np.zeros(len(data_frame[colomn]),dtype=bool)
    cnt = 0
    delete = 0
    for row in data_frame[colomn]:
        if row is not np.nan and isfloat(row) == True:
            clean[cnt] = True
        else:
            clean[cnt] = False
        cnt += 1
    Clean_data_frame = data_frame[clean] #This delets all non numbers
    Clean_data_frame[colomn] = Clean_data_frame[colomn].astype(np.float)
    return Clean_data_frame
```

The data for short-term absences is stored in the colomn "PERCTOT".


```python
Absences_polution.plot.scatter('PERCTOT', 'NO2ug/m3 mean 2013', title = 'Pollution-Absences relationship', )
plt.xlabel("Absences")
plt.ylabel("Concentration of NO2(ug/m3)")
```




    Text(0, 0.5, 'Concentration of NO2(ug/m3)')




    
![png](output_30_1.png)
    



```python
#Here we split the table in Above- institutions above the polution limit at the time and Under- institutions below the polution limit at the time
Above = Absences_polution.loc[Absences_polution['Above limit']=='Yes']
Under = Absences_polution.loc[Absences_polution['Above limit']=='No']
```


```python
np.median(Absences_polution['PERCTOT'])
```




    4.6




```python
np.median(Above['PERCTOT'])
```




    4.7




```python
np.median(Under['PERCTOT'])
```




    4.6



We used plot.scatter to produce a scatter graph for two variables which are 'level of NO2' and 'Percentage of absences'

Also, we calculated the median for the:
- total absences 4.6
- absences of institutions above the pollution limit 4.7
- absences of institutions under the pollution limit 4.6

As the graph shows, the level of NO2 and the percentage of absences do not have a significant correlation as most of the points are allocated on the similar area instead of locating as a linear regression line.

Also, the medians have very close values.

We conclude that pollution and absences are not strongly correlated.

We moved on to prolonged absences.

The data for prolonged absences is stored in the column "PPERSABS15".


```python
Absences_polution = all_floats(Absences_polution, 'PPERSABS15')
Absences_polution.head()
```

    <ipython-input-9-fe5454955d23>:20: DeprecationWarning: `np.float` is a deprecated alias for the builtin `float`. To silence this warning, use `float` by itself. Doing this will not modify any behavior and is safe. If you specifically wanted the numpy scalar type, use `np.float64` here.
    Deprecated in NumPy 1.20; for more details and guidance: https://numpy.org/devdocs/release/1.20.0-notes.html#deprecations
      Clean_data_frame[colomn] = Clean_data_frame[colomn].astype(np.float)
    <ipython-input-9-fe5454955d23>:20: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      Clean_data_frame[colomn] = Clean_data_frame[colomn].astype(np.float)
    




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
      <th>LA</th>
      <th>ESTAB</th>
      <th>URN</th>
      <th>PERCTOT</th>
      <th>PPERSABS15</th>
      <th>Establisment name</th>
      <th>Type of Establishment</th>
      <th>Phase of education</th>
      <th>NO2ug/m3 mean 2013</th>
      <th>Above limit</th>
      <th>Establishment Status</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>201</td>
      <td>3614</td>
      <td>100000.0</td>
      <td>3.5</td>
      <td>0.0</td>
      <td>Sir John Cass's Foundation Primary School</td>
      <td>Voluntary Aided School</td>
      <td>Primary</td>
      <td>62.6</td>
      <td>Yes</td>
      <td>Open</td>
    </tr>
    <tr>
      <th>2</th>
      <td>202</td>
      <td>2019</td>
      <td>100008.0</td>
      <td>5.4</td>
      <td>5.8</td>
      <td>Argyle Primary School</td>
      <td>Community School</td>
      <td>Primary</td>
      <td>58.8</td>
      <td>Yes</td>
      <td>Open</td>
    </tr>
    <tr>
      <th>3</th>
      <td>202</td>
      <td>2036</td>
      <td>100009.0</td>
      <td>4.1</td>
      <td>2.7</td>
      <td>Beckford Primary School</td>
      <td>Community School</td>
      <td>Primary</td>
      <td>39.4</td>
      <td>No</td>
      <td>Open</td>
    </tr>
    <tr>
      <th>4</th>
      <td>202</td>
      <td>2065</td>
      <td>100010.0</td>
      <td>5.6</td>
      <td>5.3</td>
      <td>Brecknock Primary School</td>
      <td>Community School</td>
      <td>Primary</td>
      <td>43.9</td>
      <td>Yes</td>
      <td>Open</td>
    </tr>
    <tr>
      <th>5</th>
      <td>202</td>
      <td>2078</td>
      <td>100011.0</td>
      <td>5.4</td>
      <td>4.1</td>
      <td>Brookfield Primary School</td>
      <td>Community School</td>
      <td>Primary</td>
      <td>35.3</td>
      <td>No</td>
      <td>Open</td>
    </tr>
  </tbody>
</table>
</div>




```python
Absences_polution.plot.scatter('PPERSABS15', 'NO2ug/m3 mean 2013', title = 'Pollution-Extended Absences relationship')
plt.xlabel("Extended Absences")
plt.ylabel("Concentration of NO2(ug/m3)")
```




    Text(0, 0.5, 'Concentration of NO2(ug/m3)')




    
![png](output_40_1.png)
    



```python
Above = Absences_polution.loc[Absences_polution['Above limit']=='Yes']
Under = Absences_polution.loc[Absences_polution['Above limit']=='No']
```


```python
PPERSABS15_Above_mean = np.mean(Above['PPERSABS15'])
PPERSABS15_Above_mean
```




    3.8624573378839577




```python
PPERSABS15_Under_mean = np.mean(Under['PPERSABS15'])
PPERSABS15_Under_mean
```




    3.6452461799660476




```python
PPERSABS15_General_mean = np.mean(Absences_polution['PPERSABS15'])
PPERSABS15_General_mean
```




    3.688511216859283



Previously, we have 3 codings which are:

 1. Absences = pd.read_csv('england_abs.csv')
 2. Absences_polution=pd.merge(Absences, primary_data, on='URN')
 3. Absences_polution = Absences_polution.drop (['Unnamed: 12', 'Close Date', 'PPERSABS15'], axis=1)

In the process of tidying up the coding, we found out these 3 coding has been run before which means they are three extra codings that can be avoided. Therefore, we decided to eliminate these 3 codings to make our project more clearer and understandable.


# Literature review about 'Pollution and its effect on academic performance'

- In Miller and his team-mates’ study and their literature about ‘The Effects of air pollution on educational outcomes: Evidence from Chile’, they adopted the school-level pollution data from 1997 to 2012 that is reported and published by the SINCA (Sistema de Information Nacional de Calidad del Aire) of the environment ministry. For the school performance data, they used the test scores generated by the SIMCE (Sistema de Medicion de la Calidad de la Educacion) test that is undertaken by the Education Ministry for all schools in Chile. Then they made a assumption that unnoticed time-varying variables would not have a impact on both test scores and pollution. Through a set of experiments and analysis, they concluded that they found out some regressions that indicated a strong negative association between pollution and school performance (Miller and Vela, 2013)



- Reference: Miller, S. and Vela, M. (2013). The Effects of Air Pollution on Educational Outcomes: Evidence from Chile. SSRN Electronic Journal. [online] Available at: https://publications.iadb.org/en/publication/11349/effects-air-pollution-educational-outcomes-evidence-chile [Accessed 17 Mar. 2019].



# Pollution and its effect on academic performance


```python
performance_data = pd.read_csv('england_ks2final_primary.csv')
```

    D:\Anaconda_new\lib\site-packages\IPython\core\interactiveshell.py:3165: DtypeWarning: Columns (44,270,272,274) have mixed types.Specify dtype option on import or set low_memory=False.
      has_raised = await self.run_ast_nodes(code_ast.body, cell_name,
    


```python
performance_data.columns
```




    Index(['RECTYPE', 'ALPHAIND', 'LEA', 'ESTAB', 'URN', 'SCHNAME', 'ADDRESS1',
           'ADDRESS2', 'ADDRESS3', 'TOWN',
           ...
           'GAP_2YR_PRWRIT', 'GAP_3YR_PRMAT', 'GAPN_3YR_RWMX_FSMCLA',
           'GAPN_2YR_PRREAD_FSMCLA', 'GAPN_2YR_PRWRIT_FSMCLA',
           'GAPN_3YR_PRMAT_FSMCLA', 'GAPN_3YR_RWMX_NOTFSMCLA',
           'GAPN_2YR_PRREAD_NOTFSMCLA', 'GAPN_2YR_PRWRIT_NOTFSMCLA',
           'GAPN_3YR_PRMAT_NOTFSMCLA'],
          dtype='object', length=300)



There are many columns we don't need. So, we will cut the unnecessary columns.


```python
performance_data = performance_data.drop(['RECTYPE','PCODE','TELNUM','TKS1EXP_L','PKS1EXP_L', 'TKS1EXP_M', 'PKS1EXP_M', 'TKS1EXP_H', 'PKS1EXP_H', 'TFSMCLA', 'PTFSMCLA', 'PT2MATH12', 'PT2READ12', 'PT2WRITTA12', 'AVGLEVEL', 'PT2MATH12_B', 'PT2MATH12_G', 'PT2MATH12_L', 'PT2MATH12_M', 'PT2MATH12_H', 'PTREADX', 'PTREADAT', 'GAPN_3YR_RWMX_FSMCLA', 'GAPN_2YR_PRREAD_NOTFSMCLA', 'GAPN_2YR_PRWRIT_NOTFSMCLA', 'GAPN_3YR_PRMAT_NOTFSMCLA'],axis=1)
```

During codeing review we found this code to be inefficient:

- performance_abs_pol = pd.merge(Absences_polution, performance_data, on='URN')


```python
performance_pol = pd.merge(schoolpolutiondata, performance_data, on='URN')
```


```python
Clean_perf_pol = all_floats (performance_pol, 'TAPS')
```

    <ipython-input-9-fe5454955d23>:20: DeprecationWarning: `np.float` is a deprecated alias for the builtin `float`. To silence this warning, use `float` by itself. Doing this will not modify any behavior and is safe. If you specifically wanted the numpy scalar type, use `np.float64` here.
    Deprecated in NumPy 1.20; for more details and guidance: https://numpy.org/devdocs/release/1.20.0-notes.html#deprecations
      Clean_data_frame[colomn] = Clean_data_frame[colomn].astype(np.float)
    <ipython-input-9-fe5454955d23>:20: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      Clean_data_frame[colomn] = Clean_data_frame[colomn].astype(np.float)
    

Now we will calculate the median values of the performance of institutions above and below the accepted pollution limit.


```python
Above = Clean_perf_pol[Clean_perf_pol['Above limit']=='Yes']
Under = Clean_perf_pol[Clean_perf_pol['Above limit']=='No']
```


```python
print(np.median(Clean_perf_pol['TAPS']))
print(np.median(Above['TAPS']))
print(np.median(Under['TAPS']))
```

    29.0
    28.7
    29.1
    


```python
Clean_perf_pol.plot.scatter('TAPS', 'NO2ug/m3 mean 2013',title = 'Pollution-Academic Performance relationship')
plt.xlabel("Academic Performance")
plt.ylabel("Concentration of NO2(ug/m3)")
```




    Text(0, 0.5, 'Concentration of NO2(ug/m3)')




    
![png](output_60_1.png)
    


Those points that are loacated at the left hand side of the scatter graph seems odd. Therefore, we want to figure out if these schools are special schools, such as school for disabled students,  as this would explain this unusual distribution.


```python
Taps_less_than_20= Clean_perf_pol['TAPS'] < 20
```


```python
np.count_nonzero(Taps_less_than_20)
```




    37




```python
Clean_perf_pol[Taps_less_than_20]
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
      <th>URN</th>
      <th>Establisment name</th>
      <th>Type of Establishment</th>
      <th>Phase of education</th>
      <th>NO2ug/m3 mean 2013</th>
      <th>Above limit</th>
      <th>Establishment Status</th>
      <th>ALPHAIND</th>
      <th>LEA</th>
      <th>ESTAB</th>
      <th>...</th>
      <th>PREADWRITTAMATX_3YR_FSMCLA</th>
      <th>PREADWRITTAMATX_3YR_NOTFSMCLA</th>
      <th>GAP_3YR_RWMX</th>
      <th>GAP_2YR_PRREAD</th>
      <th>GAP_2YR_PRWRIT</th>
      <th>GAP_3YR_PRMAT</th>
      <th>GAPN_2YR_PRREAD_FSMCLA</th>
      <th>GAPN_2YR_PRWRIT_FSMCLA</th>
      <th>GAPN_3YR_PRMAT_FSMCLA</th>
      <th>GAPN_3YR_RWMX_NOTFSMCLA</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>67</th>
      <td>131023</td>
      <td>Stephen Hawking School</td>
      <td>Community Special School</td>
      <td>Not applicable</td>
      <td>49.0</td>
      <td>Yes</td>
      <td>Open</td>
      <td>51988.0</td>
      <td>211.0</td>
      <td>7169.0</td>
      <td>...</td>
      <td>SUPP</td>
      <td>SUPP</td>
      <td>SUPP</td>
      <td>SUPP</td>
      <td>SUPP</td>
      <td>SUPP</td>
      <td>SUPP</td>
      <td>SUPP</td>
      <td>SUPP</td>
      <td>SUPP</td>
    </tr>
    <tr>
      <th>149</th>
      <td>100378</td>
      <td>Queensmill School</td>
      <td>Community Special School</td>
      <td>Not applicable</td>
      <td>45.2</td>
      <td>Yes</td>
      <td>Open</td>
      <td>39148.0</td>
      <td>205.0</td>
      <td>7014.0</td>
      <td>...</td>
      <td>0%</td>
      <td>0%</td>
      <td>0</td>
      <td>SUPP</td>
      <td>SUPP</td>
      <td>8</td>
      <td>SUPP</td>
      <td>SUPP</td>
      <td>-62</td>
      <td>-77</td>
    </tr>
    <tr>
      <th>202</th>
      <td>100096</td>
      <td>Swiss Cottage School - Development &amp; Research ...</td>
      <td>Community Special School</td>
      <td>Not applicable</td>
      <td>44.2</td>
      <td>Yes</td>
      <td>Open</td>
      <td>53064.0</td>
      <td>202.0</td>
      <td>7205.0</td>
      <td>...</td>
      <td>0%</td>
      <td>0%</td>
      <td>0</td>
      <td>5</td>
      <td>0</td>
      <td>18</td>
      <td>-66</td>
      <td>-74</td>
      <td>-53</td>
      <td>-77</td>
    </tr>
    <tr>
      <th>204</th>
      <td>100987</td>
      <td>Phoenix School</td>
      <td>Community Special School</td>
      <td>Not applicable</td>
      <td>44.1</td>
      <td>Yes</td>
      <td>Open</td>
      <td>37776.0</td>
      <td>211.0</td>
      <td>7095.0</td>
      <td>...</td>
      <td>0%</td>
      <td>0%</td>
      <td>0</td>
      <td>SUPP</td>
      <td>SUPP</td>
      <td>0</td>
      <td>SUPP</td>
      <td>SUPP</td>
      <td>-81</td>
      <td>-77</td>
    </tr>
    <tr>
      <th>208</th>
      <td>100469</td>
      <td>Samuel Rhodes MLD School</td>
      <td>Community Special School</td>
      <td>Not applicable</td>
      <td>43.9</td>
      <td>Yes</td>
      <td>Open</td>
      <td>48750.0</td>
      <td>206.0</td>
      <td>7146.0</td>
      <td>...</td>
      <td>SUPP</td>
      <td>SUPP</td>
      <td>SUPP</td>
      <td>SUPP</td>
      <td>SUPP</td>
      <td>SUPP</td>
      <td>SUPP</td>
      <td>SUPP</td>
      <td>SUPP</td>
      <td>SUPP</td>
    </tr>
    <tr>
      <th>229</th>
      <td>100881</td>
      <td>Cherry Garden School</td>
      <td>Community Special School</td>
      <td>Not applicable</td>
      <td>43.4</td>
      <td>Yes</td>
      <td>Open</td>
      <td>9960.0</td>
      <td>210.0</td>
      <td>7186.0</td>
      <td>...</td>
      <td>0%</td>
      <td>0%</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>-85</td>
      <td>-88</td>
      <td>-81</td>
      <td>-77</td>
    </tr>
    <tr>
      <th>277</th>
      <td>100467</td>
      <td>Richard Cloudesley PH School</td>
      <td>Community Special School</td>
      <td>Not applicable</td>
      <td>42.1</td>
      <td>Yes</td>
      <td>Open</td>
      <td>40134.0</td>
      <td>206.0</td>
      <td>7030.0</td>
      <td>...</td>
      <td>SUPP</td>
      <td>SUPP</td>
      <td>SUPP</td>
      <td>SUPP</td>
      <td>SUPP</td>
      <td>SUPP</td>
      <td>SUPP</td>
      <td>SUPP</td>
      <td>SUPP</td>
      <td>SUPP</td>
    </tr>
    <tr>
      <th>305</th>
      <td>134030</td>
      <td>The Bridge School</td>
      <td>Community Special School</td>
      <td>Not applicable</td>
      <td>41.7</td>
      <td>Yes</td>
      <td>Open</td>
      <td>6654.0</td>
      <td>206.0</td>
      <td>7031.0</td>
      <td>...</td>
      <td>0%</td>
      <td>0%</td>
      <td>0</td>
      <td>SUPP</td>
      <td>SUPP</td>
      <td>-11</td>
      <td>SUPP</td>
      <td>SUPP</td>
      <td>-81</td>
      <td>-77</td>
    </tr>
    <tr>
      <th>598</th>
      <td>100878</td>
      <td>Haymerle School</td>
      <td>Community Special School</td>
      <td>Not applicable</td>
      <td>37.9</td>
      <td>No</td>
      <td>Open</td>
      <td>21650.0</td>
      <td>210.0</td>
      <td>7126.0</td>
      <td>...</td>
      <td>0%</td>
      <td>0%</td>
      <td>0</td>
      <td>SUPP</td>
      <td>SUPP</td>
      <td>-2</td>
      <td>SUPP</td>
      <td>SUPP</td>
      <td>-66</td>
      <td>-77</td>
    </tr>
    <tr>
      <th>599</th>
      <td>100643</td>
      <td>Turney Primary and Secondary Special School</td>
      <td>Foundation Special School</td>
      <td>Not applicable</td>
      <td>37.9</td>
      <td>No</td>
      <td>Open</td>
      <td>54900.0</td>
      <td>208.0</td>
      <td>5950.0</td>
      <td>...</td>
      <td>SUPP</td>
      <td>SUPP</td>
      <td>SUPP</td>
      <td>SUPP</td>
      <td>SUPP</td>
      <td>SUPP</td>
      <td>SUPP</td>
      <td>SUPP</td>
      <td>SUPP</td>
      <td>SUPP</td>
    </tr>
    <tr>
      <th>657</th>
      <td>101582</td>
      <td>Manor School</td>
      <td>Community Special School</td>
      <td>Not applicable</td>
      <td>37.3</td>
      <td>No</td>
      <td>Closed</td>
      <td>30398.0</td>
      <td>304.0</td>
      <td>7006.0</td>
      <td>...</td>
      <td>0%</td>
      <td>0%</td>
      <td>0</td>
      <td>11</td>
      <td>0</td>
      <td>0</td>
      <td>-74</td>
      <td>-88</td>
      <td>-81</td>
      <td>-77</td>
    </tr>
    <tr>
      <th>679</th>
      <td>133440</td>
      <td>The Livity School</td>
      <td>Community Special School</td>
      <td>Not applicable</td>
      <td>37.1</td>
      <td>No</td>
      <td>Open</td>
      <td>29046.0</td>
      <td>208.0</td>
      <td>7194.0</td>
      <td>...</td>
      <td>SUPP</td>
      <td>SUPP</td>
      <td>SUPP</td>
      <td>SUPP</td>
      <td>SUPP</td>
      <td>SUPP</td>
      <td>SUPP</td>
      <td>SUPP</td>
      <td>SUPP</td>
      <td>SUPP</td>
    </tr>
    <tr>
      <th>694</th>
      <td>101102</td>
      <td>Paddock School</td>
      <td>Community Special School</td>
      <td>Not applicable</td>
      <td>36.9</td>
      <td>No</td>
      <td>Open</td>
      <td>36594.0</td>
      <td>212.0</td>
      <td>7183.0</td>
      <td>...</td>
      <td>0%</td>
      <td>0%</td>
      <td>0</td>
      <td>SUPP</td>
      <td>SUPP</td>
      <td>0</td>
      <td>SUPP</td>
      <td>SUPP</td>
      <td>-81</td>
      <td>-77</td>
    </tr>
    <tr>
      <th>698</th>
      <td>100760</td>
      <td>Brent Knoll School</td>
      <td>Foundation Special School</td>
      <td>Not applicable</td>
      <td>36.9</td>
      <td>No</td>
      <td>Open</td>
      <td>6516.0</td>
      <td>209.0</td>
      <td>7038.0</td>
      <td>...</td>
      <td>0%</td>
      <td>0%</td>
      <td>0</td>
      <td>-14</td>
      <td>25</td>
      <td>-2</td>
      <td>-66</td>
      <td>-63</td>
      <td>-58</td>
      <td>-77</td>
    </tr>
    <tr>
      <th>836</th>
      <td>102554</td>
      <td>Marjory Kinnon School</td>
      <td>Community Special School</td>
      <td>Not applicable</td>
      <td>35.4</td>
      <td>No</td>
      <td>Open</td>
      <td>30622.0</td>
      <td>313.0</td>
      <td>7005.0</td>
      <td>...</td>
      <td>0%</td>
      <td>0%</td>
      <td>0</td>
      <td>11</td>
      <td>54</td>
      <td>14</td>
      <td>-60</td>
      <td>-13</td>
      <td>-45</td>
      <td>-77</td>
    </tr>
    <tr>
      <th>880</th>
      <td>130958</td>
      <td>Russet House School</td>
      <td>Community Special School</td>
      <td>Not applicable</td>
      <td>34.8</td>
      <td>No</td>
      <td>Open</td>
      <td>41496.0</td>
      <td>308.0</td>
      <td>7008.0</td>
      <td>...</td>
      <td>0%</td>
      <td>0%</td>
      <td>0</td>
      <td>SUPP</td>
      <td>SUPP</td>
      <td>-14</td>
      <td>SUPP</td>
      <td>SUPP</td>
      <td>-68</td>
      <td>-77</td>
    </tr>
    <tr>
      <th>911</th>
      <td>100766</td>
      <td>Watergate School</td>
      <td>Foundation Special School</td>
      <td>Not applicable</td>
      <td>34.4</td>
      <td>No</td>
      <td>Open</td>
      <td>56264.0</td>
      <td>209.0</td>
      <td>7182.0</td>
      <td>...</td>
      <td>0%</td>
      <td>0%</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>-85</td>
      <td>-88</td>
      <td>-81</td>
      <td>-77</td>
    </tr>
    <tr>
      <th>915</th>
      <td>102558</td>
      <td>The Cedars Primary School</td>
      <td>Community Special School</td>
      <td>Not applicable</td>
      <td>34.4</td>
      <td>No</td>
      <td>Open</td>
      <td>9302.0</td>
      <td>313.0</td>
      <td>7010.0</td>
      <td>...</td>
      <td>0%</td>
      <td>0%</td>
      <td>0</td>
      <td>SUPP</td>
      <td>SUPP</td>
      <td>22</td>
      <td>SUPP</td>
      <td>SUPP</td>
      <td>-46</td>
      <td>-77</td>
    </tr>
    <tr>
      <th>1012</th>
      <td>101968</td>
      <td>Mandeville School</td>
      <td>Community Special School</td>
      <td>Not applicable</td>
      <td>33.5</td>
      <td>No</td>
      <td>Open</td>
      <td>30232.0</td>
      <td>307.0</td>
      <td>7010.0</td>
      <td>...</td>
      <td>0%</td>
      <td>0%</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>-85</td>
      <td>-88</td>
      <td>-81</td>
      <td>-77</td>
    </tr>
    <tr>
      <th>1040</th>
      <td>101093</td>
      <td>Linden Lodge School</td>
      <td>Community Special School</td>
      <td>Not applicable</td>
      <td>33.2</td>
      <td>No</td>
      <td>Open</td>
      <td>28656.0</td>
      <td>212.0</td>
      <td>7067.0</td>
      <td>...</td>
      <td>0%</td>
      <td>0%</td>
      <td>0</td>
      <td>25</td>
      <td>13</td>
      <td>8</td>
      <td>-60</td>
      <td>-75</td>
      <td>-73</td>
      <td>-77</td>
    </tr>
    <tr>
      <th>1050</th>
      <td>102176</td>
      <td>Vale School</td>
      <td>Community Special School</td>
      <td>Not applicable</td>
      <td>33.1</td>
      <td>No</td>
      <td>Open</td>
      <td>55382.0</td>
      <td>309.0</td>
      <td>7001.0</td>
      <td>...</td>
      <td>0%</td>
      <td>0%</td>
      <td>0</td>
      <td>SUPP</td>
      <td>SUPP</td>
      <td>-25</td>
      <td>SUPP</td>
      <td>SUPP</td>
      <td>-81</td>
      <td>-77</td>
    </tr>
    <tr>
      <th>1061</th>
      <td>102177</td>
      <td>The Brook School</td>
      <td>Community Special School</td>
      <td>Not applicable</td>
      <td>33.0</td>
      <td>No</td>
      <td>Open</td>
      <td>7270.0</td>
      <td>309.0</td>
      <td>7005.0</td>
      <td>...</td>
      <td>0%</td>
      <td>0%</td>
      <td>0</td>
      <td>8</td>
      <td>0</td>
      <td>0</td>
      <td>-77</td>
      <td>-88</td>
      <td>-81</td>
      <td>-77</td>
    </tr>
    <tr>
      <th>1168</th>
      <td>101966</td>
      <td>Castlebar School</td>
      <td>Community Special School</td>
      <td>Not applicable</td>
      <td>32.2</td>
      <td>No</td>
      <td>Open</td>
      <td>9016.0</td>
      <td>307.0</td>
      <td>7007.0</td>
      <td>...</td>
      <td>0%</td>
      <td>0%</td>
      <td>0</td>
      <td>-19</td>
      <td>-13</td>
      <td>-11</td>
      <td>-85</td>
      <td>-88</td>
      <td>-48</td>
      <td>-77</td>
    </tr>
    <tr>
      <th>1193</th>
      <td>136423</td>
      <td>Drumbeat School and ASD Service</td>
      <td>Community Special School</td>
      <td>Not applicable</td>
      <td>32.0</td>
      <td>No</td>
      <td>Open</td>
      <td>14512.0</td>
      <td>209.0</td>
      <td>7183.0</td>
      <td>...</td>
      <td>0%</td>
      <td>0%</td>
      <td>0</td>
      <td>11</td>
      <td>0</td>
      <td>11</td>
      <td>-60</td>
      <td>-88</td>
      <td>-70</td>
      <td>-77</td>
    </tr>
    <tr>
      <th>1200</th>
      <td>102881</td>
      <td>Hatton School and Special Needs Centre</td>
      <td>Community Special School</td>
      <td>Not applicable</td>
      <td>32.0</td>
      <td>No</td>
      <td>Open</td>
      <td>21426.0</td>
      <td>317.0</td>
      <td>7007.0</td>
      <td>...</td>
      <td>0%</td>
      <td>0%</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>-4</td>
      <td>-76</td>
      <td>-88</td>
      <td>-74</td>
      <td>-77</td>
    </tr>
    <tr>
      <th>1270</th>
      <td>100763</td>
      <td>New Woodlands School</td>
      <td>Community Special School</td>
      <td>Not applicable</td>
      <td>31.2</td>
      <td>No</td>
      <td>Open</td>
      <td>33568.0</td>
      <td>209.0</td>
      <td>7141.0</td>
      <td>...</td>
      <td>10%</td>
      <td>10%</td>
      <td>0</td>
      <td>SUPP</td>
      <td>SUPP</td>
      <td>-11</td>
      <td>SUPP</td>
      <td>SUPP</td>
      <td>-52</td>
      <td>-67</td>
    </tr>
    <tr>
      <th>1293</th>
      <td>102952</td>
      <td>Clarendon School</td>
      <td>Community Special School</td>
      <td>Not applicable</td>
      <td>30.9</td>
      <td>No</td>
      <td>Closed</td>
      <td>10950.0</td>
      <td>318.0</td>
      <td>7000.0</td>
      <td>...</td>
      <td>0%</td>
      <td>0%</td>
      <td>0</td>
      <td>-6</td>
      <td>-6</td>
      <td>6</td>
      <td>-74</td>
      <td>-77</td>
      <td>-62</td>
      <td>-77</td>
    </tr>
    <tr>
      <th>1298</th>
      <td>101485</td>
      <td>Woodside School</td>
      <td>Community Special School</td>
      <td>Not applicable</td>
      <td>30.9</td>
      <td>No</td>
      <td>Open</td>
      <td>59584.0</td>
      <td>303.0</td>
      <td>7000.0</td>
      <td>...</td>
      <td>0%</td>
      <td>0%</td>
      <td>0</td>
      <td>-13</td>
      <td>-8</td>
      <td>-15</td>
      <td>-77</td>
      <td>-80</td>
      <td>-65</td>
      <td>-77</td>
    </tr>
    <tr>
      <th>1324</th>
      <td>101395</td>
      <td>Northway School</td>
      <td>Community Special School</td>
      <td>Not applicable</td>
      <td>30.6</td>
      <td>No</td>
      <td>Open</td>
      <td>34638.0</td>
      <td>302.0</td>
      <td>7005.0</td>
      <td>...</td>
      <td>0%</td>
      <td>0%</td>
      <td>0</td>
      <td>10</td>
      <td>18</td>
      <td>16</td>
      <td>-60</td>
      <td>-63</td>
      <td>-55</td>
      <td>-77</td>
    </tr>
    <tr>
      <th>1328</th>
      <td>102069</td>
      <td>Oaktree School</td>
      <td>Community Special School</td>
      <td>Not applicable</td>
      <td>30.6</td>
      <td>No</td>
      <td>Open</td>
      <td>35212.0</td>
      <td>308.0</td>
      <td>7005.0</td>
      <td>...</td>
      <td>SUPP</td>
      <td>SUPP</td>
      <td>SUPP</td>
      <td>SUPP</td>
      <td>SUPP</td>
      <td>SUPP</td>
      <td>SUPP</td>
      <td>SUPP</td>
      <td>SUPP</td>
      <td>SUPP</td>
    </tr>
    <tr>
      <th>1427</th>
      <td>102556</td>
      <td>Lindon Bennett School</td>
      <td>Community Special School</td>
      <td>Not applicable</td>
      <td>29.6</td>
      <td>No</td>
      <td>Open</td>
      <td>28682.0</td>
      <td>313.0</td>
      <td>7007.0</td>
      <td>...</td>
      <td>0%</td>
      <td>0%</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>-85</td>
      <td>-88</td>
      <td>-81</td>
      <td>-77</td>
    </tr>
    <tr>
      <th>1433</th>
      <td>133316</td>
      <td>Woodlands School</td>
      <td>Community Special School</td>
      <td>Not applicable</td>
      <td>29.5</td>
      <td>No</td>
      <td>Open</td>
      <td>59444.0</td>
      <td>310.0</td>
      <td>7006.0</td>
      <td>...</td>
      <td>0%</td>
      <td>0%</td>
      <td>0</td>
      <td>-8</td>
      <td>0</td>
      <td>-8</td>
      <td>-85</td>
      <td>-88</td>
      <td>-81</td>
      <td>-77</td>
    </tr>
    <tr>
      <th>1450</th>
      <td>133399</td>
      <td>Willow Dene School</td>
      <td>Community Special School</td>
      <td>Not applicable</td>
      <td>29.3</td>
      <td>No</td>
      <td>Open</td>
      <td>58424.0</td>
      <td>203.0</td>
      <td>7201.0</td>
      <td>...</td>
      <td>0%</td>
      <td>0%</td>
      <td>0</td>
      <td>1</td>
      <td>13</td>
      <td>12</td>
      <td>-79</td>
      <td>-70</td>
      <td>-69</td>
      <td>-77</td>
    </tr>
    <tr>
      <th>1509</th>
      <td>102465</td>
      <td>Hedgewood School</td>
      <td>Community Special School</td>
      <td>Not applicable</td>
      <td>28.3</td>
      <td>No</td>
      <td>Open</td>
      <td>21984.0</td>
      <td>312.0</td>
      <td>7009.0</td>
      <td>...</td>
      <td>0%</td>
      <td>0%</td>
      <td>0</td>
      <td>23</td>
      <td>-13</td>
      <td>-7</td>
      <td>-43</td>
      <td>-72</td>
      <td>-58</td>
      <td>-77</td>
    </tr>
    <tr>
      <th>1559</th>
      <td>138157</td>
      <td>Grangewood School</td>
      <td>Academy Special Converter</td>
      <td>Not applicable</td>
      <td>26.8</td>
      <td>No</td>
      <td>Open</td>
      <td>19332.0</td>
      <td>312.0</td>
      <td>7012.0</td>
      <td>...</td>
      <td>0%</td>
      <td>0%</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>-85</td>
      <td>-88</td>
      <td>-81</td>
      <td>-77</td>
    </tr>
    <tr>
      <th>1570</th>
      <td>101855</td>
      <td>Red Gates School</td>
      <td>Community Special School</td>
      <td>Not applicable</td>
      <td>26.6</td>
      <td>No</td>
      <td>Open</td>
      <td>39730.0</td>
      <td>306.0</td>
      <td>7006.0</td>
      <td>...</td>
      <td>0%</td>
      <td>0%</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>-85</td>
      <td>-88</td>
      <td>-81</td>
      <td>-77</td>
    </tr>
    <tr>
      <th>1627</th>
      <td>102362</td>
      <td>Corbets Tey School</td>
      <td>Foundation Special School</td>
      <td>Not applicable</td>
      <td>23.7</td>
      <td>No</td>
      <td>Open</td>
      <td>11976.0</td>
      <td>311.0</td>
      <td>7000.0</td>
      <td>...</td>
      <td>0%</td>
      <td>0%</td>
      <td>0</td>
      <td>SUPP</td>
      <td>SUPP</td>
      <td>17</td>
      <td>SUPP</td>
      <td>SUPP</td>
      <td>-59</td>
      <td>-77</td>
    </tr>
  </tbody>
</table>
<p>37 rows × 280 columns</p>
</div>



By looking at the TAPS value that are less than 20, we could clearly see that these schools are all special schools which explains the scatterplot distribution.

Let's see how funding works into this.

# This analysis uses grants offered to schools for various reasons
Includes funds delegated by the LA, funding for 6th form students, SEN funding, funding for minority ethnic pupils, standards fund, other government grants, other grants and payments, SSG pupil focused, pupil focused extended school funding and/or grants and Additional grant for schools.


```python
funds = pd.read_csv('england_cfr.csv')
Pol_funds = pd.merge(Clean_perf_pol, funds, on = 'URN')
Pol_funds.plot.scatter('TAPS', 'NO2ug/m3 mean 2013',c = 'GRANTFUNDING', colormap = 'gist_rainbow',title = 'Pollution-Academic Performance-Grant Funding relationship')
plt.xlabel("Academic Performance")
plt.ylabel("Concentration of NO2(ug/m3)")
```

    D:\Anaconda_new\lib\site-packages\IPython\core\interactiveshell.py:3165: DtypeWarning: Columns (3,28,33,38,43,90) have mixed types.Specify dtype option on import or set low_memory=False.
      has_raised = await self.run_ast_nodes(code_ast.body, cell_name,
    




    Text(0, 0.5, 'Concentration of NO2(ug/m3)')




    
![png](output_68_2.png)
    



```python
Pol_funds.plot.scatter('NO2ug/m3 mean 2013', 'GRANTFUNDING',title = 'Grant Funding-Pollution relationship')
plt.xlabel("Concentration of NO2(ug/m3)")
plt.ylabel("Grant funding")
```




    Text(0, 0.5, 'Grant funding')




    
![png](output_69_1.png)
    



```python
Pol_funds[Pol_funds['GRANTFUNDING']>200000]
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
      <th>URN</th>
      <th>Establisment name</th>
      <th>Type of Establishment</th>
      <th>Phase of education</th>
      <th>NO2ug/m3 mean 2013</th>
      <th>Above limit</th>
      <th>Establishment Status</th>
      <th>ALPHAIND</th>
      <th>LEA</th>
      <th>ESTAB</th>
      <th>...</th>
      <th>PT1112CAT3LA</th>
      <th>PT1213CAT3LA</th>
      <th>PT0910CAT4LA</th>
      <th>PT1011CAT4LA</th>
      <th>PT1112CAT4LA</th>
      <th>PT1213CAT4LA</th>
      <th>PT0910CAT5LA</th>
      <th>PT1011CAT5LA</th>
      <th>PT1112CAT5LA</th>
      <th>PT1213CAT5LA</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1130</th>
      <td>100763</td>
      <td>New Woodlands School</td>
      <td>Community Special School</td>
      <td>Not applicable</td>
      <td>31.2</td>
      <td>No</td>
      <td>Open</td>
      <td>33568.0</td>
      <td>209.0</td>
      <td>7141.0</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>1 rows × 407 columns</p>
</div>



This school is for kids with Social, emotional and mental health (SEMH) needs. These are a type of special educational needs in which children/young people have severe difficulties in managing their emotions and behaviour. The school runs lots of outreach programmes for parents of the children who attend. These help to improve the home life of the pupils, which may be a factor in their behaviour. This is the reason why the school gets lots of funding, because it spends lots of money on supporting the pupils and their parents.

We don't have problems with this school below because we only work with primary school data in the Absences_polution data frame, and with the label of "special school", our coding doesn't select it.


```python
funding_absences = pd.merge(Absences_polution, funds, on='URN')
funding_absences.plot.scatter('PERCTOT', 'NO2ug/m3 mean 2013',c = 'GRANTFUNDING', colormap = 'gist_rainbow', title = 'Pollution-Absence-Grant Funding relationship')
plt.xlabel("Absence")
plt.ylabel("Concentration of NO2(ug/m3)")
```




    Text(0, 0.5, 'Concentration of NO2(ug/m3)')




    
![png](output_73_1.png)
    



```python
funding_absences.plot.scatter('NO2ug/m3 mean 2013', 'GRANTFUNDING',title = 'Grant Funding-Pollution relationship')
plt.xlabel("Concentration of NO2(ug/m3)")
plt.ylabel("Grant funding")
```




    Text(0, 0.5, 'Grant funding')




    
![png](output_74_1.png)
    



```python
funding_absences.plot.scatter('PERCTOT', 'GRANTFUNDING',title = 'Grant Funding-Absences relationship')
plt.xlabel("Absences")
plt.ylabel("Grant funding")
```




    Text(0, 0.5, 'Grant funding')




    
![png](output_75_1.png)
    


# This uses income generated by the facility
Includes income from facilities and services, receipts from other insurance claims, income from contributions to visits etc, donations and/or private funds.


```python
Pol_funds.plot.scatter('TAPS', 'NO2ug/m3 mean 2013',c = 'SELFGENERATEDINCOME', colormap = 'gist_rainbow', title = 'Pollution-Academic Performance-Self Generated Income relationship')
plt.xlabel("Academic Performance")
plt.ylabel("Concentration of NO2(ug/m3)")
```




    Text(0, 0.5, 'Concentration of NO2(ug/m3)')




    
![png](output_77_1.png)
    



```python
Pol_funds.plot.scatter('TAPS', 'SELFGENERATEDINCOME', title = 'Self Generated Income-Academic Performance relationship')
plt.xlabel("Academic Performance")
plt.ylabel('Self generated income')
```




    Text(0, 0.5, 'Self generated income')




    
![png](output_78_1.png)
    



```python
Pol_funds[Pol_funds['SELFGENERATEDINCOME']>60000]
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
      <th>URN</th>
      <th>Establisment name</th>
      <th>Type of Establishment</th>
      <th>Phase of education</th>
      <th>NO2ug/m3 mean 2013</th>
      <th>Above limit</th>
      <th>Establishment Status</th>
      <th>ALPHAIND</th>
      <th>LEA</th>
      <th>ESTAB</th>
      <th>...</th>
      <th>PT1112CAT3LA</th>
      <th>PT1213CAT3LA</th>
      <th>PT0910CAT4LA</th>
      <th>PT1011CAT4LA</th>
      <th>PT1112CAT4LA</th>
      <th>PT1213CAT4LA</th>
      <th>PT0910CAT5LA</th>
      <th>PT1011CAT5LA</th>
      <th>PT1112CAT5LA</th>
      <th>PT1213CAT5LA</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1130</th>
      <td>100763</td>
      <td>New Woodlands School</td>
      <td>Community Special School</td>
      <td>Not applicable</td>
      <td>31.2</td>
      <td>No</td>
      <td>Open</td>
      <td>33568.0</td>
      <td>209.0</td>
      <td>7141.0</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>1 rows × 407 columns</p>
</div>



This is the same school that is receiving the most grants.


```python
funding_absences.plot.scatter('NO2ug/m3 mean 2013', 'SELFGENERATEDINCOME', title = 'Self Generated Income-Pollution relationship')
plt.xlabel("Concentration of NO2(ug/m3)")
plt.ylabel("Self generated income")
```




    Text(0, 0.5, 'Self generated income')




    
![png](output_81_1.png)
    



```python
funding_absences.plot.scatter('PERCTOT', 'SELFGENERATEDINCOME', title = 'Self Generated Income-Absences relationship')
plt.xlabel("Absences")
plt.ylabel("Self generated income")
```




    Text(0, 0.5, 'Self generated income')




    
![png](output_82_1.png)
    


# Let's now look at the total income


```python
Pol_funds.plot.scatter('TAPS', 'NO2ug/m3 mean 2013',c = 'TOTALINCOME', colormap = 'gist_rainbow', title = 'Pollution-Academic Performance-Total Income relationship')
plt.xlabel("Academic performance")
plt.ylabel("Concentration of NO2(ug/m3)")
```




    Text(0, 0.5, 'Concentration of NO2(ug/m3)')




    
![png](output_84_1.png)
    



```python
funding_absences.plot.scatter('PERCTOT', 'NO2ug/m3 mean 2013',c = 'TOTALINCOME', colormap = 'gist_rainbow', title = 'Pollution-Absence-TotalIncome relationship')
plt.xlabel("Absences")
plt.ylabel("Concentration of NO2(ug/m3)")
plt.show()
```


    
![png](output_85_0.png)
    



```python
funding_absences.plot.scatter('NO2ug/m3 mean 2013', 'TOTALINCOME', title = 'Total Income-Pollution relationship')
plt.xlabel("Concentration of NO2 mean in 2013 (ug/m3)")
plt.ylabel("Total income")
```




    Text(0, 0.5, 'Total income')




    
![png](output_86_1.png)
    



```python
funding_absences.plot.scatter('PERCTOT', 'TOTALINCOME', title = 'Total Income-Absences relationship')
plt.xlabel("Absences")
plt.ylabel("Total income")
```




    Text(0, 0.5, 'Total income')




    
![png](output_87_1.png)
    


We observe nothing new, so we will move on to the correlation we found between pollution and grants.

## A closer look at the pollution-grants correlation


```python
from scipy.stats import linregress
line = linregress(funding_absences['NO2ug/m3 mean 2013'], funding_absences['GRANTFUNDING'])
slope = line [0]
intercept = line[1]
```

scipy.stats.linrgress is a function that can calculate a linear least-squares regression for two sets of measurements. 


```python
predict = funding_absences['NO2ug/m3 mean 2013']*slope + intercept
```


```python
funding_absences.plot.scatter('NO2ug/m3 mean 2013', 'GRANTFUNDING', label = 'data')
plt.plot(funding_absences['NO2ug/m3 mean 2013'], predict,'ro', label = 'line of best fit')
plt.xlabel("Concentration of NO2 mean in 2013 (ug/m3)")
plt.ylabel("Grant money")
plt.title('GrantFunding-Pollution relationship')
plt.legend()
```




    <matplotlib.legend.Legend at 0x22ca72e6f40>




    
![png](output_93_1.png)
    


This plot displays the line of best fit(red), over the correlation data(blue).


```python
yerr = funding_absences['GRANTFUNDING']-predict
```


```python
plt.plot(funding_absences['NO2ug/m3 mean 2013'], predict,'ro', label = 'Line of best fit')
plt.errorbar(funding_absences['NO2ug/m3 mean 2013'],predict,yerr=yerr, fmt='ko', elinewidth=0.5, barsabove = True, label='error bars')
plt.xlabel("Concentration of NO2 mean in 2013 (ug/m3)")
plt.ylabel("Grant money")
plt.legend(loc='lower right')
plt.title('Grant Funding-Pollution relationship')
```




    Text(0.5, 1.0, 'Grant Funding-Pollution relationship')




    
![png](output_96_1.png)
    


This graph shows the line of best fit over the error bars of each point on the line.

## Conclusion

Although the it was concluded by the literature reviews that there was positive association between level of pollution and absence and there was strong negative association between level of pollution and academic performance from primary education, the scatter plots and differences between median values of institutions above and under pollution limit showed that there was no correlation between absence and pollution and also no correlation between academic performance. Then the analysis of the relationship of grant funding, self-generated income and total income with pollution, academic performance and absences respectively was done. There was a special school called New Woodlands School that had extremely high funding and income. In the last section, the best fit lines were added into the scatter plot of grant funding and pollution, and it was clear to see that there was a strong positive correlation between these two variables. The reason we may not have found a correlation between pollution and performance and absence, like there was in the literature, is because our data wasnt collected from controlled experiments. Also the data would have been collected on different days, and for different lengths of time, so the influence of chance would have a great impact on the data.
