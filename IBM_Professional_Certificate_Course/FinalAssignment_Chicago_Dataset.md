
<a href="https://cognitiveclass.ai"><img src = "https://ibm.box.com/shared/static/ugcqz6ohbvff804xp84y4kqnvvk3bq1g.png" width = 300, align = "center"></a>

<h1 align=center><font size = 5>Assignment: Notebook for Peer Assignment</font></h1>

# Introduction

Using this Python notebook you will:
1. Understand 3 Chicago datasets  
1. Load the 3 datasets into 3 tables in a Db2 database
1. Execute SQL queries to answer assignment questions 

## Understand the datasets 
To complete the assignment problems in this notebook you will be using three datasets that are available on the city of Chicago's Data Portal:
1. <a href="https://data.cityofchicago.org/Health-Human-Services/Census-Data-Selected-socioeconomic-indicators-in-C/kn9c-c2s2">Socioeconomic Indicators in Chicago</a>
1. <a href="https://data.cityofchicago.org/Education/Chicago-Public-Schools-Progress-Report-Cards-2011-/9xs2-f89t">Chicago Public Schools</a>
1. <a href="https://data.cityofchicago.org/Public-Safety/Crimes-2001-to-present/ijzp-q8t2">Chicago Crime Data</a>

### 1. Socioeconomic Indicators in Chicago
This dataset contains a selection of six socioeconomic indicators of public health significance and a “hardship index,” for each Chicago community area, for the years 2008 – 2012.

For this assignment you will use a snapshot of this dataset which can be downloaded from:
https://ibm.box.com/shared/static/05c3415cbfbtfnr2fx4atenb2sd361ze.csv

A detailed description of this dataset and the original dataset can be obtained from the Chicago Data Portal at:
https://data.cityofchicago.org/Health-Human-Services/Census-Data-Selected-socioeconomic-indicators-in-C/kn9c-c2s2



### 2. Chicago Public Schools

This dataset shows all school level performance data used to create CPS School Report Cards for the 2011-2012 school year. This dataset is provided by the city of Chicago's Data Portal.

For this assignment you will use a snapshot of this dataset which can be downloaded from:
https://ibm.box.com/shared/static/0g7kbanvn5l2gt2qu38ukooatnjqyuys.csv

A detailed description of this dataset and the original dataset can be obtained from the Chicago Data Portal at:
https://data.cityofchicago.org/Education/Chicago-Public-Schools-Progress-Report-Cards-2011-/9xs2-f89t




### 3. Chicago Crime Data 

This dataset reflects reported incidents of crime (with the exception of murders where data exists for each victim) that occurred in the City of Chicago from 2001 to present, minus the most recent seven days. 

This dataset is quite large - over 1.5GB in size with over 6.5 million rows. For the purposes of this assignment we will use a much smaller sample of this dataset which can be downloaded from:
https://ibm.box.com/shared/static/svflyugsr9zbqy5bmowgswqemfpm1x7f.csv

A detailed description of this dataset and the original dataset can be obtained from the Chicago Data Portal at:
https://data.cityofchicago.org/Public-Safety/Crimes-2001-to-present/ijzp-q8t2


### Download the datasets
In many cases the dataset to be analyzed is available as a .CSV (comma separated values) file, perhaps on the internet. Click on the links below to download and save the datasets (.CSV files):
1. __CENSUS_DATA:__ https://ibm.box.com/shared/static/05c3415cbfbtfnr2fx4atenb2sd361ze.csv
1. __CHICAGO_PUBLIC_SCHOOLS__ https://ibm.box.com/shared/static/0g7kbanvn5l2gt2qu38ukooatnjqyuys.csv
1. __CHICAGO_CRIME_DATA:__ https://ibm.box.com/shared/static/svflyugsr9zbqy5bmowgswqemfpm1x7f.csv

__NOTE:__ Ensure you have downloaded the datasets using the links above instead of directly from the Chicago Data Portal. The versions linked here are subsets of the original datasets and have some of the column names modified to be more database friendly which will make it easier to complete this assignment.

### Store the datasets in database tables
To analyze the data using SQL, it first needs to be stored in the database.

