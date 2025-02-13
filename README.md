[![CI](https://github.com/nogibjj/SQLite_YCLiu/actions/workflows/cicd.yml/badge.svg)](https://github.com/nogibjj/SQLite_YCLiu/actions/workflows/cicd.yml)
## Complex queries using SQL on SQLite databases

This repository demonstrates a SQL query that **JOIN**s tables, aggregate (**GROUPBY**) numbers under constraints (**WHERE**), displayed in a certain **ORDER**. Two tables were created using SQLite to **test the query's functionality**. 

Below is an overview of the files in this project:

1. **Database setup and query**
   <br>a. _mylib/create.py_: **Build a database**, **create tables**, fill in values.
   <br>b. _mylib/query.py_: **Select** and ouput **data**.
   
3. **Main functions for querying on databse**
   <br>c. _main.py_: execute command-line-like functions from ./mylib to create a database, tables, and to query on the created database. Specifically, it does the following:
<br>         1. Build a SQLite database _Transaction.db_.
<br>         2. Create a table named *Customer*, with the following columns: *cust_id*, *name*, *sex*. Below is the content of the resulted table.

**Customer table**

| cust_id | name | sex |
|---|---|---|
|001| John | Male |
|**_002_**| **_Devin_** | **_Female_** |
|**_003_**| **_Sharon_** | **_Female_** |
|004| Tim | Male | 
|**_005_**| **_Tina_** | **_Female_** |

<br>         3. Create a table named *TXR* (short for transaction), with the following columns: *cust_id*, *item*, *amount*. Below is the content of the resulted table.

**TXR table**
| cust_id | item | amount |
|---|---|---|
|001| Hot Dog | 100 |
|001| Hot Dog | 20 |
|**_002_**| Hamburger | `80` |
|**_002_**| Hot Dog | `120` |
|**_003_**| Hamburger | `60` |
|**_003_**| Hot Dog | `200` |
|004| Hot Dog | 40 |
|004| Hamburger | 140 |
|**_005_**| Hamburger | `150` |
|**_005_**| Hamburger | `80` |


<br>         4. Query total _amount_ purchased of all _female_ customers and display the resulted table in _descending order_ (by total amount purchased per customer). The **associated SQL code is displayed in the _code block_** below **with detailed explanation in comment**. The resulted table is displayed in **Query Result** further down.

```
#SQL Query

SELECT t1.cust_id, t1.name, t1.sex,      # select the columns from t1.
        SUM(t2.amount) AS total_amount   # sum the numbers in the amount column from t2, name it as *total_amount*.
        FROM Customer t1                 # identify a source table, the Customer table, named as t1
        INNER JOIN TXR t2                # join another source table, the TXR table, named as t2
                                         # INNER JOIN means to connect the tables with a key column
                                         # where only columns values presented in *both* tables will be included
        ON t1.cust_id = t2.cust_id       # identify the key column to join the tables: *cust_id*
        WHERE t1.sex ='Female'           # specify that only rows with *sex* column values that equal to 'female' will be queried
        GROUP BY t1.cust_id              # specify the result (*sum* in the 2nd line of code) is aggregated on cust_id
        ORDER BY total_amount DESC       # specify that the query is displayed from the cust_id with 
                                         # highest total_amount (sum of amount per cust_id) to lowest (DESC)                                      
```

**Query Result**

The resulted query contains the columns and rows in **bold** in the above Customer and TXR table, which is the **total amount** of purchase of every **female** customer (with their _cust_id_, _name_ and _sex_). Note that the _values_ of the _total_amount_ column (with `gray background`) is the **sum of amount** (with `gray background` in the TXR table) purchased by each female customer. The resulted table is displayed in descending order of _total_amount_, which means the first row is the female customer with the **highest** _total_amount_ purchased (here, Sharon) and the last row is the female customer with the **lowest** _total_mount_ purchased (here, Devin).

| cust_id | name | sex | total_amount |
|---|---|---|---|
|003| Sharon | Female | `260` |
|005| Tina | Female | `230` |
|002| Devin | Female | `200` |

   <br>d. _test_main.py_: Run all steps in main.py and test if the output query is correct.
   
5. **Github actions setup for continuous integration**
  <br>e. _.github/workflows/cicd.yml_: Quality control actions are triggered when pushed/ pulled to main branch. After setting up the environment, actions of **installing packages**, **linting**, **testing**, **formatting** would be executed in order (specified in Makefile). 

6. **Other files for development environment settings**
  <br>f. _.devcontainer_: set up the environment for development.
  <br>g. _.gitignore_: specify file names to ignore.
  <br>h. _requirements.txt_: list required packages for the project.

7. **Description of the project**
   <br>i. _README.md_: THIS FILE, explaining the purpose and structure of the directory, with screenshot of example output.


