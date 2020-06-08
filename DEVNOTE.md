# 開發注意事項
###### 補充
* [建制說明文件](https://github.com/104corp/vipphp-mirror/wiki/%E5%BB%BA%E7%BD%AE-VIP2-%E6%9C%AC%E6%A9%9F%E9%96%8B%E7%99%BC%E7%92%B0%E5%A2%83)
---
### VIP Web beta

網址: https://localhost/vip/preLogin/login 

測試帳號(一)
帳號: hank.chang@104.com.tw 
密碼: 123qwe

測試帳號(二)
帳號: jodie.hu@104.com.tw 
密碼: 123qwe

---
##### 測試網址
* Lab: https://pro.104-dev.com.tw/vip/
* Staging: https://pro.104-staging.com.tw/vip/
* Prod: https://pro.104.com.tw/vip/
---
### Deploy 規則
1. 專案 04vip-f2e-bvue 在 git 環境 Lab, staging, prod 有各自的版號，在要 push 時一定要加對應的版號 tag 在否則 Deploy 會出問題。

2. Deploy完後要再到專案 vipphp 裡下列路徑修改 SPA_VERSION 的環境變數。
`vip/yii/project/protected/config`
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
 `vip/yii/project/protectedcontrollers/CustController.php`
 
* 修改  vipphp 專案下config版號: 到下列路徑底下對 env_LAB.php, env_STAGING.php 及 env_PROD.php 加上對應環境的版號。
 `vip/yii/ project/protected/config`

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
### 專案 vipphp 使用注意事項
> GitHub: https://github.com/104corp/vipphp-mirror

#### 一 環境建置: 
1. 使用 homebrew 安裝下列套件：
`docker-compose`
`composer`
`docker`
`mkcert (https 驗證需求)`
`nss (Firefox 使用需求)`
2. clone 下列兩個專案(要放入同個根目錄)
- main: https://github.com/104corp/104vip-wsp-jb-b_main
- vipphp: https://github.com/104corp/vipphp-mirror 
3. 建立 HTTPS 憑證:
- 產生並安裝 local CA:
```node
$ mkcert -install \ -key-file dockerEnvFiles/certs/localhost-key.pem \ -cert-file dockerEnvFiles/certs/localhost.pem \
localhost
```
- 掛載到 viphp service，在專案 vipphp 下的 docker-compose.yml，下列路徑貼上下列兩行：`services -> vipphp-dev -> volumes`
```javascript
// Local CA
./dockerEnvFiles/certs/localhost.pem:/etc/ssl/certs/ssl-cert-snakeoil.pem
./dockerEnvFiles/certs/localhost-key.pem:/etc/ssl/private/ssl-cert-snakeoil.key
```
#### 二 運行: 
- 在 vipphp 專案底下執行 docker-compose up(若是第一次 clone 要先執行 npm install 及 npm run build-next-dev)
- 要注意網址，一定要用 https ，否則在 local 會無法正常登入使用(prod 環境會強制轉換成 https)


#### 三 lab 開發串接 API
在 lab 開發串接 API 需要開通 toggle 否則會被擋住
路徑: `vip/yii/project/protected/config/main_LOCAL.php` 底下的 toggle

---
### 專案 104vip-f2e-bvue 使用注意事項
- Github: https://github.com/104corp/104vip-f2e-bvue/tree/VIPOP-5208 
- 104vip-f2e-common Github: https://github.com/104corp/104vip-f2e-common 

#### 一 環境建置： 
1. Clone 專案到 local (目前 branch lab 或 VIPOP-5208 皆可)
2. 執行 yarn 
3. 導入 global component “104vip-f2e-common”
`在 104vip-f2e-bvue 專案底下執行 “git submodule update --init --recursive”，一定要引入否則會報錯(需要先設定submodule)`。
4. 建立 HTTPS 憑證
- 執行 `make generate-certs` (若沒有則無法 Hot reload)
- 暫時修改 vipphp-mirror 專案下 `vip/yii/project/protected/config/env_LOCAL.php`
打開 define("SPA_DEV_URI", "https://localhost:8888”)，之後要記得改回去。

#### 二 執行
1. yarn serve