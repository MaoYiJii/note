# Typescript


## Dictionary

在 js 中只要是物件都可以用自訂的字串來存取內容
``` js
var foo = {};
foo["bar"] = "baz";
```

在 ts 中需要定義物件類型像 `{ [key: string]: string }` 才可以用 `[]` 來存取
``` ts
const foo: { [key: string]: string } = {};
foo["bar"] = "baz";
```


## 以字串變數來存取物件內容

在 js 中可以透過 `[key]` 來操作物件
``` js
var foo = {
    bar: "baz"
};
var key = "bar";
console.log(foo[key]);
```

在 ts 中需要用 `keyof` 來定義字串變數的類型
``` ts
const foo = {
  bar: "baz"
};
const key: keyof typeof foo = "bar";
console.log(foo[key]);
```


## 泛型

定義泛型類別
``` ts
class Foo<T> {

}
```
定義泛型方法
``` ts
function bar<T>(arg: T) {
}
```

宣告泛型類別變數
``` ts
let foo: Foo<string>;
```
呼叫泛型方法
``` ts
bar<string>('bar');
```


## 泛型條件約束

類別
``` ts
class Foo<T extends TBase> {

}
```

方法
``` ts
function bar<T extends TBase>(arg: T) {
}
```

搭配 `keyof` 來使用
``` ts
function baz<T, TKey extends keyof T>(target: T, key: TKey): T[TKey] {
  return target[key];
}
```


## 完整定義 Function 類型

在 js 中可以將 function 存放在變數裡，並透過這個變數來執行
``` js
var foo = function(arg) {
  return arg + 1;
}
var bar = foo(1);
```

在 ts 中可以簡單的將變數定義為 `Function`
``` ts
let foo: Function = function(arg: number): number {
  return arg + 1;
}
let bar = foo(1);
```
但是會發現無法明確知道 `foo` 需要什麼參數以及回傳什麼類型

可以透過 `()` 與 `=>` 搭配來更準確定義 Function 的參數與回傳類型
``` ts
let foo: (arg: number) => number = function(arg: number): number {
  return arg + 1;
}
let bar = foo(1);
```
