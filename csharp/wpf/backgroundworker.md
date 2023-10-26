# BackgroundWorker

### 使用 BackgroundWorker

#### 屬性

* WorkerReportsProgress 是否需要回報進度 (搭配事件 ProgressChanged) 預設 false
* WorkerSupportsCancellation 是否可以取消 預設 false

#### 事件

* DoWork 背景執行的動作 (不能使用 UI 物件)
* ProgressChanged (需要將屬性 WorkerReportsProgress 設為 true) 更新進度的事件 (可以使用 UI 物件)
* RunWorkerCompleted 背景作業完成事件 (可以使用 UI 物件)

#### 異常

當 DoWork 發生 Exception 時會觸發 RunWorkerCompleted 會將 Exception 放到 RunWorkerCompleted 事件參數 RunWorkerCompletedEventArgs 的 Error 因次在 RunWorkerCompleted 事件需要判斷 Error 是否為 null 來確認背景作業是否正常完成

### 完整流程

```cs
// 建立背景作業
BackgroundWorker worker = new BackgroundWorker();

// 是否需要回報進度
worker.WorkerReportsProgress = true;
// 是否可以取消
worker.WorkerSupportsCancellation = true;

// 背景作業執行動作
worker.DoWork += (object sender, DoWorkEventArgs e) =>
{
    // TODO:

    // 回報進度
    (sender as BackgroundWorker).ReportProgress(10);

    // 在什麼條件下取消作業
    if (condition)
    {
        e.Cancel = true;
    }

    // 執行結果
    e.Result = "Complated";
};

// 接收回報進度事件
worker.ProgressChanged += (object sender, ProgressChangedEventArgs e) =>
{
    // TODO: 接收到回報進度的動作
};

// 背景作業完成事件
worker.RunWorkerCompleted += (object sender, RunWorkerCompletedEventArgs e) =>
{
    // 確認背景作業是否有異常
    if (e.Error != null)
    {
        // TODO:
    }

    // 確認背景作業是否被取消
    if (e.Cancelled)
    {
        // TODO:
    }

    // 取得背景作業執行結果
    var result = e.Result;

    // TODO:
};

// 開始背景作業
worker.RunWorkerAsync();
```

### 簡易流程

```cs
// 建立背景作業
BackgroundWorker worker = new BackgroundWorker();

// 背景作業執行動作
worker.DoWork += (object sender, DoWorkEventArgs e) =>
{
    // TODO:
};

// 背景作業完成事件
worker.RunWorkerCompleted += (object sender, RunWorkerCompletedEventArgs e) =>
{
    // 確認背景作業是否有異常
    if (e.Error != null)
    {
        // TODO:
    }
};

// 開始背景作業
worker.RunWorkerAsync();
```

### 寫成擴充方法

```cs
/// <summary>
/// 背景作業
/// </summary>
public static void BackgroundWork<T>(this Application app,
    Func<BackgroundWorker, T> action,
    Action<T> success = null,
    Action<Exception> error = null,
    Action<RunWorkerCompletedEventArgs> complete = null)
{
    BackgroundWorker worker = new BackgroundWorker();
    worker.DoWork += (object sender, DoWorkEventArgs e) =>
    {
        e.Result = action.Invoke(sender as BackgroundWorker);
    };
    worker.RunWorkerCompleted += (object sender, RunWorkerCompletedEventArgs e) =>
    {
        try
        {
            if (e.Error == null)
            {
                success?.Invoke((T)e.Result);
            }
            else
            {
                error?.Invoke(e.Error);
            }
        }
        finally
        {
            complete?.Invoke(e);
        }
    };
    worker.RunWorkerAsync();
}
/// <summary>
/// 背景作業 (沒有回傳值)
/// </summary>
public static void BackgroundWork(this Application app,
    Action<BackgroundWorker> action,
    Action success = null,
    Action<Exception> error = null,
    Action<RunWorkerCompletedEventArgs> complete = null)
{
    app.BackgroundWork<object>(work =>
    {
        action.Invoke(work);
        return null;
    }, x =>
    {
        success?.Invoke();
    }, error, complete);
}
```
