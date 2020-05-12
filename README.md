# Mysql Best Practices

## SECURITY


## PERFORMANCE

1. Always use proper datatype
```
Use datatypes based on the nature of data. If you use irrelevant datatypes it may consume more space or may lead to errors.
Example: Using varchar (20) to store date time values instead of DATETIME datatype will lead to errors during date time-related calculations and there is also a possible case of storing invalid data.
```
2. Use CHAR (1) over VARCHAR(1)
If you string a single character, use CHAR(1) instead of VARCHAR(1) because VARCHAR(1) will take extra byte to store information

3. Use CHAR datatype to store only fixed length data
Example: Using char(1000) instead of varchar(1000) will consume more space if the length of data is less than 1000

4. Avoid using regional date formats
When you use DATETIME or DATE datatype always use YYYY-MM-DD date format or ISO date format that suits your SQL Engine. Other regional formats like DD-MM-YYY, MM-DD-YYYY will not be stored properly.

5. Index key columns
Make sure to index the columns which are used in JOIN clauses so that the query returns the result fast.
If you use UPDATE statement that involves more than one table make sure that all the columns which are used to join the tables are indexed

6. Do not use functions over indexed columns
Using functions over indexed columns defeats the purpose of the index. Suppose you want to get data where first two character of customer code is AK, do not write
SELECT columns FROM table WHERE left (customer_code,2)=’AK’
but rewrite it using
SELECT columns FROM table WHERE customer_code like ‘AK%’
which will make use of index which results in faster response time.

7. Use SELECT * only if needed
Do not just blindly use SELECT * in the code. If there are many columns in the table, all will get returned which will slow down the response time particularly if you send the result to a front-end application.
Explicitly type out the column names which are actually needed.

8. Use ORDER BY Clause only if needed
If you want to show the result in front-end application, let it ORDER the result set. Doing this in SQL may slow down the response time in the multi-user environment.

9. Choose proper Database Engine
If you develop an application that reads data more often than writing (ex: search engine), choose MyISAM storage engine.
If you develop an application that writes data more often than reading (ex: real-time bank transactions), choose INNODB storage engine.
Choosing wrong storage engine will affect the performance

10. Use EXISTS clause wherever needed
If you want to check the existence of data, do not use
If (SELECT count(*) from Table WHERE col=’some value’)>0
instead, use EXISTS clause
If EXISTS(SELECT * from Table WHERE col=’some value’)
which is faster in response time.


