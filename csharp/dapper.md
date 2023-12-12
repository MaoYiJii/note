
### Query DataTable

``` cs
/// <summary>
/// 執行 SQL 指令並以 DataTable 回傳第一個完整結果
/// </summary>
public virtual DataTable QueryDataTable(IDbConnection conn, IDbTransaction tran, string sql, object param = null, int? commandTimeout = null, CommandType? commandType = null)
{
    var reader = conn.ExecuteReader(sql, param, tran, commandTimeout, commandType);
    var dt = new DataTable();
    dt.Load(reader);
    return dt;
}
```

### Query DataSet

``` cs
/// <summary>
/// 執行 SQL 指令並以 DataSet 回傳所有完整結果
/// </summary>
public virtual DataSet QueryDataSet(IDbConnection conn, IDbTransaction tran, string sql, object param = null, int? commandTimeout = null, CommandType? commandType = null)
{
    DataSet ds = new DataSet();
    var reader = conn.ExecuteReader(sql, param, tran, commandTimeout, commandType);
    while (!reader.IsClosed && reader.FieldCount > 0)
    {
        DataTable dt = new DataTable();
        dt.Load(reader);
        ds.Tables.Add(dt);
    }
    return ds;
}
```

### Query Dictionary Collection

``` cs
/// <summary>
/// 執行 SQL 指令並以 IDictionary 的集合回傳第一個完整結果
/// </summary>
public virtual IList<IDictionary<string, object>> QueryDictionaryList(IDbConnection conn, IDbTransaction tran, string sql, object param = null, int? commandTimeout = null, CommandType? commandType = null)
{
    return conn.Query(sql, param, tran, commandTimeout: commandTimeout, commandType: commandType).Cast<IDictionary<string, object>>().ToList();
}
```

### Query T Collection

``` cs
/// <summary>
/// 執行 SQL 指令並以指定類型的集合回傳第一個完整結果
/// </summary>
public virtual IList<T> QueryList<T>(IDbConnection conn, IDbTransaction tran, string sql, object param = null, int? commandTimeout = null, CommandType? commandType = null)
{
    return conn.Query<T>(sql, param, tran, commandTimeout: commandTimeout, commandType: commandType).ToList();
}
```

### Query T1 Collection and T2 Collection

``` cs
public virtual (IList<T1>, IList<T2>) QueryMultiple<T1, T2>(IDbConnection conn, IDbTransaction tran, string sql, object param = null, int? commandTimeout = null, CommandType? commandType = null)
{
    IList<T1> list1 = null;
    IList<T2> list2 = null;
    using (var reader = conn.QueryMultiple(sql, param, tran, commandTimeout, commandType))
    {
        if (!reader.IsConsumed)
        {
            list1 = reader.Read<T1>().ToList();
        }
        if (!reader.IsConsumed)
        {
            list2 = reader.Read<T2>().ToList();
        }
    }
    return (list1, list2);
}
```


## 資料表值參數

共用類別
``` cs
public class TableValuedParameter : SqlMapper.ICustomQueryParameter
{
    public string DbTypeName { get; }
    public DataTable Table { get; }

    public TableValuedParameter(string dbTypeName, DataTable table)
    {
        DbTypeName = dbTypeName;
        Table = table;
    }

    public virtual void AddParameter(IDbCommand command, string parameterName)
    {
        Table.AsTableValuedParameter(DbTypeName).AddParameter(command, parameterName);
    }
}
```