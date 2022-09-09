Pollution and academic performance¶
This is the README for Pollution and Academic Performance.
In order to replicate our analysis, please run all cells in this notebook.
The report is this notebook, with all outputs. There are also supplementary notebooks, showing the development of our project.
Our question is:
Does the level of N02 pollution around schools in London affect the attendance and attainment of the pupils?
Since the level of environmental pollution is at an increasing rate in the 21st century, we are keen to find if there is a correlation between the level of environmental pollution and the quality of educational performance. We selected the relevant data set of schools in London and extracted several related factors which are N02 (ug/m3 mean), attendance and average grade. We are going to compare the data to see if there is a correlation.
It is important to see the correlation between pollution and attendance. N02 has been proven to cause respiratory issues, such as asthma. Health issues will affect attendance and performance.
We did lots of research in this field, and we have discovered this has never been studied in the UK before. This was a further benefit of choosing to study this, we could contribute new information to the scientific community.
In [1]:
import numpy as np
import matplotlib.pyplot as plt
%matplotlib inline

# Fancy plots
plt.style.use('fivethirtyeight')

# Data frame library
import pandas as pd
Numpy is a python library. As we imported it, we added supports for multi-dimensional and large matrices and arrays, along with large collections of high level of mathematical functions to operate on these arrays.
We used matplotlib which is a plotting library as it allows us to produce 2D graphics in various interactive environments. We used panda, which is a software library for python, it enables us to analyse and manipulate the data.
Literature review about 'Pollution and its effect on school absense'¶
    1. In Janet Currie and her team-mates’ study and their literature about ‘Does pollution increase school absences’, they adopted the Texas Schools project which is a longitudinal administrative dataset of Texas’ student absenteeism. Then they aggregated the data of pollution from the Texas commission on quality of environment into 6week time blocks to combined with the administrative data of absenteeism. And they assumed that each person in different regions react similarly to the weather changes regardless of the implications of their actions for general air quality. This assumption allows them to make relevant behavior changes uncorrelated with idiosyncratic changes in the quality of air so that they could put their focus on how the absences relate to the level of pollution. Through a set of experiments they did, they concluded that although they found a positive association between absences and level of pollution, they did not find out a monotonically increasing relationship between these two factors (Currie et al., 2009). 
    • Reference: Currie, J., Hanushek, E., Kahn, E., Neidell, M. and Rivkin, S. (2009). Does Pollution Increase School Absences?. Review of Economics and Statistics, [online] 91(4), pp.682-694. Available at: https://www.jstor.org/stable/25651370?seq=9#metadata_info_tab_contents [Accessed 17 Mar. 2019]. 
Pollution and its effect on school absence¶
This project is using a data set from this links:
    • https://data.gov.uk/dataset/90b02236-3a4e-4899-8bdc-7f5122df4d9e/schools-and-educational-institutions-air-quality-exposure-data-broken-down-by-parliamentary-constituency 
    • https://www.compare-school-performance.service.gov.uk/download-data?currentstep=datatypes&regiontype=all&la=0&downloadYear=2012-2013&datatypes=spine&datatypes=ks2&datatypes=ks4&datatypes=ks4underlying&datatypes=ks5&datatypes=pupilabsence&datatypes=Workforce&datatypes=spendperpupil&datatypes=spendperpupilgrouped 
We found this by looking at the open source data on data.gov
Now we load our data and display the columns in order to drop those not needed.
In [2]:
schoolpolutiondata = pd.read_csv('school polution.csv')
schoolpolutiondata.columns
Out[2]:
Index(['URN', 'Local Authority name', 'Establisment name', 'Easting',
       'Northing', 'Type of Establishment', 'Phase of education',
       'NO2ug/m3 mean 2013', 'Above limit', 'Parliamentary Constituences',
       'Establishment Status', 'Close Date', 'Unnamed: 12'],
      dtype='object')
In [3]:
schoolpolutiondata = schoolpolutiondata.drop(['Local Authority name', 'Easting', 'Northing', 'Parliamentary Constituences', 'Close Date', 'Unnamed: 12'], axis=1)
Reference for the drop function: https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.drop.html
In [4]:
primary_data = schoolpolutiondata[schoolpolutiondata['Phase of education'] == 'Primary']
primary_data.head()
Out[4]:

