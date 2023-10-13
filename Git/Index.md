### 比對兩個目錄哪些檔案不同
```
git diff --name-only --no-index "{path1}" "{path2}" > "{outputFilePath}.txt"
```


## GitHub Pages

### 方法 1: 建立 repository
建立一個 repository `<user>.github.io`
> 以 `https://<user>.github.io/about/contact-us.html` 瀏覽 `/about/contact-us.html`

### 方法 2: 建立 branch
在任意一個公開的 repository 建立 branch `gh-pages`
> 以 `https://<user>.github.io/<repository>/about/contact-us.html` 瀏覽 `/about/contact-us.html`