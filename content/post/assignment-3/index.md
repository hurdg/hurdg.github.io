---
title: Assignment 3
date: '2024-02-07'
---


## Instructions - Read this first!

This is an individual homework assignment. This means that:

- You may discuss the problems in this assignment with other students in this course and your instructor/TA, but YOUR WORK MUST BE YOUR OWN.
- Do not show other students code or your own work on this assignment.
- You may consult external references, but not actively receive help from individuals not involved in this course.
- Cite all references outside of the course you used, including conversations with other students which were helpful. (This helps us give credit where it is due!). All references must use a commonly accepted reference format, for example, APA or IEEE (or another citation style of your choice).

If any of these rules seem ambiguous, please check with with your instructor for help interpreting them.

We suggest completing this assignment using the provided notebook. Each question should be answered using a SQL query (or combination or SQL queries) unless the text indicates that you may (or should) do something else. You may submit your queries embedded in Python, using SQLAlchemy or the MySQL Connector, or as plain text in Markdown.

## When you submit your work

Your submission will be graded manually. To ensure that everything goes smoothly, please follow these instructions to prepare your notebook for submission to the D2L Dropbox for Assignment 3:

- Please remove any print statments used to test your work (you can comment them out)
- Please provide your solutions where asked; please do not alter any other parts of this notebook.
- If you need to add cells to test your code please move them to the end of the notebook before submission- or you may included your commented out answers and tests in the cells provided

## Introduction

In this assignment, you will continue to practice and extend your SQL skills, and compare your work to MongoDB.

