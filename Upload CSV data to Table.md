### 3 | Upload CSV data to TABLES

- Having created a non-superuser to whom we gave write access, we can upload data to the <code>database</code>
- Being connected to our database <code>super_awesome_application=#</code>
- Created <code>TABLES</code> can only be removed by the owners

#### 1. Using postgreSQL

- First, lets create a <code>table</code> header <code>details</code>, which is in **CAPS**

```
CREATE TABLE DETAILS(emp_id SERIAL,
first_name   VARCHAR(50),
last_name VARCHAR(50),
dob DATE,
city VARCHAR(40));
```

- Next, We can copy data, located in a local file to newly created table <code>details</code>, written in **lower case**

```
COPY details(emp_id,first_name,last_name,dob,city)
FROM '/Users/andrey/Documents/data.txt' 
DELIMITER ','
CSV HEADER;
```

#### 2. Utilising psycopg2

- Utilising <code>psycopg2</code> (via python) may seem a little longer, but we can run it via a script

```python

import psycopg2

DATABASE = 'X'    # database [\list] (which we \connect to)
USER = 'X'        # superused id
PASSWORD = 'X'    # password
HOST = 'XXX.X.X.X' # host ip
PORT = 'XXXX'      # port number  
  
conn = psycopg2.connect(database=f'{DATABASE}',
                        user=f'{USER}', password=f'{PASSWORD}', 
                        host=f'{HOST}', port=f'{PORT}'
)
  
conn.autocommit = True
cursor = conn.cursor()
  
# Create Table Header (SQL query)
sql = '''CREATE TABLE DETAILS(emp_id SERIAL,
first_name   VARCHAR(50),
last_name VARCHAR(50),
dob DATE,
city VARCHAR(40));'''

cursor.execute(sql)
  
# Copy Data into Table (SQL query)
sql2 = '''COPY details(emp_id,first_name,last_name,dob,city)
FROM '/Users/andrey/Documents/data.txt' # absolute path to file
DELIMITER ','
CSV HEADER;'''
  
cursor.execute(sql2)
  
# Fetch the table data (SQL query)
sql3 = '''select * from details;'''
cursor.execute(sql3)
for i in cursor.fetchall():
    print(i)
  
conn.commit()
conn.close()    

```

- Confirm the data has been uploaded via 

```
TABLE details;
```

```
 emp_id | first_name | last_name |    dob     |   city   
--------+------------+-----------+------------+----------
      1 | Max        | Smith     | 2002-02-03 | Sydney
      2 | Karl       | Summers   | 2004-04-10 | Brisbane
      3 | Sam        | Wilde     | 2005-02-06 | Perth
```

#### Table operations

List databases <code>\list</code> & Connect to databse <code>\connect</code>:

- <code>\dt</code> - show tables 
- <code>CREATE TABLE DETAILS((table formats))</code> create table header
- <code>TABLE table_name</code> view table stored in database
- <code>DROP table_name</code> remove table from database 