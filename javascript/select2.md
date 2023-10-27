# Select2

## Select2 的多種場景應用

### 開始使用 Select2

固定選項

```js
$select2.select2({
    data: [
        {
            id: "value1",
            text: "text1"
        }, {
            id: "value2",
            text: "text2"
        }, {
            id: "value3",
            text: "text3"
        }
    ]
});
```

給予一個寬度

```js
$select2.select2({
    width: 300,
    data: [
        {
            id: "value1",
            text: "text1"
        }, {
            id: "value2",
            text: "text2"
        }, {
            id: "value3",
            text: "text3"
        }
    ]
});
```

加入一個空選項

```js
$select2.select2({
    width: 300,
    data: [
        {
            id: "",
            text: ""
        }, {
            id: "value1",
            text: "text1"
        }, {
            id: "value2",
            text: "text2"
        }, {
            id: "value3",
            text: "text3"
        }
    ]
});
```

清除功能

```js
$select2.select2({
    width: 300,
    allowClear: true,
    placeholder: "", // allowClear 為 true 時，一定要設置 placeholder，不然清除功能會發生異常
    data: [
        {
            id: "",
            text: ""
        }, {
            id: "value1",
            text: "text1"
        }, {
            id: "value2",
            text: "text2"
        }, {
            id: "value3",
            text: "text3"
        }
    ]
});
```

移除搜尋功能

```js
$select2.select2({
    width: 300,
    allowClear: true,
    placeholder: "", // allowClear 為 true 時，一定要設置 placeholder，不然清除功能會發生異常
    minimumResultsForSearch: Infinity,
    data: [
        {
            id: "",
            text: ""
        }, {
            id: "value1",
            text: "text1"
        }, {
            id: "value2",
            text: "text2"
        }, {
            id: "value3",
            text: "text3"
        }
    ]
});
```

固定選項+無搜尋+單選 固定選項+無搜尋+多選 固定選項+搜尋+單選 固定選項+搜尋+多選

動態選項+無搜尋+單選 動態選項+無搜尋+多選 動態選項+搜尋+單選 動態選項+搜尋+多選