While it is easier to read the dataset into a Pandas dataframe and then PERSIST it into the database as we saw in Week 3 Lab 3, it results in mapping to default datatypes which may not be optimal for SQL querying. For example a long textual field may map to a CLOB instead of a VARCHAR. 

Therefore, __it is highly recommended to manually load the table using the database console LOAD tool, as indicated in Week 2 Lab 1 Part II__. The only difference with that lab is that in Step 5 of the instructions you will need to click on create "(+) New Table" and specify the name of the table you want to create and then click "Next". 

<img src = "https://ibm.box.com/shared/static/uc4xjh1uxcc78ks1i18v668simioz4es.jpg">

##### Now open the Db2 console, open the LOAD tool, Select / Drag the .CSV file for the first dataset, Next create a New Table, and then follow the steps on-screen instructions to load the data. Name the new tables as folows:
1. __CENSUS_DATA__
1. __CHICAGO_PUBLIC_SCHOOLS__
1. __CHICAGO_CRIME_DATA__

### Connect to the database 
Let us first load the SQL extension and establish a connection with the database


```python
%load_ext sql
```

In the next cell enter your db2 connection string. Recall you created Service Credentials for your Db2 instance in Part III of the first lab in the course. From the __uri__ field of your Db2 service credentials copy everything after db2:// (except the double quote at the end) and paste it in the cell below after ibm_db_sa://

<img src ="https://ibm.box.com/shared/static/hzhkvdyinpupm2wfx49lkr71q9swbpec.jpg">


```python
# Remember the connection string is of the format:
# %sql ibm_db_sa://my-username:my-password@my-hostname:my-port/my-db-name
# Enter the connection string for your Db2 on Cloud database instance below
%sql ibm_db_sa://sqg52290:d73%5Ef9mbjhjb1wl3@dashdb-txn-sbox-yp-lon02-01.services.eu-gb.bluemix.net:50000/BLUDB
```




    'Connected: sqg52290@BLUDB'



## Problems
Now write and execute SQL queries to solve assignment problems

### Problem 1

##### How many rows are in each dataset?


```python
# Rows in Census Data (Socieconimic Indicators)
%sql SELECT COUNT(*) FROM CENSUS_DATA
```

     * ibm_db_sa://sqg52290:***@dashdb-txn-sbox-yp-lon02-01.services.eu-gb.bluemix.net:50000/BLUDB
    Done.





<table>
    <tr>
        <th>1</th>
    </tr>
    <tr>
        <td>78</td>
    </tr>
</table>




```python
# Rows in Public Schools
%sql SELECT COUNT(*) FROM CHICAGO_PUBLIC_SCHOOLS
```

     * ibm_db_sa://sqg52290:***@dashdb-txn-sbox-yp-lon02-01.services.eu-gb.bluemix.net:50000/BLUDB
    Done.





<table>
    <tr>
        <th>1</th>
    </tr>
    <tr>
        <td>566</td>
    </tr>
</table>




```python
# Rows in Crime Data
%sql SELECT COUNT(*) FROM CHICAGO_CRIME_DATA
```

     * ibm_db_sa://sqg52290:***@dashdb-txn-sbox-yp-lon02-01.services.eu-gb.bluemix.net:50000/BLUDB
    Done.





<table>
    <tr>
        <th>1</th>
    </tr>
    <tr>
        <td>533</td>
    </tr>
</table>



### Problem 2

##### Find average college enrollments by community area

(When taking a screenshot for sharing, the first 5-10 rows of the result set are sufficient)


```python
%sql SELECT COMMUNITY_AREA_NAME, AVG(COLLEGE_ENROLLMENT) AS AVG_ENROLLMENTS FROM CHICAGO_PUBLIC_SCHOOLS GROUP BY COMMUNITY_AREA_NAME;
```

     * ibm_db_sa://sqg52290:***@dashdb-txn-sbox-yp-lon02-01.services.eu-gb.bluemix.net:50000/BLUDB
    Done.