URN
Establisment name
Type of Establishment
Phase of education
NO2ug/m3 mean 2013
Above limit
Establishment Status
4
101136
St Mary's Bryanston Square CofE School
Voluntary Aided School
Primary
67.0
Yes
Open
6
100351
St Paul's CofE Primary School
Voluntary Aided School
Primary
65.2
Yes
Open
7
101127
St Clement Danes CofE Primary School
Voluntary Aided School
Primary
65.0
Yes
Open
10
101140
St Peter's Eaton Square CofE Primary School
Voluntary Aided School
Primary
64.3
Yes
Open
11
100828
St George's Cathedral Catholic Primary School
Voluntary Aided School
Primary
64.3
Yes
Open
In [5]:
Absences = pd.read_csv('england_abs.csv')
Absences.head()
Out[5]:

LA
ESTAB
URN
PERCTOT
PPERSABS15
0
201
3614
100000.0
3.5
0
1
202
2000
136807.0
5.4
SUPP
2
202
2019
100008.0
5.4
5.8
3
202
2036
100009.0
4.1
2.7
4
202
2065
100010.0
5.6
5.3
    • This code is to make pandas open and read the csv file. 
In [6]:
Absences_polution = pd.merge(Absences, primary_data, on='URN')
Absences_polution.head()
Out[6]:

LA
ESTAB
URN
PERCTOT
PPERSABS15
Establisment name
Type of Establishment
Phase of education
NO2ug/m3 mean 2013
Above limit
Establishment Status
0
201
3614
100000.0
3.5
0
Sir John Cass's Foundation Primary School
Voluntary Aided School
Primary
62.6
Yes
Open
1
202
2000
136807.0
5.4
SUPP
St Luke's Church of England Primary
Free Schools
Primary
43.4
Yes
Open
2
202
2019
100008.0
5.4
5.8
Argyle Primary School
Community School
Primary
58.8
Yes
Open
3
202
2036
100009.0
4.1
2.7
Beckford Primary School
Community School
Primary
39.4
No
Open
4
202
2065
100010.0
5.6
5.3
Brecknock Primary School
Community School
Primary
43.9
Yes
Open
We merged the "Primary_data" which is a column of data extracted from "school pollution" data frame with the different data frame named as "Absences" by using the coding that we searched from internet sources
Reference: (https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.merge.html)
We made a data frame with only primary schools, this was because we decided to look at primary schools first, as there was an abundance of data to use. Once we had the programme written to analyze primary schools, it would be easy to adapt it for other ages.
We will define two functions needed for cleaning up the data, we chose to define functions in order to boost reusability:
How it works: I found a function that checks if a value can be converted into a float and put it in the cell below. Source: Eric Leschinski: https://stackoverflow.com/questions/736043/checking-if-a-string-can-be-converted-to-float-in-python
The way it does that is: it attempts to convert the variable to float by using the function float(). We expect to have the following non float values: NaN, NA and SUPP. NaN will pass throu the function and will be verified directly. The other non float values will raise "ValueError". This error is accounted for in the function and makes the function return "False".
In [7]:
def isfloat(value):
  try:
    float(value)      #try to convert to float
    return True
  except ValueError:  #it's a word so it can't convert to float
    return False
Small tests:
In [8]:
print(isfloat('12.98'))
print(isfloat('SUPP'))
print(isfloat('NaN'))
True
False
True
Observe how NaN passes throu the function. This means that it will have to be taken care of seperatly.
What the function below does:
    • we make a bool array(clean) of length equal to that of the column we want to sort (the array will have True at the positions of numbers and False at the positions of words and NaN values) 
    • we use the array to sort the data frame 
    • transform the strings to floats 
In [9]:
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
The data for short-term absences is stored in the colomn "PERCTOT".
In [10]:
Absences_polution.plot.scatter('PERCTOT', 'NO2ug/m3 mean 2013', title = 'Pollution-Absences relationship', )
plt.xlabel("Absences")
plt.ylabel("Concentration of NO2(ug/m3)")
Out[10]:
Text(0, 0.5, 'Concentration of NO2(ug/m3)')
 
In [11]:
#Here we split the table in Above- institutions above the polution limit at the time and Under- institutions below the polution limit at the time
Above = Absences_polution.loc[Absences_polution['Above limit']=='Yes']
Under = Absences_polution.loc[Absences_polution['Above limit']=='No']
In [12]:
np.median(Absences_polution['PERCTOT'])
Out[12]:
4.6
In [13]:
np.median(Above['PERCTOT'])
Out[13]:
4.7
In [14]:
np.median(Under['PERCTOT'])
Out[14]:
4.6
We used plot.scatter to produce a scatter graph for two variables which are 'level of NO2' and 'Percentage of absences'
Also, we calculated the median for the:
    • total absences 4.6 
    • absences of institutions above the pollution limit 4.7 
    • absences of institutions under the pollution limit 4.6 
