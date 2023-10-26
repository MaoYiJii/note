# jquery-tmpl

[jquery-tmpl](https://github.com/BorisMoore/jquery-tmpl)

#### 樣板

```html
<script id="CityOptionTmpl" type="text/x-jquery-tmpl">
    <option value="${ZipCode}">${Title}</option>
</script>
```

#### 取用

```js
$("#CityOptionTmpl").tmpl(data).appendTo("#City");
```

#### 從樣板拿回資料

```js
$.tmplItem(this).data
```
