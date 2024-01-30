# Lodash

#### lodash

```
_
.chain(this.playingPanel.chess.tiles!.filter(x => x.group != undefined))
.groupBy("group")
.map((value, key) => value)
.value()
```


## 取得等差數列的陣列

|                | lodash                                       | 純 js 寫法                                  |
| -------------- | -------------------------------------------- | ------------------------------------------- |
| 0 ~ (N-1)      | `_.range(n)`                                 | `Array(n).fill(0).map((x, i) => x + i)`     |
| 1 ~ N          | `_.range(1, n + 1)`                          | `Array(n).fill(1).map((x, i) => x + i)`     |
| 2, 4, 6, 8, 10 | `_.range(2, 11, 2)`<br />`_.range(2, 12, 2)` | `Array(5).fill(2).map((x, i) => x + i * 2)` |
