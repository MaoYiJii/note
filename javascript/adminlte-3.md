# AdminLTE 3

#### DirectChat

[Docs](https://adminlte.io/docs/3.0/javascript/direct-chat.html)

```html
<div class="direct-chat">
    <div id="chat-pane-toggle" class="direct-chat-contacts">
        
    </div>
</div>
```

```js
$("#chat-pane-toggle").DirectChat("toggle");
```

#### 讓 body 的高度重新計算

```js
$("body").data("lte.layout").fixLayoutHeight();
```
