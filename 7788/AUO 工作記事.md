## 工作資訊

### 完整電腦名稱
TW100105484.corpnet.auo.com

### 工號
O2103008  
02107008 (ADP)

### Email
Kai.Huang.Ares@auo.com

### 分機
506009
86-507074 (Jabber)

### 施工單號

| 年月    | 單號    | 備註     |
| ------- | ------- | -------- |
| 2023/06 | 1018098 |          |

### GIT
[AUO Git](http://auhqplmap02.corpnet.auo.com/Bonobo.Git.Server/Repository/Index)  
KaiHuangAres  
auo@0627

### ADP
kaihuangauo  
O2107008  
Adp+01141

### 承商安全管理系統
20873360  
20873360@ares

### IDS
[查看測試信件](https://mail-test.corpnet.auo.com/?_task=mail&_err=session)

## CAP 常用帳號

| 名稱   | 帳號          | 工號    | 備註                     |
| ------ | ------------- | ------- | ------------------------ |
| <b>ADTBA3</b> |
|        | vickeychen    | 0104450 |                          |
| 美玲   | MeilingChen   | 1704218 |                          |
| 凱心   | WinnieChung   | 1706032 |                          |
| 大立   | tlliu         | 1905007 |                          |
| 人傑   | alexrchsieh   | 2108053 |                          |
| 文君   | wenchunpeng   | 2111021 |                          |
| 意淇   | ginaycchen    | 2209046 |                          |
| <b>常用</b>
| Maggie | MaggieWu      | 0802081 |                          |
| 湘芸   | hsiangyunchen | 1611039 |                          |
| Ivy    | ivyyrchen     | 1709211 |                          |
| <b>不常用</b>
| Mike   | mikewang      | 0706038 |                          |
| 致彰   | 1710207       | 1710207 |                          |
|        | aralechen     | 1704001 |                          |
| 李曉菁 | hsiaochinlee  | 1710195 | eOffering 改善計畫表編輯 |


## 遠端

| 名稱         | 帳號        | 密碼                            | 備註 |
| ------------ | ----------- | ------------------------------- | ---- |
| AUOHQHRMTT01 | paulyanghrc | adtba3!0308076 / adtba3@0308076 |      |

## 資料庫

| 伺服器       | 資料庫           | 帳號            | 密碼                | 備註 |
| ------------ | ---------------- | --------------- | ------------------- | ---- |
| MAZ-ODEVDB01 |                  | uAccessControl  | hrtest              |      |
| MAZ-ODEVDB01 |                  | uAccessControl  | hrtest              |      |
| MAZ-ODEVDB01 | eOffering_Global | ueoffer_owner   | ueoffer_owner@tspwd |      |
| AUOHQHRMTT01 | OSIS_AUO         | uosis_auo       | osis$123            |      |
| AUOHQHRMTT01 | PYPortal         | PYPortal        | PYPortal@itbc1      |      |
| AUOHQHRMTT01 | eCareerJourney   | ueCareerJourney | ueCareerJourney     |      |
| AUOHQHRMTT01 | eOffering_Global | ueoffer         | eoffer123           |      |
| AUOHQHRMTT01 | HrTmplSE         | uHrTmplSE       | uHrTmplSE#123       |      |
| MAZ-ODEVDB01 | SmartOrg         | usmartorg       | usmartorg@tspwd     |      |

## 解決方案

### 移除 Admin
在 `C:\Users\kaihuangares\Documents\IISExpress\config\applicationhost.config` 新增站台
``` xml
<site name="BossQuery" id="123" serverAutoStart="true">
    <application path="/">
        <virtualDirectory path="/" physicalPath="%IIS_SITES_HOME%\WebSite1" />
    </application>
    <application path="/BossQuery">
        <virtualDirectory path="/" physicalPath="D:\KaiHuangAres\Workspace\BossQuery\BossQuery.WebMVC" />
    </application>
    <application path="/BossQuery_API">
        <virtualDirectory path="/" physicalPath="D:\KaiHuangAres\Workspace\BossQuery\BossQuery.ApiService" />
    </application>
    <bindings>
        <binding protocol="http" bindingInformation="*:80:TW100105484.corpnet.auo.com" />
    </bindings>
</site>
```
用 cmd 執行指令
```
"C:\Program Files\IIS Express\iisexpress.exe" /siteid:2
"C:\Program Files\IIS Express\iisexpress.exe" /siteid:3
"C:\Program Files\IIS Express\iisexpress.exe" /siteid:4
"C:\Program Files\IIS Express\iisexpress.exe" /siteid:5
```
移除 admin 後無法開啟專案
1. 以文字檔打開 .csproj
2. 把 UseIISExpress 設為 true
``` xml
<UseIISExpress>true</UseIISExpress>
```
3. 把 IISUrl 設定超過 1024 的 port
``` xml
<IISUrl>http://localhost:23456</IISUrl>
```
4. 把 .user 跟外層的 .vs 刪除後重新開啟專案

### git
設置 proxy  
開啟 `C:\Users\KaiHuangAres\.gitconfig` 加入以下區塊
```
[http]
    proxy = AUO%5CKaiHuangAres:Auo+0322@http://AUO%5CKaiHuangAres:Auo%2B0322@auohqwsg.corpnet.auo.com:8080
    sslVerify = false
```
關於 `sslVerify = false` 的風險可參考[這裡](https://blog.darkthread.net/blog/vs2017-git-ssl-issue/)

### npm
設置 proxy
```
npm config set proxy http://AUO%5CKaiHuangAres:Auo%2B0422Auo%2B0422@auohqwsg.corpnet.auo.com:8080
npm config set https-proxy http://AUO%5CKaiHuangAres:Auo%2B0422Auo%2B0422@auohqwsg.corpnet.auo.com:8080
npm config set strict-ssl false
```

### npm run
```
npm --prefix "D:\KaiHuangAres\Workspace\code-gui\CodeGui\ClientApp" run serve
```

### Nuget
設置 proxy
```
nuget.exe config -set http_proxy=http://AUO%5CKaiHuangAres:Auo%2B0322Auo%2B0322@auohqwsg.corpnet.auo.com:8080
nuget.exe config -set http_proxy.user=AUO\KaiHuangAres
nuget.exe config -set http_proxy.password=Auo+0322
```

### VS Code
設置 Proxy  
點擊左下角的 `齒輪 > Settings`  
再點擊右上角的 `Open Settings (JSON)`  
補上下面三個參數
```
{
    "http.proxy": "http://AUO%5CKaiHuangAres:Auo%2B0322Auo%2B0322@auohqwsg.corpnet.auo.com:8080",
    "https.proxy": "http://AUO%5CKaiHuangAres:Auo%2B0322Auo%2B0322@auohqwsg.corpnet.auo.com:8080",
    "http.proxyStrictSSL": false
}
```
或是在 `Settings` 的介面中找到 `Application > Proxy` 進行設置


### 產多國語資源檔
"C:\Program Files (x86)\Microsoft SDKs\Windows\v10.0A\bin\NETFX 4.8 Tools\ResGen.exe" rmc.txt
"D:\KaiHuangAres\Workspace\OSIS_TmplSE\OSIS.ApiService\bin\roslyn\csc.exe" rmc.resources

### dotnet run
NaTmpl
```
dotnet run --project "D:\KaiHuangAres\Workspace\auo-na-tmpl\NaTmpl.ApiService"
dotnet run --project "D:\KaiHuangAres\Workspace\auo-na-tmpl\NaTmpl.WebMVC"
```
PYPortal
```
dotnet run --project "D:\KaiHuangAres\Workspace\PYPortal\PYPortal.ApiService"
dotnet run --project "D:\KaiHuangAres\Workspace\PYPortal\PYPortal.WebMVC"
```
eOffering
```
dotnet run --project "D:\KaiHuangAres\Workspace\eCareerJourney_Cloud\E_CareerJourney.ApiService"
dotnet run --project "D:\KaiHuangAres\Workspace\eCareerJourney_Cloud\E_CareerJourney.WebMVC"
```
拉密
```
npm --prefix "D:\KaiHuangAres\Workspace\code-gui\CodeGui\ClientApp" run serve
dotnet run --project "D:\KaiHuangAres\Workspace\code-gui\CodeGui"
```
n6
```
npm --prefix "D:\KaiHuangAres\Workspace\n6vt-prototype\Web" run serve
dotnet run --project "D:\KaiHuangAres\Workspace\n6vt-prototype\API"
```

## 專案進度

### eOffering

基礎工程
 - [測試機](http://auohqhrmaptt01.corpnet.auo.com/eOffering/WebForm/Main.aspx)
 - [權限管理](http://auohqhrmaptt01.corpnet.auo.com/eOffering/SysWebForm/Auth/Main.aspx)
 - [人事異動](http://auohqhrmaptt01.corpnet.auo.com/eOffering/HRCWebForm/SysWebForm/HRCApprover/Main.aspx)
 - [新人指導關懷](http://auohqhrmaptt01.corpnet.auo.com/eOffering/HRDWebForm/SysAdmin/HRD_adm_site.aspx)
 - [頁面資訊](http://auohqhrmaptt01.corpnet.auo.com/eOffering/SysWebForm/MainInfo/Main.aspx)
 - [說明文件](http://auohqhrmaptt01.corpnet.auo.com/eOffering/SysWebForm/Help/Main.aspx)
 - [表單參數](http://auohqhrmaptt01.corpnet.auo.com/eOffering/SysWebForm/CodeMap/Main.aspx)
 - [收件人設定](http://auohqhrmaptt01.corpnet.auo.com/eOffering/SysWebForm/Mail/Main.aspx)
 - [給號規則](http://auohqhrmaptt01.corpnet.auo.com/eOffering/SysWebForm/CreateEmpNo/Rule.aspx)

考核
 - [一般培訓紀錄編輯](http://auohqhrmaptt01.corpnet.auo.com/eOffering/HRDWebForm/NTDs/HRDChecklistEdit.aspx)
 - [新人心得編輯](http://auohqhrmaptt01.corpnet.auo.com/eOffering/HRDWebForm/NTDs/HRDNewComerEdit.aspx)
 - [試用考核 / 說明文件](http://auohqhrmaptt01.corpnet.auo.com/eOffering/WebForm/Trial/Help.aspx)
 - [A-Team考核(試用/期滿) / 說明文件](http://auohqhrmaptt01.corpnet.auo.com/eOffering/WebForm/ATeamTrial/Help.aspx)
 - [A-Team考核項目](http://auohqhrmaptt01.corpnet.auo.com/eOffering/SysWebForm/ATeamTrial/TrialItem.aspx)

招募任用
 - [說明文件](http://auohqhrmaptt01.corpnet.auo.com/eOffering/WebForm/Recommend/Help.aspx)
 - [新增人才推薦單](http://auohqhrmaptt01.corpnet.auo.com/eOffering/WebForm/Recommend/Apply.aspx)
 - [查詢人才推薦單](http://auohqhrmaptt01.corpnet.auo.com/eOffering/WebForm/Recommend/Query.aspx)

新舊程式對應
| 舊系統                                        | 新系統                                                    | 備註                                                                     |
| --------------------------------------------- | --------------------------------------------------------- | ------------------------------------------------------------------------ |
| ~/HRDWebForm/NTDs/Checklist/CheckEdit.ascx    | ~\Areas\Hrd\Views\Ntds\ChecklistEdit.cshtml               |                                                                          |
| ~/HRDWebForm/NTDs/Improve/Performance.ascx    | ~\Areas\Hrd\Views\Ntds\ImproveEdit.cshtml                 |                                                                          |
| ~/HRDWebForm/NTDs/NewComer/ComerApprover.ascx | ~\Areas\Hrd\Views\Ntds\_ComerApprover.cshtml              |                                                                          |
| ~/HRDWebForm/NTDs/NewComer/ComerBossEdit.ascx |                                                           | 舊程式僅 ComerApprover.ascx 用到，所以新程式整合在 _ComerApprover.cshtml |
| ~/HRDWebForm/NTDs/NewComer/ComerEdit.ascx     | ~\Areas\Hrd\Views\Ntds\NewComerEdit.cshtml                |                                                                          |
| ~/HRDWebForm/NTDs/HRDViewComer.aspx           | ~\Areas\Hrd\Views\Shared\ViewComer.cshtml                 |                                                                          |
| ~/HRDWebForm/NTDs/HRDViewImprove.aspx         | ~\Areas\Hrd\Views\Shared\ViewImprove.cshtml               |                                                                          |
| ~/HRDWebForm/NTDs/WorkEdit/ProjectEdit.ascx   | ~\Areas\Hrd\Views\Ntds\_ProjectEdit.cshtml                |                                                                          |
| ~/HRDWebForm/NTDs/WorkEdit/EmpInfo.ascx       | ~\Areas\Hrd\Views\Ntds\_EmpInfo.cshtml                    |                                                                          |
| ~/HRDWebForm/NTDs/QAdmin.ascx                 | ~\Areas\Hrd\Views\Ntds\QueryAdmin.cshtml                  |                                                                          |
| ~/HRDWebForm/NTDs/Query_admin.aspx            | ~\Areas\Hrd\Views\Ntds\QueryAdmin.cshtml                  |                                                                          |
| ~/SysWebForm/Help/PublicList.ascx             | ~\Areas\Hrd\Views\Shared\HelpList.cshtml                  | My:help (設定在 Web.config)                                              |
| ~/HRDWebForm/NTDs/NewComer/TrialComer.ascx    | ~\Areas\Hrd\Views\Trial\_Comer.cshtml                     |                                                                          |
| ~/WebForm/Trial/Form.ascx                     | ~\Areas\Hrd\Views\Trial\_Form.cshtml                      |                                                                          |
| ~/SysWebForm/Mail/MailList.ascx               | ~\Areas\Hrd\Views\Trial\_MailList.cshtml                  |                                                                          |
| ~/CustomControl/uc/EmpPopup2.ascx             | ~/Views/Shared/VueComponents/EofferEmpPopup2.cshtml       |                                                                          |
| ~/CustomControl/uc/EmpPopup3.ascx             | ~/Views/Shared/VueComponents/EofferEmpPopup3.cshtml       |                                                                          |
| ~/CustomControl/uc2/DoubleListPopup.ascx      | ~/Views/Shared/VueComponents/EofferDoubleListPopup.cshtml |                                                                          |
| ~/CustomControl/uc2/SchoolPopup.ascx          | ~/Views/Shared/VueComponents/EofferSchoolPopup.cshtml     | My:SchoolPopup (設定在 Web.config)                                       |
| ~/CustomControl/UploadControlFLMV2.ascx       | ~/Views/Shared/VueComponents/EofferUploadFlmV2.cshtml     |                                                                          |
| ~/ApproveForm/Common/TransferPopup.ascx       | ~/Views/Shared/VueComponents/EofferTransferPopup.cshtml   |                                                                          |
| ~/WebForm/Recommend/common/EducationV3.ascx   | ~/Views/Shared/VueComponents/EofferEducationV3.cshtml     | 舊版對應對話框 ~\CustomControl\uc2\SchoolPopup.ascx                      |


找可以用 [改善計畫表編輯] 的人
``` sql
SELECT emp.*
FROM   trial t
       INNER JOIN emp_data emp ON emp.emp_no = t.emp_no
       INNER JOIN my_aspnet_users u ON u.emp_no = emp.emp_no
       INNER JOIN my_aspnet_membership mem ON mem.userId = u.id
WHERE  t.status = '5'
       AND emp.active = 'Y'
       AND emp.employee = 'Y'
       AND u.active = 'Y' 
```
找可以用 [新人工作專案指派] 的人
``` sql
SELECT *
FROM   emp_data
WHERE  emp_no IN (SELECT boss_no
                  FROM   emp_data
                  WHERE  active = 'Y'
                         AND job_idl = 'IDL'
                         AND employee = 'Y'
                         AND come_date >= '2013/8/12'
                         AND title NOT LIKE '%特別助理%'
                         AND title NOT LIKE '%顧問%'
                         AND title NOT LIKE '%特助%'
                         AND title NOT LIKE '%經理%'
                         AND emp_kind IN ( 'A14', 'A20', 'A30', 'A40',
                                           'A60', 'A70' )
                         AND employee = 'Y'
                         AND emp_type IN ( '一般員工', '外籍員工', '派遣員工' )
                         AND company = 'AUHQ') 
```
找可以用 [指導關懷紀錄查詢] 的人
``` sql
SELECT *
FROM   (SELECT rpline.report_line,
               CASE
                 WHEN nc.status = 'A' AND pi.status = 'A' THEN 'Complete'
                 WHEN nc.status = 'A' AND pi.status IS NULL AND ( t.status <> '5' OR t.status IS NULL ) THEN 'Complete'
                 ELSE 'Tracing'
               END process
        FROM   emp_data_all e
               LEFT JOIN hrd_form_newcomer_main nc ON e.emp_no = nc.emp_no AND nc.active = 'Y'
               LEFT JOIN hrd_form_improve_main pi ON e.emp_no = pi.emp_no AND pi.active = 'Y'
               LEFT JOIN trial t ON t.emp_no = e.emp_no
               LEFT JOIN boss_report_line rpline ON rpline.emp_no = e.emp_no
        WHERE  e.come_date >= '2013/8/12'
               AND e.active = 'Y'
               AND e.job_idl = 'IDL'
               AND e.title NOT LIKE '%特別助理%'
               AND e.title NOT LIKE '%顧問%'
               AND e.title NOT LIKE '%特助%'
               AND e.title NOT LIKE '%經理%'
               AND e.emp_kind IN ( 'A14', 'A20', 'A30', 'A40', 'A60', 'A70' )
               AND e.employee = 'Y'
               AND e.emp_type IN ( '一般員工', '外籍員工', '派遣員工' )
               AND e.company = 'AUHQ') table1
WHERE  process = 'Tracing' 
-- 把結果用記事本取代 | 成 ',' 再用 
-- SELECT *
-- FROM   emp_data_all
-- WHERE  emp_no IN ( '' ) 
-- 取得可登入的人
```


包 crad component

本機按看看 維護/查詢 MappingTable 的 Excel 看會不會很慢


--- Vue i18n
$i18n.availableLocales

ADTBB1 Juliana Kao 高睿璟
select * from emp_data_all where org_id = 'ADTBB1'
select * from emp_data_all where emp_no = '0211121'
select * from emp_data_all where boss_no = '0211121'
select * from emp_data_all where boss_no in ('0005403','0909182','1907006','0706038','1004305','O000670','O000671')






vue Redirect
```
// named route
router.push({ name: 'user', params: { userId: '123' } })

// with query, resulting in /register?plan=private
router.push({ path: 'register', query: { plan: 'private' } })

// with hash, resulting in /about#team
router.push({ path: '/about', hash: '#team' })
```

vue route set params
```
props: {
    default: { iid: "6478" }
},
components: {
    default: HomeView,
    layout: () => import('@/layouts/LeftMenuLayout.vue')
}


get params
$attrs.iid
```
        
虛擬應用程式的路徑會不見
/Home/Index


7/17 發信說 boss_report_line (檢視) 不見了
7/18 發信說時間不夠



排除列表

刪除列表


目錄/檔案

開頭 忽略大小寫
結尾
包含
Regex




勾選
重新命名路徑


project-rename
專案重新命名工具

明天重開機裝 url rewrite

punch
    I Miss U
    I will always love you
    
    
$.post("http://localhost:8089/api/User/Login", { Email: "aa@aa.com", Password: "123456" })


"eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJuYmYiOjE2OTAyNDU1NTMsImV4cCI6MTY5MDI0OTE1MywiaWF0IjoxNjkwMjQ1NTUzLCJpc3MiOiJXZWJQcm90b3R5cGUifQ.M9L1Az2GAU9ISZRJoJpUb8ZsldgL4u8ZzC08ceWLZaQ"

$.ajax({
    type: "GET",
    url: "http://localhost:8089/api/User/Name",
    beforeSend: function(request) {
        request.setRequestHeader("Authorization", "Bearer " + "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJuYW1laWQiO…BlIn0.aB7b1ghmIrTlLoLkS7yB9ag6VoAPiUJwc4MP14yjm-U");
    },
    success: function(response) {
        console.log(response);
    }
});



server=AUOHQHRMTT01;database=HrTmplSE;uid=uHrTmplSE;pwd=uHrTmplSE#123

No plugin found for prefix 'archetype' in the current project and in the plugin groups [org.apache.maven.plugins, org.codehaus.mojo] available from the repositories [local (C:\Users\KaiHuangAres\.m2\repository), central (https://repo.maven.apache.org/maven2)]


http://localhost:8089/api/Common/GenerateRandomBase64/8


#30e8e800,#3054cf2b,#30009fc7,#307a2ac9,#30db00db,#30cf0000,#30db9200


#304d4d00,#301f4d10,#30003e4d,#302e104d,#304d004d,#304d0000,#304d3300

#30333300,#3015330b,#30002933,#301f0b33,#30330033,#30330000,#30332200


8月21日發信 說 試用考核.直屬主管考核. 需要做 5 天

~/CustomControl/UploadControlFLMV2.ascx
~/CustomControl/uc/OrgPopup3.ascx

select * from emp_data_all where boss_level = 6 and hr_site_id in ('AETJ', 'AUCN')


## v-model pattern for js plugin as vue component

- when value changed: value to input and input to localValue
- when input changed: input to localValue and localValue to input

``` js
{
    template: '<input ref="input" type="text" />',
    props: {
        value: {
        }
    },
    data: function (_self) {
        return {
            localValue: null
        };
    },
    watch: {
        value: function (newValue, oldValue) {
            if (this.localValue != newValue) {
                this.localValue = newValue;
            }
        },
        localValue: function (newValue, oldValue) {
            if (this.value != newValue) {
                this.$emit("input", newValue);
            }
            if (this.$refs.input) {
                $(this.$refs.input).val(newValue);
            }
        }
    },
    mounted: function () {
        var _self = this;
        $(this.$refs.input)
            .on("input", function (e) {
                if (_self.localValue != e.target.value) {
                    _self.localValue = e.target.value;
                }
            });
        this.localValue = this.value;
    }
}
```

datepicker
``` js
{
    template: '<input ref="input" type="text" autocomplete="off" />',
    props: {
        value: {
            type: [Date, Number, String]
        },
        format: {
            type: String,
            default: "yyyy/mm/dd"
        },
        options: {
            type: Object,
            default: function () {
                return {
                    autoclose: true,
                    enableOnReadonly: false
                };
            }
        }
    },
    data: function (_self) {
        return {
            localValue: _self.value
        };
    },
    computed: {
        dateValue: function () {
            if (this.value) {
                if (this.value instanceof Date) {
                    return this.value;
                }
                if (typeof this.value == "string") {
                    return new Date(this.value);
                }
                if (typeof this.value.toString == "function") {
                    return new Date(this.value.toString());
                }
            }
        }
    },
    watch: {
        dateValue: function (newValue, oldValue) {
            if (this.localValue != newValue) {
                $(this.$refs.input).datepicker("setDate", newValue);
            }
        },
        localValue: function (newValue, oldValue) {
            this.$emit("input", newValue);
        },
        format: function (newValue, oldValue) {
            this.destroy();
            this.setup();
        },
        options: {
            handler: function (newValue, oldValue) {
                this.destroy();
                this.setup();
            },
            deep: true
        }
    },
    methods: {
        setup: function () {
            var _self = this;
            var $input = $(this.$refs.input);
            $input
                .datepicker($.extend({
                    format: this.format
                }, this.options))
                .on("input changeDate clearDate", function (e) {
                    _self.localValue = e.target.value;
                });
            $input.datepicker("setDate", this.dateValue);
        },
        destroy: function () {
            $(this.$refs.input)
                .off("input changeDate clearDate")
                .datepicker("destroy");
        },
    },
    mounted: function () {
        this.setup();
    },
    beforeDestroy: function () {
        this.destroy();
    }
}
```


一般 SQL 擲回錯誤

RAISERROR("錯誤訊息", 16, 10);


1.2.1    PCMT
1.2.4    PCMT
1.3.1    PCMT
1.4.1    PCMT
1.4.3    CDAMS
1.4.7    PCMT
1.5.0    OSIS
1.6.1    BossQuery
1.6.3    BossQuery
1.7.3    OSIS_TmplSE
1.7.5    CDAMS
1.7.6.1  HrTmplStudio


\w+
{\n    key: "$0",\n    label: "$0"\n},


select * from PYPortal_TableLayout
where layout_name = 'IDLSalHRAddUserList'
and layout_region = 'body'
order by col_index

select * from PYPortal_Command where command_name = 'GetIDLSalHRAddUserList'


delete PYPortal_Command where command_name in ('GetIDLSalHRAddProjectList', 'GetIDLSalHRAddStageList', 'GetIDLSalHRAddUserList')
delete PYPortal_TableLayout where layout_name in ('IDLSalHRAddProjectList', 'IDLSalHRAddStageList', 'IDLSalHRAddUserList')


http://auohqhrmaptt01.corpnet.auo.com/PYPortal/
