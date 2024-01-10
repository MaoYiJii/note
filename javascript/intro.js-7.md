# Intro.js 7

## 每日消息

``` html
<style type="text/css">
    .mvw-90 {
        max-width: 90vw !important;
    }
</style>

<template id="intro1" class="d-none">
    <div>
        提供跨集團及跨區域的人才流動平台，加速優秀人才發展，讓同仁有更寛闊的發展舞台，一同走向全球市場，開啟未來視界無限可能!
    </div>
    <img src="~/app/images/intro1.png" style="width: 876px; height: 212px;" />
    <div class="d-flex justify-content-around">
        <a href="#">立即查看內部職缺</a>
        <a href="#">立即查看集團/海外職缺</a>
    </div>
</template>
<template id="intro2" class="d-none">
    <img src="~/app/images/intro2.png" style="width: 650px; height: 333px;" />
</template>
<template id="intro3" class="d-none">
    <p>
        <span class="text-info">【 水情拉緊報 】省水尖兵動起來！</span><br />
        目前，<br />
        HC/HL/TC已進入第一階段限水；<br />
        TN/KH已進入第二階段限水；<br />
        公司已展開各項節水措施，請同仁一同落實節水！
    </p>
</template>
<template id="intro4" class="d-none">
    <p>
        AUO ONE能運動會與您活力相約!
    </p>
</template>

<script type="text/javascript">
    $(function () {
        introJs()
            .setOptions({
                showBullets: true,
                dontShowAgain: true,
                dontShowAgainLabel: "今日不再顯示",
                dontShowAgainCookie: "dontShowAgain",
                dontShowAgainCookieDays: 0.7,
                tooltipClass: "mvw-90",
                prevLabel: "上一步",
                nextLabel: "下一步",
                //skipLabel: "關閉",
                steps: [
                    {
                        title: "內部職務競任【 Job Bid 2.0 】即刻重磅登場！",
                        intro: $("#intro1").html()
                    }, {
                        title: "【DEI小教室】友善共融職場第一章 : 從尊重彼此開始",
                        intro: $("#intro2").html()
                    }, {
                        title: "【節水總動員】節今日之水，解明日之渴！",
                        intro: $("#intro3").html()
                    }, {
                        title: "【AUO ONE能運動會】重磅回歸！",
                        intro: $("#intro4").html()
                    }
                ]
            })
            .start();
    });
</script>
```

## 引導

``` js
function showTours() {
    introJs()
        .setOptions({
            showBullets: false,
            prevLabel: "上一步",
            nextLabel: "下一步",
            //skipLabel: "關閉",
            steps: [
                {
                    element: document.querySelector(".main-header .navbar-nav [res^='AppName'], .main-header .navbar-nav [res^='Area_']"),
                    title: "子系統切換",
                    intro: "按下子系統可進行切換",
                    disableInteraction: true
                }, {
                    element: document.querySelector("#dropdownLanguage"),
                    title: "語言切換",
                    intro: "按下圖示可切語言,系統介面文字將自動進行更換",
                    disableInteraction: true
                }, {
                    element: document.querySelector("#buttonLogout"),
                    title: "登出",
                    intro: "退出系統",
                    disableInteraction: true
                }
            ]
        })
        .start();
}
```

### Tour API 事件

進入一個步驟依序觸發以下三個事件
onbeforechange
onchange
onafterchange

離開時依序觸發以下三個事件
onbeforeexit
onexit
oncomplete

設定事件
> 在事件方法內，可以用 `this` 取得 `introJs()` 產生的物件
``` js
introJs()
    .onbeforechange(function(targetElement) {
      alert("before new step");
    })
    .onchange(function(targetElement) {
      alert("new step");
    })
    .onafterchange(function(targetElement) {
      alert("after new step");
    })
    .onbeforeexit(function() {
      console.log("on before exit")

      // returning false means don't exit the intro
      return false;
    })
    .onexit(function() {
      alert("exit of introduction");
    })
    .oncomplete(function() {
      alert("end of introduction");
    });
```


### 使用經驗

有一些設置在沒有關閉引導的情況下，使用 refresh 並無法套用
``` js
var tours = introJs();
// 例如原本設置 showButtons: true
tours
    .setOptions({
        showButtons: true
    })
    .start();

// 想要在下一步的時候改用 showButtons: false
// 實際並沒有成功套用修改的 showButtons
tours
    .setOptions({
        showButtons: false
    })
    .refresh()
    .nextStep();
```

## Hints

### 定義
``` js
var hints = introJs()
    .setOptions({
        //showBullets: false,
        hints: [
            {
                element: document.querySelector("#dropdownLanguage"),
                hint: "切換語系",
                hintPosition: "middle-middle"
            }, {
                element: document.querySelector(".sidebar .user-panel"),
                hint: "登入資訊",
                hintPosition: "middle-middle"
            }, {
                element: document.querySelector(".content-header [name='pageTitle']"),
                hint: "功能名稱",
                hintPosition: "middle-middle"
            }
        ]
    });
```

### 顯示

``` js
hints.showHints();
```


### 隱藏

``` js
hints.hideHints();
```