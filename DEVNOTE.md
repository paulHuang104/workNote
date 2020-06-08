# 開發注意事項
###### 補充
* [建制說明文件](https://github.com/104corp/vipphp-mirror/wiki/%E5%BB%BA%E7%BD%AE-VIP2-%E6%9C%AC%E6%A9%9F%E9%96%8B%E7%99%BC%E7%92%B0%E5%A2%83)
---
### Deploy 規則
1. 專案 04vip-f2e-bvue 在 git 環境 Lab, staging, prod 有各自的版號，在要 push 時一定要加對應的版號 tag 在否則 Deploy 會出問題。

2. Deploy完後要再到專案 vipphp 裡下列路徑修改 SPA_VERSION 的環境變數。
```
vip/yii/project/protected/config 
```
---
### 上線流程注意事項

###### Deploy
* 順序: 開發用的分支-> lab -> staging -> prod
* 在Deploy後一定要變更版號 tab (在 Sourcetree 上)，否則 CICD 會對應不到
* Prod 環境的 Tab 不會有後綴，ex: v25.1.0 (很重要)
* Staging 環境的 Tab 會有後綴， ex: v.25.1.0-rc (很重要)
* Lab 環境的 Tab 會有後綴， ex: v.25.1.0-beta
* Deploy 到 prod 時要確認後端的版號是否一至

###### 版號
* 規則: 大版號.中版號.小版號
* 大版號: 開發專案的版本
* 中版號: 專案單元開發更新的版本，因會對應到 API ，需與後端同步和溝通
* 小版號: 專案單元開發更新的版本，僅前端修改無需對應 API ，仍須與後端同步和溝通

###### 開發
* 更改 vipphp 專案的渲染路徑: 到下列路徑更改渲染路徑。
 ```
 vip/yii/ project/protectedcontrollers/CustController.php
 ```
 
* 修改  vipphp 專案下config版號: 到下列路徑底下對 env_LAB.php, env_STAGING.php 及 env_PROD.php 加上對應環境的版號。
 ```
 vip/yii/ project/protected/config
 ```

###### 流程
* 從 local 推到 lab，除了自己測試外也會請企劃測試
* 從 lab 推到 staging 會找 QA 進來測試
* 當 staging 測試沒問題就可以推到 Prod

###### 測試注意事項
* 瀏覽器要測到 IE11，使用VirtualBox + Win10
[Win10快照](https://developer.microsoft.com/en-us/microsoft-edge/tools/vms/)
* 快照登入密碼: <font color="#dd0000">Passw0rd!</font>

###### 其他注意事項
* 在正式站測試時要注意“不要”去用到客戶的資料，要有”104”開頭的公司名才可以。

###### CodeStyle:
* 樣式命名規則是使用 BEM ，參考資料: https://ithelp.ithome.com.tw/articles/10213186

###### Request API 流程：
* View dispatch vuex actions -> vuex actions call  customize api Mather -> has response call vuex mutations 
---
## 測試網址

* Lab: https://pro.104-dev.com.tw/vip/
* Staging: https://pro.104-staging.com.tw/vip/
* Prod: https://pro.104.com.tw/vip/