We will be using two datasets from the City of Toronto's Open Data Portal. 

 * `earlyOn_info_geom4326.csv` lists locations participating in Ontario's EarlyON program. You can find out more about this dataset from the [City of Toronto's Open Data Portal](https://open.toronto.ca/dataset/earlyon-child-and-family-centres/)
 * `tdsb_info_geom4326.csv` lists all locations of schools that are operated by the Toronto District School Board. You can find out more about this dataset from the [City of Toronto's Open Data Portal](https://open.toronto.ca/dataset/toronto-district-school-board-locations/).
One dataset lists the locations participating in Ontario's EarlyON program.  

For those who are interested, both CSVs use the WGS84 projection for their location data.

Both datasets are licensed under the [Open Government License - Toronto](https://open.toronto.ca/open-data-license/).

You may want to look up both EarlyON and the Toronto District School Board, to acquire more context for this assignment.

## Data cleaning and import

First, import the two CSVs () into your own database. You may use what is available to you on `datasciencedb` or `datasciencedb2`. You may also create indexes and define keys if appropriate for the column(s) of your choice.

In the section below, you have the option to discuss any data cleaning and wrangling steps performed during this process. This is not a requirement and will not be assessed directly for grading; however, this may help to clarify to your reader exactly what was done, to make your work below more understandable.

```python
import pandas as pd
import sqlalchemy as sq

# read in CSV as a dataframe
earlyON_forupload = pd.read_csv('earlyON_info_geom4326.csv')
tdsb_forupload = pd.read_csv('tdsb_info_geom4326.csv')

# connect to database
engine = sq.create_engine('mysql+mysqlconnector://gavinthomas_hurd:9zyd5IPmI7NOf@datasciencedb.ucalgary.ca/gavinthomas_hurd')

# write dataframe into a table
earlyON_forupload.to_sql("earlyon", engine, index=False, if_exists='replace')
tdsb_forupload.to_sql("tdsb", engine, index=False, if_exists='replace')

# test import success
earlyON_df = pd.read_sql_table("earlyon", engine)
tdsb_df = pd.read_sql_table("tdsb", engine)
# and print some information (not the entire table) about your second dataframe
display(earlyON_df.head())
display(tdsb_df.head())

from sqlalchemy import sql
conn = engine.connect()
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
      <th>_id</th>
      <th>service_system_manager</th>
      <th>agency</th>
      <th>loc_id</th>
      <th>program_name</th>
      <th>languages</th>
      <th>french_language_program</th>
      <th>indigenous_program</th>
      <th>programTypes</th>
      <th>serviceName</th>
      <th>...</th>
      <th>email</th>
      <th>contact_fullname</th>
      <th>contact_title</th>
      <th>phone</th>
      <th>contact_email</th>
      <th>dropinHours</th>
      <th>registeredHours</th>
      <th>virtualHours</th>
      <th>geometry</th>
      <th>centre_type</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>City of Toronto</td>
      <td>Regent Park Community Health Centre</td>
      <td>13650</td>
      <td>101 Spruce St EarlyON Child and Family Centre</td>
      <td>None</td>
      <td>None</td>
      <td>None</td>
      <td>None</td>
      <td>Drop-in</td>
      <td>...</td>
      <td>Shani.Halfon@toronto.ca</td>
      <td>Maleda Mulu</td>
      <td>PROGRAM MANAGER</td>
      <td>416-362-0805 X 230</td>
      <td>maledam@regentparkchc.org</td>
      <td>Wednesday: 9:00 a.m. - 11:30 a.m.</td>
      <td>Thursday: 9:00 a.m. - 11:30 a.m.</td>
      <td>NaN</td>
      <td>{'type': 'MultiPoint', 'coordinates': [[-79.36...</td>
      <td>Centre</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>City of Toronto</td>
      <td>West Neighbourhood House O/a St. Christopher H...</td>
      <td>14380</td>
      <td>1033 EarlyON Child and Family Centre</td>
      <td>None</td>
      <td>None</td>
      <td>None</td>
      <td>None</td>
      <td>None</td>
      <td>...</td>
      <td>Shani.Halfon@toronto.ca</td>
      <td>Angela Elzinga Cheng</td>
      <td>DIRECTOR OF COMMUNITY PROGRAMS</td>
      <td>647-438-0038</td>
      <td>angelael@westnh.org</td>
      <td>None</td>
      <td>None</td>
      <td>NaN</td>
      <td>{'type': 'MultiPoint', 'coordinates': [[-79.41...</td>
      <td>Centre</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>City of Toronto</td>
      <td>West Scarborough Neighbourhood Community Centre</td>
      <td>12554</td>
      <td>2555 EarlyON Child and Family Centre</td>
      <td>None</td>
      <td>None</td>
      <td>None</td>
      <td>None</td>
      <td>None</td>
      <td>...</td>
      <td>Jene.Gordon@toronto.ca</td>
      <td>Jessie Chen</td>
      <td>EARLYON COORDINATOR</td>
      <td>416-266-8289</td>
      <td>jessiec@wsncc.org</td>
      <td>Monday: 9:00 a.m. - noon  ; 1:00 p.m. - 3:30 p...</td>
      <td>Monday: 10:00 a.m. - 11:00 a.m.  ; 1:00 p.m. -...</td>
      <td>NaN</td>
      <td>{'type': 'MultiPoint', 'coordinates': [[-79.27...</td>
      <td>Centre</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>City of Toronto</td>
      <td>Macaulay Child Development Centre</td>
      <td>12562</td>
      <td>2700 Dufferin EarlyON Child and Family Centre</td>
      <td>None</td>
      <td>None</td>
      <td>None</td>
      <td>None</td>
      <td>None</td>
      <td>...</td>
      <td>mmcdona5@toronto.ca</td>
      <td>Sandra Aretusi</td>
      <td>SUPERVISOR</td>
      <td>416-789-7441</td>
      <td>saretusi@macaulaycentre.org</td>
      <td>Monday: 10:00 a.m. - noon  ; 3:30 p.m. - 6:00 ...</td>
      <td>Tuesday: 10:00 a.m. - noon   | Thursday: 10:00...</td>
      <td>NaN</td>
      <td>{'type': 'MultiPoint', 'coordinates': [[-79.45...</td>
      <td>Centre</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>City of Toronto</td>
      <td>Regent Park Community Health Centre</td>
      <td>13648</td>
      <td>38 Regent EarlyON Child and Family Centre</td>
      <td>None</td>
      <td>None</td>
      <td>None</td>
      <td>None</td>
      <td>None</td>
      <td>...</td>
      <td>Shani.Halfon@toronto.ca</td>
      <td>Maleda Mulu</td>
      <td>PROGRAM MANAGER</td>
      <td>416-362-0805 X 230</td>
      <td>maledam@regentparkchc.org</td>
      <td>Monday: 9:00 a.m. - 11:30 a.m.  ; 1:00 p.m. - ...</td>
      <td>Monday: 9:00 a.m. - 11:30 a.m.  ; 1:00 p.m. - ...</td>
      <td>NaN</td>
      <td>{'type': 'MultiPoint', 'coordinates': [[-79.36...</td>
      <td>Centre</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 35 columns</p>
</div>

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
      <th>_id</th>
      <th>ID</th>
      <th>SCH_NAME</th>
      <th>ADDRESS</th>
      <th>CITY</th>
      <th>POSTAL_CODE</th>
      <th>MET_PANEL_ID</th>
      <th>ADDRESS_POINT_ID</th>
      <th>ADDRESS_NUMBER</th>
      <th>LINEAR_NAME_FULL</th>
      <th>...</th>
      <th>PLACE_NAME</th>
      <th>GENERAL_USE_CODE</th>
      <th>CENTRELINE_ID</th>
      <th>LO_NUM</th>
      <th>LO_NUM_SUF</th>
      <th>HI_NUM</th>
      <th>HI_NUM_SUF</th>
      <th>LINEAR_NAME_ID</th>
      <th>OBJECTID</th>
      <th>geometry</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>1</td>
      <td>Thorncliffe Park Public School</td>
      <td>80 Thorncliffe Park Drive</td>
      <td>East York</td>
      <td>M4H 1K3</td>
      <td>E</td>
      <td>3829444</td>
      <td>80</td>
      <td>Thorncliffe Park Dr</td>
      <td>...</td>
      <td>Thorncliffe Park Public School</td>
      <td>115001</td>
      <td>3829449</td>
      <td>80</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>354</td>
      <td>1</td>
      <td>{'type': 'MultiPoint', 'coordinates': [[-79.34...</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>2</td>
      <td>Highfield Junior School</td>
      <td>85 Mount Olive Drive</td>
      <td>Etobicoke</td>
      <td>M9V 2C9</td>
      <td>E</td>
      <td>1020028</td>
      <td>85</td>
      <td>Mount Olive Dr</td>
      <td>...</td>
      <td>Highfield Public School</td>
      <td>115001</td>
      <td>906730</td>
      <td>85</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2145</td>
      <td>2</td>
      <td>{'type': 'MultiPoint', 'coordinates': [[-79.58...</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>3</td>
      <td>Victoria Village Public School</td>
      <td>88 Sweeney Drive</td>
      <td>North York</td>
      <td>M4A 1T7</td>
      <td>E</td>
      <td>566907</td>
      <td>88</td>
      <td>Sweeney Dr</td>
      <td>...</td>
      <td>Victoria Village Public School</td>
      <td>115001</td>
      <td>442884</td>
      <td>88</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>6834</td>
      <td>3</td>
      <td>{'type': 'MultiPoint', 'coordinates': [[-79.31...</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>4</td>
      <td>Grenoble Public School</td>
      <td>9 Grenoble Drive</td>
      <td>North York</td>
      <td>M3C 1C3</td>
      <td>E</td>
      <td>523752</td>
      <td>9</td>
      <td>Grenoble Dr</td>
      <td>...</td>
      <td>Grenoble Public School</td>
      <td>115001</td>
      <td>30093281</td>
      <td>9</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>5756</td>
      <td>4</td>
      <td>{'type': 'MultiPoint', 'coordinates': [[-79.33...</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>5</td>
      <td>Manhattan Park Junior Public School</td>
      <td>90 Manhattan Drive</td>
      <td>Scarborough</td>
      <td>M1R 3V8</td>
      <td>E</td>
      <td>359023</td>
      <td>90</td>
      <td>Manhattan Dr</td>
      <td>...</td>
      <td>Manhattan Park</td>
      <td>115001</td>
      <td>109829</td>
      <td>90</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>8407</td>
      <td>5</td>
      <td>{'type': 'MultiPoint', 'coordinates': [[-79.29...</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 22 columns</p>
</div>

## Part A: Warm-up Questions (10 marks)

Answer the questions below, including any queries you used where necessary. Not all questions will necessarily require a SQL query for a correct response. You may wish to use as a source the references which are already provided as part of this notebook.

First, let's look at the Toronto District School Board Data.

1. How many schools are included in this file? **(1 mark)**

2.  While not all columns are equally interesting, let's focus on a few columns. Name three columns which could be used as a primary key for this dataset. **(3 marks)**

 
3. How would you describe the difference between the CITY and the MUNICIPALITY column? (Hint: You may wish to use information [at this link](https://www.toronto.ca/city-government/accountability-operations-customer-service/access-city-information-or-records/city-of-toronto-archives/using-the-archives/research-by-topic/resources-on-former-municipalities/) as a starting point **(2 marks)**

```python
#1. Number of schools

output = conn.execute(sql.text(''' -- sql 
                               SELECT COUNT(DISTINCT sch_name) FROM tdsb 
                               WHERE sch_name IS NOT NULL;
                               '''))
print(output.fetchall())

#585 unique schools
```

    [(585,)]
    

1. Based on the above SQL query, there are 585 distinct schools in the file.
2. As there are the same number of records in the dataset as their are unique schools, we each entry in the dataset cooresponds to a distinct school. Based on this finding, the school name (*sch_name*), address (*ADDRESS_FULL*), and municipality (*MUNICIPALITY*) could serve as a primary key.
3. It appears that the *CITY* column refers to the same geospatial locations as the *MUNICIPALITY* column, but the former contains the areas name prior to their conversion into a municipality of the Greater Toronto Area.

Next, let's look at the EarlyON table. 

4. How many different programs in total are listed in this dataset? **(1 mark)**

5. Are there any programs without websites listed? **(1 mark)**

6. Which columns would be suitable primary keys for this table? **(1 mark)**

7. Which major_intersection is listed for the most programs (and is not null)? Include the name of the intersection with the number of services. **(1 mark)**

```python
#4. Number of programs
output = conn.execute(sql.text(''' -- sql 
                               SELECT COUNT(DISTINCT program_name) FROM earlyon 
                               WHERE program_name IS NOT NULL;
                               '''))
print(output.fetchall())

#253 unique programs

#5. Programs without website
output = conn.execute(sql.text(''' -- sql 
                               SELECT COUNT(DISTINCT program_name) FROM earlyon 
                               WHERE website IS NULL;
                               '''))
print(output.fetchall())
#68 programs without website

#7. Major intersection associated with most programs
output = conn.execute(sql.text(''' -- sql 
select major_intersection, COUNT(program_name)
from earlyon
group by major_intersection
order by COUNT(program_name) desc
LIMIT 2;
                               ;
                               '''))
print(output.fetchall())
#King St W/shaw St appears to be the most common non-null major intersection, 
# #although with a count of 1 it is likley tied with all other non-null intersections.
```

    [(253,)]
    [(68,)]
    [(None, 219), ('King St W/shaw St', 1)]
    

4. Based upon the above query, there are 253 unique programs in the dataset.
5. There appears to be 68 programs that do not have an associated website.
6. The program names (*program_name*) column appear to be a good candidate as a primary key, however there are more records than unique values (as seen in q4). Therefore the *program_name* column might best be used in combination with the *_id* field to act as a primary key. Latly, the *address* in combination with the *_id* field could also be used as a primary key.
7. From the final SQL query result (above) we see that the *major_intersection* value that is associated with the most programs is actually *NULL*. The greatest non-null value is *King St W/shaw St*, however it only has a value of 1 and therefore is likley tied with many other major intersections.

## Part B: SQL with multiple tables (13 marks)

How many French-language EarlyON programs are currently operating in schools, and in what municipalities? **(2 marks)**

Provide a breakdown of schools offering EarlyON programs, by school type (for example, elementary, or secondary). include the ID for both the EarlyON program and the school.  **(2 marks)**

List all schools offering EarlyON programs which do not belong to the Toronto District School Board. **(1 mark)**

List all schools belonging to the Toronto District School Board which do not offer EarlyON programs **(1 mark)**

Out of all programs offered, what percentage of programTypes (Drop-in, Registered, Virtual) are offered by the EarlyON programs? Does this differ for programs housed in TDSB schools, compared to those which are not? **(2 marks)**

Prepare a list of community information in a single table, as follows: **(5 marks)**
- All EarlyOn programs should be listed, using the name of the program, and the full address.
- Where an EarlyON is hosted at a TDSB school, include the municipality, place name, and whether the school is an elementary school or secondary school.
- Any TDSB school that does not host an EarlyON program should be listed, using the address, municipality and place name.

```python
#How many French-language EarlyON programs are currently operating in schools, and in what municipalities? 

output = conn.execute(sql.text(''' -- sql 
SELECT t.municipality, e.address FROM
(select french_language_program, `address`
from earlyon
WHERE french_language_program = 'Yes') as e
inner join
(select municipality, 'address'
FROM tdsb) as t
ON e.address = t.address;
                               '''))
print(output.fetchall())
#3 french language programs, none are operating in schools
```

    ---------------------------------------------------------------------------

    OperationalError                          Traceback (most recent call last)

    File c:\Users\hurdg\anaconda3\Lib\site-packages\sqlalchemy\engine\base.py:1814, in Connection._execute_context(self, dialect, constructor, statement, parameters, execution_options, *args, **kw)
       1812         conn = self._revalidate_connection()
    -> 1814     context = constructor(
       1815         dialect, self, conn, execution_options, *args, **kw
       1816     )
       1817 except (exc.PendingRollbackError, exc.ResourceClosedError):
    

    File c:\Users\hurdg\anaconda3\Lib\site-packages\sqlalchemy\engine\default.py:1399, in DefaultExecutionContext._init_compiled(cls, dialect, connection, dbapi_connection, execution_options, compiled, parameters, invoked_statement, extracted_parameters, cache_hit)
       1397 self.unicode_statement = compiled.string
    -> 1399 self.cursor = self.create_cursor()
       1401 if self.compiled.insert_prefetch or self.compiled.update_prefetch:
    

    File c:\Users\hurdg\anaconda3\Lib\site-packages\sqlalchemy\engine\default.py:1723, in DefaultExecutionContext.create_cursor(self)
       1722 self._is_server_side = False
    -> 1723 return self.create_default_cursor()
    

    File c:\Users\hurdg\anaconda3\Lib\site-packages\sqlalchemy\engine\default.py:1729, in DefaultExecutionContext.create_default_cursor(self)
       1728 def create_default_cursor(self):
    -> 1729     return self._dbapi_connection.cursor()
    

    File c:\Users\hurdg\anaconda3\Lib\site-packages\sqlalchemy\pool\base.py:1491, in _ConnectionFairy.cursor(self, *args, **kwargs)
       1490 assert self.dbapi_connection is not None
    -> 1491 return self.dbapi_connection.cursor(*args, **kwargs)
    

    File c:\Users\hurdg\anaconda3\Lib\site-packages\mysql\connector\connection_cext.py:706, in CMySQLConnection.cursor(self, buffered, raw, prepared, cursor_class, dictionary, named_tuple)
        705 if not self.is_connected():
    --> 706     raise OperationalError("MySQL Connection not available.")
        707 if cursor_class is not None:
    

    OperationalError: MySQL Connection not available.

    
    The above exception was the direct cause of the following exception:
    

    OperationalError                          Traceback (most recent call last)

    c:\Users\hurdg\Documents\MDSA\DATA 604\Assignment\Assignment 3\Assignment 3.ipynb Cell 12 line 3
          <a href='vscode-notebook-cell:/c%3A/Users/hurdg/Documents/MDSA/DATA%20604/Assignment/Assignment%203/Assignment%203.ipynb#X31sZmlsZQ%3D%3D?line=0'>1</a> #How many French-language EarlyON programs are currently operating in schools, and in what municipalities? 
    ----> <a href='vscode-notebook-cell:/c%3A/Users/hurdg/Documents/MDSA/DATA%20604/Assignment/Assignment%203/Assignment%203.ipynb#X31sZmlsZQ%3D%3D?line=2'>3</a> output = conn.execute(sql.text(''' -- sql 
          <a href='vscode-notebook-cell:/c%3A/Users/hurdg/Documents/MDSA/DATA%20604/Assignment/Assignment%203/Assignment%203.ipynb#X31sZmlsZQ%3D%3D?line=3'>4</a> SELECT t.municipality, e.address FROM
          <a href='vscode-notebook-cell:/c%3A/Users/hurdg/Documents/MDSA/DATA%20604/Assignment/Assignment%203/Assignment%203.ipynb#X31sZmlsZQ%3D%3D?line=4'>5</a> (select french_language_program, `address`
          <a href='vscode-notebook-cell:/c%3A/Users/hurdg/Documents/MDSA/DATA%20604/Assignment/Assignment%203/Assignment%203.ipynb#X31sZmlsZQ%3D%3D?line=5'>6</a> from earlyon
          <a href='vscode-notebook-cell:/c%3A/Users/hurdg/Documents/MDSA/DATA%20604/Assignment/Assignment%203/Assignment%203.ipynb#X31sZmlsZQ%3D%3D?line=6'>7</a> WHERE french_language_program = 'Yes') as e
          <a href='vscode-notebook-cell:/c%3A/Users/hurdg/Documents/MDSA/DATA%20604/Assignment/Assignment%203/Assignment%203.ipynb#X31sZmlsZQ%3D%3D?line=7'>8</a> inner join
          <a href='vscode-notebook-cell:/c%3A/Users/hurdg/Documents/MDSA/DATA%20604/Assignment/Assignment%203/Assignment%203.ipynb#X31sZmlsZQ%3D%3D?line=8'>9</a> (select municipality, 'address'
         <a href='vscode-notebook-cell:/c%3A/Users/hurdg/Documents/MDSA/DATA%20604/Assignment/Assignment%203/Assignment%203.ipynb#X31sZmlsZQ%3D%3D?line=9'>10</a> FROM tdsb) as t
         <a href='vscode-notebook-cell:/c%3A/Users/hurdg/Documents/MDSA/DATA%20604/Assignment/Assignment%203/Assignment%203.ipynb#X31sZmlsZQ%3D%3D?line=10'>11</a> ON e.address = t.address;
         <a href='vscode-notebook-cell:/c%3A/Users/hurdg/Documents/MDSA/DATA%20604/Assignment/Assignment%203/Assignment%203.ipynb#X31sZmlsZQ%3D%3D?line=11'>12</a>                                '''))
         <a href='vscode-notebook-cell:/c%3A/Users/hurdg/Documents/MDSA/DATA%20604/Assignment/Assignment%203/Assignment%203.ipynb#X31sZmlsZQ%3D%3D?line=12'>13</a> print(output.fetchall())
         <a href='vscode-notebook-cell:/c%3A/Users/hurdg/Documents/MDSA/DATA%20604/Assignment/Assignment%203/Assignment%203.ipynb#X31sZmlsZQ%3D%3D?line=13'>14</a> #3 french language programs, none are operating in schools
    

    File c:\Users\hurdg\anaconda3\Lib\site-packages\sqlalchemy\engine\base.py:1416, in Connection.execute(self, statement, parameters, execution_options)
       1414     raise exc.ObjectNotExecutableError(statement) from err
       1415 else:
    -> 1416     return meth(
       1417         self,
       1418         distilled_parameters,
       1419         execution_options or NO_OPTIONS,
       1420     )
    

    File c:\Users\hurdg\anaconda3\Lib\site-packages\sqlalchemy\sql\elements.py:516, in ClauseElement._execute_on_connection(self, connection, distilled_params, execution_options)
        514     if TYPE_CHECKING:
        515         assert isinstance(self, Executable)
    --> 516     return connection._execute_clauseelement(
        517         self, distilled_params, execution_options
        518     )
        519 else:
        520     raise exc.ObjectNotExecutableError(self)
    

    File c:\Users\hurdg\anaconda3\Lib\site-packages\sqlalchemy\engine\base.py:1639, in Connection._execute_clauseelement(self, elem, distilled_parameters, execution_options)
       1627 compiled_cache: Optional[CompiledCacheType] = execution_options.get(
       1628     "compiled_cache", self.engine._compiled_cache
       1629 )
       1631 compiled_sql, extracted_params, cache_hit = elem._compile_w_cache(
       1632     dialect=dialect,
       1633     compiled_cache=compiled_cache,
       (...)
       1637     linting=self.dialect.compiler_linting | compiler.WARN_LINTING,
       1638 )
    -> 1639 ret = self._execute_context(
       1640     dialect,
       1641     dialect.execution_ctx_cls._init_compiled,
       1642     compiled_sql,
       1643     distilled_parameters,
       1644     execution_options,
       1645     compiled_sql,
       1646     distilled_parameters,
       1647     elem,
       1648     extracted_params,
       1649     cache_hit=cache_hit,
       1650 )
       1651 if has_events:
       1652     self.dispatch.after_execute(
       1653         self,
       1654         elem,
       (...)
       1658         ret,
       1659     )
    

    File c:\Users\hurdg\anaconda3\Lib\site-packages\sqlalchemy\engine\base.py:1820, in Connection._execute_context(self, dialect, constructor, statement, parameters, execution_options, *args, **kw)
       1818     raise
       1819 except BaseException as e:
    -> 1820     self._handle_dbapi_exception(
       1821         e, str(statement), parameters, None, None
       1822     )
       1824 if (
       1825     self._transaction
       1826     and not self._transaction.is_active
       (...)
       1830     )
       1831 ):
       1832     self._invalid_transaction()
    

    File c:\Users\hurdg\anaconda3\Lib\site-packages\sqlalchemy\engine\base.py:2343, in Connection._handle_dbapi_exception(self, e, statement, parameters, cursor, context, is_sub_exec)
       2341 elif should_wrap:
       2342     assert sqlalchemy_exception is not None
    -> 2343     raise sqlalchemy_exception.with_traceback(exc_info[2]) from e
       2344 else:
       2345     assert exc_info[1] is not None
    

    File c:\Users\hurdg\anaconda3\Lib\site-packages\sqlalchemy\engine\base.py:1814, in Connection._execute_context(self, dialect, constructor, statement, parameters, execution_options, *args, **kw)
       1811     if conn is None:
       1812         conn = self._revalidate_connection()
    -> 1814     context = constructor(
       1815         dialect, self, conn, execution_options, *args, **kw
       1816     )
       1817 except (exc.PendingRollbackError, exc.ResourceClosedError):
       1818     raise
    

    File c:\Users\hurdg\anaconda3\Lib\site-packages\sqlalchemy\engine\default.py:1399, in DefaultExecutionContext._init_compiled(cls, dialect, connection, dbapi_connection, execution_options, compiled, parameters, invoked_statement, extracted_parameters, cache_hit)
       1395             self.execute_style = ExecuteStyle.EXECUTEMANY
       1397 self.unicode_statement = compiled.string
    -> 1399 self.cursor = self.create_cursor()
       1401 if self.compiled.insert_prefetch or self.compiled.update_prefetch:
       1402     self._process_execute_defaults()
    

    File c:\Users\hurdg\anaconda3\Lib\site-packages\sqlalchemy\engine\default.py:1723, in DefaultExecutionContext.create_cursor(self)
       1721 else:
       1722     self._is_server_side = False
    -> 1723     return self.create_default_cursor()
    

    File c:\Users\hurdg\anaconda3\Lib\site-packages\sqlalchemy\engine\default.py:1729, in DefaultExecutionContext.create_default_cursor(self)
       1728 def create_default_cursor(self):
    -> 1729     return self._dbapi_connection.cursor()
    

    File c:\Users\hurdg\anaconda3\Lib\site-packages\sqlalchemy\pool\base.py:1491, in _ConnectionFairy.cursor(self, *args, **kwargs)
       1489 def cursor(self, *args: Any, **kwargs: Any) -> DBAPICursor:
       1490     assert self.dbapi_connection is not None
    -> 1491     return self.dbapi_connection.cursor(*args, **kwargs)
    

    File c:\Users\hurdg\anaconda3\Lib\site-packages\mysql\connector\connection_cext.py:706, in CMySQLConnection.cursor(self, buffered, raw, prepared, cursor_class, dictionary, named_tuple)
        704 self.handle_unread_result(prepared)
        705 if not self.is_connected():
    --> 706     raise OperationalError("MySQL Connection not available.")
        707 if cursor_class is not None:
        708     if not issubclass(cursor_class, CMySQLCursor):
    

    OperationalError: (mysql.connector.errors.OperationalError) MySQL Connection not available.
    [SQL:  -- sql 
    SELECT t.municipality, e.address FROM
    (select french_language_program, `address`
    from earlyon
    WHERE french_language_program = 'Yes') as e
    inner join
    (select municipality, 'address'
    FROM tdsb) as t
    ON e.address = t.address;
                                   ]
    (Background on this error at: https://sqlalche.me/e/20/e3q8)

```python
#Provide a breakdown of schools offering EarlyON programs, by school type (for example, elementary, or secondary). 
#include the ID for both the EarlyON program and the school.

output = conn.execute(sql.text(''' -- sql 
select e._id, t.id, t.sch_name, CASE
        WHEN sch_name LIKE '%high%' or sch_name LIKE '%secondary%' or sch_name LIKE '%senior%' THEN 'Secondary'
        WHEN sch_name LIKE '%middle%' THEN 'Middle'   
        WHEN sch_name LIKE '%elementary%' or sch_name LIKE  '%primary%' or sch_name LIKE  '%junior%' THEN 'Elementary'                                                           
        ELSE 'unknown'
    END as class from
(select `address`, _id
from earlyon) as e
inner join
(select `address`, sch_name, id
from tdsb) as t
ON e.address = t.address;
                               '''))
for i in output.fetchall():
    print(i)
```

    (93, 220, 'H A Halbert Junior Public School', 'Elementary')
    (88, 460, 'George Webster Elementary School', 'Elementary')
    (13, 492, 'Agnes Macphail Public School', 'unknown')
    (22, 501, 'Banting and Best Public School', 'unknown')
    (172, 502, 'Port Royal Public School', 'unknown')
    (141, 505, 'Military Trail Public School', 'unknown')
    (136, 538, 'Market Lane Junior and Senior Public School', 'Secondary')
    (253, 539, 'Yorkwoods Public School', 'unknown')
    

```python
#List all schools offering EarlyON programs which do not belong to the Toronto District School Board.
output = conn.execute(sql.text(''' -- sql 
select school_name FROM
earlyon
where school_name not in (
SELECT distinct(sch_name) from tdsb);
                               '''))
for i in output.fetchall():
    print(i)
```

    ('BRUCE JUNIOR PUBLIC SCHOOL',)
    ('CARLETON VILLAGE JUNIOR AND SENIOR PUBLIC SCHOOL',)
    ('ACADÉMIE ALEXANDRE-DUMAS',)
    ('ÉCOLE ÉLÉMENTAIRE GABRIELLE-ROY',)
    ('CHARLES E WEBSTER JUNIOR PUBLIC SCHOOL',)
    ('DR RITA COX - KINA MINOGOK PUBLIC SCHOOL',)
    ('EASTVIEW JUNIOR PUBLIC SCHOOL',)
    ('ETOBICOKE SECONDARY ALTERNATIVE SCHOOL',)
    ('HOLY SPIRIT CATHOLIC SCHOOL',)
    ("D'ARCY MCGEE CATHOLIC SCHOOL",)
    ('KNOB HILL JUNIOR PUBLIC SCHOOL',)
    ('MORSE',)
    ('OUR LADY OF THE ASSUMPTION CATHOLIC SCHOOL',)
    ('RODEN JUNIOR PUBLIC SCHOOL',)
    ('ST. PAUL CATHOLIC SCHOOL',)
    ('SPRUCECOURT JUNIOR PUBLIC SCHOOL',)
    ('ST. AIDAN CATHOLIC SCHOOL',)
    ('ST. ALBERT CATHOLIC SCHOOL',)
    ('ST. ANGELA CATHOLIC SCHOOL',)
    ('ST. ANTHONY CATHOLIC SCHOOL',)
    ('ST. BARBARA CATHOLIC SCHOOL',)
    ('ST. BARNABAS CATHOLIC SCHOOL',)
    ('ST. CHARLES GARNIER CATHOLIC SCHOOL',)
    ('ST. DOROTHY CATHOLIC SCHOOL',)
    ('ST. FRANCIS DE SALES CATHOLIC SCHOOL',)
    ('ST. HELEN CATHOLIC SCHOOL',)
    ('ST. JOHN XXIII CATHOLIC SCHOOL',)
    ('ST. LEO CATHOLIC SCHOOL',)
    ('ST. MARGUERITE BOURGEOYS CATHOLIC SCHOOL',)
    ('SAINT PAUL VI CATHOLIC SCHOOL',)
    ('ST. ROSE OF LIMA CATHOLIC SCHOOL',)
    ('ST. WILFRID CATHOLIC SCHOOL',)
    ('ST. COLUMBA CATHOLIC SCHOOL',)
    ('WHITE HAVEN JUNIOR PUBLIC SCHOOL',)
    

```python
#List all schools belonging to the Toronto District School Board which do not offer EarlyON programs

output = conn.execute(sql.text(''' -- sql 
SELECT t.sch_name
FROM tdsb t
LEFT JOIN earlyon e ON t.sch_name = e.school_name
WHERE e.school_name IS NULL;
                               '''))
for i in output.fetchall():
    print(i)
```

    ('Thorncliffe Park Public School',)
    ('Victoria Village Public School',)
    ('Manhattan Park Junior Public School',)
    ('Brian Public School',)
    ('North Preparatory Junior Public School',)
    ('Bliss Carman Senior Public School',)
    ('Bloordale Middle School',)
    ('Courcelette Public School',)
    ('Derrydown Public School',)
    ('Maplewood High School',)
    ('George B Little Public School',)
    ('John G Althouse Middle School',)
    ('Warren Park Junior Public School',)
    ('Humbercrest Public School',)
    ('Cliffwood Public School',)
    ('Cedarvale Community School',)
    ('Greenland Public School',)
    ('Victoria Park Collegiate Institute',)
    ('White Haven Public School',)
    ('Seventh Street Junior School',)
    ('Norseman Junior Middle School',)
    ('Lillian Public School',)
    ('Riverdale Collegiate Institute',)
    ('King Edward Junior and Senior Public School',)
    ('Bloor Collegiate Institute',)
    ('Monarch Park Collegiate Institute',)
    ('C R Marchant Middle School',)
    ('Central Etobicoke High School',)
    ('Weston Collegiate Institute',)
    ('Contact Alternative School',)
    ('Scarborough Village Public School',)
    ('Winchester Junior and Senior Public School',)
    ('George Harvey Collegiate Institute',)
    ('Orde Street Public School',)
    ('Inglenook Community School',)
    ('Charles E Webster Public School',)
    ('ALPHA Alternative Junior School',)
    ('Oasis Alternative Secondary School (Arts & Social Change Program)',)
    ('Eastview Public School',)
    ('Weston Memorial Junior Public School',)
    ('George R Gauld Junior School',)
    ('Niagara Street Junior Public School',)
    ('Fairbank Public School',)
    ('Leslieville Junior Public School',)
    ('Annette Street Junior and Senior Public School',)
    ('High Park Alternative Junior School',)
    ('Lucy McCormick Senior School',)
    ('Downsview Public School',)
    ('Harbord Collegiate Institute',)
    ('H J Alexander Community School',)
    ('Howard Junior Public School',)
    ('Carleton Village Junior and Senior Sports and Wellness Academy',)
    ('David Hornell Junior School',)
    ('Ogden Junior Public School',)
    ('Lord Lansdowne Junior Public School',)
    ('City View Alternative Senior School',)
    ('School of Experiential Education',)
    ('Horizon Alternative Senior School',)
    ('Kensington Community School',)
    ('Bloorlea Middle School',)
    ('Inglewood Heights Junior Public School',)
    ('Brookview Middle School',)
    ('Tumpane Public School',)
    ('Jarvis Collegiate Institute',)
    ('Hawthorne II Bilingual Alternative Junior School',)
    ('Harwood Public School',)
    ('Huron Street Junior Public School',)
    ('Cedarbrook Public School',)
    ('Runnymede Collegiate Institute',)
    ('Central Toronto Academy',)
    ('Zion Heights Middle School',)
    ('Beverley School',)
    ('Heydon Park Secondary School',)
    ('Sprucecourt Public School',)
    ('Eastdale Collegiate Institute',)
    ('Rosedale Heights School of the Arts',)
    ('Central Technical School',)
    ('West End Alternative School',)
    ('Downtown Alternative School',)
    ('Ursula Franklin Academy',)
    ('James S Bell Junior Middle Sports and Wellness Academy',)
    ('Brock Public School',)
    ('First Nations School of Toronto',)
    ('Ledbury Park Elementary and Middle School',)
    ('Winona Drive Senior Public School',)
    ('Steelesview Public School',)
    ('Sir William Osler High School',)
    ('Tredway Woodsworth Public School',)
    ('McMurrich Junior Public School',)
    ('Elkhorn Public School',)
    ('Milne Valley Middle School',)
    ('Crestview Public School',)
    ('Whitney Junior Public School',)
    ('Henry Kelsey Senior Public School',)
    ('Lawrence Park Collegiate Institute',)
    ('Yorkview Public School',)
    ('Fenside Public School',)
    ('West Humber Junior Middle School',)
    ("Hunter's Glen Junior Public School",)
    ('Lord Roberts Junior Public School',)
    ('Pleasant View Middle School',)
    ('Smithfield Middle School',)
    ('Dallington Public School',)
    ('Pelmo Park Public School',)
    ('Rosethorn Junior School',)
    ('Dunlace Public School',)
    ('Donview Middle Health and Wellness Academy',)
    ('George S Henry Academy',)
    ('John D Parker Junior School',)
    ('Bessborough Drive Elementary and Middle School',)
    ('Rosedale Junior Public School',)
    ('Wellesworth Junior School',)
    ('Daystrom Public School',)
    ('Westway Junior School',)
    ('Beverley Heights Middle School',)
    ('Cliffside Public School',)
    ('Rivercrest Junior School',)
    ('Oakdale Park Middle School',)
    ('Calico Public School',)
    ('Valleyfield Junior School',)
    ('Hilltop Middle School',)
    ('Claireville Junior School',)
    ('Maurice Cody Junior Public School',)
    ('Broadacres Junior School',)
    ('Cresthaven Public School',)
    ('Bellmere Junior Public School',)
    ('A Y Jackson Secondary School',)
    ('Elmlea Junior School',)
    ('Stilecroft Public School',)
    ('Vradenburg Junior Public School',)
    ('Martingrove Collegiate Institute',)
    ('Chine Drive Public School',)
    ('Melody Village Junior School',)
    ('Lambton-Kingsway Junior Middle School',)
    ('Arbor Glen Public School',)
    ('Alternative Scarborough Education 1',)
    ('St Andrews Public School',)
    ('George P Mackie Junior Public School',)
    ('Briarcrest Junior School',)
    ('Bendale Junior Public School',)
    ('Seneca Hill Public School',)
    ('Hollycrest Middle School',)
    ('Princess Margaret Junior School',)
    ('Brookhaven Public School',)
    ('Beaumonde Heights Junior Middle School',)
    ('West Preparatory Junior Public School',)
    ('Park Lawn Junior Middle School',)
    ('Churchill Heights Public School',)
    ('Bennington Heights Elementary School',)
    ('Three Valleys Public School',)
    ('Lester B Pearson Collegiate Institute',)
    ('Roden Public School',)
    ('Birch Cliff Public School',)
    ('West Humber Collegiate Institute',)
    ('Diefenbaker Elementary School',)
    ('Glen Ames Senior Public School',)
    ('Galloway Road Public School',)
    ('North Kipling Junior Middle School',)
    ('Gulfstream Public School',)
    ('Rene Gordon Health and Wellness Academy',)
    ('Fleming Public School',)
    ('William Lyon Mackenzie Collegiate Institute',)
    ('Leaside High School',)
    ('Highland Middle School',)
    ('Rippleton Public School',)
    ('Elia Middle School',)
    ('Millwood Junior School',)
    ('Woburn Collegiate Institute',)
    ('Guildwood Junior Public School',)
    ('Willowdale Middle School',)
    ('Williamson Road Junior Public School',)
    ('Stephen Leacock Collegiate Institute',)
    ('John Buchan Senior Public School',)
    ('Forest Manor Public School',)
    ('Muirhead Public School',)
    ('King George Junior Public School',)
    ('Centennial Road Junior Public School',)
    ('Dorset Park Public School',)
    ('Woodbine Middle School',)
    ('Silverthorn Collegiate Institute',)
    ('Rouge Valley Public School',)
    ('General Crerar Public School',)
    ('F H Miller Junior Public School',)
    ('Georges Vanier Secondary School',)
    ('Northlea Elementary and Middle School',)
    ('Rolph Road Elementary School',)
    ('Fairmount Public School',)
    ('Don Valley Middle School',)
    ('Sir Alexander Mackenzie Senior Public School',)
    ('Emery Collegiate Institute',)
    ('Lescon Public School',)
    ('C W Jefferys Collegiate Institute',)
    ('Henry Hudson Senior Public School',)
    ('Runnymede Junior and Senior Public School',)
    ('Windfields Middle School',)
    ('Buchanan Public School',)
    ('Adam Beck Junior Public School',)
    ('Mill Valley Junior School',)
    ('Ancaster Public School',)
    ('Albion Heights Junior Middle School',)
    ('Sunny View Junior and Senior Public School',)
    ('Brown Junior Public School',)
    ('J G Workman Public School',)
    ('York Mills Collegiate Institute',)
    ('Etienne Brule Junior School',)
    ('Norman Ingram Public School',)
    ('Cedarbrae Collegiate Institute',)
    ('Seneca School',)
    ('William G Miller Public School',)
    ('North Agincourt Junior Public School',)
    ('Jesse Ketchum Junior and Senior Public School',)
    ('Humber Valley Village Junior Middle School',)
    ('Etobicoke School of the Arts',)
    ('Downsview Secondary School',)
    ('John G Diefenbaker Public School',)
    ('Tecumseh Senior Public School',)
    ('Norman Cook Junior Public School',)
    ('Golf Road Junior Public School',)
    ('Ellesmere-Statton Public School',)
    ('Stanley Public School',)
    ('Westview Centennial Secondary School',)
    ('Meadowvale Public School',)
    ('Forest Hill Junior and Senior Public School',)
    ('Bowmore Road Junior and Senior Public School',)
    ('Harrison Public School',)
    ('Northern Secondary School',)
    ('Etobicoke Collegiate Institute',)
    ('Ionview Public School',)
    ('Robert Service Senior Public School',)
    ('Westmount Junior School',)
    ('Regal Road Junior Public School',)
    ('Sloane Public School',)
    ('Chester Elementary School',)
    ('Selwyn Elementary School',)
    ('George Peck Public School',)
    ('Dublin Heights Elementary and Middle School',)
    ('Queen Victoria Public School',)
    ('York Humber High School',)
    ('Pauline Junior Public School',)
    ('Earl Haig Secondary School',)
    ('Earl Grey Senior Public School',)
    ('William Burgess Elementary School',)
    ('Glen Park Public School',)
    ('Kew Beach Junior Public School',)
    ('Wexford Collegiate School for the Arts',)
    ('Birch Cliff Heights Public School',)
    ('Pierre Laporte Middle School',)
    ('William G Davis Junior Public School',)
    ('Fern Avenue Junior and Senior Public School',)
    ('Lynngate Junior Public School',)
    ('John Ross Robertson Junior Public School',)
    ("St Andrew's Middle School",)
    ('Maryvale Public School',)
    ('Balmy Beach Community School',)
    ('Perth Avenue Junior Public School',)
    ('Baycrest Public School',)
    ('Victoria Park Elementary School',)
    ('Armour Heights Public School',)
    ('Humewood Community School',)
    ('Earl Haig Public School',)
    ('Newtonbrook Secondary School',)
    ('Bendale Business and Technical Institute',)
    ('Subway Academy I',)
    ('Elizabeth Simcoe Junior Public School',)
    ('Avondale Elementary Alternative School',)
    ('Avondale Secondary Alternative School',)
    ('Richview Collegiate Institute',)
    ('Cordella Junior Public School',)
    ('Gracefield Public School',)
    ('Morse Street Junior Public School',)
    ('Queen Alexandra Middle School',)
    ('Churchill Public School',)
    ('Keelesdale Junior Public School',)
    ('Amesbury Middle School',)
    ('Swansea Junior and Senior Public School',)
    ('Parkdale Collegiate Institute',)
    ('Blake Street Junior Public School',)
    ('East Alternative School of Toronto',)
    ('Samuel Hearne Middle School',)
    ('Cameron Public School',)
    ('Highview Public School',)
    ('Fairglen Junior Public School',)
    ('Eglinton Junior Public School',)
    ('Winston Churchill Collegiate Institute',)
    ('Garden Avenue Junior Public School',)
    ('R J Lang Elementary and Middle School',)
    ('Dovercourt Public School',)
    ('Deer Park Junior and Senior Public School',)
    ('Sir John A Macdonald Collegiate Institute',)
    ('Rawlinson Community School',)
    ('Greenwood Secondary School',)
    ('School of Life Experience',)
    ('John Wanless Junior Public School',)
    ('Hillmount Public School',)
    ('Withrow Avenue Junior Public School',)
    ('Quest Alternative Senior School',)
    ('Blaydon Public School',)
    ('Knob Hill Public School',)
    ('Presteign Heights Elementary School',)
    ('North Albion Collegiate Institute',)
    ('Agincourt Collegiate Institute',)
    ('York Memorial Collegiate Institute',)
    ('D A Morrison Middle School',)
    ('David and Mary Thomson Collegiate Institute',)
    ('Finch Public School',)
    ('Humberside Collegiate Institute',)
    ('Gordon A Brown Middle School',)
    ('Hodgson Middle School',)
    ('Charles H Best Junior Middle School',)
    ('Agincourt Junior Public School',)
    ('Blantyre Public School',)
    ('West Hill Public School',)
    ('Montrose Junior Public School',)
    ('Delta Alternative Senior School',)
    ('Subway Academy II',)
    ('McKee Public School',)
    ('Sunnylea Junior School',)
    ('Lakeshore Collegiate Institute',)
    ('West Hill Collegiate Institute',)
    ('Hollywood Public School',)
    ('Birchmount Park Collegiate Institute',)
    ('Ossington/Old Orchard Junior Public School',)
    ('R H King Academy',)
    ('Allenby Junior Public School',)
    ('John Fisher Junior Public School',)
    ('Parkside Elementary School',)
    ('West Rouge Junior Public School',)
    ('Glenview Senior Public School',)
    ('Fisherville Senior Public School',)
    ('Islington Junior Middle School',)
    ('Hillcrest Community School',)
    ('William J McCordic School',)
    ('Walter Perry Junior Public School',)
    ('Lanor Junior Middle School',)
    ('John A Leslie Public School',)
    ('Clinton Street Junior Public School',)
    ('Wedgewood Junior School',)
    ('John Polanyi Collegiate Institute',)
    ('Claude Watson School for the Arts',)
    ('Kimberley Junior Public School',)
    ('Beaches Alternative Junior School',)
    ('Frank Oke Secondary School',)
    ('Lester B Pearson Elementary School',)
    ('Bruce Public School',)
    ('Cosburn Middle School',)
    ('Wilkinson Junior Public School',)
    ('Sir Oliver Mowat Collegiate Institute',)
    ('Sir Adam Beck Junior School',)
    ('Norway Junior Public School',)
    ('Malvern Collegiate Institute',)
    ('Earl Beatty Junior and Senior Public School',)
    ('Northview Heights Secondary School',)
    ('R H McGregor Elementary School',)
    ('Regent Heights Public School',)
    ('Bala Avenue Community School',)
    ('Humber Summit Middle School',)
    ('Donwood Park Public School',)
    ('Dewson Street Junior Public School',)
    ('Topcliff Public School',)
    ('East York Collegiate Institute',)
    ('East York Alternative Secondary School',)
    ('Drewry Secondary School',)
    ('Cummer Valley Middle School',)
    ('North Toronto Collegiate Institute',)
    ('Forest Hill Collegiate Institute',)
    ('Palmerston Avenue Junior Public School',)
    ('Jackman Avenue Junior Public School',)
    ('Oriole Park Junior Public School',)
    ('Heritage Park Public School',)
    ('Heather Heights Junior Public School',)
    ('Danforth Collegiate and Technical Institute',)
    ('Bedford Park Public School',)
    ('Frankland Community School',)
    ('Cottingham Junior Public School',)
    ('Keele Street Public School',)
    ('Mountview Alternative Junior School',)
    ('Oakwood Collegiate Institute',)
    ('Westwood Middle School',)
    ('Broadlands Public School',)
    ('Delphi Secondary Alternative School',)
    ('Chartland Junior Public School',)
    ('Pineway Public School',)
    ('Owen Public School',)
    ('J B Tyrrell Senior Public School',)
    ('David Lewis Public School',)
    ('Valley Park Middle School',)
    ('Sir Samuel B Steele Junior Public School',)
    ('Marc Garneau Collegiate Institute',)
    ('Summit Heights Public School',)
    ('Sir Ernest MacMillan Senior Public School',)
    ('Ernest Public School',)
    ('Brimwood Boulevard Junior Public School',)
    ('Burrows Hall Junior Public School',)
    ('Timberbank Junior Public School',)
    ('Chief Dan George Public School',)
    ('Terry Fox Public School',)
    ('Gracedale Public School',)
    ('Danforth Gardens Public School',)
    ('Dr Norman Bethune Collegiate Institute',)
    ("Tam O'Shanter Junior Public School",)
    ('Silver Springs Public School',)
    ('Brookmill Boulevard Junior Public School',)
    ('Charles Gordon Senior Public School',)
    ('Iroquois Junior Public School',)
    ('Gosford Public School',)
    ('Shaughnessy Public School',)
    ('Lamberton Public School',)
    ('Percy Williams Junior Public School',)
    ('Cherokee Public School',)
    ('Rockcliffe Middle School',)
    ('Cassandra Public School',)
    ('North Bridlewood Junior Public School',)
    ('Denlow Public School',)
    ('Gateway Public School',)
    ('Bridlewood Junior Public School',)
    ('Morrish Public School',)
    ('Beverly Glen Junior Public School',)
    ('Charlottetown Junior Public School',)
    ('Alvin Curling Public School',)
    ('Humberwood Downs Junior Middle Academy',)
    ('Alexmuir Junior Public School',)
    ('Faywood Arts-Based Curriculum School',)
    ('Lucy Maud Montgomery Public School',)
    ('Terraview-Willowfield Public School',)
    ('Berner Trail Junior Public School',)
    ('Milliken Public School',)
    ('Mary Shadd Public School',)
    ('Highland Creek Public School',)
    ('Grey Owl Junior Public School',)
    ('Joseph Howe Senior Public School',)
    ('Tom Longboat Junior Public School',)
    ('Highcastle Public School',)
    ('Alexander Stirling Public School',)
    ('Malvern Junior Public School',)
    ('Emily Carr Public School',)
    ('Greenholme Junior Middle School',)
    ('Western Technical-Commercial School',)
    ('THESTUDENTSCHOOL',)
    ('General Brock Public School',)
    ('Blythwood Junior Public School',)
    ('Kennedy Public School',)
    ('Thistletown Collegiate Institute',)
    ('Clairlea Public School',)
    ('North Bendale Junior Public School',)
    ('Anson Park Public School',)
    ('SATEC @ WA Porter Collegiate Institute',)
    ('Blacksmith Public School',)
    ('Glamorgan Junior Public School',)
    ('Ranchdale Public School',)
    ("St George's Junior School",)
    ('Sir Wilfrid Laurier Collegiate Institute',)
    ('Jack Miner Senior Public School',)
    ('Albert Campbell Collegiate Institute',)
    ("L'Amoreaux Collegiate Institute",)
    ('Park Lane Public School',)
    ('The Waterfront School',)
    ('City School',)
    ('Bayview Middle School',)
    ('Don Mills Collegiate Institute',)
    ('Don Mills Middle School',)
    ('Dr Marion Hilliard Senior Public School',)
    ('Dixon Grove Junior Middle School',)
    ('Kipling Collegiate Institute',)
    ('Lawrence Heights Middle School',)
    ('Cornell Junior Public School',)
    ('Poplar Road Junior Public School',)
    ('Givins/Shaw Junior Public School',)
    ('Island Public/Natural Science School',)
    ('SEED Alternative School',)
    ('Native Learning Centre',)
    ('Avondale Public School',)
    ('Burnhamthorpe Collegiate Institute',)
    ('CALC Secondary School',)
    ('Scarborough Centre for Alternative Studies',)
    ('Caring and Safe School LC4',)
    ('Caring and Safe School LC2',)
    ('North West Year Round Alternative Centre',)
    ('Yorkdale Secondary School',)
    ('Etobicoke Year Round Alternative Centre',)
    ('South East Year Round Alternative Centre',)
    ('North East Year Round Alternative Centre',)
    ('Brookside Public School',)
    ('ALPHA II Alternative School',)
    ('Karen Kain School of the Arts',)
    ('Africentric Alternative School',)
    ('da Vinci School',)
    ('The Grove Community School',)
    ('Equinox Holistic Alternative School',)
    ('Caring and Safe School LC3',)
    ('Parkview Alternative School',)
    ('Boys Leadership Academy',)
    ("Jean Augustine Girls' Leadership Academy",)
    ('Ben Heppner Vocal Music Academy',)
    ('Downtown Vocal Music Academy of Toronto',)
    ('Native Learning Centre East',)
    ('Caring and Safe School LC1',)
    ('Emery EdVance Secondary',)
    ('Oasis Alternative SS (Skateboard Factory) (Scadding Court)',)
    ('Oasis Triangle Program',)
    ('Norseman Junior Middle School- Castlebar',)
    ('Davisville Junior Public School',)
    ('Spectrum Alternative Senior School',)
    

```python
#Out of all programs offered, what percentage of programTypes (Drop-in, Registered, Virtual) are offered by the EarlyON programs? 
#Does this differ for programs housed in TDSB schools, compared to those which are not?

#Get lengths of total early
lengths = conn.execute(sql.text(''' -- sql 
SELECT e.e_count, et.et_count FROM
(SELECT COUNT(*) as e_count
FROM earlyon) as e
JOIN
(SELECT COUNT(*) as et_count
FROM earlyon 
WHERE school_name NOT IN (SELECT DISTINCT(sch_name) FROM tdsb)) as et;
                               '''))
print(lengths.fetchall())

output = conn.execute(sql.text(''' -- sql 
SELECT `drop`.drop_perc, `virtual`.virtual_perc, `registered`.registered_perc FROM
(SELECT (COUNT(dropinHours)*100 / 254) as drop_perc
FROM earlyon 
WHERE dropinHours IS NOT NULL) as `drop`
JOIN
(SELECT (COUNT(virtualHours)*100 / 254) as virtual_perc
FROM earlyon 
WHERE virtualHours IS NOT NULL) as `virtual`
JOIN
(SELECT (COUNT(registeredHours)*100 / 254) as registered_perc
FROM earlyon 
WHERE registeredHours IS NOT NULL) as `registered`;
                               '''))
for i in output.fetchall():
    print(i)

output2 = conn.execute(sql.text(''' -- sql 
SELECT `drop`.drop_perc, `virtual`.virtual_perc, `registered`.registered_perc FROM
(SELECT (COUNT(dropinHours)*100 / 34) as drop_perc
FROM earlyon 
WHERE dropinHours IS NOT NULL
    AND school_name NOT IN (SELECT DISTINCT(sch_name) FROM tdsb)) as `drop`
JOIN
(SELECT (COUNT(virtualHours)*100 / 34) as virtual_perc
FROM earlyon 
WHERE virtualHours IS NOT NULL 
    AND school_name NOT IN (SELECT DISTINCT(sch_name) FROM tdsb)) as `virtual`
JOIN
(SELECT (COUNT(registeredHours)*100 / 34) as registered_perc
FROM earlyon 
WHERE registeredHours IS NOT NULL
    AND school_name NOT IN (SELECT DISTINCT(sch_name) FROM tdsb)) as `registered`;
                               '''))
for i in output2.fetchall():
    print(i)

#From this we see that the percentage of virtual programtypes are zero for all of the programs and only the ones housed in TDSB schools. 
# In contrast, we see that drop in hours make up 89% of the total offered programs, but 100% of those housed in TDSB schools.
#The opposite trend is observed in registered hours, with 25% of total programs being allocated to this type, but only 3% of those occuring in TDSB school.
#The fact that both the aggregate percent of total programs offered by EarlyON and programs offered exclusively in TDSB schools is greater than 100% indicates that there is an overlap
#between drop-in and registered programs (i.e. they occur simultaneously)
```

    [(254, 34)]
    (Decimal('89.3701'), Decimal('0.0000'), Decimal('25.1969'))
    (Decimal('100.0000'), Decimal('0.0000'), Decimal('2.9412'))
    

```python
#Prepare a list of community information in a single table, as follows:
#- All EarlyOn programs should be listed, using the name of the program, and the full address.
#- Where an EarlyON is hosted at a TDSB school, include the municipality, place name, and whether the school is an elementary school or secondary school.
#- Any TDSB school that does not host an EarlyON program should be listed, using the address, municipality and place name.

sql_query = pd.read_sql_query (
    '''
SELECT * FROM
(SELECT e.program_name , e.full_address, t.sch_name, t.municipality, t.place_name, t.class FROM
    (SELECT program_name, full_address, school_name
    FROM earlyon) as e
    LEFT JOIN
    (SELECT sch_name, municipality, place_name, (CASE
        WHEN sch_name LIKE '%high%' or sch_name LIKE '%secondary%' or sch_name LIKE '%senior%' THEN 'Secondary'
        WHEN sch_name LIKE '%elementary%' or sch_name LIKE  '%primary%' or sch_name LIKE  '%junior%' THEN 'Elementary'                                                           
        ELSE 'unknown'
        END) as class FROM tdsb) as t
	ON e.school_name = t.sch_name) as earlyon_tdsb
    UNION
    (SELECT 'NA' as program_name, address_full as full_address, sch_name, municipality, place_name, (CASE
        WHEN sch_name LIKE '%high%' or sch_name LIKE '%secondary%' or sch_name LIKE '%senior%' THEN 'Secondary'
        WHEN sch_name LIKE '%elementary%' or sch_name LIKE  '%primary%' or sch_name LIKE  '%junior%' THEN 'Elementary'                                                           
        ELSE 'unknown'
        END) as class 
    FROM tdsb)
    ORDER BY full_address
    ''',
    conn)
display(sql_query)
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
      <th>program_name</th>
      <th>full_address</th>
      <th>sch_name</th>
      <th>municipality</th>
      <th>place_name</th>
      <th>class</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>NA</td>
      <td>1 Danforth Ave</td>
      <td>CALC Secondary School</td>
      <td>former Toronto</td>
      <td>City Adult Learning Centre (Calc)</td>
      <td>Secondary</td>
    </tr>
    <tr>
      <th>1</th>
      <td>NA</td>
      <td>1 Hanson St</td>
      <td>Monarch Park Collegiate Institute</td>
      <td>former Toronto</td>
      <td>Monarch Park Collegiate</td>
      <td>unknown</td>
    </tr>
    <tr>
      <th>2</th>
      <td>NA</td>
      <td>1 Hanson St</td>
      <td>School of Life Experience</td>
      <td>former Toronto</td>
      <td>Monarch Park Collegiate</td>
      <td>unknown</td>
    </tr>
    <tr>
      <th>3</th>
      <td>NA</td>
      <td>1 Ralph St</td>
      <td>C R Marchant Middle School</td>
      <td>York</td>
      <td>C. R. Marchant Middle School</td>
      <td>unknown</td>
    </tr>
    <tr>
      <th>4</th>
      <td>NA</td>
      <td>1 Selwyn Ave</td>
      <td>Selwyn Elementary School</td>
      <td>East York</td>
      <td>Selwyn Elementary School</td>
      <td>Elementary</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>834</th>
      <td>NA</td>
      <td>99 Mountview Ave</td>
      <td>Mountview Alternative Junior School</td>
      <td>former Toronto</td>
      <td>Keele Street Public School</td>
      <td>Elementary</td>
    </tr>
    <tr>
      <th>835</th>
      <td>NA</td>
      <td>990 Jane St</td>
      <td>Roselands Junior Public School</td>
      <td>York</td>
      <td>Roselands Junior Public School</td>
      <td>Elementary</td>
    </tr>
    <tr>
      <th>836</th>
      <td>Roseland EarlyON Child and Family Centre</td>
      <td>990 Jane St, York, ON M6N 4E2</td>
      <td>Roselands Junior Public School</td>
      <td>York</td>
      <td>Roselands Junior Public School</td>
      <td>Elementary</td>
    </tr>
    <tr>
      <th>837</th>
      <td>NA</td>
      <td>991 St Clair Ave W</td>
      <td>Oakwood Collegiate Institute</td>
      <td>former Toronto</td>
      <td>Oakwood Collegiate Institute</td>
      <td>unknown</td>
    </tr>
    <tr>
      <th>838</th>
      <td>NA</td>
      <td>994 Carlaw Ave</td>
      <td>Westwood Middle School</td>
      <td>East York</td>
      <td>Westwood Park</td>
      <td>unknown</td>
    </tr>
  </tbody>
</table>
<p>839 rows × 6 columns</p>
</div>

## Part C: Evaluating your results  (7 marks)

**Question 1 (2 marks)**

The two datasets provided for this assignment offer a limited set of data, for different purposes. How do you think the use of these datasets might differ?

The tdsb dataset appears to be a repository of school locations and their attributes. This would be useful in geopsatial analyses such as the distribution of school densities/accessibility by region in the GTA. On the other hand, the EarlyON dataset contains information about the time and location of various community programs; such a dataset would be more geared towards developing a public-use application about when and where to access community services.

**Question 2 (1 mark)**

One thing you may have had consider is your selection of columns from both datasets to use as a key for any joins you performed. Discuss your reasoning behind your choice of key. If you don't think you had a reason in particular, then name another pair of colums which could have been used to execute your joins.

The choice of primary key for joins was often determined by the question itself. In several instances, the goal was to obtain information from the tdsb dataset for all instances where the school was common to the EarlyON dataset. For these cases, it made sense to structure the query on the school_name attribute such that the join itself would filter the tdsb dataset to the desired subset. After this, the relevant information could easily be extracted from the subset/joined dataset.

**Question 3 (4 marks)**

It is possible to download both datasets as GeoJSON files rather than as CSVs. Imagine that we have loaded these GeoJSON files into MongoDB instead of a relational database.

Pick one of the queries from Part B to discuss. Do you think it is more difficult to retrieve the information requested for this query from the pair of relational database tables provided to you, or from a MongoDB collection set up as described? Explain why or why not. 

For the final question in Part b that asked: *"Prepare a list of community information in a single table..."*, I imagine that parts of it are well suited towards a GeoJSON/MongoDB collection, while other aspects are not. The key:value format of a JSON based query would allow the efficient extraction and combination of the schools in the tdsb collection that offer earlyON programs. However, I am less confident in MongoDB's ability to create the new category of *class* (e.g. elementary or secondary) across different levels in the hierarchy. Additionally, I am not sure if MongoDB would be able to effectively populate missing data points for attributes that are not shared in part of the query (i.e. the NAs in the program_name column for tdsb schools that did not host EarlyON programs).
Unfortunately, much of the above is based solely on my admitedly vague understanding of MongoDB. I do plan on broadening my DBMS competency with experrience in MongoDB, but for the time being I think it is wise to assume that my understanding of MongoDB is limited.

## Part E: Reflection (5 marks)

In a brief paragraph for each (no more than 500 words total), answer the following:

1) Identify a skill or concept which you are now more knowledgable or comfortable with now, compared to at the start of DATA 604. 

2) What best helped you to learn this skill or concept? Was it something covered in class, part of an assignment or project, or another resource?

3) Based upon what you have learned, where do you see an opportunity to continue to develop your understanding of this skill? 

