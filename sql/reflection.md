# Reflection

### 取得所有資料表

``` sql
SELECT [name]
FROM   sysobjects
WHERE  [type] = 'U'
ORDER  BY [name] 
```

### 取得所有資料表與欄位

``` sql
SELECT o.[name] AS [table],
       c.[name] AS [column]
FROM   sysobjects o
       LEFT JOIN syscolumns c ON c.id = o.id
WHERE  o.[type] = 'U'
ORDER  BY o.[name], c.colorder 
```

### 取得所有資料表、欄位與類型

``` sql
SELECT o.[name] AS [table],
       c.[name] AS [column],
       t.[name] AS [type]
FROM   sysobjects o
       LEFT JOIN syscolumns c ON c.id = o.id
       LEFT JOIN systypes t ON t.xtype = c.xtype AND t.usertype = c.usertype
WHERE  o.[type] = 'U'
ORDER  BY o.[name], c.colorder 
```

### 尋找資料包含指定字串的資料表與欄位
``` sql
DECLARE @SearchStr NVARCHAR(200) = N'%.png'
DECLARE @Command NVARCHAR(max)
-- https://blog.miniasp.com/post/2010/07/12/Search-all-columns-of-all-tables-in-a-database-for-a-keyword
;
WITH t
     AS (SELECT 'SELECT ''' + t.TABLE_SCHEMA + ''' AS [Schema], ''' + t.TABLE_NAME + ''' AS [Table], ''' + COLUMN_NAME + ''' AS [Column], ' + Quotename(COLUMN_NAME) + ' COLLATE Chinese_Taiwan_Stroke_CI_AS AS [Value]' + CHAR(13)
                + 'FROM ' + Quotename(t.TABLE_SCHEMA) + '.' + Quotename(t.TABLE_NAME) + ' (NOLOCK) ' + CHAR(13)
                + 'WHERE ' + Quotename(COLUMN_NAME) + ' LIKE ' + Quotename(@SearchStr, '''') command
         FROM   information_schema.TABLES t
                INNER JOIN information_schema.COLUMNS c ON c.TABLE_SCHEMA = t.TABLE_SCHEMA AND c.TABLE_NAME = t.TABLE_NAME
         WHERE  t.TABLE_TYPE = 'BASE TABLE'
                AND Objectproperty(Object_id(Quotename(t.TABLE_SCHEMA) + '.' + Quotename(t.TABLE_NAME)), 'IsMSShipped') = 0
                AND c.DATA_TYPE IN ( 'char', 'varchar', 'nchar', 'nvarchar', 'text', 'ntext' ))

SELECT @Command = (SELECT (SELECT command + CHAR(13) + 'UNION ALL' + CHAR(13) FROM t FOR xml path(''), type).value('.', 'NVARCHAR(MAX)'))

SET @Command = (SELECT Substring(@Command, 1, Len(@Command) - Len('UNION ALL' + CHAR(13))))
--SELECT @Command
EXEC(@Command) 
```

### 尋找資料包含指定數字範圍的資料表與欄位

``` sql
DECLARE @SearchMin DECIMAL = 10
DECLARE @SearchMax DECIMAL = 20
DECLARE @Command NVARCHAR(max)

;
WITH t
     AS (SELECT 'SELECT ''' + t.TABLE_SCHEMA + ''' AS [Schema], ''' + t.TABLE_NAME + ''' AS [Table], ''' + COLUMN_NAME + ''' AS [Column], ' + Quotename(COLUMN_NAME) + ' AS [Value]' + CHAR(13)
                + 'FROM ' + Quotename(t.TABLE_SCHEMA) + '.' + Quotename(t.TABLE_NAME) + ' (NOLOCK) ' + CHAR(13)
                + 'WHERE ' + Quotename(COLUMN_NAME) + ' >= ' + CONVERT(varchar(10), @SearchMin) + ' AND ' + Quotename(COLUMN_NAME) + ' <= ' + CONVERT(varchar(10), @SearchMax) command
         FROM   information_schema.TABLES t
                INNER JOIN information_schema.COLUMNS c ON c.TABLE_SCHEMA = t.TABLE_SCHEMA AND c.TABLE_NAME = t.TABLE_NAME
         WHERE  t.TABLE_TYPE = 'BASE TABLE'
                AND Objectproperty(Object_id(Quotename(t.TABLE_SCHEMA) + '.' + Quotename(t.TABLE_NAME)), 'IsMSShipped') = 0
                AND c.DATA_TYPE IN ( 'tinyint', 'smallint', 'int', 'bigint', 'float', 'real', 'decimal', 'numeric' ))

SELECT @Command = (SELECT (SELECT command + CHAR(13) + 'UNION ALL' + CHAR(13) FROM t FOR xml path(''), type).value('.', 'NVARCHAR(MAX)'))

SET @Command = (SELECT Substring(@Command, 1, Len(@Command) - Len('UNION ALL' + CHAR(13))))
--SELECT @Command
EXEC(@Command) 
```