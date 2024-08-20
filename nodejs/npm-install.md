# npm install

### 編輯 `package.json` 後重新安裝

1. 編輯 `package.json` (新增或移除套件)
2. 執行 `npm install --package-lock-only` (讓 `package-lock.json` 與 `package.json` 內容同步)
3. 執行 `npm ci`

### 錯誤處理

1.  #### 錯誤訊息

    ```
    IsNearDeath': 不是 'Nan::Persistent< v8::Object,v8::NonCopyablePersistentTraits<T>>' 的成員
    ```

    #### 解決方式

    移除 `package-lock.json` 與 `node_modules` 再重新執行 `npm install`