1. Following the past few months, my abilities in SQL syntax and knowledge of Databses has grown enormously. At the start of this course I could not have confidently said what the advantages of a designated databse were. Now I understand the invaluable speed and security benefits provided by databsase management systems, and I could even set up a very basic one. Additionally, I can confidently perform basic SQL queries, and I believe I have the tools to learn how to structure complex queries.
2. For me, nothing can beat hands-on experience. I found the assignments and the projects to be the best way to further my understanding of working with 'big data.' As a side not, I may actually have learned the most when things went wrong. For example, when the UofC db server crashed, I created a local server which was a great learning opportunity. Similarly, during the project when I tried to join tables based on geospatial attributed and the computation time proved to be too much, I learned how to more efficiently structure *joins* by joining at two different levels of specificity, and consequently decreasing the 'Big O.'
3. As mentioned in the above question, I have limited experience with other DBMS. For me, obtianing hands-on experience with MongoDB and other systems would be hugely beneficial. This would be best done through a project, which might mean that it would be good for me to find a way to complete an upcoming project in the MDSA program through the use of a different DBMS. 

## References

Both datasets used in this Assignment are licensed under the Open Government License - City of Toronto.

EarlyON child and Family Centres [online], 2023. Open Data (City of Toronto). Available from: https://open.toronto.ca/dataset/earlyon-child-and-family-centres/ [Accessed 23 Nov 2023].

Toronto District School Board Locations [online], 2023. Open Data (City of Toronto). Available from: https://open.toronto.ca/dataset/toronto-district-school-board-locations/ [Accessed 23 Nov 2023].

