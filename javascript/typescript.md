# Typescript

## 以字串作為屬性名稱

在 js 中可以透過 `[key]` 來操作物件
``` js
var foo = {
    bar: "bar1"
};
var key = "bar";
console.log(foo[key]);
```

在 ts 中則需要讓編譯器知道物件是有 `key` 內容對應的屬性
``` ts
const foo = {
    bar: "bar1"
};

// 將 foo 的屬性名稱做為一個 type
type FooKey = keyof typeof foo;
// 讓 key 為這個 type
const key = "bar" as FooKey;

console.log(foo[key]);
```