As the graph shows, the level of NO2 and the percentage of absences do not have a significant correlation as most of the points are allocated on the similar area instead of locating as a linear regression line.
Also, the medians have very close values.
We conclude that pollution and absences are not strongly correlated.
We moved on to prolonged absences.
The data for prolonged absences is stored in the column "PPERSABS15".
In [15]:
Absences_polution = all_floats(Absences_polution, 'PPERSABS15')
Absences_polution.head()
<ipython-input-9-fe5454955d23>:20: DeprecationWarning: `np.float` is a deprecated alias for the builtin `float`. To silence this warning, use `float` by itself. Doing this will not modify any behavior and is safe. If you specifically wanted the numpy scalar type, use `np.float64` here.
Deprecated in NumPy 1.20; for more details and guidance: https://numpy.org/devdocs/release/1.20.0-notes.html#deprecations
  Clean_data_frame[colomn] = Clean_data_frame[colomn].astype(np.float)
<ipython-input-9-fe5454955d23>:20: SettingWithCopyWarning: 
A value is trying to be set on a copy of a slice from a DataFrame.
Try using .loc[row_indexer,col_indexer] = value instead

See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
  Clean_data_frame[colomn] = Clean_data_frame[colomn].astype(np.float)
Out[15]:

LA
ESTAB
URN
PERCTOT
PPERSABS15
Establisment name
Type of Establishment
Phase of education
NO2ug/m3 mean 2013
Above limit
Establishment Status
0
201
3614
100000.0
3.5
0.0
Sir John Cass's Foundation Primary School
Voluntary Aided School
Primary
62.6
Yes
Open
2
202
2019
100008.0
5.4
5.8
Argyle Primary School
Community School
Primary
58.8
Yes
Open
3
202
2036
100009.0
4.1
2.7
Beckford Primary School
Community School
Primary
39.4
No
Open
4
202
2065
100010.0
5.6
5.3
Brecknock Primary School
Community School
Primary
43.9
Yes
Open
5
202
2078
100011.0
5.4
4.1
Brookfield Primary School
Community School
Primary
35.3
No
Open
In [16]:
Absences_polution.plot.scatter('PPERSABS15', 'NO2ug/m3 mean 2013', title = 'Pollution-Extended Absences relationship')
plt.xlabel("Extended Absences")
plt.ylabel("Concentration of NO2(ug/m3)")
Out[16]:
Text(0, 0.5, 'Concentration of NO2(ug/m3)')
 
In [17]:
Above = Absences_polution.loc[Absences_polution['Above limit']=='Yes']
Under = Absences_polution.loc[Absences_polution['Above limit']=='No']
In [18]:
PPERSABS15_Above_mean = np.mean(Above['PPERSABS15'])
PPERSABS15_Above_mean
Out[18]:
3.8624573378839577
In [19]:
PPERSABS15_Under_mean = np.mean(Under['PPERSABS15'])
PPERSABS15_Under_mean
Out[19]:
3.6452461799660476
In [20]:
PPERSABS15_General_mean = np.mean(Absences_polution['PPERSABS15'])
PPERSABS15_General_mean
Out[20]:
3.688511216859283
Previously, we have 3 codings which are:
    1. Absences = pd.read_csv('england_abs.csv') 
    2. Absences_polution=pd.merge(Absences, primary_data, on='URN') 
    3. Absences_polution = Absences_polution.drop (['Unnamed: 12', 'Close Date', 'PPERSABS15'], axis=1) 
In the process of tidying up the coding, we found out these 3 coding has been run before which means they are three extra codings that can be avoided. Therefore, we decided to eliminate these 3 codings to make our project more clearer and understandable.
Literature review about 'Pollution and its effect on academic performance'¶
    • In Miller and his team-mates’ study and their literature about ‘The Effects of air pollution on educational outcomes: Evidence from Chile’, they adopted the school-level pollution data from 1997 to 2012 that is reported and published by the SINCA (Sistema de Information Nacional de Calidad del Aire) of the environment ministry. For the school performance data, they used the test scores generated by the SIMCE (Sistema de Medicion de la Calidad de la Educacion) test that is undertaken by the Education Ministry for all schools in Chile. Then they made a assumption that unnoticed time-varying variables would not have a impact on both test scores and pollution. Through a set of experiments and analysis, they concluded that they found out some regressions that indicated a strong negative association between pollution and school performance (Miller and Vela, 2013) 
    • Reference: Miller, S. and Vela, M. (2013). The Effects of Air Pollution on Educational Outcomes: Evidence from Chile. SSRN Electronic Journal. [online] Available at: https://publications.iadb.org/en/publication/11349/effects-air-pollution-educational-outcomes-evidence-chile [Accessed 17 Mar. 2019]. 
