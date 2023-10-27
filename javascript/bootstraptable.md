# BootstrapTable

#### table class

```js
classes: "table table-bordered table-striped table-hover"
```

#### thead class

```js
theadClasses: "bg-light-blue"
```

#### 沒有資料時，隱藏標題列

```js
onPostHeader: function (t) {
    t.$header.toggle(t.data.length > 0);
}
```

#### 自訂沒有資料的文字

```js
formatNoMatches: function () {
    return "查無資料";
}
```

#### Client 端過濾

```js
$table.bootstrapTable("filterBy", {
    keyword: "{keyword}"
}, {
    filterAlgorithm: function (row, filters) {
        // get keyword by filters.keyword
        return {true|false};
    }
});
```

## 實際應用範例

``` html
<div class="card card-primary card-outline">
    <div class="card-header">
        <div class="row">
            <div class="col-md-6 form-group">
                <span style="color: red;">*</span>
                <label>工號</label>
                <select class="custom-select" id="EmpNo"></select>
            </div>
            <div class="col-md-6 form-group">
                <label>姓名</label>
                <input type="text" class="form-control" id="EmpName" readonly />
            </div>
            <div class="col-md-6 form-group">
                <label>部門</label>
                <input type="text" class="form-control" id="OrgName" readonly />
            </div>
            <div class="col-md-6 form-group">
                <label>分機</label>
                <input type="text" class="form-control" id="ExtNo" readonly />
            </div>
            <div class="col-md-6 form-group">
                <span style="color: red;">*</span>
                <label>認證名稱</label>
                <input type="text" class="form-control" id="CertName" />
            </div>
            <div class="col-md-6 form-group">
                <span style="color: red;">*</span>
                <label>認證編號</label>
                <input type="text" class="form-control" id="CertNo" />
            </div>
            <div class="col-md-6 form-group">
                <span style="color: red;">*</span>
                <label>認證機構</label>
                <input type="text" class="form-control" id="CertCompany" />
            </div>
            <div class="col-md-6 form-group">
                <span style="color: red;">*</span>
                <label>認證日期</label>
                <input type="text" class="form-control fas" id="CertDate" placeholder="&#xF073;" autocomplete="off" />
            </div>
            <div class="col-md-12 form-group">
                <button type="button" class="btn btn-success" onclick="rowAdd();">
                    <i class="fas fa-plus"></i>
                    <span>新增</span>
                </button>
                <button type="button" class="btn btn-secondary" onclick="rowClear();">
                    <i class="fas fa-eraser"></i>
                    <span>取消</span>
                </button>
            </div>
        </div>
    </div>
    <div class="card-body">
        <div class="table-responsive">
            <table id="list"></table>
        </div>
    </div>
</div>

<script type="text/javascript">
    /** bootstrapTable 的元件 */
    var $table;

    $(function () {
        // 日期元件
        $("#CertDate").datepicker({
            format: "yyyy/mm/dd",
            autoclose: true,
            container: "body > .wrapper"
        });
        // 設定人員下拉選單資料來源
        $("#EmpNo").select2({
            theme: "bootstrap4",
            width: "100%",
            allowClear: true,
            maximumInputLength: 20,
            minimumInputLength: 1,
            placeholder: "輸入工號或姓名搜尋",
            ajax: {
                delay: 255,
                transport: function (params, success, failure) {
                    client
                        .queryListAsync("GetEmpOptions", {
                            Keyword: params.data.term
                        })
                        .then(function (response) {
                            success({
                                results: response.List.map(function (x) {
                                    return {
                                        id: x.Value,
                                        text: x.Text
                                    };
                                })
                            });
                        })
                        .catch(function (reason) {
                            failure();
                        });
                }
            }
        });
        // 帶出選擇人員的資料
        $("#EmpNo").on("change", function () {
            $("#EmpName").val("");
            $("#OrgName").val("");
            $("#ExtNo").val("");
            if ($("#EmpNo").val()) {
                client
                    .queryFirstAsync("GetEmpData", {
                        EmpNo: $("#EmpNo").val()
                    })
                    .then(function (response) {
                        if (response.First) {
                            $("#EmpName").val(response.First.EmpName);
                            $("#OrgName").val(response.First.OrgName);
                            $("#ExtNo").val(response.First.ExtNo);
                        }
                        else {
                            alert("查無此工號");
                        }
                    });
            }
            // 帶出已存在資料
            $table.bootstrapTable("refresh");
        });
        // 非同步載入資料
        ui.usingProgressAsync(
            client
                .getColumnsAsync("GetList")
                .then(function (response) {
                    listInit(response.Columns.map(function (x) {
                        return {
                            escape: true,
                            field: x.Name,
                            title: x.Alias
                        };
                    }));
                })
        );
    });

    /** 列表設定 */
    function listInit(columns) {
        $table = $("#list");
        $table.bootstrapTable({
            search: false,
            //pagination: true,
            //sidePagination: "client",
            //pageList: [10, 25, 100, 'All'],
            theadClasses: "bg-gradient-auo-darkblue",
            classes: "table table-striped table-hover",
            ajax: function (request) {
                if ($("#EmpNo").val()) {
                    // 有輸入工號才查詢資料
                    client
                        .queryListAsync("GetList", {
                            EmpNo: $("#EmpNo").val()
                        })
                        .then(function (response) {
                            request.success(response.List);
                        });
                }
                else {
                    // 沒輸入工號給空陣列把列表清空
                    request.success([]);
                }
            },
            columns: [
                {
                    field: "Action",
                    title: "操作",
                    width: 180,
                    align: "center",
                    valign: "middle",
                    formatter: actionFormatter,
                },
            ].concat(columns.map(function (column) {
                switch (column.field) {
                    case "cert_name":
                    case "cert_no":
                    case "cert_company":
                        return $.extend(column, {
                            formatter: textboxFormatter,
                            events: {
                                "change :text": inputOnChange
                            }
                        });
                    case "cert_date":
                        return $.extend(column, {
                            formatter: datepickerFormatter,
                            events: {
                                "change :text": inputOnChange
                            }
                        });
                    default:
                        return column;
                }
            })),
            onPostBody: function (data) {
                // 有輸入工號才顯示列表
                if ($("#EmpNo").val()) {
                    $table.show();
                }
                else {
                    $table.hide();
                }
                // 把可編輯欄位原始的值紀錄下來 (編輯取消時還原用)
                data.forEach(function (row) {
                    if (!row.OnEdit) {
                        ["cert_name", "cert_no", "cert_company", "cert_date"].forEach(function (field) {
                            row["origin_" + field] = row[field];
                        });
                    }
                });
                // 日期元件
                $table.find("[data-field].datepicker").datepicker({
                    format: "yyyy-mm-dd",
                    autoclose: true,
                    container: "body > .wrapper"
                });
                // 日期元件 選擇值
                $table.find("[data-field].datepicker").each(function (i, e) {
                    var $input = $(e);
                    var row = data[Number($input.closest("tr").attr("data-index"))];
                    var field = $input.attr("data-field");
                    $input.datepicker("update", row[field]);
                });
            }
        });
    }
    /** 按鈕 */
    function actionFormatter(value, row, index) {
        var buttons = [];
        if (!row.OnEdit) {
            // 編輯 + 刪除
            buttons.push('<button type="button" class="btn btn-outline-secondary" title="Edit" onclick="rowEdit(' + index + ');"><i class="fas fa-pen"></i></button>');
            buttons.push('<button type="button" class="btn btn-outline-secondary" title="Delete" onclick="rowDelete(' + index + ');"><i class="fas fa-trash-alt"></i></button>');
        }
        else {
            // 儲存 + 取消
            buttons.push('<button type="button" class="btn btn-outline-secondary" title="Save" onclick="rowUpdate(' + index + ');"><i class="fas fa-save"></i></button>');
            buttons.push('<button type="button" class="btn btn-outline-secondary" title="Cancel" onclick="rowCancel(' + index + ');"><i class="fas fa-times"></i></button>');
        }
        return buttons.join("");
    }
    /** 日期輸入框 */
    function datepickerFormatter(value, row, index, field) {
        if (row.OnEdit) {
            var $input = $('<input type="text" class="form-control fas datepicker" placeholder="&#xF073;" autocomplete="off" />');
            $input.attr("data-field", field);
            $input.attr("value", value);
            if (row.OnSaving) {
                $input.attr("disabled", "disabled");
            }
            return $input[0].outerHTML;
        }
        return htmlEncode(value);
    }
    /** 文字輸入框 */
    function textboxFormatter(value, row, index, field) {
        if (row.OnEdit) {
            var $input = $('<input type="text" class="form-control" />');
            $input.attr("data-field", field);
            $input.attr("value", value);
            if (row.OnSaving) {
                $input.attr("readonly", "readonly");
            }
            return $input[0].outerHTML;
        }
        return htmlEncode(value);
    }
    /** 輸入框 事件 (change) */
    function inputOnChange(e, value, row, index) {
        this.$el.bootstrapTable("updateCell", {
            index: index,
            field: $(e.target).attr("data-field"),
            value: $(e.target).val(),
            reinit: false
        });
    }

    /** 新增 */
    function rowAdd() {
        if ($table.bootstrapTable("getData").some(function (x) {
            if (x.OnEdit == true) {
                return true;
            }
        })) {
            alert("請先儲存或取消正在編輯的項目，再進行新增");
            return;
        }
        ui.usingBlockAsync(
            client
                .queryExecuteAsync("AddRow", {
                    EmpNo: $("#EmpNo").val(),
                    CertName: $("#CertName").val(),
                    CertNo: $("#CertNo").val(),
                    CertCompany: $("#CertCompany").val(),
                    CertDate: $("#CertDate").val()
                })
        )
            .then(function (response) {
                $("#CertName").val("");
                $("#CertNo").val("");
                $("#CertCompany").val("");
                $("#CertDate").datepicker("update", "");
                $table.bootstrapTable("refresh");
            });
    }
    /** 取消 (重填) */
    function rowClear() {
        $("#EmpNo").select2("trigger", "select", {
            data: { id: "", text: "" }
        });
        $("#CertName").val("");
        $("#CertNo").val("");
        $("#CertCompany").val("");
        $("#CertDate").datepicker("update", "");
    }

    /** 編輯 */
    function rowEdit(index) {
        $table.bootstrapTable("updateCell", {
            index: index,
            field: "OnEdit",
            value: true
        });
    }
    /** 儲存 */
    function rowUpdate(index) {
        if (!confirm("確認要更新嗎")) {
            return;
        }
        var row = $table.bootstrapTable("getData")[index];
        // 把 row 標記為儲存中
        $table.bootstrapTable("updateCell", {
            index: index,
            field: "OnSaving",
            value: true
        });
        // 進行更新
        client
            .queryExecuteAsync("UpdateRow", {
                Id: row.id,
                CertName: row.cert_name,
                CertNo: row.cert_no,
                CertCompany: row.cert_company,
                CertDate: row.cert_date
            })
            .then(function (response) {
                row.OnEdit = false;
            })
            .finally(function () {
                // 取消儲存中的標記
                row.OnSaving = false;
                $table.bootstrapTable("updateRow", {
                    index: index,
                    row: row
                });
            });
    }
    /** 取消 (還原) */
    function rowCancel(index) {
        var row = $table.bootstrapTable("getData")[index];
        // 把值還原
        ["cert_name", "cert_no", "cert_company", "cert_date"].forEach(function (field) {
            row[field] = row["origin_" + field];
        });
        row.OnEdit = false;
        $table.bootstrapTable("updateRow", {
            index: index,
            row: row
        });
    }
    /** 刪除 */
    function rowDelete(index) {
        if (!confirm("確認要刪除嗎")) {
            return;
        }
        var row = $table.bootstrapTable("getData")[index];
        ui.usingBlockAsync(
            client
                .queryExecuteAsync("DeleteRow", {
                    Id: row.id
                })
        )
            .then(function (response) {
                $table.bootstrapTable("remove", {
                    field: '$index',
                    values: [index]
                });
            });
    }
</script>
```