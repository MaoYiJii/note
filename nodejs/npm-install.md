# npm install

### 錯誤處理

1.  #### 錯誤訊息

    ```
    IsNearDeath': 不是 'Nan::Persistent< v8::Object,v8::NonCopyablePersistentTraits<T>>' 的成員
    ```

    #### 解決方式

    移除 `package-lock.json` 與 `node_modules` 再重新執行 `npm install`
