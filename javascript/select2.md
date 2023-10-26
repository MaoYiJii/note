# Select2

## Select2 的多種場景應用

### 開始使用 Select2

固定選項

```js
$select2.select2({
    data: [
        {
            id: "value1",
            text: "text1"
        }, {
            id: "value2",
            text: "text2"
        }, {
            id: "value3",
            text: "text3"
        }
    ]
});
```

給予一個寬度

```js
$select2.select2({
    width: 300,
    data: [
        {
            id: "value1",
            text: "text1"
        }, {
            id: "value2",
            text: "text2"
        }, {
            id: "value3",
            text: "text3"
        }
    ]
});
```

加入一個空選項

```js
$select2.select2({
    width: 300,
    data: [
        {
            id: "",
            text: ""
        }, {
            id: "value1",
            text: "text1"
        }, {
            id: "value2",
            text: "text2"
        }, {
            id: "value3",
            text: "text3"
        }
    ]
});
```

清除功能

```js
$select2.select2({
    width: 300,
    allowClear: true,
    placeholder: "", // allowClear 為 true 時，一定要設置 placeholder，不然清除功能會發生異常
    data: [
        {
            id: "",
            text: ""
        }, {
            id: "value1",
            text: "text1"
        }, {
            id: "value2",
            text: "text2"
        }, {
            id: "value3",
            text: "text3"
        }
    ]
});
```

移除搜尋功能

```js
$select2.select2({
    width: 300,
    allowClear: true,
    placeholder: "", // allowClear 為 true 時，一定要設置 placeholder，不然清除功能會發生異常
    minimumResultsForSearch: Infinity,
    data: [
        {
            id: "",
            text: ""
        }, {
            id: "value1",
            text: "text1"
        }, {
            id: "value2",
            text: "text2"
        }, {
            id: "value3",
            text: "text3"
        }
    ]
});
```

固定選項+無搜尋+單選 固定選項+無搜尋+多選 固定選項+搜尋+單選 固定選項+搜尋+多選

動態選項+無搜尋+單選 動態選項+無搜尋+多選 動態選項+搜尋+單選 動態選項+搜尋+多選

$select2.select2({ width: 200, allowClear: true, placeholder: "", minimumResultsForSearch: Infinity, //tags: true, data: \[ { id: "value1", text: "text1" }, { id: "value2", text: "text2" }, { id: "value3", text: "text3" } ] });
