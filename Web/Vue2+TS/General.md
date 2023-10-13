### 新增、刪除 body 的 class
```
document.body.classList.add("foo");

document.body.classList.remove("foo");
```

### 靜態資源
```
// 宣告變數
logo: NodeRequire | undefined;
// 載入資源
this.logo = require("admin-lte/dist/img/AdminLTELogo.png");
```


### lodash
```
_
.chain(this.playingPanel.chess.tiles!.filter(x => x.group != undefined))
.groupBy("group")
.map((value, key) => value)
.value()
```

### 使用 jquery
讓 ts 看懂
```
npm install @types/jquery --save-dev
```
引用
``` js
import $ from 'jquery';
```