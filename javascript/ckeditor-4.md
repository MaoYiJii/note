# CKEditor 4

### CKEditor 無法作用在 bootstrap modal 的解法

```js
$.fn.modal.Constructor.prototype._enforceFocus = function () {
    var modalElement = this;
    $(document).on('focusin.modal', function (e) {
        if (modalElement !== e.target
            && !$(modalElement).has(e.target).length
            && $(e.target.parentNode).hasClass('cke_contents cke_reset')) {
            $(modalElement).focus();
        }
    })
};
```
