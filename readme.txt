Install Extensions:
for commits:
Git: create git

Configuration:
    - Installation of extensions:
    -mysql
    -mysql-connector
    -mysql-connector-python
    - pandas - pandas is a software library written for the Python
    programming language
    for data manipulation and analysis. In particular, it offers
    data structures and operations for
    manipulating numerical tables and time series.

file>settings>name of project > project interpreter, search each extension then
install
database un: root pw:student
screenshot of test connection after database

Establish communication to mysql


1.) Content for main.py

import mysql.connector
from mysql.connector import Error

def create_server_connection(host_name, user_name, user_password):
    connection = None
    try:
        connection = mysql.connector.connect(
            host = host_name,
            user = user_name,
            passwd = user_password
        )
        print("MySQL Database Connection Successful")
    except Error as err:
        print(f"Error {err}")
    return connection


#calling statement
connection = create_server_connection("localhost", "root", "student")

2.)creation of databases

def create_database(connection, query):
    cursor = connection.cursor()
    try:
        cursor.execute(query)
        print("Database created successfully")
    except Error as err:
        print(f"Error: {err}")
#call create_database function to create DB in mySQL
create_database(connection, create_database_query)

#Queries
create_database_query = "create database EXOTIC_DEALERSHIP"

#calling statement
connection = create_server_connection("localhost", "root", "student")
#call create_database function to create DB in mySQL
create_database(connection, create_database_query)


3.) Add database name to "create_server_connection" function and calling statement

def create_server_connection(host_name, user_name, user_password, db_name): <------ add db_name
    connection = None
    try:
        connection = mysql.connector.connect(
            host = host_name,
            user = user_name,
            passwd = user_password,
            database = db_name   <----- assign argument
        )
        print("MySQL Database Connection Successful")
    except Error as err:
        print(f"Error {err}")
    return connection


connection = create_server_connection("localhost", "root", "student","exotic_dealership") <-- place DB name

4.)Place work horse function to run queries:

def execute_query(connection, query):
    cursor = connection.cursor()
    try:
        cursor.execute(query)
        connection.commit()
        print("Query sucessful")
    except Error as err:
        print(f"Error: {err}")


5.) create sql query to create coupe table in DB

#create coupe table

create_coupe_table = """
create table COUPE_MODELS(
vin_number VARCHAR(12) PRIMARY KEY,
make VARCHAR(50) NOT NULL,
model VARCHAR(50) NOT NULL,
mileage integer NOT NULL,
price integer NOT NULL);"""

#call work horse function to run query
execute_query(connection,create_coupe_table)

6.) create sql query to create suv table in DB3

 #create suv table

create_suv_table = """
create table SUV_MODELS(
vin_number VARCHAR(12) PRIMARY KEY,
make VARCHAR(50) NOT NULL,
model VARCHAR(50) NOT NULL,
mileage integer NOT NULL,
price integer NOT NULL);
"""
#call work horse function to run query
execute_query(connection,create_suv_table)

7.)Populate coupe table:

#populate coupe table
coupe_vehicles = """ < ----- create variable to hold query
insert into COUPE_MODELS values
('123abc321','Ashton Martin', 'Vanquish', 200, 115000),
('asd748541', 'Audi', 'RS 7', 1200, 12500) """  <--- end of value

#call work horse function to run query
execute_query(connection,coupe_vehicles)<----calling statement.

8.)Populate SUV table:

#populate suv table
suv_table = """
insert into SUV_MODELS values
('123abc321','Lamborghini', 'Urus', 200, 115000),
('asd748541', 'BMW', 'X5', 1200, 12500) """

#call work horse function to run query
execute_query(connection,suv_table)

9.)Read information from DB in phycharm:

def read_query(connection, query):   <----- insert this function to read info from mysql
    cursor = connection.cursor()
    result = None
    try:
        cursor.execute(query)
        result = cursor.fetchall()
        return result
    except Error as err:
        print(f"Error: {err}")  <---- end of content to place

#read values from coupe table
display_coupe_models_table = """
SELECT * FROM COUPE_MODELS;
"""


#call read query function to fetch information from MySQL
results = read_query(connection, display_coupe_models_table)
#iterate through the table to display all information
for result in results:
    print(result)

#read values from suv table
display_suv_models_table = """
SELECT * FROM SUV_MODELS;
"""
#call read query function to fetch information from MySQL
results = read_query(connection, display_suv_models_table)
#iterate through the table to display all information
for result in results:
    print(result)


10.)Update mileage count for vehicle in SUV table:
update_firstSUV_mileage = """
update suv_models
SET mileage = 5000
where vin_number = '123abc321'
"""
#call work horse function to run query
execute_query(connection,update_firstSUV_mileage)<-------update work horse
#call create_database function to create DB in mySQL
#create_database(connection, create_database_query)
#call read query function to fetch information from MySQL
results = read_query(connection, display_suv_models_table)
#iterate through the table to display all information
for result in results:
    print(result)

10.)Update mileage count for vehicle in coupe table:

update_firstCoupe_mileage = """
update COUPE_MODELS
SET mileage = 2000
where vin_number = '123abc322'
"""

#call work horse function to run query
execute_query(connection,update_firstCoupe_mileage)
#call create_database function to create DB in mySQL
#create_database(connection, create_database_query)
#call read query function to fetch information from MySQL
results = read_query(connection, display_coupe_models_table)
#iterate through the table to display all information
for result in results:
    print(result)