<table>
    <tr>
        <th>community_area_name</th>
        <th>avg_enrollments</th>
    </tr>
    <tr>
        <td>ALBANY PARK</td>
        <td>858.000000</td>
    </tr>
    <tr>
        <td>ARCHER HEIGHTS</td>
        <td>2411.500000</td>
    </tr>
    <tr>
        <td>ARMOUR SQUARE</td>
        <td>486.000000</td>
    </tr>
    <tr>
        <td>ASHBURN</td>
        <td>810.375000</td>
    </tr>
    <tr>
        <td>AUBURN GRESHAM</td>
        <td>417.500000</td>
    </tr>
    <tr>
        <td>AUSTIN</td>
        <td>475.347826</td>
    </tr>
    <tr>
        <td>AVALON PARK</td>
        <td>507.333333</td>
    </tr>
    <tr>
        <td>AVONDALE</td>
        <td>910.000000</td>
    </tr>
    <tr>
        <td>BELMONT CRAGIN</td>
        <td>1198.833333</td>
    </tr>
    <tr>
        <td>BEVERLY</td>
        <td>409.000000</td>
    </tr>
    <tr>
        <td>BRIDGEPORT</td>
        <td>633.400000</td>
    </tr>
    <tr>
        <td>BRIGHTON PARK</td>
        <td>1205.875000</td>
    </tr>
    <tr>
        <td>BURNSIDE</td>
        <td>549.000000</td>
    </tr>
    <tr>
        <td>CALUMET HEIGHTS</td>
        <td>261.333333</td>
    </tr>
    <tr>
        <td>CHATHAM</td>
        <td>560.222222</td>
    </tr>
    <tr>
        <td>CHICAGO LAWN</td>
        <td>1012.285714</td>
    </tr>
    <tr>
        <td>CLEARING</td>
        <td>521.250000</td>
    </tr>
    <tr>
        <td>DOUGLAS</td>
        <td>424.545454</td>
    </tr>
    <tr>
        <td>DUNNING</td>
        <td>761.333333</td>
    </tr>
    <tr>
        <td>EAST GARFIELD PARK</td>
        <td>410.538461</td>
    </tr>
    <tr>
        <td>EAST SIDE</td>
        <td>1061.000000</td>
    </tr>
    <tr>
        <td>EDGEWATER</td>
        <td>766.666666</td>
    </tr>
    <tr>
        <td>EDISON PARK</td>
        <td>455.000000</td>
    </tr>
    <tr>
        <td>ENGLEWOOD</td>
        <td>401.882352</td>
    </tr>
    <tr>
        <td>FOREST GLEN</td>
        <td>477.000000</td>
    </tr>
    <tr>
        <td>FULLER PARK</td>
        <td>265.500000</td>
    </tr>
    <tr>
        <td>GAGE PARK</td>
        <td>991.500000</td>
    </tr>
    <tr>
        <td>GARFIELD RIDGE</td>
        <td>910.400000</td>
    </tr>
    <tr>
        <td>GRAND BOULEVARD</td>
        <td>351.125000</td>
    </tr>
    <tr>
        <td>GREATER GRAND CROSSING</td>
        <td>405.100000</td>
    </tr>
    <tr>
        <td>HEGEWISCH</td>
        <td>481.500000</td>
    </tr>
    <tr>
        <td>HERMOSA</td>
        <td>993.750000</td>
    </tr>
    <tr>
        <td>HUMBOLDT PARK</td>
        <td>663.076923</td>
    </tr>
    <tr>
        <td>HYDE PARK</td>
        <td>482.500000</td>
    </tr>
    <tr>
        <td>IRVING PARK</td>
        <td>862.666666</td>
    </tr>
    <tr>
        <td>JEFFERSON PARK</td>
        <td>877.500000</td>
    </tr>
    <tr>
        <td>KENWOOD</td>
        <td>612.428571</td>
    </tr>
    <tr>
        <td>LAKE VIEW</td>
        <td>641.363636</td>
    </tr>
    <tr>
        <td>LINCOLN PARK</td>
        <td>802.142857</td>
    </tr>
    <tr>
        <td>LINCOLN SQUARE</td>
        <td>826.400000</td>
    </tr>
    <tr>
        <td>LOGAN SQUARE</td>
        <td>668.272727</td>
    </tr>
    <tr>
        <td>LOOP</td>
        <td>871.000000</td>
    </tr>
    <tr>
        <td>LOWER WEST SIDE</td>
        <td>659.727272</td>
    </tr>
    <tr>
        <td>MCKINLEY PARK</td>
        <td>388.000000</td>
    </tr>
    <tr>
        <td>MONTCLARE</td>
        <td>1317.000000</td>
    </tr>
    <tr>
        <td>MORGAN PARK</td>
        <td>654.200000</td>
    </tr>
    <tr>
        <td>MOUNT GREENWOOD</td>
        <td>522.750000</td>
    </tr>
    <tr>
        <td>NEAR NORTH SIDE</td>
        <td>480.285714</td>
    </tr>
    <tr>
        <td>NEAR SOUTH SIDE</td>
        <td>459.333333</td>
    </tr>
    <tr>
        <td>NEAR WEST SIDE</td>
        <td>498.437500</td>
    </tr>
    <tr>
        <td>NEW CITY</td>
        <td>609.384615</td>
    </tr>
    <tr>
        <td>NORTH CENTER</td>
        <td>1077.285714</td>
    </tr>
    <tr>
        <td>NORTH LAWNDALE</td>
        <td>321.625000</td>
    </tr>
    <tr>
        <td>NORTH PARK</td>
        <td>842.000000</td>
    </tr>
    <tr>
        <td>NORWOOD PARK</td>
        <td>808.625000</td>
    </tr>
    <tr>
        <td>OAKLAND</td>
        <td>140.000000</td>
    </tr>
    <tr>
        <td>OHARE</td>
        <td>786.000000</td>
    </tr>
    <tr>
        <td>PORTAGE PARK</td>
        <td>993.428571</td>
    </tr>
    <tr>
        <td>PULLMAN</td>
        <td>324.000000</td>
    </tr>
    <tr>
        <td>RIVERDALE</td>
        <td>386.750000</td>
    </tr>
    <tr>
        <td>ROGERS PARK</td>
        <td>678.000000</td>
    </tr>
    <tr>
        <td>ROSELAND</td>
        <td>540.000000</td>
    </tr>
    <tr>
        <td>SOUTH CHICAGO</td>
        <td>577.571428</td>
    </tr>
    <tr>
        <td>SOUTH DEERING</td>
        <td>464.750000</td>
    </tr>
    <tr>
        <td>SOUTH LAWNDALE</td>
        <td>672.409090</td>
    </tr>
    <tr>
        <td>SOUTH SHORE</td>
        <td>504.777777</td>
    </tr>
    <tr>
        <td>UPTOWN</td>
        <td>626.857142</td>
    </tr>
    <tr>
        <td>WASHINGTON HEIGHTS</td>
        <td>445.111111</td>
    </tr>
    <tr>
        <td>WASHINGTON PARK</td>
        <td>529.600000</td>
    </tr>
    <tr>
        <td>WEST ELSDON</td>
        <td>1233.333333</td>
    </tr>
    <tr>
        <td>WEST ENGLEWOOD</td>
        <td>457.384615</td>
    </tr>
    <tr>
        <td>WEST GARFIELD PARK</td>
        <td>327.750000</td>
    </tr>
    <tr>
        <td>WEST LAWN</td>
        <td>1051.750000</td>
    </tr>
    <tr>
        <td>WEST PULLMAN</td>
        <td>324.000000</td>
    </tr>
    <tr>
        <td>WEST RIDGE</td>
        <td>910.777777</td>
    </tr>
    <tr>
        <td>WEST TOWN</td>
        <td>471.450000</td>
    </tr>
    <tr>
        <td>WOODLAWN</td>
        <td>525.750000</td>
    </tr>
