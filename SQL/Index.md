### 取得 identity
``` sql
SELECT IDENT_CURRENT({tableName})
```

### 新增包含 identity
``` sql
SET IDENTITY_INSERT [dbo].[{tableName}] ON
--INSERT INTO 
SET IDENTITY_INSERT [dbo].[{tableName}] OFF
```

### 查詢分頁資料
``` sql
SELECT * FROM {tableName}
ORDER BY {columnName}
OFFSET {skip} ROWS FETCH NEXT {take} ROWS ONLY
```

### LIKE

| 字元   | 功能                          | 備註 |
| ------ | ----------------------------- | ---- |
| %      | 匹配 0~N 個任意字元           |      |
| _      | 匹配 1   個任意字元           |      |
| [...]  | 匹配 1 個 [] 內的任何一個字元 |      |
| ESCAPE | 以指定字元做完跳脫字元        |      |

範例
```



```

### partition
SELECT Row_number() OVER( partition BY fr.ParentFormId, fr.ParentFieldName, fr.FormId, fr.[Name] ORDER BY ar.EndTime DESC )

### for

### foreach
``` sql
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

### PIVOT

### UNPIVOT

### Apply