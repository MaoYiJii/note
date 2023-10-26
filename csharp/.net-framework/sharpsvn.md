# SharpSVN

#### 判斷檔案或目錄是否存在

```cs
private bool SvnIsExist(string svnPath)
{
    bool isExist = false;
    using (SvnClient client = new SvnClient())
    {
        client.Authentication.DefaultCredentials = new System.Net.NetworkCredential(svnUserID, svnPasswork);
        Collection<SvnInfoEventArgs> info;
        SvnTarget target = SvnTarget.FromUri(new Uri(svnPath));
        isExist = client.GetInfo(target, new SvnInfoArgs { ThrowOnError = false }, out info);
    }
    return isExist;
}
```

#### 取得檔案或目錄的 Revision

```cs
private long SvnGetRevision(string svnPath)
{
    using (SvnClient client = new SvnClient())
    {
        client.Authentication.DefaultCredentials = new System.Net.NetworkCredential(svnUserID, svnPasswork);
        SvnInfoEventArgs svnInfo;
        client.GetInfo(new Uri(svnPath), out svnInfo);
        return svnInfo.Revision;
    }
}
```

#### 取得目錄下的清單

```cs
private List<string> GetSvnFileList(string svnPath)
{
    List<string> returnList = new List<string>();

    using (SvnClient client = new SvnClient())
    {
        client.Authentication.DefaultCredentials = new System.Net.NetworkCredential(svnUserID, svnPasswork);
        Collection<SvnListEventArgs> CollectionList;
        SvnListArgs args = new SvnListArgs();
        args.Depth = SvnDepth.Infinity;		// 包含子資料夾內的清單
        if (client.GetList(new Uri(svnPath), args, out CollectionList))
        {
            foreach (SvnListEventArgs list in CollectionList)
            {
                if (list.Path.Length > 0)
                {
                    if (list.Entry.NodeKind == SvnNodeKind.File)	// 僅檔案 (也可不過濾)
                    {
                        returnList.Add(list.Path);
                    }
                }
            }
        }
    }

    returnList.Sort();
    return returnList;
}
```

#### 取得 LOG

```cs
using (SvnClient client = new SvnClient())
{
    client.Authentication.DefaultCredentials = new System.Net.NetworkCredential(svnUserID, svnPasswork);
    Collection<SvnLogEventArgs> logs;
    client.GetLog(new Uri("svn://192.168.1.131/RCDATA"), out logs);
    foreach (var log in logs)
    {
        long revision = log.Revision;
        string author = log.Author;
        DateTime checkindate = log.Time;
        string message = log.LogMessage;
    }
}
```
