### table class
``` js
classes: "table table-bordered table-striped table-hover"
```

### thead class
``` js
theadClasses: "bg-light-blue"
```

### 沒有資料時，隱藏標題列
``` js
onPostHeader: function (t) {
    t.$header.toggle(t.data.length > 0);
}
```

### 自訂沒有資料的文字
``` js
formatNoMatches: function () {
    return "查無資料";
}
```

### Client 端過濾
``` js
$table.bootstrapTable("filterBy", {
    keyword: "{keyword}"
}, {
    filterAlgorithm: function (row, filters) {
        // get keyword by filters.keyword
        return {true|false};
    }
});
```