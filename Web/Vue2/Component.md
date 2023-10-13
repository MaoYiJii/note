## 轉換成 Component

以下是一個計數器功能
``` html
<div id="app">
    <button v-on:click="increment">
        Count is: {{ count }}
    </button>
</div>
<script type="text/javascript">
    $(function () {
        new Vue({
            el: "#app",
            data: function () {
                return {
                    count: 0
                };
            },
            methods: {
                increment: function () {
                    this.count++
                }
            }
        });
    });
</script>
```
將它寫成一個可重複使用的組件 *(全域註冊)*
``` html
<script type="text/x-template" id="counter">
    <button v-on:click="increment">
        Count is: {{ count }}
    </button>
</script>
<script type="text/javascript">
    $(function () {
        Vue.component("counter", {
            template: "#counter",
            data: function () {
                return {
                    count: 0
                };
            },
            methods: {
                increment: function () {
                    this.count++
                }
            }
        });
    });
</script>
```

### 改變的東西
- `<div id="app">` 變成了 `<script type="text/x-template" id="counter">`
- `new Vue` 變成了 `Vue.component` 並且在前面加一個參數 `counter` 做為 `組件名稱`
- `el: "#app"` 變成了 `template: "#counter"`

### 使用組件

當上面執行完 `Vue.component("counter", {` 僅是註冊了一個名稱為 `counter` 的組件，並不會立即在畫面上呈現組件

而是在需要使用組件的地方以 `組件名稱` 做為 HTML 的 `tagName` 來使用組件
``` html
<div id="app">
    <counter></counter>
</div>
<script type="text/javascript">
    $(function () {
        new Vue({
            el: "#app"
        });
    });
</script>
```

## 組件參數

當建立了一個可重複使用的組件後，通常會希望可以透過參數來改變組件的部分行為
這時候可以透過 `props` 來定義組件的參數

以上面的計數器為例
現在為它加入兩個參數:
1. `initial`: 計數器初始值
2. `step`: 每次點擊時增加的數量

### 定義參數

最簡單的方式可以用字串陣列來定義參數名稱
``` js
Vue.component("counter", {
    template: "#counter",
    props: ["initial", "step"],
    ...
});
```

### 取得參數的值

在 `data` 裡面可以透過 function 的 `第一個參數` 來取得 props 的值
``` js
Vue.component("counter", {
    template: "#counter",
    props: ["initial", "step"],
    data: function (vm) {
        return {
            count: vm.initial
        };
    },
    ...
});
```
在 `instance` 建立後，可以直接透過 `this` 取得 props 的值
``` js
Vue.component("counter", {
    template: "#counter",
    props: ["initial", "step"],
    data: function (vm) {
        return {
            count: vm.initial
        };
    },
    methods: {
        increment: function () {
            this.count += this.step;
        }
    }
});
```

### 給予參數的值

接著可以透過 `v-bind` 來設定參數的值
``` html
<div id="app">
    <counter :initial="10" :step="2"></counter>
</div>
<script type="text/javascript">
    $(function () {
        new Vue({
            el: "#app"
        });
    });
</script>
```

### 定義參數優化

以上的組件運行起來看似沒有問題，但其實缺少了 `防呆` 機制

- 問題 1:
  回到沒有給與參數的使用方式會造成異常
  ``` html
  <counter></counter>
  ```
  
- 問題 2:
  當給予的參數不是一個數字時會造成異常
  ``` html
  <counter :initial="'foo'" :step="'bar'"></counter>
  ```

這時候可以將 props 寫成物件的形式 *([參考](https://v2.vuejs.org/v2/guide/components-props#Prop-Validation))*
- `屬性名稱` 當作 `參數名稱`
- `屬性的值` 可以是一個 `類型` 或一個 `物件`
  - 以物件中的 `type` 來定義 `可接收的類型` *(可解決問題 2)*
  - 以物件中的 `default` 來定義 `預設值` *(可解決問題 1)*
``` js
Vue.component("counter", {
    template: "#counter",
    props: {
        initial: {
            type: Number,
            default: 0
        },
        step: {
            type: Number,
            default: 1
        }
    },
    ...
});
```

### 可變動的參數

