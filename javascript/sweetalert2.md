# SweetAlert2

## Alert
``` js
Swal
    .fire({
        title: "提示訊息",
        text: message,
        confirmButtonText: "確認",
        customClass: {
            confirmButton: "btn btn-primary"
        },
        buttonsStyling: false,
        allowOutsideClick: false
    })
    .then(function () {
        // todo when close
    });
```


## Confirm
``` js
Swal
    .fire({
        title: "待確認訊息",
        text: message,
        type: "question",
        showCancelButton: true,
        confirmButtonText: "確認",
        cancelButtonText: "取消",
        customClass: {
            confirmButton: "btn btn-primary m-1",
            cancelButton: "btn btn-secondary m-1"
        },
        buttonsStyling: false,
        allowOutsideClick: false,
    })
    .then(function (result) {
        // check confirm result
        if (Boolean(result.value)) {
            // todo when confirm
        }
    })
```

## Prompt
``` js
Swal
    .fire({
        title: title,
        input: "text",
        inputAttributes: {
            autocapitalize: "off"
        },
        // 如果沒有要必填，設為 null
        inputValidator: function (value) {
            if (!value) {
                return "Required."
            }
        },
        showCancelButton: true,
        confirmButtonText: "確認",
        cancelButtonText: "取消",
        customClass: {
            confirmButton: "btn btn-primary m-1",
            cancelButton: "btn btn-secondary m-1"
        },
        buttonsStyling: false,
        allowOutsideClick: false
    })
    .then(function (result) {
        // get result.value
    })
```