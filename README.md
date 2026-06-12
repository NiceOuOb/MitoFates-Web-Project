# MitoFates Web Service 

這是一個將粒線體預測生物資訊工具 **MitoFates** 現代化的 Web 專案。本專案透過 **Node.js** 重新封裝 Perl 核心計算引擎，並提供直觀的任務管理與數據視覺化介面。

專案連結： https://mitofates-web.xyz

---

## 🛠️ 環境需求 (Prerequisites)

本專案運行於 Linux 環境（建議使用 Ubuntu 20.04/22.04+），啟動前請執行以下指令安裝各項核心組件：

## 🛠️ 環境需求 (Prerequisites)
專案運行於 Linux 環境（建議使用 Ubuntu 20.04/22.04+），本機僅需預先安裝：
1. **Node.js**: v14.x 或更高版本（含 `npm`）。
2. **Perl**: 系統內建 Perl 5 即可。

```bash
# 安裝 Node.js 與 npm (以 Ubuntu 為例)
sudo apt update
sudo apt install -y nodejs npm
```

```bash
# 安裝 Perl 及其必要處理工具
sudo apt install -y perl gawk build-essential
```

## 🚀 快速啟動與安裝步驟

請複製專案後，直接進入 `mitofates-web` 目錄執行自動化指令：

```bash
# 1. 取得專案並進入網頁服務目錄
git clone [本專案連結]
cd MitoFates-Web-Project/mitofates-web
```

### ⚙️ NPM 自動化部署流程（依序執行）

```bash
# 2. 系統初始化：安裝 Linux 工具、Perl 核心數學/C 橋接套件，並自動修復 svm-predict 軟連結
npm run init

# 3. 核心下載：自動將官方 MitoFates 演算法引擎與模型 Git Clone 至指定的外層相對路徑
npm run install

# 4. 生產環境配置：讀取 lock 檔精準安裝 Node.js 套件，並自動忽略開發期專用套件
npm run setup

# 5. 啟動 Web 服務伺服器
npm start
```

服務啟動後，本機請於瀏覽器輸入網址開始使用：
👉 [http://localhost:3000](http://localhost:3000)
> 💡若需要進行前端 UI 或後端程式碼即時修改，可執行 `npm run dev-start`。系統將啟用 `nodemon` 監聽機制，在您儲存檔案時自動重啟伺服器。


## 📖 使用指南 (User Guide)

### 1. 提交預測任務
- **貼上或上傳序列 (FASTA)**：手動在文字方塊輸入標準蛋白質 FASTA 序列。
- **範例與清除**：點擊 **「填入範例序列 (Example)」** 可載入測試資料；點擊 **「清除內容」** 可一鍵重設表單。
- **模型選擇**：根據蛋白來源選擇 `Fungi` (真菌)、`Metazoa` (動物) 或 `Plant` (植物)。
- **快速導覽**：點擊右上角的 **「分析紀錄」** 按鈕可隨時切換至歷史紀錄頁面。

### 2. 解析與操作分析結果
- **自動跳轉與時間補完**：任務建立成功後，網頁自動跳轉至 `/result/[Task_ID]`。
- **數據視覺化指標**：
  - `Probability` (機率) 欄位成彩色**進度條**，綠色代表高機率（>0.385），灰色代表一般機率。
  - `Cleavage Site` (切割位點) 與其餘生資指標將自動表格化呈現。
- **分享結果**：點擊 **「分享結果」** 按鈕，系統會將當前網址複製到剪貼簿。
- **下載結果**：點擊 **「下載結果(json)」** 按鈕，系統原始 JSON 轉化為 Data URL，下載名為 `result-[Task_ID].json` 的檔案。

### 3. 個人歷史紀錄管理
- **歷史看板**：呈現使用者 14 天內所有的預測紀錄，包含任務 ID、分析時間、生物類別。點擊卡片上的 **「查看結果」** 可直通該報告。
- **無痕管理**：
  - **單一刪除**：點擊 **「刪除記錄」**，系統會跳出提示，確認後移除該紀錄。
  - **一鍵清空**：點擊下方的 **「刪除歷史記錄」**，可完全清空本地緩衝。

---

## 📂 專案架構與檔案說明

```text
.
├── MitoFates/           # 核心預測引擎 (由 npm run install 自動下載至外層相對目錄)
│   ├── script/          # MitoFates.pl 主程式與相關 Modules 檔案
│   └── models/          # 預先訓練好的各式生物（Fungi/Metazoa/Plant）SVM 模型資料
└── mitofates-web/       # Node.js 網頁服務架構主體
    ├── server.js        # 後端核心：處理 API 路由、背景 14 天過期任務自動清理與調度
    ├── package.json     # 腳本化定義檔案
    ├── package-lock.json# 確切套件版本鎖定快照檔
    ├── public/          # 前端網頁靜態資源目錄
    │   ├── index.html   # 分析提交首頁
    │   ├── result.html  # 獨立結果展示頁面
    │   └── history.html # 14天內個人歷史紀錄看板
    ├── results/         # 儲存背景生成的分析結果 JSON 目錄
    └── uploads/         # 上傳序列暫存目錄
```

