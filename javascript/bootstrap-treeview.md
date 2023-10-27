# Bootstrap TreeView

### 範例1: 選單權限，上下層選取邏輯

```js
$("#tree").on("nodeSelected nodeUnselected", function (event, node) {
    // 同步 selected 與 checked
    $("#tree").treeview(node.state.selected ? "checkNode" : "uncheckNode", [node.nodeId, { silent: false }]);
});
$("#tree").on("nodeChecked nodeUnchecked", function (event, node) {
    // 同步 selected 與 checked
    $("#tree").treeview(node.state.checked ? "selectNode" : "unselectNode", [node.nodeId, { silent: true }]);
    // 向下全選 / 取消全選
    if (node.nodes && node.nodes.length && !$("#tree").data("event-from-child")) {
        node.nodes.forEach(function (x) {
            $("#tree").treeview(node.state.checked ? "checkNode" : "uncheckNode", [x.nodeId, { silent: false }]);
        });
    }
    if (node.parentId || node.parentId == 0) {
        if (node.state.checked) {
            // 選取的話，向上同步選取
            $("#tree").data("event-from-child", true);
            $("#tree").treeview("checkNode", [node.parentId, { silent: false }]);
            $("#tree").data("event-from-child", false);
        }
        else {
            // 取消選取的話，同一層都取消選取才將上層取消選取
            var parentNode = $("#tree").treeview("getNode", node.parentId);
            if (parentNode.nodes.filter(function (x) { return x.state.checked; }).length == 0) {
                $("#tree").treeview("uncheckNode", [parentNode.nodeId, { silent: false }]);
            }
        }
    }
});
```

[https://github.com/jonmiles/bootstrap-treeview](https://github.com/jonmiles/bootstrap-treeview)
