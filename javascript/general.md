# General

#### 字串比對 (字串排序)

[localeCompare](https://www.techonthenet.com/js/string\_localecompare.php)

```
// 忽略大小寫
str1.localeCompare(str2, undefined, { sensitivity: 'accent' })
// 以數字排序
str1.localeCompare(str2, undefined, { numeric: true })
```

#### 日期處理

取得 N 個月前 1 號到今天

```js
/**
 * 以指定日期為基準加幾個月
 * @param {Date} target
 * @param {Number} months
 * @param {Boolean} carry 當改變 MM 後，dd 的值超過該月份的天數時，是否取得下個月的第一天
 */
function addMonths(target, months, carry) {
    var ref = new Date(target);
    if (ref instanceof Date && !isNaN(ref)) {
        // 先把預期的「日」記下來
        var targetDate = ref.getDate();
        // 取得預期結果月份的第一天
        ref.setDate(1);
        var monFirst = new Date(ref.setMonth(ref.getMonth() + months));
        // 取得預期結果月份的最後一天
        var monLast = new Date(monFirst.getFullYear(), monFirst.getMonth() + 1, 0);
        // 判斷預期結果月份最後一天的「日」是不是小於預期的「日」
        if (monLast.getDate() < targetDate) {
            // 如果該月份沒有預期的「日」，回傳該月份的最後一天
            if (carry === true) {
                // 如果將 carry 設置為 true，則回傳下個月的第一天
                monLast.setDate(monLast.getDate() + 1);
            }
            return monLast;
        }
        monFirst.setDate(targetDate);
        return monFirst;
    }
}
```

#### 改變 Url (不跳轉)

```js
window.history.pushState("", "", getPageData("AppVirtualPath") + "Profile/Index");
```

### 上傳檔案

#### 方法 1：使用 Form

```html
<form method="post" enctype="multipart/form-data" action="/Foo/UploadFile">
    <input type="file" name="file" />
    <button type="submit">上傳</button>
</form>
```

#### 方法 2：使用 AJAX

```js
var data = new FormData();
data.append("file", $('#file').prop('files')[0]);
$.ajax({
    url: "/Foo/UploadFile",
    cache: false,
    contentType: false,
    processData: false,
    data: data,
    type: 'post',
    success: function (data) {

    }
});
```
