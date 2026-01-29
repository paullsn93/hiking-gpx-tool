# 將 GPX 工具打包為 Android App 的完整指南

是的，這個程式完全可以打包成 Android App。
由於這是一個標準的 Web 應用 (HTML/JS/CSS)，最推薦使用 **CapacitorJS** 將其封裝。

## 📋 前置準備
在開始之前，您的電腦需要安裝：
1.  **Node.js** (建議 LTS 版本)
2.  **Android Studio** (包含 Android SDK)

## 🚀 實作步驟 (使用 Capacitor)

因為目前只是一個單獨的 `index.html`，我們需要稍微調整一下結構才能打包。

### 步驟 1: 初始化專案
建立一個新的資料夾來存放 App 專案，然後初始化：

```bash
# 1. 建立資料夾
mkdir hiking-app
cd hiking-app

# 2. 初始化 npm
npm init -y

# 3. 安裝 Capacitor 核心
npm install @capacitor/core @capacitor/cli @capacitor/android

# 4. 初始化 Capacitor (App ID 類似 com.example.app)
npx cap init "Hiking Tool" com.mysite.hikingtool
```

### 步驟 2: 準備網頁檔案
Capacitor 預設會讀取 `www` 或 `dist` 資料夾中的內容。

1.  在 `hiking-app` 資料夾中建立一個名為 `www` 的資料夾。
2.  將您的 **`index.html`** (包含所有 CSS/JS) 複製到 `www` 資料夾中。
3.  確認 `capacitor.config.json` 中的 `webDir` 設定為 `"www"`。

```json
{
  "appId": "com.mysite.hikingtool",
  "appName": "Hiking Tool",
  "webDir": "www",
  "bundledWebRuntime": false
}
```

### 步驟 3: 加入 Android 平台
```bash
npx cap add android
```

### 步驟 4: 測試與建置
```bash
# 同步網頁檔案到 Android 專案
npx cap sync

# 開啟 Android Studio
npx cap open android
```

### 步驟 5: 在 Android Studio 中打包
1.  Android Studio 開啟後，等待 Gradle 同步完成。
2.  連接您的 Android 手機 (開啟開發者模式 USB 偵錯)。
3.  點擊上方的 **Run (綠色三角形)** 按鈕，App 就會安裝到手機上了。
4.  若要產生 APK 檔案：
    *   點選 `Build` > `Build Bundle(s) / APK(s)` > `Build APK(s)`。

---

## 💡 進階建議：針對 App 的優化

如果您決定要走 App 路線，我們的程式碼可以做一些優化：

1.  **離線地圖 (更強大)**：
    *   **網頁版**：只能用前面提到的「圖片預載」存到瀏覽器快取 (較不穩定)。
    *   **App 版**：可以使用 Capacitor 的 `Filesystem` API，將地圖圖片真正下載並存到手機儲存空間，保證永久有效。

2.  **GPS 背景執行**：
    *   目前網頁版在螢幕關閉時，GPS 可能會被瀏覽器暫停以省電。
    *   App 版可以加入 Background Geolocation plugin，確保螢幕關閉也能記錄軌跡。

3.  **全螢幕體驗**：
    *   App 預設就是全螢幕，也不會有瀏覽器的網址列佔用空間。