</table>




```python

```

### Problem 3

##### Find the number of schools that are healthy school certified


```python
%sql SELECT COUNT(*) FROM CHICAGO_PUBLIC_SCHOOLS WHERE HEALTHY_SCHOOL_CERTIFIED='Yes'
```

     * ibm_db_sa://sqg52290:***@dashdb-txn-sbox-yp-lon02-01.services.eu-gb.bluemix.net:50000/BLUDB
    Done.





<table>
    <tr>
        <th>1</th>
    </tr>
    <tr>
        <td>16</td>
    </tr>
</table>



### Problem 4

##### How many observations have a Location Description value of GAS STATION



```python
%sql SELECT COUNT(*) FROM CHICAGO_CRIME_DATA WHERE LOCATION_DESCRIPTION='GAS STATION'
```

     * ibm_db_sa://sqg52290:***@dashdb-txn-sbox-yp-lon02-01.services.eu-gb.bluemix.net:50000/BLUDB
    Done.





<table>
    <tr>
        <th>1</th>
    </tr>
    <tr>
        <td>6</td>
    </tr>
</table>



### Problem 5

##### Retrieve a list of the top 10 community areas which have most number of schools and sorted in descending order.


```python
%sql SELECT COMMUNITY_AREA_NAME, COUNT(NAME_OF_SCHOOL) AS NUMBER_OF_SCHOOLS \
FROM CHICAGO_PUBLIC_SCHOOLS GROUP BY COMMUNITY_AREA_NAME \
ORDER BY COUNT(NAME_OF_SCHOOL) DESC LIMIT 10
```

     * ibm_db_sa://sqg52290:***@dashdb-txn-sbox-yp-lon02-01.services.eu-gb.bluemix.net:50000/BLUDB
    Done.





