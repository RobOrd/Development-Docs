# Articulos sobre Tunning #

No ignorar sort warning
[https://www.simple-talk.com/sql/performance/never-ignore-a-sort-warning-in-sql-server/](https://www.simple-talk.com/sql/performance/never-ignore-a-sort-warning-in-sql-server/ "No ingnorar sort warning")

Como evitar uso de tempdb
[http://sqltouch.blogspot.mx/2013/06/order-bygroup-by-operation-spills-in.html](http://sqltouch.blogspot.mx/2013/06/order-bygroup-by-operation-spills-in.html "Como evitar uso de tempdb")

Tips de Optimización de Indices
[http://www.mssqlcity.com/tips/tipind.htm](http://www.mssqlcity.com/tips/tipind.htm "Tips de Optimización de Indices")

## Index Scans and Table Scans ##

One common problem that exists is the lack of indexes or incorrect indexes and therefore SQL Server has to process more data to find the records that meet the queries criteria.  These issues are known as Index Scans and Table Scans.

An index scan or table scan is when SQL Server has to scan the data or index pages to find the appropriate records.  A scan is the opposite of a seek, where a seek uses the index to pinpoint the records that are needed to satisfy the query.  The reason you would want to find and fix your scans is because they generally require more I/O and also take longer to process.  This is something you will notice with an application that grows over time.  When it is first released performance is great, but over time as more data is added the index scans take longer and longer to complete.

## Spills ##
Queries are spilling out to tempdb. This means that SQL Server has poorly estimated the amount of rows that will be returned from an operator. When the row estimate is wrong, the memory grant will be wrong; SQL Server is going to have to use extra space on disk to do the work.

To find the queries that are causing spills, you can run the following query:

   ```sql
	SELECT  st.text,
        qp.query_plan,
        qs.*
	FROM    (
    	SELECT  TOP 50 *
	    FROM    sys.dm_exec_query_stats
    	ORDER BY total_worker_time DESC
	) AS qs
	CROSS APPLY sys.dm_exec_sql_text(qs.sql_handle) AS st
	CROSS APPLY sys.dm_exec_query_plan(qs.plan_handle) AS qp
	WHERE qp.query_plan.value(
             'declare namespace p="http://schemas.microsoft.com/sqlserver/2004/07/showplan";
             max(//p:SpillToTempDb/@SpillLevel)', 'int') > 0
```

# Tips #
- Usar DBCCDBREINDEX para reconstruir todos los indices periodicamente, por ejemplo 1 vez por semana, esto para reducir la fragmentación de la base de datos

- Seek usa un indice para hacer una búsqueda de forma mas eficiente

- Scan hace un barrido de toda la tabla en busca de un registro

- `tempdb` debería estar en un DD de alto rendimiento de I/O

