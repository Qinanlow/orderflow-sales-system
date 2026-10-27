# Wura Sales Order & Invoicing System — Prototype

這是依照 Wura Electrical Appliances Retail Store 需求報告製作的純 HTML、CSS、JavaScript Web 原型，使用 Firebase Hosting、Cloud Firestore 與 Anonymous Authentication。GitHub Actions 會在 `main` 分支的 `site.zip` 更新時自動解壓並部署。

## 系統功能

- 英文／馬來文介面切換
- 即時查看貨品售價、最低／最高允許售價與庫存
- 銷售人員可在允許範圍內設定成交價；超出範圍時即時警告
- 庫存不足提示及開單時二次校驗
- 自動計算總額、建立銷售單、扣減庫存
- 顧客與辦公室兩份單據、雲端存檔、搜尋與列印
- 辦公與外勤部門庫存出入紀錄及即時更新
- 銷售、發票、售出數量、庫存與熱門貨品管理報表
- 銷售、辦公與外勤、經理三種原型權限檢視
- 桌面與手機響應式界面
- Firebase 未設定時自動進入本機示範模式

## 第一次設定（只做一次）

### 1. 建立 Firebase 免費專案

1. 到 <https://console.firebase.google.com/> 建立專案，先使用 Spark 免費方案。
2. 在「Build → Authentication → Sign-in method」啟用 **Anonymous**。
3. 在「Build → Firestore Database」建立資料庫；地區一旦選定通常不能更改，請選靠近你的使用者的區域。
4. 到「Project settings → General → Your apps」，新增一個 Web App，複製 `firebaseConfig` 大括號內的完整 JSON。
5. 到「Project settings → Service accounts」，產生新的 private key，下載 JSON。此檔案不要上傳到 GitHub。

### 2. 建立 GitHub Repository

先解壓最外層交付 ZIP，將裡面的所有內容（包括 `.github`、`site.zip`、`firebase.json`）上傳到 GitHub repository 的 `main` 分支。不要只把最外層 ZIP 原封不動上傳。

### 3. 加入 GitHub 設定

在 repository 的 **Settings → Secrets and variables → Actions** 加入：

| 類型 | 名稱 | 內容 |
|---|---|---|
| Repository secret | `FIREBASE_WEB_CONFIG` | Web App 的 `firebaseConfig` JSON，例如 `{\"apiKey\":\"...\", ...}` |
| Repository secret | `FIREBASE_SERVICE_ACCOUNT` | Service account private key JSON 的完整內容 |
| Repository variable | `FIREBASE_PROJECT_ID` | Firebase Project ID，例如 `orderflow-12345` |

設定完成後，在 GitHub 的 **Actions** 页面运行 `Deploy site.zip to Firebase`，或重新上传一次 `site.zip`。完成后网址通常是：

`https://你的-project-id.web.app`

## 以後更新網站

1. 取得新版 `site.zip`。
2. 在 GitHub repository 删除旧的 `site.zip` 并上传新版，或直接用同名档案覆盖后 commit 到 `main`。
3. GitHub Actions 会自动解压、注入 Firebase 设置并发布，不需要再登入 Firebase 手动部署。

## 重要安全事項

- 绝对不要把 service account JSON 放进 repository 或 `site.zip`；它只能存放在 GitHub Secret。
- Firebase Web config 会出现在浏览器前端，这是 Firebase 的正常设计；资料保护依赖 Authentication 与 Firestore Security Rules。
- 目前的部門選擇器是原型權限示範，Anonymous Authentication 適合教程、原型或受控團隊使用。正式上線時應改為 Google／Email 登入，並以 Firebase Claims 或帳號資料落實伺服器端角色權限。
- Spark 是免费方案，但仍受用量配额限制；请在 Firebase Console 查看使用量。

## 修改原始碼

将 `site.zip` 解压后可直接修改：

- `index.html`：页面结构
- `styles.css`：视觉设计
- `app.js`：开单、库存、Firestore 与列印逻辑
- `firebase-config.js`：本机占位设置；线上部署时会被 GitHub Secret 自动覆盖

修改后重新压缩这四个档案为 `site.zip`，再上传 GitHub 即可自动更新。