Pollution and its effect on academic performance¶
In [21]:
performance_data = pd.read_csv('england_ks2final_primary.csv')
D:\Anaconda_new\lib\site-packages\IPython\core\interactiveshell.py:3165: DtypeWarning: Columns (44,270,272,274) have mixed types.Specify dtype option on import or set low_memory=False.
  has_raised = await self.run_ast_nodes(code_ast.body, cell_name,
In [22]:
performance_data.columns
Out[22]:
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
In [23]:
performance_data = performance_data.drop(['RECTYPE','PCODE','TELNUM','TKS1EXP_L','PKS1EXP_L', 'TKS1EXP_M', 'PKS1EXP_M', 'TKS1EXP_H', 'PKS1EXP_H', 'TFSMCLA', 'PTFSMCLA', 'PT2MATH12', 'PT2READ12', 'PT2WRITTA12', 'AVGLEVEL', 'PT2MATH12_B', 'PT2MATH12_G', 'PT2MATH12_L', 'PT2MATH12_M', 'PT2MATH12_H', 'PTREADX', 'PTREADAT', 'GAPN_3YR_RWMX_FSMCLA', 'GAPN_2YR_PRREAD_NOTFSMCLA', 'GAPN_2YR_PRWRIT_NOTFSMCLA', 'GAPN_3YR_PRMAT_NOTFSMCLA'],axis=1)
During codeing review we found this code to be inefficient:
    • performance_abs_pol = pd.merge(Absences_polution, performance_data, on='URN') 
In [24]:
performance_pol = pd.merge(schoolpolutiondata, performance_data, on='URN')
In [25]:
Clean_perf_pol = all_floats (performance_pol, 'TAPS')
<ipython-input-9-fe5454955d23>:20: DeprecationWarning: `np.float` is a deprecated alias for the builtin `float`. To silence this warning, use `float` by itself. Doing this will not modify any behavior and is safe. If you specifically wanted the numpy scalar type, use `np.float64` here.
Deprecated in NumPy 1.20; for more details and guidance: https://numpy.org/devdocs/release/1.20.0-notes.html#deprecations
  Clean_data_frame[colomn] = Clean_data_frame[colomn].astype(np.float)
<ipython-input-9-fe5454955d23>:20: SettingWithCopyWarning: 
A value is trying to be set on a copy of a slice from a DataFrame.
Try using .loc[row_indexer,col_indexer] = value instead

See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
  Clean_data_frame[colomn] = Clean_data_frame[colomn].astype(np.float)
Now we will calculate the median values of the performance of institutions above and below the accepted pollution limit.
In [26]:
Above = Clean_perf_pol[Clean_perf_pol['Above limit']=='Yes']
Under = Clean_perf_pol[Clean_perf_pol['Above limit']=='No']
In [27]:
print(np.median(Clean_perf_pol['TAPS']))
print(np.median(Above['TAPS']))
print(np.median(Under['TAPS']))
29.0
28.7
29.1
In [28]:
Clean_perf_pol.plot.scatter('TAPS', 'NO2ug/m3 mean 2013',title = 'Pollution-Academic Performance relationship')
plt.xlabel("Academic Performance")
plt.ylabel("Concentration of NO2(ug/m3)")
Out[28]:
Text(0, 0.5, 'Concentration of NO2(ug/m3)')
 
Those points that are loacated at the left hand side of the scatter graph seems odd. Therefore, we want to figure out if these schools are special schools, such as school for disabled students, as this would explain this unusual distribution.
In [29]:
Taps_less_than_20= Clean_perf_pol['TAPS'] < 20
In [30]:
np.count_nonzero(Taps_less_than_20)
Out[30]:
37
In [31]:
Clean_perf_pol[Taps_less_than_20]
Out[31]:

URN
Establisment name
Type of Establishment
Phase of education
NO2ug/m3 mean 2013
Above limit
Establishment Status
ALPHAIND
LEA
ESTAB
...
PREADWRITTAMATX_3YR_FSMCLA
PREADWRITTAMATX_3YR_NOTFSMCLA
GAP_3YR_RWMX
GAP_2YR_PRREAD
GAP_2YR_PRWRIT
GAP_3YR_PRMAT
GAPN_2YR_PRREAD_FSMCLA
GAPN_2YR_PRWRIT_FSMCLA
GAPN_3YR_PRMAT_FSMCLA
GAPN_3YR_RWMX_NOTFSMCLA
67
131023
Stephen Hawking School
Community Special School
Not applicable
49.0
Yes
Open
51988.0
211.0
7169.0
...
SUPP
SUPP
SUPP
SUPP
SUPP
SUPP
SUPP
SUPP
SUPP
SUPP
149
100378
Queensmill School
Community Special School
Not applicable
45.2
Yes
Open
39148.0
205.0
7014.0
...
0%
0%
0
SUPP
SUPP
8
SUPP
SUPP
-62
-77
202
100096
Swiss Cottage School - Development & Research ...
Community Special School
Not applicable
44.2
Yes
Open
53064.0
202.0
7205.0
...
0%
0%
0
5
0
18
-66
-74
-53
-77
204
100987
Phoenix School
Community Special School
Not applicable
44.1
Yes
Open
37776.0
211.0
7095.0
...
0%
0%
0
SUPP
SUPP
0
SUPP
SUPP
-81
-77
208
100469
Samuel Rhodes MLD School
Community Special School
Not applicable
43.9
Yes
Open
48750.0
206.0
7146.0
...
SUPP
SUPP
SUPP
SUPP
SUPP
SUPP
SUPP
SUPP
SUPP
SUPP
229
100881
Cherry Garden School
Community Special School
Not applicable
43.4
Yes
Open
9960.0
210.0
7186.0
...
0%
0%
0
0
0
0
-85
-88
-81
-77
277
100467
Richard Cloudesley PH School
Community Special School
Not applicable
42.1
Yes
Open
40134.0
206.0
7030.0
...
SUPP
SUPP
SUPP
SUPP
SUPP
SUPP
SUPP
SUPP
SUPP
SUPP
305
134030
The Bridge School
Community Special School
Not applicable
41.7
Yes
Open
6654.0
206.0
7031.0
...
0%
0%
0
SUPP
SUPP
-11
SUPP
SUPP
-81
-77
598
100878
Haymerle School
Community Special School
Not applicable
37.9
No
Open
21650.0
210.0
7126.0
...
0%
0%
0
SUPP
SUPP
-2
SUPP
SUPP
-66
-77
599
100643
Turney Primary and Secondary Special School
Foundation Special School
Not applicable
37.9
No
Open
54900.0
208.0
5950.0
...
SUPP
SUPP
SUPP
SUPP
SUPP
SUPP
SUPP
SUPP
SUPP
SUPP
657
101582
Manor School
Community Special School
Not applicable
37.3
No
Closed
30398.0
304.0
7006.0
...
0%
0%
0
11
0
0
-74
-88
-81
-77
679
133440
The Livity School
Community Special School
Not applicable
37.1
No
Open
29046.0
208.0
7194.0
...
SUPP
SUPP
SUPP
SUPP
SUPP
SUPP
SUPP
SUPP
SUPP
SUPP
694
101102
Paddock School
Community Special School
Not applicable
36.9
No
Open
36594.0
212.0
7183.0
...
0%
0%
0
SUPP
SUPP
0
SUPP
SUPP
-81
-77
698
100760
Brent Knoll School
Foundation Special School
Not applicable
36.9
No
Open
6516.0
209.0
7038.0
...
0%
0%
0
-14
25
-2
-66
-63
-58
-77
836
102554
Marjory Kinnon School
Community Special School
Not applicable
35.4
No
Open
30622.0
313.0
7005.0
...
0%
0%
0
11
54
14
-60
-13
-45
-77
880
130958
Russet House School
Community Special School
Not applicable
34.8
No
Open
41496.0
308.0
7008.0
...
0%
0%
0
SUPP
SUPP
-14
SUPP
SUPP
-68
-77
911
100766
Watergate School
Foundation Special School
Not applicable
34.4
No
Open
56264.0
209.0
7182.0
...
0%
0%
0
0
0
0
-85
-88
-81
-77
915
102558
The Cedars Primary School
Community Special School
Not applicable
34.4
No
Open
9302.0
313.0
7010.0
...
0%
0%
0
SUPP
SUPP
22
SUPP
SUPP
-46
-77
1012
101968
Mandeville School
Community Special School
Not applicable
33.5
No
Open
30232.0
307.0
7010.0
...
0%
0%
0
0
0
0
-85
-88
-81
-77
1040
101093
Linden Lodge School
Community Special School
Not applicable
33.2
No
Open
28656.0
212.0
7067.0
...
0%
0%
0
25
13
8
-60
-75
-73
-77
1050
102176
Vale School
Community Special School
Not applicable
33.1
No
Open
55382.0
309.0
7001.0
...
0%
0%
0
SUPP
SUPP
-25
SUPP
SUPP
-81
-77
1061
102177
The Brook School
Community Special School
Not applicable
33.0
No
Open
7270.0
309.0
7005.0
...
0%
0%
0
8
0
0
-77
-88
-81
-77
1168
101966
Castlebar School
Community Special School
Not applicable
32.2
No
Open
9016.0
307.0
7007.0
...
0%
0%
0
-19
-13
-11
-85
-88
-48
-77
1193
136423
Drumbeat School and ASD Service
Community Special School
Not applicable
32.0
No
Open
14512.0
209.0
7183.0
...
0%
0%
0
11
0
11
-60
-88
-70
-77
1200
102881
Hatton School and Special Needs Centre
Community Special School
Not applicable
32.0
No
Open
21426.0
317.0
7007.0
...
0%
0%
0
1
0
-4
-76
-88
-74
-77
1270
100763
New Woodlands School
Community Special School
Not applicable
31.2
No
Open
33568.0
209.0
7141.0
...
10%
10%
0
SUPP
SUPP
-11
SUPP
SUPP
-52
-67
1293
102952
Clarendon School
Community Special School
Not applicable
30.9
No
Closed
10950.0
318.0
7000.0
...
0%
0%
0
-6
-6
6
-74
-77
-62
-77
1298
101485
Woodside School
Community Special School
Not applicable
30.9
No
Open
59584.0
303.0
7000.0
...
0%
0%
0
-13
-8
-15
-77
-80
-65
-77
1324
101395
Northway School
Community Special School
Not applicable
30.6
No
Open
34638.0
302.0
7005.0
...
0%
0%
0
10
18
16
-60
-63
-55
-77
1328
102069
Oaktree School
Community Special School
Not applicable
30.6
No
Open
35212.0
308.0
7005.0
...
SUPP
SUPP
SUPP
SUPP
SUPP
SUPP
SUPP
SUPP
SUPP
SUPP
1427
102556
Lindon Bennett School
Community Special School
Not applicable
29.6
No
Open
28682.0
313.0
7007.0
...
0%
0%
0
0
0
0
-85
-88
-81
-77
1433
133316
Woodlands School
Community Special School
Not applicable
29.5
No
Open
59444.0
310.0
7006.0
...
0%
0%
0
-8
0
-8
-85
-88
-81
-77
1450
133399
Willow Dene School
Community Special School
Not applicable
29.3
No
Open
58424.0
203.0
7201.0
...
0%
0%
0
1
13
12
-79
-70
-69
-77
1509
102465
Hedgewood School
Community Special School
Not applicable
28.3
No
Open
21984.0
312.0
7009.0
...
0%
0%
0
23
-13
-7
-43
-72
-58
-77
1559
138157
Grangewood School
Academy Special Converter
Not applicable
26.8
No
Open
19332.0
312.0
7012.0
...
0%
0%
0
0
0
0
-85
-88
-81
-77
1570
101855
Red Gates School
Community Special School
Not applicable
26.6
No
Open
39730.0
306.0
7006.0
...
0%
0%
0
0
0
0
-85
-88
-81
-77
1627
102362
Corbets Tey School
Foundation Special School
Not applicable
23.7
No
Open
11976.0
311.0
7000.0
...
0%
0%
0
SUPP
SUPP
17
SUPP
SUPP
-59
-77
37 rows × 280 columns
By looking at the TAPS value that are less than 20, we could clearly see that these schools are all special schools which explains the scatterplot distribution.
Let's see how funding works into this.
This analysis uses grants offered to schools for various reasons¶
Includes funds delegated by the LA, funding for 6th form students, SEN funding, funding for minority ethnic pupils, standards fund, other government grants, other grants and payments, SSG pupil focused, pupil focused extended school funding and/or grants and Additional grant for schools.
In [32]:
funds = pd.read_csv('england_cfr.csv')
Pol_funds = pd.merge(Clean_perf_pol, funds, on = 'URN')
Pol_funds.plot.scatter('TAPS', 'NO2ug/m3 mean 2013',c = 'GRANTFUNDING', colormap = 'gist_rainbow',title = 'Pollution-Academic Performance-Grant Funding relationship')
plt.xlabel("Academic Performance")
plt.ylabel("Concentration of NO2(ug/m3)")
D:\Anaconda_new\lib\site-packages\IPython\core\interactiveshell.py:3165: DtypeWarning: Columns (3,28,33,38,43,90) have mixed types.Specify dtype option on import or set low_memory=False.
  has_raised = await self.run_ast_nodes(code_ast.body, cell_name,
Out[32]:
Text(0, 0.5, 'Concentration of NO2(ug/m3)')
 
In [33]:
Pol_funds.plot.scatter('NO2ug/m3 mean 2013', 'GRANTFUNDING',title = 'Grant Funding-Pollution relationship')
plt.xlabel("Concentration of NO2(ug/m3)")
plt.ylabel("Grant funding")
Out[33]:
Text(0, 0.5, 'Grant funding')
 
In [34]:
Pol_funds[Pol_funds['GRANTFUNDING']>200000]
Out[34]:

URN
Establisment name
Type of Establishment
Phase of education
NO2ug/m3 mean 2013
Above limit
Establishment Status
ALPHAIND
LEA
ESTAB
...
PT1112CAT3LA
PT1213CAT3LA
PT0910CAT4LA
PT1011CAT4LA
PT1112CAT4LA
PT1213CAT4LA
PT0910CAT5LA
PT1011CAT5LA
PT1112CAT5LA
PT1213CAT5LA
1130
100763
New Woodlands School
Community Special School
Not applicable
31.2
No
Open
33568.0
209.0
7141.0
...
NaN
NaN
NaN
NaN
NaN
NaN
NaN
NaN
NaN
NaN
1 rows × 407 columns
This school is for kids with Social, emotional and mental health (SEMH) needs. These are a type of special educational needs in which children/young people have severe difficulties in managing their emotions and behaviour. The school runs lots of outreach programmes for parents of the children who attend. These help to improve the home life of the pupils, which may be a factor in their behaviour. This is the reason why the school gets lots of funding, because it spends lots of money on supporting the pupils and their parents.
We don't have problems with this school below because we only work with primary school data in the Absences_polution data frame, and with the label of "special school", our coding doesn't select it.
In [35]:
funding_absences = pd.merge(Absences_polution, funds, on='URN')
funding_absences.plot.scatter('PERCTOT', 'NO2ug/m3 mean 2013',c = 'GRANTFUNDING', colormap = 'gist_rainbow', title = 'Pollution-Absence-Grant Funding relationship')
plt.xlabel("Absence")
plt.ylabel("Concentration of NO2(ug/m3)")
Out[35]:
Text(0, 0.5, 'Concentration of NO2(ug/m3)')
 
In [36]:
funding_absences.plot.scatter('NO2ug/m3 mean 2013', 'GRANTFUNDING',title = 'Grant Funding-Pollution relationship')
plt.xlabel("Concentration of NO2(ug/m3)")
plt.ylabel("Grant funding")
Out[36]:
Text(0, 0.5, 'Grant funding')
 
In [37]:
funding_absences.plot.scatter('PERCTOT', 'GRANTFUNDING',title = 'Grant Funding-Absences relationship')
plt.xlabel("Absences")
plt.ylabel("Grant funding")
Out[37]:
Text(0, 0.5, 'Grant funding')
 
This uses income generated by the facility¶
Includes income from facilities and services, receipts from other insurance claims, income from contributions to visits etc, donations and/or private funds.
In [38]:
Pol_funds.plot.scatter('TAPS', 'NO2ug/m3 mean 2013',c = 'SELFGENERATEDINCOME', colormap = 'gist_rainbow', title = 'Pollution-Academic Performance-Self Generated Income relationship')
plt.xlabel("Academic Performance")
plt.ylabel("Concentration of NO2(ug/m3)")
Out[38]:
Text(0, 0.5, 'Concentration of NO2(ug/m3)')
 
In [39]:
Pol_funds.plot.scatter('TAPS', 'SELFGENERATEDINCOME', title = 'Self Generated Income-Academic Performance relationship')
plt.xlabel("Academic Performance")
plt.ylabel('Self generated income')
Out[39]:
Text(0, 0.5, 'Self generated income')
 
In [40]:
Pol_funds[Pol_funds['SELFGENERATEDINCOME']>60000]
Out[40]:

URN
Establisment name
Type of Establishment
Phase of education
NO2ug/m3 mean 2013
Above limit
Establishment Status
ALPHAIND
LEA
ESTAB
...
PT1112CAT3LA
PT1213CAT3LA
PT0910CAT4LA
PT1011CAT4LA
PT1112CAT4LA
PT1213CAT4LA
PT0910CAT5LA
PT1011CAT5LA
PT1112CAT5LA
PT1213CAT5LA
1130
100763
New Woodlands School
Community Special School
Not applicable
31.2
No
Open
33568.0
209.0
7141.0
...
NaN
NaN
NaN
NaN
NaN
NaN
NaN
NaN
NaN
NaN
1 rows × 407 columns
This is the same school that is receiving the most grants.
In [41]:
funding_absences.plot.scatter('NO2ug/m3 mean 2013', 'SELFGENERATEDINCOME', title = 'Self Generated Income-Pollution relationship')
plt.xlabel("Concentration of NO2(ug/m3)")
plt.ylabel("Self generated income")
Out[41]:
Text(0, 0.5, 'Self generated income')
 
In [42]:
funding_absences.plot.scatter('PERCTOT', 'SELFGENERATEDINCOME', title = 'Self Generated Income-Absences relationship')
plt.xlabel("Absences")
plt.ylabel("Self generated income")
Out[42]:
Text(0, 0.5, 'Self generated income')
 
Let's now look at the total income¶
In [43]:
Pol_funds.plot.scatter('TAPS', 'NO2ug/m3 mean 2013',c = 'TOTALINCOME', colormap = 'gist_rainbow', title = 'Pollution-Academic Performance-Total Income relationship')
plt.xlabel("Academic performance")
plt.ylabel("Concentration of NO2(ug/m3)")
Out[43]:
Text(0, 0.5, 'Concentration of NO2(ug/m3)')
 
In [44]:
funding_absences.plot.scatter('PERCTOT', 'NO2ug/m3 mean 2013',c = 'TOTALINCOME', colormap = 'gist_rainbow', title = 'Pollution-Absence-TotalIncome relationship')
plt.xlabel("Absences")
plt.ylabel("Concentration of NO2(ug/m3)")
plt.show()
 
In [45]:
funding_absences.plot.scatter('NO2ug/m3 mean 2013', 'TOTALINCOME', title = 'Total Income-Pollution relationship')
plt.xlabel("Concentration of NO2 mean in 2013 (ug/m3)")
plt.ylabel("Total income")
Out[45]:
Text(0, 0.5, 'Total income')
 
In [46]:
funding_absences.plot.scatter('PERCTOT', 'TOTALINCOME', title = 'Total Income-Absences relationship')
plt.xlabel("Absences")
plt.ylabel("Total income")
Out[46]:
Text(0, 0.5, 'Total income')
 
We observe nothing new, so we will move on to the correlation we found between pollution and grants.
A closer look at the pollution-grants correlation¶
In [47]:
from scipy.stats import linregress
line = linregress(funding_absences['NO2ug/m3 mean 2013'], funding_absences['GRANTFUNDING'])
slope = line [0]
intercept = line[1]
scipy.stats.linrgress is a function that can calculate a linear least-squares regression for two sets of measurements.
In [48]:
predict = funding_absences['NO2ug/m3 mean 2013']*slope + intercept
In [49]:
funding_absences.plot.scatter('NO2ug/m3 mean 2013', 'GRANTFUNDING', label = 'data')
plt.plot(funding_absences['NO2ug/m3 mean 2013'], predict,'ro', label = 'line of best fit')
plt.xlabel("Concentration of NO2 mean in 2013 (ug/m3)")
plt.ylabel("Grant money")
plt.title('GrantFunding-Pollution relationship')
plt.legend()
Out[49]:
<matplotlib.legend.Legend at 0x22ca72e6f40>
 
This plot displays the line of best fit(red), over the correlation data(blue).
In [50]:
yerr = funding_absences['GRANTFUNDING']-predict
In [51]:
plt.plot(funding_absences['NO2ug/m3 mean 2013'], predict,'ro', label = 'Line of best fit')
plt.errorbar(funding_absences['NO2ug/m3 mean 2013'],predict,yerr=yerr, fmt='ko', elinewidth=0.5, barsabove = True, label='error bars')
plt.xlabel("Concentration of NO2 mean in 2013 (ug/m3)")
plt.ylabel("Grant money")
plt.legend(loc='lower right')
plt.title('Grant Funding-Pollution relationship')
Out[51]:
Text(0.5, 1.0, 'Grant Funding-Pollution relationship')
 
This graph shows the line of best fit over the error bars of each point on the line.
Conclusion¶
Although the it was concluded by the literature reviews that there was positive association between level of pollution and absence and there was strong negative association between level of pollution and academic performance from primary education, the scatter plots and differences between median values of institutions above and under pollution limit showed that there was no correlation between absence and pollution and also no correlation between academic performance. Then the analysis of the relationship of grant funding, self-generated income and total income with pollution, academic performance and absences respectively was done. There was a special school called New Woodlands School that had extremely high funding and income. In the last section, the best fit lines were added into the scatter plot of grant funding and pollution, and it was clear to see that there was a strong positive correlation between these two variables. The reason we may not have found a correlation between pollution and performance and absence, like there was in the literature, is because our data wasnt collected from controlled experiments. Also the data would have been collected on different days, and for different lengths of time, so the influence of chance would have a great impact on the data.
