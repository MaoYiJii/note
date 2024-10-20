# SignalR

### NET 6

#### 建立 ChatHub
``` cs
using Microsoft.AspNetCore.SignalR;

namespace CoreUIAdmin.Features;

public class ChatHub : Hub
{
    public async Task NewMessage(long username, string message)
    {
        await Clients.All.SendAsync("messageReceived", username, message);
    }
}
```

#### 設置 Program.cs
``` cs
builder.Services.AddSignalR();
```
``` cs
app.MapHub<ChatHub>("/hub");
```

#### 指定使用者
`user` 對應 `ClaimTypes.NameIdentifier`
``` cs
Clients.User(user).SendAsync("ReceiveMessage", message);
```

#### 加入群組
``` cs
await Groups.AddToGroupAsync(Context.ConnectionId, groupName);
```

#### 退出群組
``` cs
await Groups.RemoveFromGroupAsync(Context.ConnectionId, groupName);
```

#### 指定群組
``` cs
await Clients.Group(groupName).SendAsync("Send", $"{Context.ConnectionId} has joined the group {groupName}.");
```

#### 從 Client 端呼叫 Hub
```
connection
    .invoke("NewMessage", username, message)
    .catch(err => {
        console.error(err);
    });
```


#### 從 Server 端呼叫 Hub

注入 `IHubContext<ChatHub> hubContext`
``` cs
await _hubContext.Clients.All.SendAsync("NewMessage", username, message);
```




### Vue 3 (Typescript)

#### 安裝套件
```
npm i @microsoft/signalr@6.0.25
```


### .NET 8


#### 注入服務
.NET 8 的 Hub 方法參數會盡可能從 DI 解析服務
``` cs
public class ChatHub : Hub
{
    public async Task NewMessage(long user, string message, IDatabaseService dbService)
    {
        var username = dbService.GetUserName(user);
        await Clients.All.SendAsync("messageReceived", username, message);
    }
}
```

#### 預設為不解析 DI
在 `Program.cs` 將 `DisableImplicitFromServicesParameters` 設為 `true`
``` cs
services.AddSignalR(options =>
{
    options.DisableImplicitFromServicesParameters = true;
});
```


