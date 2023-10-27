# General

## Json

### 序列化
``` js
var json = JSON.stringify(obj);
```

### 反序列化
``` js
var obj = JSON.parse(json);
```

## 字串比對 (字串排序)

[localeCompare](https://www.techonthenet.com/js/string\_localecompare.php)

```
// 忽略大小寫
str1.localeCompare(str2, undefined, { sensitivity: 'accent' })
// 以數字排序
str1.localeCompare(str2, undefined, { numeric: true })
```

## 日期處理

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

## 改變 Url (不跳轉)

```js
window.history.pushState("", "", getPageData("AppVirtualPath") + "Profile/Index");
```

## 頁面跳轉

### 變更 QueryString

```js
var url = new URL(location.href);
url.searchParams.set('sso', 1);
location.href = url.href;
```

_IE不支援_

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

## 連結的 active 隨著捲軸變動

```html
<style>
    .mobile-sticky-container {
        padding: 5px 15px;
        background: #505050;
    }

        .mobile-sticky-container.sticky {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
        }
</style>

<div class="mobile-sticky-container">
    <a href="#mobile_t_content" class="btn btn-primary mt-2 mb-2">總計</a>
    <a href="#mobile_a_content" class="btn btn-primary mt-2 mb-2">應發</a>
    <a href="#mobile_o_content" class="btn btn-primary mt-2 mb-2">應發-加班</a>
    <a href="#mobile_d_content" class="btn btn-primary mt-2 mb-2">自付額</a>
    <a href="#mobile_l_content" class="btn btn-primary mt-2 mb-2">缺勤</a>
</div>
<div id="mobile_t_content" style="background-color: red; height: 450px;"></div>
<div id="mobile_a_content" style="background-color: orange; height: 450px;"></div>
<div id="mobile_o_content" style="background-color: yellow; height: 450px;"></div>
<div id="mobile_d_content" style="background-color: green; height: 450px;"></div>
<div id="mobile_l_content" style="background-color: blue; height: 450px;"></div>

<script>
    $(function () {
        mobileAnimateBinder();
        $(window).scroll(function () {
            if ($(".d-md-none").is(":visible")) {
                mobileButtonActiveListener();
                mobileStickyListener();
            }
        });
    });
    /** 讓 #id 的 link 有滑動效果 */
    function mobileAnimateBinder() {
        var $container = $(".mobile-sticky-container");
        $container.find("a[href^='#']").on("click", function (e) {
            e.preventDefault();
            var $dest = $(this.hash);
            $("html, body").stop().animate({
                "scrollTop": $dest.offset().top + 2
            }, 500, "swing", function () {
            });
        });
    }
    /** 根據捲軸位置切換 #id 按鈕的 active */
    function mobileButtonActiveListener() {
        var $container = $(".mobile-sticky-container");
        var $links = $container.find("a[href^='#']");
        var $contents = $links.map(function () {
            var $content = $($(this).attr("href"));
            if ($content.length) {
                return $content[0];
            }
        });
        var scrollPosition = $(window).scrollTop() + $container.outerHeight() + 15;
        var $currentContent = $contents.filter(function () {
            return $(this).offset().top < scrollPosition;
        }).last();
        var $currentLink = $links.filter("[href='#" + $currentContent.attr("id") + "']");
        $links.removeClass("active");
        $currentLink.addClass("active");
    }
    var mobileStickyTop;
    /** 讓捲軸把特定元素捲到最上方時固定在最上方 */
    function mobileStickyListener() {
        var $container = $(".mobile-sticky-container");
        if ($container.hasClass("sticky")) {
            if (window.pageYOffset < mobileStickyTop) {
                $container.removeClass("sticky");
            }
        }
        else {
            var top = $container[0].getBoundingClientRect().top
            if (top < 0) {
                mobileStickyTop = top + window.pageYOffset;
                $container.addClass("sticky");
            }
        }
    }
</script>
```

## 以按鈕控制卷軸

```html
<style type="text/css">
    #tabs-container.nav-tabs {
        overflow-x: hidden;
        overflow-y: hidden;
        flex-wrap: nowrap;
    }

        #tabs-container.nav-tabs .nav-link {
            white-space: nowrap;
        }
</style>

<div class="box box-primary">
    <div class="box-body">
        <div class="row">
            <div class="col-12 d-flex">
                <ul class="nav nav-tabs" role="tablist" id="tabs-container">
                    @for (int n = 1; n <= 100; n++)
                    {
                        <li class="nav-item" data-schedule-item="item@(n)">
                            <a class="nav-link" data-toggle="tab" href="#tab-content-item@(n)" role="tab" aria-controls="tab-content-item@(n)" aria-selected="false">
                                <span>@n - James F Chen</span>
                            </a>
                        </li>
                    }
                </ul>
                <button type="button" class="btn btn-secondary" id="btn-scroll-next">
                    <i class="fas fa-angle-right"></i>
                </button>
            </div>
        </div>
    </div>
</div>

<script>
    var containerSelector = "#tabs-container";
    var buttonSelector = "#btn-scroll-next";
    $(function () {
        $(buttonSelector).on("click", scrollToNext);
    });
    function scrollToNext() {
        var nextElement = findScrollNextElement();
        if (nextElement) {
            scrollByNext(nextElement);
        }
        if (!findScrollNextElement()) {
            $(buttonSelector).find(".fas").removeClass("fa-angle-right").addClass("fa-angle-double-left");
            $(buttonSelector).off("click");
            $(buttonSelector).on("click", scrollToStart);
        }
    }
    function findScrollNextElement() {
        var containerWidth = document.querySelector(containerSelector).getBoundingClientRect().width;
        var nextElement = null;
        $(containerSelector).find(".nav-item:visible").toArray().some(function (e) {
            var rect = e.getBoundingClientRect();
            if (rect.left + (rect.width / 2) >= containerWidth) {
                nextElement = e;
                return true;
            }
        });
        return nextElement;
    }
    function scrollByNext(elem) {
        elem.scrollIntoView({ behavior: "auto", block: "start", inline: "start" });
    }
    function scrollToStart() {
        document.querySelector(containerSelector).scrollLeft = 0;
        $(buttonSelector).find(".fas").removeClass("fa-angle-double-left").addClass("fa-angle-right");
        $(buttonSelector).off("click");
        $(buttonSelector).on("click", scrollToNext);
    }
</script>
```

## 子母頁面互相傳值

```html
<h2>Test1</h2>

<a href="@Url.Action("Test2")" target="_blank" rel="opener">open Test2</a><br />
<a href="@Url.Action("Test3")" target="_blank" rel="opener">open Test3</a><br />
<a href="javascript: sendDataToTest2();">send data to Test2</a><br />
<a href="javascript: sendDataToTest3();">send data to Test3</a><br />

<div>Data</div>
<div id="message"></div>

<script type="text/javascript">
    var test2, test3;

    function setData(data) {
        $("#message").append(data + "<br />");
    }

    function sendDataToTest2() {
        if (test2) {
            test2.setData("from Test1");
        }
    }
    function sendDataToTest3() {
        if (test3) {
            test3.setData("from Test1");
        }
    }
</script>
```

```html
<h2>Test2</h2>

<a href="javascript: sendDataToTest1();">send data to Test1</a><br />
<a href="javascript: sendDataToTest3();">send data to Test3</a><br />

<div>Data</div>
<div id="message"></div>

<script>
    $(function () {
        if (window.opener) {
            window.opener.test2 = window;
        }
        else {
            alert("not get window.opener");
        }
    });

    function setData(data) {
        $("#message").append(data + "<br />");
    }

    function sendDataToTest1() {
        window.opener.setData("from Test2");
    }
    function sendDataToTest3() {
        window.opener.test3.setData("from Test2");
    }
</script>
```

```html
<h2>Test3</h2>

<a href="javascript: sendDataToTest1();">send data to Test1</a><br />
<a href="javascript: sendDataToTest2();">send data to Test2</a><br />

<div>Data</div>
<div id="message"></div>

<script>
    $(function () {
        if (window.opener) {
            window.opener.test3 = window;
        }
        else {
            alert("not get window.opener");
        }
    });

    function setData(data) {
        $("#message").append(data + "<br />");
    }

    function sendDataToTest1() {
        window.opener.setData("from Test3");
    }
    function sendDataToTest2() {
        window.opener.test2.setData("from Test3");
    }
</script>
```
