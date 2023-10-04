vue create web-prototype
npm install axios
npm install bootstrap
npm install -D @types/bootstrap
npm install bootstrap-icons
npm install inversify reflect-metadata --save
npm install lodash
npm install -D @types/lodash
npm install moment
npm install vue-i18n@9
npm install sweetalert2
npm install vue3-table-lite

dotnet tool install --global dotnet-ef
dotnet ef migrations add InitialCreate --output-dir "Data\Migrations"
dotnet ef database update

dotnet ef migrations add 001


Vue2 完全攻略 (1) 為什麼使用前端框架

## 改變資料就能更新畫面

在傳統 HTML 的時代，會需要自行維護資料與畫面呈現的一致性
以基本的計數器功能範例如下，當計算完 count 之後需要再找到顯示 count 的元素自行更新
``` html
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

套用了 Vue 之後，只要設定好資料顯示在哪裡，方法怎麼被調用
在方法中只需要專注在資料處理上，畫面就會依照資料自動更新了
``` html
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


Vue2 完全攻略 (2) Vue 的基本操作




## Vue3

向下傳遞 slot
``` 
<template v-for="(slot, index) of Object.keys($slots)" :key="index" v-slot:[slot]>
    <slot :name="slot"></slot>
</template>
```


[使用 i18n](https://muki.tw/tech/vue/typescript-vue3-vue-i18n/)


[Bootstrap 5](https://bootstrap5.hexschool.com/docs/5.1/getting-started/introduction/)
[Bootstrap Icons](https://icons.getbootstrap.com/icons/plus/)


[Vue3 Table Lite](https://vue3-lite-table.vercel.app/quick-start)



[mixins template](https://stackoverflow.com/questions/45001604/is-there-way-to-inherit-templates-with-mixins-in-vuejs)



Vue2 自己寫組件 (Component)

props

作用在於接收外部的資料

也可以用物件的形式來定義
type
default
  如果不是字串布林要使用 function
valid

data
必須使用 function 的形式


$emit




更善用 computed 與 watch



Vue 與 Vuex 的對應
| Vue                                        | Vuex                                       | 備註                                                                     |
| --------------------------------------------- | -------------------------------------------- | ------------------------------------------------------------------------ |
| data    | state  |                                                                          |
| computed    | getters    |                                                                          |
| methods | mutations |                                                                          | store.commit('increment')


store.commit('increment', {
  amount: 10
})

mutations: {
  increment (state, payload) {
    state.count += payload.amount
  }
}



const store = new Vuex.Store({
  state: {
    count: 0
  },
  mutations: {
    increment (state) {
      state.count++
    }
  },
  actions: {
    increment (context) {
      context.commit('increment')
    }
  }
})

store.dispatch('incrementAsync', {
  amount: 10
})




### Typescript Modal Queue
``` tsx
/** 紀錄目前開啟的對話框 */
protected currentModal?: ModalWithElement;
/** 存放所有等待被開啟的 Modal */
protected modalQueue: ModalWithElement[] = [];

protected onModalHidden(resolve: Function, event: bootstrap.Modal.Event) {
  let previousModal = this.currentModal!;
  if (this.modalQueue.length) {
    this.currentModal = this.modalQueue.pop()!;
    this.currentModal.modal.show();
  }
  if (!this.modalQueue.includes(previousModal)) {
    resolve(event);
    previousModal.modal.dispose();
  }
}

/** 將 modal 排在佇列的最後，等前面的 modal 都關閉後才顯示 */
enqueueModal(el: Element, options?: Partial<bootstrap.Modal.Options>) {
  let newModal = new ModalWithElement(el, new bootstrap.Modal(el, options));
  if (this.currentModal) {
    this.modalQueue.unshift(newModal);
  }
  else {
    this.currentModal = newModal;
    this.currentModal.modal.show();
  }
  return new Promise<bootstrap.Modal.Event>((resolve, reject) => {
    newModal.el.addEventListener('hidden.bs.modal', event => {
      this.onModalHidden(resolve, event);
    });
  });
}
/** 將 modal 放在堆疊的最上面，並立即顯示，如果有當前開啟的 modal 會先隱藏起來等這個 modal 被關閉後再顯示 */
pushModal(el: Element, options?: Partial<bootstrap.Modal.Options>) {
  let newModal = new ModalWithElement(el, new bootstrap.Modal(el, options));
  if (this.currentModal) {
    this.modalQueue.push(this.currentModal);
    this.modalQueue.push(newModal);
    this.currentModal.modal.hide();
  }
  else {
    this.currentModal = newModal;
    this.currentModal.modal.show();
  }
  return new Promise<bootstrap.Modal.Event>((resolve, reject) => {
    newModal.el.addEventListener('hidden.bs.modal', event => {
      this.onModalHidden(resolve, event);
    });
  });
}
```

## Vue2 到 Vue3

Mixins --> Composition API
[1](https://www.tpisoftware.com/tpu/articleDetails/2459)
[2](https://ithelp.ithome.com.tw/articles/10250319)

https://chupai.github.io/posts/2104/compositionapi/
https://www.letswrite.tw/vue3-composition-api/

vue render jsx

JSX
``` js
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