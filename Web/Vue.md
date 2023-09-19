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