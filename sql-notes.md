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

An SQL error "Unknown column '4' in 'order clause'" may be returned when the column integer in the ORDER operator doesn't exist, so now we know there are 3 columns in this table.

### UNION operator
> https://www.w3schools.com/sql/sql_union.asp
> 
> ?id=1' UNION select 1,2,3 --+ 
