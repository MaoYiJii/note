# JQuery

#### AJAX

request with token

```js
$.ajax({
    type: "GET",
    url: "http://localhost:8089/api/User",
    beforeSend: function(request) {
        request.setRequestHeader("Authorization", "Bearer " + "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJuYW1laWQiO…BlIn0.aB7b1ghmIrTlLoLkS7yB9ag6VoAPiUJwc4MP14yjm-U");
    },
    success: function(response) {
        console.log(response);
    }
});
```

登入

```js
$.post("http://localhost:8089/api/User/Token", {
    Email: "aa@aa.com",
    Password: "123456"
});
```

查看登入資訊

```js
$.ajax({
    type: "GET",
    url: "http://localhost:8089/api/User",
    beforeSend: function(request) {
        request.setRequestHeader("Authorization", "Bearer "
            + "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJUMSI6IlYxIiwic3ViIjoiOTc5NmU4N2YtMDBkZS00M2Y1LTFjOTktMDhkYjhjMTRlMWZlIiwidW5pcXVlX25hbWUiOiJBQUEiLCJqdGkiOiI4MDQ3NTQ3Ni1iOGIyLTRmMDEtYmU3NS00OTc0NGViNWUxMjUiLCJpYXQiOjE2OTAyNjcwODgsInJvbGUiOiJSb2xlMSIsIlQyIjoiVjIiLCJuYmYiOjE2OTAyNjcwODgsImV4cCI6MTY5MDI3MDY4OCwiaXNzIjoiV2ViUHJvdG90eXBlIn0.Iu7zvpX54eB6WJ2x6RpzRjZcuodAJuFUI7dXTRc7yVY"
        );
    },
    success: function(response) {
        console.log(response);
    }
});
```

建立人員

```js
$.post("http://localhost:8089/api/User/Add", {
    UserName: "AAA",
    Email: "aaa@aaa.com",
    Password: "123456",
    PasswordConfirm: "123456"
});
```

建立角色

```js
$.post("http://localhost:8089/api/Role/Add", {
    RoleName: "Role1",
    Concurrency: "Group1"
});
```

將人員加入角色

```js
$.post("http://localhost:8089/api/User/AddRole", {
    UserId: "9796E87F-00DE-43F5-1C99-08DB8C14E1FE",
    RoleName: "Role1"
});
```

新增角色宣告

```js
$.post("http://localhost:8089/api/Role/AddClaim", {
    RoleName: "Role1",
    ClaimType: "T2",
    ClaimValue: "V2"
});
```
