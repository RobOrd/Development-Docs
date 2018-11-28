
### JOINING

- INNER JOIN = JOIN 
- LEFT [OUTER] JOIN = Todos los registros de la tabla Left coincidad o no en la tabla Right
- RIGHT [OUTER] JOIN = Todos los registros de la tabla Right coincidad o no en la tabla Left
- FULL OUTER JOIN = FULL JOIN = Todos los registros de ambas tablas

- LEFT/RIGHT EXCLUDING JOIN = Igual que LEFT Y RIGTH JOIN pero con 
	- B.Key IS NULL para excluir todo lo de la derecha
 	- A.Key IS NULL para excluir todo lo de la izquierda

- OUTER EXCLUDING JOIN = Igual que FULL [OUTER] JOIN pero con
	A.Key IS NULL OR B.Key IS NULL para excluir lo que coincida en ambas tablas

### APPLY
**CROSS APPLY** = Es como un [INNER] JOIN
**OUTER APPLY** = Es como un LEFT [OUTER] JOIN
    
    SELECT * FROM Department D 
    CROSS APPLY dbo.fn_GetAllEmployeeOfADepartment(D.DepartmentID)
    
    SELECT  * FROM table1
    CROSS APPLY (
    	SELECT TOP (table1.rowcount) *
    FROM table2
    ORDER BY id
    ) t2
    
    SELECT R.IdReservacion, RE.IdTipoEstatusReservacion
    FROM Reservacion R
    INNER JOIN (
    	SELECT RE.IdReservacion, RE.IdTipoEstatusReservacion, ROW_NUMBER() OVER(PARTITION BY RE.IdReservacion ORDER BY RE.Fecha DESC) [Index]
    	FROM dbo.ReservacionEstatus RE WITH(NOLOCK)
    ) AS RE ON RE.IdReservacion = R.IdReservacion AND RE.[Index] = 1
    
    SELECT r.IdReservacion, RE.IdTipoEstatusReservacion --Esta opcion tiene mejor rendimiento
    FROM Reservacion R
    CROSS APPLY(
    	SELECT TOP 1 IdTipoEstatusReservacion
    	FROM dbo.ReservacionEstatus WHERE IdReservacion = R.IdReservacion
    	ORDER BY Fecha DESC
    )RE
    
### STUFF
Pone un string dentro de otro string, STUFF(character_expression, start, length, replaceWith)

    SELECT STUFF(';KEITH;STEFAN;EDUARD;BRAD', 1, 1, '') --KEITH;STEFAN;EDUARD;BRAD

### FOR XML
Regresa el resultado de un query como XML, PATH genera un elemento por cada row
En conjunto regresa el resultado como un XML en un row en una columa

    SELECT A.IdAllotment, STUFF((
    SELECT ', ' + CAST(S.IdParteRole AS nvarchar(5))
    FROM AsignacionAllotmentParteRole S
    WHERE S.IdAllotment = A.IdAllotment
    ORDER BY S.IdParteRole
    FOR XML PATH('')
    ),1,1,'') [AllotParteRole]
    FROM Allotment A

	--------------------------------------
    IdAllotment	AllotParteRole
    1	 	68, 69, 70, 71, 72, 73, 74, 75, 76, 77, 78, 79, 80, 81, 82, 83, 84, 172, 173, 202
    2	 	130, 131, 132, 133, 134, 135, 136, 137, 138, 139, 140, 141, 142, 143, 144, 145, 146, 147, 204
    

### TABLE FUNCTION
    CREATE FUNCTION dbo.fn_GetAllEmployeeOfADepartment(@DeptID AS INT)  
    RETURNS TABLE 
    AS 
    RETURN 
       ( 
       SELECT * FROM Employee E 
       WHERE E.DepartmentID = @DeptID 
       ) 
    GO 

### COALESCE
    
    DECLARE @DepartmentName VARCHAR(1000) 
    SELECT @DepartmentName = COALESCE(@DepartmentName,'') + Name + ';'  
    FROM Department  
    
    SELECT @DepartmentName AS DepartmentNames 

### RANKING FUNCTIONS
    ROW_NUMBER() OVER ( [PARTITION BY partition_value_expression] ORDER BY order_value_expression)

	FIRST_VALUE|LAST_VALUE([scalar_expression]) OVER ( [PARTITION BY partition_value_expression] ORDER BY order_value_expression) 

	LAG([scalar_expression] [,offset] [,default]) OVER ( [PARTITION BY partition_value_expression] ORDER BY order_value_expression)

