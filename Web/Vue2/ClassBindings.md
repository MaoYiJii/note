可先參考[官方文件](https://v2.vuejs.org/v2/guide/class-and-style)

## 基本使用方式

Vue 的 class binding 可以放 `字串` `物件` `陣列` 三種型態

以下會長出三個 `div` 都會擁有 `class1` 與 `class2`
``` vue
<template>
    <div>
        <div :class="foo"></div>
        <div :class="bar"></div>
        <div :class="baz"></div>
    </div>
</template>

<script>
    module.exports = {
        data: function () {
            return {
                foo: "class1 class2",
                bar: { class1: true, class2: true },
                baz: ["class1", "class2"]
            };
        }
    }
</script>
```

## 物件綁定

物件的 `屬性名稱` 代表 class 名稱，`屬性值` 給予 `boolean` 來代表是否綁定這個 class
``` vue
<!-- 這樣只會有 class1 -->
<div :class="{ class1: true, class2: false }"></div>
```
當需要綁定的 class 的名稱有 `-` 時，將屬性名稱表示成字串即可
``` vue
<div :class="{ 'class-1': true, 'class-2': false }"></div>
```

## 陣列綁定

通常以 `字串` 作為陣列元素
``` vue
<div :class="['class1', 'class2']"></div>
```
而陣列元素也可以放 `物件` 與 `陣列`
``` vue
<div :class="[{ class1: true }, ['class2']]"></div>
```

## 合併來源

如果有一個固定的 class 可以將 `class` 與 `:class` 同時使用
``` vue
<div class="class1" :class="bar"></div>
```
但是不能使用兩個以上的 `:class`
``` vue
<!-- 錯誤的示範 -->
<div :class="foo" :class="bar"></div>
```
需要將兩個以上的來源綁定到同一個 DOM，只需要在最外層再放一層列陣即可
``` vue
<template>
    <div :class="[foo, bar, baz]"></div>
</template>

<script>
    module.exports = {
        data: function () {
            return {
                foo: "class1 class2",
                bar: ["class3", "class4"],
                baz: { class5: true, class6: true }
            };
        }
    }
</script>
```