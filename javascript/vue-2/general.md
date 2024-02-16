# General

## 改變資料就能更新畫面

在傳統 HTML 的時代，會需要自行維護資料與畫面呈現的一致性 以基本的計數器功能範例如下，當計算完 count 之後需要再找到顯示 count 的元素自行更新

```html
<div>
    <div>
        count: <span id="count">0</span>
    </div>
    <div>
        <button onclick="increment">increment</button>
    </div>
</div>
<script>
    var count = 0;
    function increment() {
        count++;
        document.getElementById('count').innerText = String(count);
    }
</script>
```

套用了 Vue 之後，只要設定好資料顯示在哪裡，方法怎麼被調用 在方法中只需要專注在資料處理上，畫面就會依照資料自動更新了

```html
<div id="app">
    <div>
        count: {{count}}
    </div>
    <div>
        <button v-on:click="increment">increment</button>
    </div>
</div>
<script>
    new Vue({
        el: '#app',
        data: {
            count: 0;
        },
        methods: {
            increment: function() {
                this.count++;
            }
        }
    });
</script>
```

而且在閱讀上也會變得更直覺

## Mixins

[mixins template](https://stackoverflow.com/questions/45001604/is-there-way-to-inherit-templates-with-mixins-in-vuejs)

## Component

props

作用在於接收外部的資料

也可以用物件的形式來定義 type default 如果不是字串布林要使用 function

valid

data 必須使用 function 的形式

$emit

更善用 computed 與 watch


## JSX

```js
export default {
    props: {
        name: {
            type: String
        }
    },
    data(vm) {
        return {
            count: 0
        };
    },
    methods: {
        add() {
            this.count++;
        }
    },
    render(createElement) {
        return (
            <div>
                <div>
                    Hello {this.name}
                </div>
                <div>
                    Count: {this.count}
                </div>
                <div>
                    <button type="button" onclick={this.add}>Add</button>
                </div>
            </div>
        )
    }
}
```

## 使用 jquery

讓 ts 看懂

```
npm install @types/jquery --save-dev
```

引用

```js
import $ from 'jquery';
```


## 存取 Cookie

### vue-cookies

啟用
``` ts
import VueCookies from 'vue-cookies'
app.use(VueCookies);
```
取得 Vue Instance
``` ts
const { proxy } = getCurrentInstance()!;
```
設置 cookie
``` ts
proxy!.$cookies.set('name', 'value');
```
取得 cookie
``` ts
proxy!.$cookies.get('name');
```
刪除 cookie
``` ts
proxy!.$cookies.remove('name');
```

### vue3-cookies

啟用
``` ts
import VueCookies from 'vue3-cookies'
app.use(VueCookies);
```
取得 API
``` ts
import { useCookies } from 'vue3-cookies'
const { cookies } = useCookies();
```
設置 cookie
``` ts
cookies.set('name', 'value');
```
取得 cookie
``` ts
cookies.get('name');
```
刪除 cookie
``` ts
cookies.remove('name');
```