<table>
    <tr>
        <th>community_area_name</th>
        <th>number_of_schools</th>
    </tr>
    <tr>
        <td>AUSTIN</td>
        <td>23</td>
    </tr>
    <tr>
        <td>SOUTH LAWNDALE</td>
        <td>22</td>
    </tr>
    <tr>
        <td>WEST TOWN</td>
        <td>20</td>
    </tr>
    <tr>
        <td>ENGLEWOOD</td>
        <td>17</td>
    </tr>
    <tr>
        <td>NEAR WEST SIDE</td>
        <td>16</td>
    </tr>
    <tr>
        <td>NORTH LAWNDALE</td>
        <td>16</td>
    </tr>
    <tr>
        <td>EAST GARFIELD PARK</td>
        <td>13</td>
    </tr>
    <tr>
        <td>ROSELAND</td>
        <td>13</td>
    </tr>
    <tr>
        <td>NEW CITY</td>
        <td>13</td>
    </tr>
    <tr>
        <td>HUMBOLDT PARK</td>
        <td>13</td>
    </tr>
</table>



### Problem 6

##### How many observations have value MOTOR VEHICLE THEFT in the Primary Type variable (this is the number of crimes related to Motor vehicles)


```python
%sql SELECT COUNT(*) FROM CHICAGO_CRIME_DATA WHERE PRIMARY_TYPE='MOTOR VEHICLE THEFT'
```

     * ibm_db_sa://sqg52290:***@dashdb-txn-sbox-yp-lon02-01.services.eu-gb.bluemix.net:50000/BLUDB
    Done.





<table>
    <tr>
        <th>1</th>
    </tr>
    <tr>
        <td>24</td>
    </tr>
</table>



### Problem 7

##### Use INNER JOIN to find the __MINIMUM__ (i.e. lowest) “Average Student Attendance” for the community area where hardship is 96. 


```python
%sql SELECT CPS.COMMUNITY_AREA_NAME, \
MIN(CAST((REPLACE(CPS.AVERAGE_STUDENT_ATTENDANCE,'%','')) AS INT)) AS AVG_STUDENT_ATTENDANCE \
FROM CHICAGO_PUBLIC_SCHOOLS AS CPS INNER JOIN CENSUS_DATA AS CD \
ON CPS.COMMUNITY_AREA_NUMBER=CD.COMMUNITY_AREA_NUMBER \
WHERE CD.HARDSHIP_INDEX=96 \
GROUP BY CPS.COMMUNITY_AREA_NAME
```

     * ibm_db_sa://sqg52290:***@dashdb-txn-sbox-yp-lon02-01.services.eu-gb.bluemix.net:50000/BLUDB
    Done.





<table>
    <tr>
        <th>community_area_name</th>
        <th>avg_student_attendance</th>
    </tr>
    <tr>
        <td>SOUTH LAWNDALE</td>
        <td>86</td>
    </tr>
</table>



Hint 1: Look at the values of the columns are you using to JOIN the tables ON. It might be easier to join on numeric fields.

Hint 2: Although not required for the solution, if your select clause has additional columns like "Community Area Name" you will need to use the GROUP BY clause.

Copyright &copy; 2018 [cognitiveclass.ai](cognitiveclass.ai?utm_source=bducopyrightlink&utm_medium=dswb&utm_campaign=bdu). This notebook and its source code are released under the terms of the [MIT License](https://bigdatauniversity.com/mit-license/).

