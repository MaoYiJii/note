# General

### 取得 identity

```sql
SELECT IDENT_CURRENT({tableName})
```

### 新增包含 identity

```sql
SET IDENTITY_INSERT [dbo].[{tableName}] ON
--INSERT INTO 
SET IDENTITY_INSERT [dbo].[{tableName}] OFF
```

### 查詢分頁資料

```sql
SELECT *
FROM   {tableName}
ORDER  BY {columnName}
OFFSET {skip} ROWS FETCH NEXT {take} ROWS ONLY
```

### LIKE

| 字元     | 功能                  | 備註 |
| ------ | ------------------- | -- |
| %      | 匹配 0\~N 個任意字元       |    |
| \_     | 匹配 1 個任意字元          |    |
| \[...] | 匹配 1 個 \[] 內的任何一個字元 |    |
| ESCAPE | 以指定字元做完跳脫字元         |    |

範例

```



```

### partition

```sql
SELECT Row_number() OVER( partition BY fr.ParentFormId, fr.ParentFieldName, fr.FormId, fr.[Name] ORDER BY ar.EndTime DESC )
```

### for

### foreach

```sql
DECLARE @Index INT

-- 每一列的欄位
DECLARE @Column1 INT
DECLARE @Column2 NVARCHAR(50)
DECLARE @Column3 DATETIME

DECLARE MY_CURSOR CURSOR CURSOR LOCAL STATIC READ_ONLY FORWARD_ONLY FOR
  -- 來源資料
  SELECT [{Column1}],
         [{Column2}],
         [{Column3}]
  FROM   [{Table}]

OPEN MY_CURSOR

SET @Index = 0
FETCH NEXT FROM MY_CURSOR INTO @Column1, @Column2, @Column3

WHILE @@FETCH_STATUS = 0
  BEGIN
  
      -- Do something here
      -- ex: PRINT CONVERT(VARCHAR(5), @Index) + ', '
      --           + Isnull(CONVERT(NVARCHAR(100), @Column1), '') + ', '
      --           + Isnull(CONVERT(NVARCHAR(100), @Column2), '') + ', '
      --           + Isnull(CONVERT(NVARCHAR(100), @Column3), '')

      SET @Index = @Index + 1
      FETCH NEXT FROM MY_CURSOR INTO @Column1, @Column2, @Column3
  END

CLOSE MY_CURSOR

DEALLOCATE MY_CURSOR 
```

#### PIVOT

#### UNPIVOT

#### Apply

### CTE

```sql
;WITH [Foo] AS (
    SELECT *
    FROM   [Bar]
)

SELECT *
FROM   [Foo]
```

#### 最後職位任職時間

```sql
;WITH N_EMP_YEAR
     AS (SELECT Row_number() OVER( partition BY emp_no ORDER BY sdate ) AS num,
                *
         FROM   C_SMO_EMP_YEAR
         -- 資料量太大需要先過濾
         WHERE  emp_no IN ( '0003212' )),
     CHANGE_EMP_YEAR
     AS (SELECT ref.emp_no,
                Isnull(Max(nex.num), 1) AS cnum
         FROM   N_EMP_YEAR ref
                LEFT JOIN N_EMP_YEAR nex ON nex.emp_no = ref.emp_no AND nex.num = ref.num + 1
         WHERE  ( nex.num IS NOT NULL AND nex.title != ref.title )
                 OR ( nex.num IS NULL AND ref.num = 1 )
         GROUP  BY ref.emp_no)

SELECT y.emp_no,
       Sum(y.[year]) AS [year]
FROM   N_EMP_YEAR y
       INNER JOIN CHANGE_EMP_YEAR cy ON cy.emp_no = y.emp_no AND cy.cnum <= y.num
GROUP  BY y.emp_no
HAVING y.emp_no IN ( '0003212' ) 
```

### 透過正規表示法尋找數字陣列的 JSON 是否包含指定數字

```sql
SELECT * 
FROM   [TableName] 
WHERE  [ColumnName] LIKE '%[\[,]' + '12345' + '[,\]]%' ESCAPE '\' 
```

### GROUP BY + ORDER BY + TOP 1

```sql
SELECT * 
FROM   (SELECT Row_number() OVER( partition BY fr.ParentFormId, fr.ParentFieldName, fr.FormId, fr.[Name] ORDER BY ar.EndTime DESC ) AS i, 
               fr.*
        FROM   FlowApplicationRecord ar 
               INNER JOIN FlowFormFieldRecord fr ON ar.Id = fr.ApplicationRecordId
        WHERE  ApplicationId = @ApplicationId ) t 
WHERE  i = 1 AND AfterValueJson IS NOT NULL 
```

### UPDATE + SELECT (更新選單排序為連續數字)

```sql
UPDATE [Menu] 
SET    Sort = t.RowNumber 
FROM   (SELECT Id, Row_number() OVER (ORDER BY Sort) RowNumber 
        FROM   [Menu] 
        WHERE  ParentId = '3A2F26FB-762A-4453-BA58-88BA47FA907E') t 
WHERE  t.Id = [Menu].Id 
```

### 以資料表的值作為欄位名稱 (PIVOT)

```sql
SELECT *
FROM   (SELECT -- 分群欄位
               word_key,
               -- 轉置欄位
               lang,
               -- 轉置欄位內容
               word_value
        FROM   [OSISLanguage]) t
       PIVOT (-- 轉置欄位內容彙總
              Max(word_value)
              -- 轉置欄位的值 (作為轉置後的欄位名稱)
              FOR lang IN ([TW], [CN], [EN], [VN])) p
```
