# java-sql

A student that completes this project shows that they can:
* Query data from a single table
* Query data from multiple tables
* Create a new datadaase using PostgreSQL

# Introduction

Working with SQL

# Instructions

ReImport the Northwind database into PostgreSQL using pgAdmin. This is the same data we used during the guided project.

clone https://github.com/pthom/northwind_psql.git

## pgAdmin

* Right Click Databases
  * Create
    * type in northwind2

* Tools -> Query Tool
  * Open file northwind.sql (from cloned repo)
  * Execute

* Look under
  * northwind2 -> Schemas -> public -> tables

* Clear query windows

Answer the following data queries. Keep track of the SQL you write by pasting it into this document under its appropriate header below. You will be submitting that through the regular fork, change, pull process.


### find all customers that live in London. Returns 6 records.
> SELECT * 
FROM customers
WHERE city = 'London'


### find all customers with postal code 1010. Returns 3 customers.
> SELECT * 
FROM customers
WHERE postal_code = '1010'


### find the phone number for the supplier with the id 11. Should be (010) 9984510.
> SELECT phone 
FROM suppliers
WHERE supplier_id = '11'


### list orders descending by the order date. The order with date 1998-05-06 should be at the top.
> SELECT * 
FROM orders
ORDER BY order_date DESC


### find all suppliers who have names longer than 20 characters. You can use `length(company_name)` to get the length of the name. Returns 11 records.
> SELECT * 
FROM suppliers
WHERE length(company_name) > 20


### find all customers that include the word 'MARKET' in the contact title. Should return 19 records.
> SELECT contact_title
FROM customers
WHERE upper(contact_title) LIKE 'MARKET%'


### add a customer record for   
* customer id is 'SHIRE'
* company name is 'The Shire'
* contact name is 'Bilbo Baggins'
* the address is '1 Hobbit-Hole'
* ths city is 'Bag End'
* the postal code is '111'
* the country is 'Middle Earth'
> INSERT INTO customers(customer_id, company_name, contact_name, address, city, postal_code, 
					 country)  
  VALUES ('SHIRE', 'The Shire', 'Bilbo Baggins', '1 Hobbit-Hole', 'Bag End', '111',
	   'Middle Earth')  


### update _Bilbo Baggins_ record so that the postal code changes to _"11122"_.
> UPDATE customers  
SET postal_code = '11122' 
WHERE customer_id = 'SHIRE'  


### list orders grouped by customer showing the number of orders per customer. _Rattlesnake Canyon Grocery_ should have 18 orders.
> SELECT COUNT(order_id), company_name
FROM customers, orders
WHERE customers.customer_id = orders.customer_id
GROUP BY company_name

> There is more information about the COUNT clause on [W3 Schools](https://www.w3schools.com/sql/sql_count_avg_sum.asp)


### list customers names and the number of orders per customer. Sort the list by number of orders in descending order. _Save-a-lot Markets should be at the top with 31 orders followed by _Ernst Handle_ with 30 orders. Last should be _Centro comercial Moctezuma_ with 1 order.
> SELECT COUNT(order_id), company_name
FROM customers, orders
WHERE customers.customer_id = orders.customer_id
GROUP BY company_name
ORDER BY count DESC


### list orders grouped by customer's city showing number of orders per city. Returns 69 Records with _Aachen_ showing 6 orders and _Albuquerque_ showing 18 orders.
> SELECT COUNT(order_id), city
FROM customers, orders
WHERE customers.customer_id = orders.customer_id
GROUP BY city
ORDER BY city


## Data Normalization

Note: This step does not use PostgreSQL!

Take the following data and normalize it into a 3NF database.

| Person Name | Pet Name | Pet Type | Pet Name 2 | Pet Type 2 | Pet Name 3 | Pet Type 3 | Fenced Yard | City Dweller |
|-------------|----------|----------|------------|------------|------------|------------|-------------|--------------|
| Jane        | Ellie    | Dog      | Tiger      | Cat        | Toby       | Turtle     | No          | Yes          |
| Bob         | Joe      | Horse    |            |            |            |            | No          | No           |
| Sam         | Ginger   | Dog      | Miss Kitty | Cat        | Bubble     | Fish       | Yes         | No           |

//Person Table

| Person Id | Person Name | Fenced Yard | City Dweller |   
|-----------|-------------|-------------|--------------|
| 1         | Jane        | No          | Yes          |   
| 2         | Bob         | No          | No           |   
| 3         | Sam         | Yes         | No           |   

// Pet Type Table

| Pet Type Id | Pet Type |
|-------------|----------|
| 1           | Dog      |
| 2           | Horse    |
| 3           | Cat      |
| 4           | Turtle   |
| 5           | Fish     |

// Pets Table

| Pet Id | Pet Name   | Pet Type | Person Id |
|--------|------------|----------|-----------|
| 1      | Ellie      | 1        | 1         |
| 2      | Tiger      | 3        | 1         |
| 3      | Toby       | 4        | 1         |
| 4      | Joe        | 2        | 2         |
| 5      | Ginger     | 1        | 3         |
| 6      | Miss Kitty | 3        | 3         |
| 7      | Bubble     | 5        | 3         |

---
## Stretch Goals

### delete all customers that have no orders. Should delete 17 (or 18 if you haven't deleted the record added) records.
> This is done with a DELETE query

> In the WHERE clause, you can provide another list with an IN keyword this list can be the result of another SELECT query. Write a query to return a list of CustomerIDs that meet the criteria above. Pass that to the IN keyword of the WHERE clause as the list of IDs to be deleted
 
> Use a LEFT JOIN to join the Orders table onto the Customers table and check for a NULL value in the OrderID column

## Create Database and Table

### Keep track of the code you write and paste at the end of this document

- use pgAdmin to create a database, naming it `budget`.
- add an `accounts` table with the following _schema_:

  - `id`, numeric value with no decimal places that should autoincrement.
  - `name`, string, add whatever is necessary to make searching by name faster.
  - `budget` numeric value.

- constraints
  - the `id` should be the primary key for the table.
  - account `name` should be unique.
  - account `budget` is required.

  CREATE TABLE accounts (
    id serial PRIMARY KEY,
    name VARCHAR (50) UNIQUE,
    budget int NOT NULL
  )