### WITH
	WITH __cuenta (IdCuentaContable, IdCuentaContablePadre, Numero, Level)
	AS
	(
		SELECT CC.IdCuentaContable, CC.IdCuentaContablePadre, CC.Numero, 0 AS Level
		FROM dbo.CuentaContable CC
		WHERE CC.IdCuentaContablePadre IS NULL

		UNION ALL

		SELECT CC.IdCuentaContable, CC.IdCuentaContablePadre, CC.Numero, Level + 1
		FROM dbo.CuentaContable CC  
		INNER JOIN __cuenta AS C ON C.IdCuentaContable = CC.IdCuentaContablePadre	
	)
	SELECT *
	FROM __cuenta 
 
### INTERSECT
### EXCEPT, MINUS
### OPERADORES ^ | & << >>
- | es OR
- & en AND
- ^ es XOR, Iguales 0, diferentes 1
- ~ es NOT
###SIZE AND RANGE OF DATATYPES###
###SYSTEM STORE PROCEDURES###
###SYSTEM OBJECTS###
###WINDOWING FUNCTIONS###
###OTHERS###
    DISTINTO DE-> 		<>   ó   !=
    SET DATEFORMAT
    SPACE(4) -> Regresa 4 espacios, soporta variable    
    LIKE
    	'[a-r]%'	rango de valores
    	'[^a-r]%'	exclusion de rango de valores
    	'[^aeiou]%'	exclusion de lista de valores
	
### DATE CONVERT

### Aggregated Functions
- `COUNT(*)` todos los registros son considerados en el conteo
- `COUNT(ColumnA)` se omiten los valores NULL de ColumnA
- `SELECT COUNT(1) FROM Table1` here **1** is a constant and has nothing with the ordinal positions of the table’s columns. 
- `SELECT COUNT(ALL ColumnA) FROM Table1` es lo mismo que COUNT(ColumnA)
- `SELECT COUNT(DISTINCT 1) FROM Table1` **1** is again only a constant. By having DISTINCT for the constant, the query will return a count of 1. The next is a similar query `SELECT COUNT(DISTINCT 2) FROM #TmpCounts; -- 1 count`
- `SELECT COUNT(DISTINCT ColumnA) FROM Table1` will count distinct values in ColumnA

### PRACTICING
CURSOR, WITH, ROW_NUMBER, LEAD, LAG, FIRST_VALUE, LAST_VALUE, PARTITION BY,
CUBE, ROLLUP, UPSERT, INSERT SELECT, HIERARCHY WITH, STUFF, PATHINDEX, REPLACE, REPLICATE, CAST, CONVERT, PARSE, STRING MANIPULATION


### SEQUENCIAL NUMBERS TABLE/VIEW

    CREATE VIEW [dbo].[vTally]
    AS
    WITH  L0 AS (SELECT 1 AS C UNION ALL SELECT 1)--2 rows
    ,L1 AS (SELECT 1 AS C FROM L0 AS A, L0 AS B)--4 rows
    ,L2 AS (SELECT 1 AS C FROM L1 AS A, L1 AS B)--16 rows
    ,L3 AS (SELECT 1 AS C FROM L2 AS A, L2 AS B)--256 rows
    ,L4 AS (SELECT 1 AS C FROM L3 AS A, L3 AS B)--65536 rows
    ,L5 AS (SELECT 1 AS C FROM L4 AS A, L4 AS B)--WHOA!
    ,Tally as (SELECT ROW_NUMBER() OVER(ORDER BY (SELECT NULL)) as N
     from L5) 
    
    SELECT TOP(1000000) N FROM Tally
    GO
    
    WITH Dates (N, CalendarDate) as 
    (SELECT TOP(1000) N, DATEADD(DAY,N-1, CONVERT(DATE,'1/1/2016')) 
      FROM vTally)
    SELECT N, CalendarDate FROM Dates;

>#### Salida
##### N CalendarDate

- 1	2016-01-01
- 2	2016-01-02
- 3	2016-01-03
- 4	2016-01-04
- 5	2016-01-05
- 6	2016-01-06

http://www.sqlservercentral.com/articles/calendar/145206/

How to Write a Better T-SQL Code (revisar este articulo)
- http://www.sqlservercentral.com/blogs/si-vis-pacem-para-sql/2018/11/16/how-to-write-a-better-t-sql-code/?utm_source=SSC&utm_medium=pubemail