$select2.select2({ width: 200, allowClear: true, placeholder: "", minimumResultsForSearch: Infinity, //tags: true, data: \[ { id: "value1", text: "text1" }, { id: "value2", text: "text2" }, { id: "value3", text: "text3" } ] });

## 搭配 BootstrapTable 實作有先後順序的 Select2

``` html
<!-- 個人資料維護 -->
<div class="d-flex justify-content-end">
    <a href="#box-personal-education" class="ml-1 ml-md-2 ml-lg-3 ml-xl-4">學歷資料</a>
    <a href="#box-personal-thesis" class="ml-1 ml-md-2 ml-lg-3 ml-xl-4">專業論文</a>
    <a href="#box-personal-journal" class="ml-1 ml-md-2 ml-lg-3 ml-xl-4">專業期刊</a>
    <a href="#box-personal-cert" class="ml-1 ml-md-2 ml-lg-3 ml-xl-4">認證紀錄</a>
    <a href="#box-personal-language" class="ml-1 ml-md-2 ml-lg-3 ml-xl-4">語言能力</a>
    <a href="#box-personal-relative" class="ml-1 ml-md-2 ml-lg-3 ml-xl-4">親屬任職AUO資料</a>
</div>

<div class="card card-primary card-outline" id="div-personal-info">
    <div class="card-body">
        <div class="row">
            <div class="col-md-8 col-lg-6 col-xl-4 form-group">
                <label>中文姓名</label>
                <span class="ml-1 text-warning" title="需繳交證明文件"><i class="fas fa-id-card"></i></span>
                <div class="input-group">
                    <div class="input-group-prepend">
                        <label class="input-group-text">姓</label>
                    </div>
                    <input class="form-control" type="text" name="LastName" autocomplete="off" style="flex: initial; width: 25%" />
                    <div class="input-group-append input-group-prepend">
                        <label class="input-group-text">名</label>
                    </div>
                    <input class="form-control" type="text" name="FirstName" autocomplete="off" style="flex: initial; width: 40%" />
                </div>
            </div>
        </div>
        <div class="row">
            <div class="col-xl-8 form-group">
                <label>戶籍地址</label>
                <span class="ml-1 text-warning" title="需繳交證明文件"><i class="fas fa-id-card"></i></span>
                <div class="input-group">
                    <div class="input-group-prepend" style="width: 10rem;">
                        <select class="custom-select" name="PermanentZipCode"></select>
                    </div>
                    <div class="input-group-append">
                        <input class="form-control" type="text" name="PermanentAddress2" style="max-width: 9rem;" />
                        <input class="form-control" type="text" name="PermanentAddress3" style="max-width: 4.5rem;" />
                    </div>
                    <div class="input-group-append input-group-prepend">
                        <label class="input-group-text">鄰</label>
                    </div>
                    <input class="form-control" type="text" name="PermanentAddress4" />
                </div>
            </div>
            <div class="col-xl-4 form-group">
                <label>戶籍電話</label>
                <input class="form-control" type="text" name="PermanentPhone" />
                <span class="text-orange">範例：078123456</span>
            </div>
        </div>
        <div class="row">
            <div class="col-xl-8 form-group">
                <label>通訊地址</label>
                <div class="input-group">
                    <div class="input-group-prepend" style="width: 10rem;">
                        <select class="custom-select" name="PresentZipCode"></select>
                    </div>
                    <div class="input-group-append">
                        <input class="form-control" type="text" name="PresentAddress2" style="max-width: 9rem;" />
                        <input class="form-control" type="text" name="PresentAddress3" style="max-width: 4.5rem;" />
                    </div>
                    <div class="input-group-append input-group-prepend">
                        <label class="input-group-text">鄰</label>
                    </div>
                    <input class="form-control" type="text" name="PresentAddress4" />
                </div>
            </div>
            <div class="col-md-6 col-xl-4 form-group">
                <label>通訊電話</label>
                <input class="form-control" type="text" name="PresentPhone" />
                <span class="text-orange">範例：078123456或0912345678</span>
            </div>
            <div class="col-md-6 col-xl-4 order-xl-4 form-group">
                <label>手機條碼</label>
                <input class="form-control" type="text" name="Barcode" />
                <span class="text-orange">範例：/W+M+Z2Q</span>
            </div>
            <div class="col-xl-8 form-group">
                <label>婚姻狀況</label>
                <div>
                    <div class="form-check form-check-inline">
                        <input class="form-check-input" type="radio" name="PersonalMarried" id="PersonalMarried0" value="0">
                        <label class="form-check-label" for="PersonalMarried0">Single</label>
                    </div>
                    <div class="form-check form-check-inline">
                        <input class="form-check-input" type="radio" name="PersonalMarried" id="PersonalMarried1" value="1">
                        <label class="form-check-label" for="PersonalMarried1">Marr.</label>
                    </div>
                </div>
            </div>
        </div>
        <div class="row">
            <div class="col-md-8 col-lg-6 col-xl-4 form-group">
                <label>緊急連絡人姓名</label>
                <div class="input-group">
                    <div class="input-group-prepend">
                        <label class="input-group-text">姓</label>
                    </div>
                    <input class="form-control" type="text" name="UrgentLastName" autocomplete="off" style="flex: initial; width: 25%" />
                    <div class="input-group-append input-group-prepend">
                        <label class="input-group-text">名</label>
                    </div>
                    <input class="form-control" type="text" name="UrgentFirstName" autocomplete="off" style="flex: initial; width: 40%" />
                </div>
            </div>
        </div>
        <div class="row">
            <div class="col-xl-8 form-group">
                <label>緊急連絡人地址</label>
                <input class="form-control" type="text" name="UrgentAddress" />
            </div>
            <div class="col-xl-4 form-group">
                <label>緊急連絡人電話</label>
                <input class="form-control" type="text" name="UrgentPhone" />
                <span class="text-orange">範例：078123456或0912345678</span>
            </div>
        </div>
    </div>
</div>

<div class="alert alert-warning" role="alert">
    *注意事項:<br />
    1.以下經歷只提供新增功能，若需新增以下經歷，但該經歷無相關欄位之資訊，則填「無」註明即可。<br />
    2.基於誠信原則請據實填答，若發現有造假之處，則友達光電股份有限公司保有追溯及處分之權利。
</div>

<div class="d-flex justify-content-end">
    <a href="#">TOP</a>
</div>
<div class="card card-primary card-outline mb-1" id="box-personal-education">
    <div class="card-header">
        <div class="h3 mb-0">
            <span>學歷資料</span>
            <span class="ml-1 text-warning" title="需繳交證明文件"><i class="fas fa-id-card"></i></span>
        </div>
    </div>
    <div class="card-body">
        <div class="table-responsive">
            <table class="table-sm" id="table-personal-education-temp"></table>
        </div>
        <table class="table-sm mt-3" id="table-personal-education"></table>
    </div>
</div>

<div class="d-flex justify-content-end">
    <a href="#">TOP</a>
</div>
<div class="card card-primary card-outline mb-1" id="box-personal-thesis">
    <div class="card-header">
        <div class="h3 mb-0">
            <span>專業論文</span>
        </div>
    </div>
    <div class="card-body">
        <div class="d-flex justify-content-start">
            <div class="mr-3">
                <label>論文名稱</label>
                <input type="text" class="form-control form-control-sm" name="PaperName" />
            </div>
            <div class="mr-3">
                <label>發表日期</label>
                <input type="text" class="form-control form-control-sm fas" name="PublishDate" placeholder="&#xF073;" autocomplete="off" />
            </div>
            <div class="mr-3">
                <label>指導教授</label>
                <input type="text" class="form-control form-control-sm" name="Teacher" />
            </div>
            <div class="d-flex align-items-end">
                <div>
                    <button type="button" class="btn btn-sm btn-success" onclick="tabs.Personal.addThesis();">
                        <i class="fas fa-plus"></i>
                        <span>新增</span>
                    </button>
                    <span class="ml-3 text-danger" style="vertical-align: middle;" name="label-add-message"></span>
                </div>
            </div>
        </div>
        <div class="mt-3">
            <table class="table-sm" id="table-personal-thesis"></table>
        </div>
    </div>
</div>

<div class="d-flex justify-content-end">
    <a href="#">TOP</a>
</div>
<div class="card card-primary card-outline mb-1" id="box-personal-journal">
    <div class="card-header">
        <div class="h3 mb-0">
            <span>專業期刊</span>
        </div>
    </div>
    <div class="card-body">
        <div class="d-flex justify-content-start">
            <div class="mr-3">
                <label>期刊名稱</label>
                <input type="text" class="form-control form-control-sm" name="JournalName" />
            </div>
            <div class="mr-3">
                <label>論文名稱</label>
                <input type="text" class="form-control form-control-sm" name="PaperName" />
            </div>
            <div class="mr-3">
                <label>發表日期</label>
                <input type="text" class="form-control form-control-sm fas" name="PublishDate" placeholder="&#xF073;" autocomplete="off" />
            </div>
            <div class="mr-3">
                <label>指導教授</label>
                <input type="text" class="form-control form-control-sm" name="Teacher" />
            </div>
            <div class="d-flex align-items-end">
                <div>
                    <button type="button" class="btn btn-sm btn-success" onclick="tabs.Personal.addJournal();">
                        <i class="fas fa-plus"></i>
                        <span>新增</span>
                    </button>
                    <span class="ml-3 text-danger" style="vertical-align: middle;" name="label-add-message"></span>
                </div>
            </div>
        </div>
        <div class="mt-3">
            <table class="table-sm" id="table-personal-journal"></table>
        </div>
    </div>
</div>

<div class="d-flex justify-content-end">
    <a href="#">TOP</a>
</div>
<div class="card card-primary card-outline mb-1" id="box-personal-cert">
    <div class="card-header">
        <div class="h3 mb-0">
            <span>認證紀錄</span>
        </div>
    </div>
    <div class="card-body">
        <div class="d-flex justify-content-start">
            <div class="mr-3">
                <label>認證名稱</label>
                <input type="text" class="form-control form-control-sm" name="CertName" />
            </div>
            <div class="mr-3">
                <label>認證編號</label>
                <input type="text" class="form-control form-control-sm" name="CertNo" />
            </div>
            <div class="mr-3">
                <label>認證機構</label>
                <input type="text" class="form-control form-control-sm" name="CertCompany" />
            </div>
            <div class="mr-3">
                <label>認證日期</label>
                <input type="text" class="form-control form-control-sm fas" name="CertDate" placeholder="&#xF073;" autocomplete="off" />
            </div>
            <div class="d-flex align-items-end">
                <div>
                    <button type="button" class="btn btn-sm btn-success" onclick="tabs.Personal.addCert();">
                        <i class="fas fa-plus"></i>
                        <span>新增</span>
                    </button>
                    <span class="ml-3 text-danger" style="vertical-align: middle;" name="label-add-message"></span>
                </div>
            </div>
        </div>
        <div class="mt-3">
            <table class="table-sm" id="table-personal-cert"></table>
        </div>
    </div>
</div>

<div class="d-flex justify-content-end">
    <a href="#">TOP</a>
</div>
<div class="card card-primary card-outline mb-1" id="box-personal-language">
    <div class="card-header">
        <div class="h3 mb-0">
            <span>語言能力</span>
        </div>
    </div>
    <div class="card-body">
        <div class="d-flex justify-content-start">
            <div class="mr-3">
                <label>測驗類別</label>
                <input type="text" class="form-control form-control-sm" name="ExamName" />
            </div>
            <div class="mr-3">
                <label>等級</label>
                <input type="text" class="form-control form-control-sm" name="ExamLevel" />
            </div>
            <div class="mr-3">
                <label>成績</label>
                <input type="text" class="form-control form-control-sm" name="ExamScore" />
            </div>
            <div class="mr-3">
                <label>測驗日期</label>
                <input type="text" class="form-control form-control-sm fas" name="ExamDate" placeholder="&#xF073;" autocomplete="off" />
            </div>
            <div class="d-flex align-items-end">
                <div>
                    <button type="button" class="btn btn-sm btn-success" onclick="tabs.Personal.addLanguage();">
                        <i class="fas fa-plus"></i>
                        <span>新增</span>
                    </button>
                    <span class="ml-3 text-danger" style="vertical-align: middle;" name="label-add-message"></span>
                </div>
            </div>
        </div>
        <div class="mt-3">
            <table class="table-sm" id="table-personal-language"></table>
        </div>
    </div>
</div>

<div class="d-flex justify-content-end">
    <a href="#">TOP</a>
</div>
<div class="card card-primary card-outline" id="box-personal-relative">
    <div class="card-header">
        <div class="h3 mb-0">
            <span>親屬任職AUO資料</span>
        </div>
    </div>
    <div class="card-body">
        <div class="alert alert-info" role="alert">
            若您有二等親內親屬任職的公司，請務必填寫親屬資料。<br />
            一等親定義為父母、子女及配偶，<br />
            二等親定義為兄弟姐妹、祖父母、孫子女、連襟、妯娌及配偶的兄弟姐妹
        </div>
        <div class="d-flex justify-content-start">
            <div class="mr-3">
                <label>工號</label>
                <select class="custom-select" name="RelativeEmpNo"></select>
            </div>
            <div class="mr-3" style="min-width: 135px;">
                <label>關係</label>
                <select class="custom-select" name="Relationship"></select>
            </div>
            <div class="d-flex align-items-end">
                <div>
                    <button type="button" class="btn btn-success" onclick="tabs.Personal.addRelative();">
                        <i class="fas fa-plus"></i>
                        <span>新增</span>
                    </button>
                    <span class="ml-3 text-danger" style="vertical-align: middle;" name="label-add-message"></span>
                </div>
            </div>
        </div>
        <div class="mt-3">
            <table class="table-sm" id="table-personal-relative"></table>
        </div>
    </div>
</div>

<div class="mt-5"></div>
<div class="fixed-bottom bg-auo-gray2 py-2" id="div-personal-buttons">
    <div class="d-flex justify-content-center">
        <button type="button" class="btn btn-primary" onclick="tabs.Personal.saveAll();">
            <i class="fas fa-paper-plane"></i>
            <span>儲存</span>
        </button>
    </div>
</div>

<div class="modal fade" id="modal-personal-need-documents" tabindex="-1" role="dialog" aria-hidden="true" data-backdrop="static" data-keyboard="false">
    <div class="modal-dialog" role="document">
        <div class="modal-content">
            <div class="modal-header">
                <h4 class="modal-title">
                    <span class="text-danger">儲存成功</span>
                </h4>
                <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                    <span aria-hidden="true">&times;</span>
                </button>
            </div>
            <div class="modal-body">
                <div>
                    相關個人資料於審核後隔日會匯入完成，提醒您可進入系統確認資料是否正確。<br />
                    <br />
                    若您變更部分，包含以下欄位，請繳交相關證明文件至各廠區窗口：<br />
                    <br />
                    1.變更「姓名或戶籍地址」欄位，請繳交證明文件(身分證影本、戶口名簿影本或戶籍謄本)<br />
                    <br />
                    2.變更「個人學歷」欄位，請繳交畢業證書正本及影本，核對後正本即歸還<br />
                    <br />
                    相關證明文件請於<span class="text-danger">7日內</span>繳交，申請後14日未繳交者，將取消該筆申請。<br />
                    <br />
                    ※ 各廠區窗口<br />
                    <div id="service-window-container"></div>
                </div>
            </div>
            <div class="modal-footer">
                <button type="button" class="btn btn-secondary" data-dismiss="modal">
                    <span>關閉視窗</span>
                </button>
            </div>
        </div>
    </div>
</div>

<script type="text/javascript">
    tabs.Personal = (function () {
        /** 學歷資料 學校 學歷 選項 */
        var degreeOptions;
        /** 學歷資料 地點 選項 */
        var slandOptions;
        /** 學歷資料 畢/肆業 選項 */
        var slabsOptions;
        var $tableEducationTemp;
        var $tableEducation;
        var $tableThesis;
        var $tableJournal;
        var $tableCert;
        var $tableLanguage;
        var $tableRelative;
        $(function () {
            // #region 戶籍地址/通訊地址 郵遞區號
            $("#div-personal-info").find("[name='PermanentZipCode'], [name='PresentZipCode']").select2({
                theme: "bootstrap4",
                width: "100%",
                allowClear: true,
                maximumInputLength: 10,
                minimumInputLength: 0,
                placeholder: "",
                ajax: {
                    transport: function (params, success, failure) {
                        client.queryListAsync("GetZipCodeOptions", {
                            Keyword: params.data.term
                        }).then(function (response) {
                            success({
                                results: response.List.map(function (x) {
                                    return {
                                        id: x.Value,
                                        text: x.Text
                                    };
                                })
                            });
                        }).catch(function (reason) {
                            failure();
                        });
                    }
                }
            });
            $('#div-personal-info').on("select2:selecting", "[name='PermanentZipCode'], [name='PresentZipCode']", function (e) {
                var $select2 = $(this).data("select2");
                if (e.params.args.data.id[0] == "_") {
                    e.preventDefault();
                    var text = e.params.args.data.text;
                    var $search = $select2.dropdown.$search || $select2.selection.$search;
                    $search.val(text);
                    $select2.trigger("query", {
                        term: text
                    });
                }
            });
            // #endregion
            // #region 學歷資料 學校 輸入框
            $('#table-personal-education-temp').on("select2:selecting", "select[data-field='insti']", function (e) {
                var $select2 = $(this).data("select2");
                var rowIndex = $(this).closest("tr").data("index");
                var row = $tableEducationTemp.bootstrapTable("getData")[rowIndex];
                if (!row._isSchoolOptions) {
                    e.preventDefault();
                    var text = e.params.args.data.text;
                    var $search = $select2.dropdown.$search || $select2.selection.$search;
                    $search.val(text);
                    $select2.trigger("query", {
                        term: text
                    });
                }
            });
            // #endregion
            // #region 學歷資料 地點(國家) 輸入框
            $('#table-personal-education-temp').on("select2:selecting", "select[data-field='sland']", function (e) {
                var $select2 = $(this).data("select2");
                var text = e.params.args.data.text;
                if (text.length == 1) {
                    e.preventDefault();
                    var $search = $select2.dropdown.$search || $select2.selection.$search;
                    $search.val(text);
                    $select2.trigger("query", {
                        term: text
                    });
                }
            });
            // #endregion
            // 日期元件
            $("#box-personal-thesis [name='PublishDate'],\
               #box-personal-journal [name='PublishDate'],\
               #box-personal-cert [name='CertDate'],\
               #box-personal-language [name='ExamDate']").datepicker({
                format: "yyyy/mm/dd",
                autoclose: true,
                container: "body > .wrapper"
            });
            // 親屬任職 人員選擇元件
            $("#box-personal-relative [name='RelativeEmpNo']").select2({
                theme: "bootstrap4",
                width: "100%",
                allowClear: true,
                maximumInputLength: 20,
                minimumInputLength: 1,
                placeholder: "輸入工號或姓名搜尋",
                ajax: {
                    transport: function (params, success, failure) {
                        client.queryListAsync("GetEmpOptions", {
                            Keyword: params.data.term
                        }).then(function (response) {
                            success({
                                results: response.List.map(function (x) {
                                    return {
                                        id: x.Value,
                                        text: x.Text
                                    };
                                })
                            });
                        }).catch(function (reason) {
                            failure();
                        });
                    }
                }
            });

            tableEducationTempInit();
            tableEducationInit();
            tableThesisInit();
            tableJournalInit();
            tableCertInit();
            tableLanguageInit();
            tableRelativeInit();
        });
        function onPageLoad() {
            return [
                client.getAsync("Profile/GetPersonalData", {
                    Token: token
                }).then(function (response) {
                    if (response.EmpInfo) {
                        var $info = $("#div-personal-info");
                        $info.find("[name='LastName']").val(response.EmpInfo.lastname || response.SapData.lastname);
                        $info.find("[name='FirstName']").val(response.EmpInfo.firstname || response.SapData.firstname);
                        if (response.EmpInfo.peradd1 || response.SapData.peradd1) {
                            $info.find("[name='PermanentZipCode']").select2("trigger", "select", {
                                data: {
                                    id: response.EmpInfo.peradd1 || response.SapData.peradd1,
                                    text: client.queryFirst("GetResumeCode", {
                                        CodeType: "4",
                                        Code: response.EmpInfo.peradd1 || response.SapData.peradd1
                                    }).First.decode
                                }
                            });
                        }
                        $info.find("[name='PermanentAddress2']").val(response.EmpInfo.peradd2 || response.SapData.peradd2);
                        $info.find("[name='PermanentAddress3']").val(response.EmpInfo.peradd3 || response.SapData.peradd3);
                        $info.find("[name='PermanentAddress4']").val(response.EmpInfo.peradd4 || response.SapData.peradd4);
                        $info.find("[name='PermanentPhone']").val(response.EmpInfo.per_tel || response.SapData.PER_TEL);
                        if (response.EmpInfo.nowadd1 || response.SapData.NOWADD1) {
                            $info.find("[name='PresentZipCode']").select2("trigger", "select", {
                                data: {
                                    id: response.EmpInfo.nowadd1 || response.SapData.NOWADD1,
                                    text: client.queryFirst("GetResumeCode", {
                                        CodeType: "4",
                                        Code: response.EmpInfo.nowadd1 || response.SapData.NOWADD1
                                    }).First.decode
                                }
                            });
                        }
                        $info.find("[name='PresentAddress2']").val(response.EmpInfo.nowadd2 || response.SapData.NOWADD2);
                        $info.find("[name='PresentAddress3']").val(response.EmpInfo.nowadd3 || response.SapData.NOWADD3);
                        $info.find("[name='PresentAddress4']").val(response.EmpInfo.nowadd4 || response.SapData.NOWADD4);
                        $info.find("[name='PresentPhone']").val(response.EmpInfo.now_tel || response.SapData.now_tel);
                        $info.find("[name='Barcode']").val(response.EmpInfo.barcode || response.SapData.barcode);
                        $info.find("[name='PersonalMarried'][value='" + (response.EmpInfo.married_status || response.SapData.married_status) + "']").prop("checked", true);
                        $info.find("[name='UrgentLastName']").val(response.EmpInfo.emeg_lastnme || response.SapData.emeg_lastnme);
                        $info.find("[name='UrgentFirstName']").val(response.EmpInfo.emeg_firstnme || response.SapData.emeg_firstnme);
                        $info.find("[name='UrgentAddress']").val(response.EmpInfo.emeg_add || response.SapData.emeg_add);
                        $info.find("[name='UrgentPhone']").val(response.EmpInfo.emeg_tel || response.SapData.emeg_tel);
                    }
                    degreeOptions = response.DegreeOptions;
                    slandOptions = response.SlandOptions;
                    slabsOptions = response.SlabsOptions;
                    // 學歷資料 暫存資料 補到 4 筆
                    while (response.EducationTemp.length < 4) {
                        response.EducationTemp.push({
                            sland: "TW"
                        });
                    };
                    // 親屬任職 關係選擇元件
                    $("#box-personal-relative [name='Relationship']").select2({
                        theme: "bootstrap4",
                        width: "100%",
                        allowClear: true,
                        maximumInputLength: 20,
                        minimumInputLength: 0,
                        placeholder: "",
                        tags: true,
                        data: [{
                            id: "",
                            text: ""
                        }].concat(response.RelationshipOptions.map(function (x) {
                            return {
                                id: x.Value,
                                text: x.Text
                            };
                        }))
                    });
                    $tableEducationTemp.bootstrapTable("load", response.EducationTemp);
                    $tableEducation.bootstrapTable("load", response.Education);
                    $tableThesis.bootstrapTable("load", response.Thesis);
                    $tableJournal.bootstrapTable("load", response.Journal);
                    $tableCert.bootstrapTable("load", response.Cert);
                    $tableLanguage.bootstrapTable("load", response.Language);
                    $tableRelative.bootstrapTable("load", response.Relative);
                    $("#service-window-container").html(response.ServiceWindowHTML);
                })
            ];
        }
        function tableEducationTempInit() {
            $tableEducationTemp = $("#table-personal-education-temp");
            $tableEducationTemp.bootstrapTable({
                search: false,
                theadClasses: "bg-gradient-auo-darkblue",
                classes: "table table-striped table-hover",
                columns: [
                    {
                        escape: false,
                        field: "insti",
                        title: "學校",
                        align: "center",
                        formatter: instiFormatter,
                        events: {
                            "change select": inputOnChange
                        }
                    }, {
                        escape: false,
                        field: "sland",
                        title: "地點",
                        align: "center",
                        width: 200,
                        formatter: slandFormatter,
                        events: {
                            "change select": inputOnChange
                        }
                    }, {
                        escape: false,
                        field: "begda",
                        title: "自",
                        align: "center",
                        formatter: begdaFormatter,
                        events: {
                            "change :text": inputOnChange
                        }
                    }, {
                        escape: false,
                        field: "endda",
                        title: "止",
                        align: "center",
                        formatter: enddaFormatter,
                        events: {
                            "change :text": inputOnChange
                        }
                    }, {
                        escape: false,
                        field: "slabs",
                        title: "畢/肆業",
                        align: "center",
                        width: 100,
                        formatter: slabsFormatter,
                        events: {
                            "change select": inputOnChange
                        }
                    },
                ],
                onPostBody: function (data) {
                    // 學歷
                    $tableEducationTemp.find("select[data-field='degree']").each(function (i, e) {
                        var row = data[i];
                        $(e).select2({
                            theme: "bootstrap4",
                            width: "100%",
                            allowClear: true,
                            maximumInputLength: 10,
                            minimumInputLength: 0,
                            placeholder: "學歷",
                            data: [{
                                id: "",
                                text: ""
                            }].concat(degreeOptions.map(function (x) {
                                return {
                                    id: x.Value,
                                    text: x.Text
                                };
                            }))
                        });
                        // 把已存在的值帶出來
                        if (row.degree) {
                            degreeOptions.some(function (x) {
                                if (row.degree.trimEnd() == x.Value.trimEnd()) {
                                    $(e).select2("trigger", "select", {
                                        data: {
                                            id: x.Value,
                                            text: x.Text
                                        }
                                    });
                                    return true;
                                }
                            });
                        }
                    });
                    // 學校
                    $tableEducationTemp.find("select[data-field='insti']").each(function (i, e) {
                        var row = data[i];
                        $(e).select2({
                            theme: "bootstrap4",
                            width: "100%",
                            allowClear: true,
                            maximumInputLength: 10,
                            minimumInputLength: 0,
                            placeholder: "學校名稱",
                            ajax: {
                                delay: 255,
                                transport: function (params, success, failure) {
                                    if (row.degree) {
                                        // 先找關鍵字的結果
                                        var isSchoolOptions = false;
                                        client.queryListAsync("GetInstiGroupOptions", {
                                            High: row.degree,
                                            Keyword: params.data.term
                                        }).then(function (response) {
                                            if (params.data.term && response.List.length == 0) {
                                                // 如果輸入文字後沒有關鍵字的結果，則以關鍵字搜尋完整學校名稱
                                                isSchoolOptions = true;
                                                return client.queryListAsync("GetInstiOptions", {
                                                    High: row.degree,
                                                    Keyword: params.data.term
                                                });
                                            }
                                            else if (params.data.term && response.List.length == 1) {
                                                // 如果輸入文字後剛好只有一個結果，則以群組搜尋完整學校名稱
                                                isSchoolOptions = true;
                                                return client.queryListAsync("GetInstiOptions", {
                                                    High: row.degree,
                                                    Group: response.List[0].Value
                                                });
                                            }
                                            return Promise.resolve(response);
                                        }).then(function (response) {
                                            row._isSchoolOptions = isSchoolOptions;
                                            if (response.List.length > 0) {
                                                success({
                                                    results: response.List.map(function (x) {
                                                        return {
                                                            id: x.Value,
                                                            text: x.Text
                                                        };
                                                    })
                                                });
                                            }
                                            else if (isSchoolOptions) {
                                                // 模擬 tags (由輸入的文字生成選項)
                                                success({
                                                    results: [{
                                                        id: params.data.term,
                                                        text: params.data.term
                                                    }]
                                                });
                                            }
                                            else {
                                                // 正常不會有這種情況
                                                console.error("except insti");
                                            }
                                        });
                                    }
                                    else {
                                        // 還沒選學歷
                                        success({
                                            results: []
                                        });
                                    }
                                }
                            },
                            language: {
                                noResults: function () {
                                    if (!row.degree) {
                                        return "請先選擇學歷";
                                    }
                                    return "請自行輸入";
                                }
                            },
                        });
                        // 把已存在的值帶出來
                        if (row.insti) {
                            row._isSchoolOptions = true;
                            $(e).select2("trigger", "select", {
                                data: {
                                    id: row.insti,
                                    text: row.insti
                                }
                            });
                        }
                    });
                    // 科系
                    $tableEducationTemp.find("select[data-field='major']").each(function (i, e) {
                        var row = data[i];
                        $(e).select2({
                            theme: "bootstrap4",
                            width: "100%",
                            allowClear: true,
                            maximumInputLength: 10,
                            minimumInputLength: 0,
                            placeholder: "主修(科/系/所)",
                            ajax: {
                                delay: 255,
                                transport: function (params, success, failure) {
                                    if (row.degree && row.insti) {
                                        // 先找關鍵字的結果
                                        client.queryListAsync("GetMajorOptions", {
                                            High: row.degree,
                                            School: row.insti
                                        }).then(function (response) {
                                            if (response.List.length > 0) {
                                                success({
                                                    results: response.List.map(function (x) {
                                                        return {
                                                            id: x.Value,
                                                            text: x.Text
                                                        };
                                                    })
                                                });
                                            }
                                            else {
                                                // 模擬 tags (由輸入的文字生成選項)
                                                success({
                                                    results: [{
                                                        id: params.data.term,
                                                        text: params.data.term
                                                    }]
                                                });
                                            }
                                        });
                                    }
                                    else {
                                        // 還沒選學校
                                        success({
                                            results: []
                                        });
                                    }
                                }
                            },
                            language: {
                                noResults: function () {
                                    if (!row.degree) {
                                        return "請先選擇學歷";
                                    }
                                    if (!row.insti) {
                                        return "請先輸入學校名稱";
                                    }
                                    return "請自行輸入";
                                }
                            },
                        });
                        // 把已存在的值帶出來
                        if (row.major) {
                            $(e).select2("trigger", "select", {
                                data: {
                                    id: row.major,
                                    text: row.major
                                }
                            });
                        }
                    });
                    // 地點
                    $tableEducationTemp.find("select[data-field='sland']").each(function (i, e) {
                        var row = data[i];
                        $(e).select2({
                            theme: "bootstrap4",
                            width: "100%",
                            allowClear: true,
                            maximumInputLength: 10,
                            minimumInputLength: 0,
                            placeholder: "",
                            ajax: {
                                delay: 255,
                                transport: function (params, success, failure) {
                                    if (params.data.term) {
                                        success({
                                            results: slandOptions.filter(function (x) {
                                                return x.Text.substr(0, params.data.term.length).toLowerCase() == params.data.term.toLowerCase()
                                                    || x.Value.substr(0, params.data.term.length).toLowerCase() == params.data.term.toLowerCase();
                                            }).map(function (x) {
                                                return {
                                                    id: x.Value,
                                                    text: x.Text
                                                };
                                            })
                                        });
                                    }
                                    else {
                                        success({
                                            results: slandOptions
                                                .map(function (x) { return x.Text[0]; })
                                                .filter(function (value, index, self) {
                                                    return self.indexOf(value) === index;
                                                })
                                                .sort()
                                                .map(function (x) { return { id: x, text: x }; })
                                        });
                                    }
                                }
                            }
                        });
                        if (row.sland) {
                            slandOptions.some(function (x) {
                                if (row.sland == x.Value) {
                                    $(e).select2("trigger", "select", {
                                        data: {
                                            id: x.Value,
                                            text: x.Text
                                        }
                                    });
                                    return true;
                                }
                                return false;
                            });
                        }
                    });
                    // 日期元件
                    $tableEducationTemp.find("[data-field].datepicker").datepicker({
                        format: "yyyy/mm/dd",
                        autoclose: true,
                        container: "body > .wrapper"
                    });
                    // 日期元件 選擇值
                    $tableEducationTemp.find("[data-field].datepicker").each(function (i, e) {
                        var $input = $(e);
                        var row = data[Number($input.closest("tr").attr("data-index"))];
                        var field = $input.attr("data-field");
                        var value = row[field];
                        if (typeof value == "string" && value.length == 8) {
                            $input.datepicker("setDate", value.substr(0, 4) + "/" + value.substr(4, 2) + "/" + value.substr(6, 2));
                        }
                    });
                },
            });
        }
        function tableEducationInit() {
            $tableEducation = $("#table-personal-education");
            $tableEducation.bootstrapTable({
                search: false,
                theadClasses: "bg-gradient-auo-darkblue",
                classes: "table table-striped table-hover",
                columns: [
                    {
                        escape: true,
                        field: "school_name",
                        title: "學校名稱",
                        align: "center",
                    }, {
                        escape: true,
                        field: "course_name",
                        title: "科系",
                        align: "center",
                    }, {
                        escape: false,
                        field: "begin_date",
                        title: "開始日期",
                        align: "center",
                        formatter: sdateFormatter,
                    }, {
                        escape: false,
                        field: "end_date",
                        title: "結束日期",
                        align: "center",
                        formatter: sdateFormatter,
                    }, {
                        escape: false,
                        field: "graduate",
                        title: "畢業否",
                        align: "center",
                        formatter: graduateFormatter,
                    }, {
                        escape: true,
                        field: "school_place",
                        title: "學校地點",
                        align: "center",
                    },
                ],
                onPostBody: function (data) {
                    // 沒有資料時隱藏列表
                    $tableEducation.toggle(data.length > 0);
                },
            });
        }
        function tableThesisInit() {
            $tableThesis = $("#table-personal-thesis");
            $tableThesis.bootstrapTable({
                search: false,
                theadClasses: "bg-gradient-auo-darkblue",
                classes: "table table-striped table-hover",
                columns: [
                    {
                        escape: false,
                        width: 120,
                        title: "Delete",
                        align: "center",
                        formatter: deleteFormatterFactory($tableThesis),
                    }, {
                        escape: true,
                        field: "paper_name",
                        title: "論文名稱",
                    }, {
                        escape: true,
                        field: "publish_date",
                        title: "發表日期",
                        align: "center",
                        formatter: dateFormatter,
                    }, {
                        escape: true,
                        field: "teacher",
                        title: "指導教授",
                    },
                ],
                onPostBody: function (data) {
                    // 沒有資料時隱藏列表
                    $tableThesis.toggle(data.length > 0);
                },
            });
        }
        function tableJournalInit() {
            $tableJournal = $("#table-personal-journal");
            $tableJournal.bootstrapTable({
                search: false,
                theadClasses: "bg-gradient-auo-darkblue",
                classes: "table table-striped table-hover",
                columns: [
                    {
                        escape: false,
                        width: 120,
                        title: "Delete",
                        align: "center",
                        formatter: deleteFormatterFactory($tableJournal),
                    }, {
                        escape: true,
                        field: "journal_name",
                        title: "期刊名稱",
                    }, {
                        escape: true,
                        field: "paper_name",
                        title: "論文名稱",
                    }, {
                        escape: false,
                        field: "publish_date",
                        title: "發表日期",
                        align: "center",
                        formatter: dateFormatter,
                    }, {
                        escape: true,
                        field: "teacher",
                        title: "指導教授",
                    },
                ],
                onPostBody: function (data) {
                    // 沒有資料時隱藏列表
                    $tableJournal.toggle(data.length > 0);
                },
            });
        }
        function tableCertInit() {
            $tableCert = $("#table-personal-cert");
            $tableCert.bootstrapTable({
                search: false,
                theadClasses: "bg-gradient-auo-darkblue",
                classes: "table table-striped table-hover",
                columns: [
                    {
                        escape: false,
                        width: 120,
                        title: "Delete",
                        align: "center",
                        formatter: deleteFormatterFactory($tableCert),
                    }, {
                        escape: true,
                        field: "cert_name",
                        title: "認證名稱",
                        align: "center",
                    }, {
                        escape: true,
                        field: "cert_no",
                        title: "認證編號",
                        align: "center",
                    }, {
                        escape: true,
                        field: "cert_company",
                        title: "認證機構",
                        align: "center",
                    }, {
                        escape: false,
                        field: "cert_date",
                        title: "認證日期",
                        align: "center",
                        formatter: dateFormatter,
                    },
                ],
                onPostBody: function (data) {
                    // 沒有資料時隱藏列表
                    $tableCert.toggle(data.length > 0);
                },
            });
        }
        function tableLanguageInit() {
            $tableLanguage = $("#table-personal-language");
            $tableLanguage.bootstrapTable({
                search: false,
                theadClasses: "bg-gradient-auo-darkblue",
                classes: "table table-striped table-hover",
                columns: [
                    {
                        escape: false,
                        width: 120,
                        title: "Delete",
                        align: "center",
                        formatter: deleteFormatterFactory($tableLanguage),
                    }, {
                        escape: true,
                        field: "exam_name",
                        title: "測驗類別",
                        align: "center",
                    }, {
                        escape: true,
                        field: "exam_level",
                        title: "等級",
                        align: "center",
                    }, {
                        escape: true,
                        field: "exam_score",
                        title: "成績",
                        align: "center",
                    }, {
                        escape: false,
                        field: "exam_date",
                        title: "測驗日期",
                        align: "center",
                        formatter: dateFormatter,
                    },
                ],
                onPostBody: function (data) {
                    // 沒有資料時隱藏列表
                    $tableLanguage.toggle(data.length > 0);
                },
            });
        }
        function tableRelativeInit() {
            $tableRelative = $("#table-personal-relative");
            $tableRelative.bootstrapTable({
                search: false,
                theadClasses: "bg-gradient-auo-darkblue",
                classes: "table table-striped table-hover",
                columns: [
                    {
                        escape: false,
                        width: 120,
                        title: "Delete",
                        align: "center",
                        formatter: deleteFormatterFactory($tableRelative),
                    }, {
                        escape: true,
                        field: "emp_name",
                        title: "姓名",
                        align: "center",
                    }, {
                        escape: true,
                        field: "relative_emp_no",
                        title: "工號",
                        align: "center",
                    }, {
                        escape: true,
                        field: "relationship",
                        title: "關係",
                        align: "center",
                    },
                ],
                onPostBody: function (data) {
                    // 沒有資料時隱藏列表
                    $tableRelative.toggle(data.length > 0);
                },
            });
        }
        function addThesis() {
            var row = {
                paper_name: $("#box-personal-thesis [name='PaperName']").val(),
                publish_date: $("#box-personal-thesis [name='PublishDate']").val(),
                teacher: $("#box-personal-thesis [name='Teacher']").val()
            };
            var messages = [];
            if (!row.paper_name) {
                messages.push("論文名稱為必填欄位");
            }
            if (!row.publish_date) {
                messages.push("發表日期為必填欄位");
            }
            else if (new Date(row.publish_date) == "Invalid Date") {
                messages.push("發表日期格式錯誤");
            }
            else if (new Date(row.publish_date) > new Date()) {
                messages.push("發表日期不可填入未來日期");
            }
            if (!row.teacher) {
                messages.push("指導教授為必填欄位");
            }
            $("#box-personal-thesis [name='label-add-message']").html(messages.join("、"));
            if (messages.length == 0) {
                $tableThesis.bootstrapTable("insertRow", {
                    index: $tableThesis.bootstrapTable("getData").length,
                    row: row
                });
            }
        }
        function addJournal() {
            var row = {
                journal_name: $("#box-personal-journal [name='JournalName']").val(),
                paper_name: $("#box-personal-journal [name='PaperName']").val(),
                publish_date: $("#box-personal-journal [name='PublishDate']").val(),
                teacher: $("#box-personal-journal [name='Teacher']").val()
            };
            var messages = [];
            if (!row.journal_name) {
                messages.push("期刊名稱為必填欄位");
            }
            if (!row.paper_name) {
                messages.push("論文名稱為必填欄位");
            }
            if (!row.publish_date) {
                messages.push("發表日期為必填欄位");
            }
            else if (new Date(row.publish_date) == "Invalid Date") {
                messages.push("發表日期格式錯誤");
            }
            else if (new Date(row.publish_date) > new Date()) {
                messages.push("發表日期不可填入未來日期");
            }
            if (!row.teacher) {
                messages.push("指導教授為必填欄位");
            }
            $("#box-personal-journal [name='label-add-message']").html(messages.join("、"));
            if (messages.length == 0) {
                $tableJournal.bootstrapTable("insertRow", {
                    index: $tableJournal.bootstrapTable("getData").length,
                    row: row
                });
            }
        }
        function addCert() {
            var row = {
                cert_name: $("#box-personal-cert [name='CertName']").val(),
                cert_no: $("#box-personal-cert [name='CertNo']").val(),
                cert_company: $("#box-personal-cert [name='CertCompany']").val(),
                cert_date: $("#box-personal-cert [name='CertDate']").val()
            };
            var messages = [];
            if (!row.cert_name) {
                messages.push("認證名稱為必填欄位");
            }
            if (!row.cert_no) {
                messages.push("認證編號為必填欄位");
            }
            if (!row.cert_company) {
                messages.push("認證機構為必填欄位");
            }
            if (!row.cert_date) {
                messages.push("認證日期為必填欄位");
            }
            else if (new Date(row.cert_date) == "Invalid Date") {
                messages.push("認證日期格式錯誤");
            }
            else if (new Date(row.cert_date) > new Date()) {
                messages.push("認證日期不可填入未來日期");
            }
            $("#box-personal-cert [name='label-add-message']").html(messages.join("、"));
            if (messages.length == 0) {
                $tableCert.bootstrapTable("insertRow", {
                    index: $tableCert.bootstrapTable("getData").length,
                    row: row
                });
            }
        }
        function addLanguage() {
            var row = {
                exam_name: $("#box-personal-language [name='ExamName']").val(),
                exam_level: $("#box-personal-language [name='ExamLevel']").val(),
                exam_score: $("#box-personal-language [name='ExamScore']").val(),
                exam_date: $("#box-personal-language [name='ExamDate']").val()
            };
            var messages = [];
            if (!row.exam_name) {
                messages.push("測驗類別為必填欄位");
            }
            if (!row.exam_level) {
                messages.push("等級為必填欄位");
            }
            if (!row.exam_score) {
                messages.push("成績為必填欄位");
            }
            if (!row.exam_date) {
                messages.push("測驗日期為必填欄位");
            }
            else if (new Date(row.exam_date) == "Invalid Date") {
                messages.push("測驗日期格式錯誤");
            }
            else if (new Date(row.exam_date) > new Date()) {
                messages.push("測驗日期不可填入未來日期");
            }
            $("#box-personal-language [name='label-add-message']").html(messages.join("、"));
            if (messages.length == 0) {
                $tableLanguage.bootstrapTable("insertRow", {
                    index: $tableLanguage.bootstrapTable("getData").length,
                    row: row
                });
            }
        }
        function addRelative() {
            var row = {
                relative_emp_no: $("#box-personal-relative [name='RelativeEmpNo']").val(),
                relationship: $("#box-personal-relative [name='Relationship']").val(),
            };
            var messages = [];
            if (!row.relative_emp_no) {
                messages.push("工號為必填欄位");
            }
            else {
                var emp = client.queryFirst("GetEmpName", {
                    EmpNo: row.relative_emp_no
                }).First;
                if (emp) {
                    row.emp_name = emp.emp_name;
                }
                else {
                    messages.push("查無資料");
                }
            }
            if (!row.relationship) {
                messages.push("關係未選擇");
            }
            $("#box-personal-relative [name='label-add-message']").html(messages.join("、"));
            if (messages.length == 0) {
                $tableRelative.bootstrapTable("insertRow", {
                    index: $tableRelative.bootstrapTable("getData").length,
                    row: row
                });
            }
        }
        // #region 學歷資料 暫存
        function instiFormatter(value, row, index, field) {
            return '\
                <div class="input-group">\
                    <div class="input-group-prepend" style="width: 20%; min-width: 100px;">\
                        <select class="custom-select" data-field="degree"></select>\
                    </div>\
                    <div class="input-group-append" style="width: 40%; min-width: 200px;">\
                        <select class="custom-select" data-field="insti"></select>\
                    </div>\
                    <div class="input-group-append" style="width: 40%; min-width: 200px;">\
                        <select class="custom-select" data-field="major"></select>\
                    </div>\
                </div>';
        }
        function slandFormatter(value, row, index, field) {
            var $input = $('<select class="custom-select"></select>');
            $input.attr("data-field", field);
            return $input[0].outerHTML;
        }
        function begdaFormatter(value, row, index, field) {
            var $input = $('<input type="text" class="form-control datepicker fas px-2" style="min-width: 100px;" placeholder="&#xF073;" autocomplete="off" />');
            $input.attr("data-field", field);
            $input.attr("value", value);
            return $input[0].outerHTML;
        }
        function enddaFormatter(value, row, index, field) {
            var $input = $('<input type="text" class="form-control datepicker fas px-2" style="min-width: 100px;" placeholder="&#xF073;" autocomplete="off" />');
            $input.attr("data-field", field);
            $input.attr("value", value);
            return $input[0].outerHTML;
        }
        function slabsFormatter(value, row, index, field) {
            var $input = $('<select class="custom-select" style="min-width: 80px;"></select>');
            $input.attr("data-field", field);
            $input.html(slabsOptions.toOptionsHTML({
                prependEmpty: true,
                selectedValue: value
            }));
            return $input[0].outerHTML;
        }
        // #endregion
        /** 輸入框 事件 (change) */
        function inputOnChange(e, value, row, index) {
            this.$el.bootstrapTable("updateCell", {
                index: index,
                field: $(e.target).attr("data-field"),
                value: $(e.target).val(),
                reinit: false
            });
        }
        /** 日期 (沒有分隔符號加入分隔符號) */
        function sdateFormatter(value, row, index, field) {
            if (typeof value == "string" && value.length == 8) {
                return value.substr(0, 4) + "/" + value.substr(4, 2) + "/" + value.substr(6, 2);
            }
        }
        /** 日期 */
        function dateFormatter(value, row, index, field) {
            if (value) {
                var format = moment(new Date(value)).format("YYYY/MM/DD");
                if (format != "Invalid date") {
                    return format;
                }
            }
        }
        /** 畢業否 */
        function graduateFormatter(value, row, index, field) {
            if (value) {
                return value;
            }
        }
        /** 刪除按鈕 */
        function deleteFormatterFactory($el) {
            var id = $el.attr("id");
            return function (value, row, index, field) {
                var buttons = [];
                buttons.push('<button type="button" class="btn btn-danger btn-sm" title="Delete" onclick="tabs.Personal.rowDelete(' + index + ', \'' + id + '\');"><i class="fas fa-trash-alt"></i></button>');
                return buttons.join("");
            };
        }
        /** 刪除資料 */
        function rowDelete(index, id) {
            var $el = $("#" + id);
            $el.bootstrapTable("remove", {
                field: "$index",
                values: [index]
            });
        }
        /** 取得 Personal 頁籤內的所有資料 */
        function getTabData() {
            return {
                Token: token,
                LastName: $("#div-personal-info [name='LastName']").val(),
                FirstName: $("#div-personal-info [name='FirstName']").val(),
                PermanentZipCode: $("#div-personal-info [name='PermanentZipCode']").val(),
                PermanentAddress2: $("#div-personal-info [name='PermanentAddress2']").val(),
                PermanentAddress3: $("#div-personal-info [name='PermanentAddress3']").val(),
                PermanentAddress4: $("#div-personal-info [name='PermanentAddress4']").val(),
                PermanentPhone: $("#div-personal-info [name='PermanentPhone']").val(),
                PresentZipCode: $("#div-personal-info [name='PresentZipCode']").val(),
                PresentAddress2: $("#div-personal-info [name='PresentAddress2']").val(),
                PresentAddress3: $("#div-personal-info [name='PresentAddress3']").val(),
                PresentAddress4: $("#div-personal-info [name='PresentAddress4']").val(),
                PresentPhone: $("#div-personal-info [name='PresentPhone']").val(),
                Barcode: $("#div-personal-info [name='Barcode']").val(),
                PersonalMarried: $("#div-personal-info [name='PersonalMarried']:checked").val(),
                UrgentLastName: $("#div-personal-info [name='UrgentLastName']").val(),
                UrgentFirstName: $("#div-personal-info [name='UrgentFirstName']").val(),
                UrgentAddress: $("#div-personal-info [name='UrgentAddress']").val(),
                UrgentPhone: $("#div-personal-info [name='UrgentPhone']").val(),
                Education: $tableEducationTemp.bootstrapTable("getData"),
                Thesis: $tableThesis.bootstrapTable("getData"),
                Journal: $tableJournal.bootstrapTable("getData"),
                Cert: $tableCert.bootstrapTable("getData"),
                Language: $tableLanguage.bootstrapTable("getData"),
                Relative: $tableRelative.bootstrapTable("getData"),
            };
        }
        /** 儲存 */
        function saveAll() {
            // 資料檢查
            var eduRows = $tableEducationTemp.bootstrapTable("getData");
            if (eduRows.some(function (row) {
                // 有選擇學歷或學校名稱才檢查那一行
                if (row.degree || row.insti) {
                    return !row.degree || !row.insti || !row.major || !row.sland || !row.begda || !row.endda || !row.slabs;
                }
            })) {
                ui.toastAsync("必填項目未填寫完成");
                return;
            }
            // 呼叫 API
            ui.usingBlockAsync(
                client.postAsync("Profile/SavePersonalData", getTabData())
            ).then(function (response) {
                if (response.NeedDocuments) {
                    $("#modal-personal-need-documents").modal("show");
                }
                else {
                    ui.toastAsync("儲存成功", "success");
                }
            });
        }

        return {
            onPageLoad: onPageLoad,
            addThesis: addThesis,
            addJournal: addJournal,
            addCert: addCert,
            addLanguage: addLanguage,
            addRelative: addRelative,
            rowDelete: rowDelete,
            getTabData: getTabData,
            saveAll: saveAll
        }
    })();
</script>
```