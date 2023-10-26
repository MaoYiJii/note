# Regular (Regex)

#### Atomic Group

以 `(?>foo|bar)` 表示 當在這個 Group 匹配了一個結果，不管整段規則最後是否有匹配，都不會再用這個 Group 的下一個項目進行匹配

舉例以 `(abc|ab)c` 匹配 `abcd` 可以匹配成功 若以 `(?>abc|ab)c` 匹配 `abcd` 則不會匹配成功

原因是 Atomic Group 的第一個 abc 已經匹配成功，所以不會再嘗試後面的 ab 所以 `(?>abc|ab)c` 無法匹配 `abcd` 但可以匹配 `abcc`

#### Positive Lookahead

以 `(?=foo)` 表示 匹配以這個表達式結尾的字串，但匹配成功後不會將這個表達式的字段當作 Group 的值

#### Positive Lookbehind

以 `(?<=foo)` 表示 匹配以這個表達式起始的字串，但匹配成功後不會將這個表達式的字段當作 Group 的值

#### Negative Lookahead

以 `(?!foo)` 表示 匹配排除以這個表達式結尾的字串，但匹配成功後不會將這個表達式的字段當作 Group 的值

#### Negative Lookbehind

以 `(?<!foo)` 表示 匹配排除以這個表達式起始的字串，但匹配成功後不會將這個表達式的字段當作 Group 的值
