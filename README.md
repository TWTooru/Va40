# 防疫幫手 (Va40 - Coronavirus Prevention)

[![Platform](https://img.shields.io/badge/Platform-Android-green.svg)](https://developer.android.com/)
[![Language](https://img.shields.io/badge/Language-Java%208-orange.svg)](https://www.java.com/)
[![Min SDK](https://img.shields.io/badge/Min%20SDK-31-blue.svg)](https://developer.android.com/about/versions/12)
[![Compile SDK](https://img.shields.io/badge/Compile%20SDK-31-brightgreen.svg)](https://developer.android.com/about/versions/12)
[![Architecture](https://img.shields.io/badge/Architecture-MVVM-lightgrey.svg)](https://developer.android.com/topic/architecture)

「**防疫幫手 (Va40)**」是一款針對 COVID-19 疫情期間個人健康管理與日常通行需求所設計的 Android 應用程式。整合了**數位疫苗護照**、**防疫實聯制足跡記錄**與**桌面小工具 (AppWidget)**，讓使用者能快速出示疫苗接種證明並便利管理日常實聯制進出場所紀錄。

---

## 📱 主要功能特色

### 1. 💉 數位疫苗護照 (Vaccine Passport)
* **卡片滑動檢視**：使用 `ViewPager2` 結合卡片縮放邊距效果 (`MarginPageTransformer`)，流暢切換檢視第 1 劑、第 2 劑、第 3 劑的施打完成狀態。
* **施打歷程清單**：使用 `RecyclerView` 呈現各劑次詳細資訊，包含施打廠牌（如高端、BNT）、接種日期時間與施打院所。
* **色彩識別指示**：不同劑次以專屬主題色（綠、黃、藍）區分，直覺辨識接種進度。
* **個人身份標示**：展示持有人基本資料，便於通行時核對身分。

### 2. 📋 防疫實聯制紀錄簿 (Contact Tracing Record)
* **足跡資料管理 (CRUD)**：
  * **新增紀錄**：彈出客製化對話框 (`dialog_gigi`)，快速輸入場所代碼、日期、時間及 AM/PM。
  * **編輯更新**：支援點擊既有項目重新編輯資料並同步更新至資料庫。
  * **批次刪除**：提供核取方塊 (Checkbox) 多選機制，可勾選多筆足跡後一鍵刪除。
  * **時間搜尋**：提供搜尋欄位，依時間關鍵字快速篩選歷史實聯制紀錄。
* **本地離線儲存**：採用 Android Jetpack **Room** 進行資料持久化，無網路環境亦可完整離線查詢。
* **即時資料更新**：結合 **LiveData** 觀察資料變化，資料增刪修時介面即時自動刷新。

### 3. 🧩 Android 桌面小工具 (AppWidget)
* **桌面快速預覽**：支援將小工具釘選至手機主畫面，直接檢視三劑疫苗施打狀態燈號。
* **一鍵快速開啟**：在桌面小工具點擊捷徑按鈕即可立即開啟「疫苗護照」介面，省去繁瑣尋找與開啟 App 的步驟。

---

## 🛠 技術架構與使用元件

| 分類 | 技術 / 函式庫 | 說明 |
| :--- | :--- | :--- |
| **開發語言** | Java 8 | 核心業務邏輯與畫面控制 |
| **架構模式** | MVVM (Model-View-ViewModel) | 透過 `ViewModel` 與 `LiveData` 實現資料與畫面解耦 |
| **資料持久化** | Android Jetpack Room (v2.4.2) | SQLite ORM 抽象層，負責本機實聯制資料庫操作 |
| **UI 視圖綁定** | ViewBinding | 類型安全的視圖綁定，取代傳統 `findViewById` |
| **介面元件** | Material Components (v1.5.0) | Material Design 風格元件與樣式 |
| **佈局管理** | ConstraintLayout (v2.1.3) | 高效能扁平化排版佈局 |
| **分頁滑動** | ViewPager2 | 疫苗卡片水平滑動切換與動態邊距轉換效果 |
| **列表元件** | RecyclerView | 高效能清單檢視與自訂 Adapter |
| **彈跳視窗** | DialogFragment | 自訂圓角彈出視窗 (`dialog_gigi`) |
| **桌面延伸** | AppWidgetProvider (`Widget1`) | Android 系統級桌面小工具 |

---

## 📂 專案目錄結構

```text
com.example.va40
├── MainFace.java                 # 應用程式首頁 / 主入口導航
├── VApassport.java               # 疫苗護照主頁面 (ViewPager2 + 歷程列表)
├── VAmail.java                   # 防疫實聯制主頁面 (列表、搜尋、刪除、新增)
├── Viewmodel.java                # 共享 ViewModel，串接 Room 資料庫
├── Widget1.java                  # 桌面小工具 AppWidgetProvider 實作
│
├── dose_fragment/                # 疫苗護照相關模組
│   ├── first_dose.java           # 第 1 劑狀態卡片 Fragment
│   ├── second_dose.java          # 第 2 劑狀態卡片 Fragment
│   ├── third_dose.java           # 第 3 劑狀態卡片 Fragment
│   ├── SlideAdapter.java         # ViewPager2 FragmentStateAdapter
│   └── doseRecyAdapter.java      # 疫苗施打紀錄 RecyclerView Adapter
│
├── dialog/                       # 對話框與列表項目
│   ├── dialog_gigi.java          # 新增/編輯實聯制對話框 (DialogFragment)
│   └── Dialog_adapter.java       # 實聯制資料清單 RecyclerView Adapter
│
└── ROOM/                         # 本地資料庫架構 (Room ORM)
    ├── Database.java             # RoomDatabase 抽象類別 (DB 名稱: WorldHenDa)
    ├── VADao.java                # 資料存取介面 (Insert, Update, Delete, Select)
    └── room.java                 # 實聯制資料實體類別 (Entity)
```

---

## 🗄️ 資料庫設計 (Room Schema)

實聯制資料儲存於本機 SQLite 資料庫，Entity 定義如下：

* **資料表 (Entity)**: `room`
* **資料庫名稱**: `WorldHenDa`

| 欄位名稱 | 型態 | 屬性 | 說明 |
| :--- | :--- | :--- | :--- |
| `id` | `int` | `@PrimaryKey(autoGenerate = true)` | 自動遞增主鍵 |
| `code` | `String` | `@ColumnInfo(name = "code")` | 場所代碼 (簡訊實聯制代碼) |
| `date` | `String` | `@ColumnInfo(name = "date")` | 登記日期 (例如：2022/04/10) |
| `time` | `String` | `@ColumnInfo(name = "time")` | 登記時間 (例如：14:20) |
| `AM_PM` | `Boolean` | `@ColumnInfo(name = "AM_PM")` | 上午/下午識別標記 (`true`: AM / `false`: PM) |

---

## ⚙️ 開發與建置環境需求

* **Android Studio**: Arctic Fox (2020.3.1) / Chipmunk (2021.2.1) 或更高版本
* **JDK 版本**: Java 8 (1.8) 或 Java 11
* **Gradle 版本**: 7.2
* **Android Gradle Plugin (AGP)**: 7.1.1
* **SDK 設定**:
  * `compileSdk`: 31
  * `minSdk`: 31 (建議測試於 Android 12+ 模擬器或實體機)
  * `targetSdk`: 31

---

## 🚀 快速開始 (Getting Started)

### 1. 取得專案原始碼
```bash
git clone <repository-url>
cd My-APP-main
```

### 2. 使用 Android Studio 開啟
1. 開啟 **Android Studio**。
2. 選擇 **File -> Open...** 並選取本專案根目錄。
3. 等待 Gradle 專案自動同步完成 (Gradle Sync)。

### 3. 編譯與建置
* **指令列編譯 Debug APK**:
  ```bash
  # Windows CMD / PowerShell
  .\gradlew.bat assembleDebug

  # macOS / Linux
  ./gradlew assembleDebug
  ```
  產生的 APK 位於：`app/build/outputs/apk/debug/app-debug.apk`。

### 4. 部署與執行
* 連接已啟用「USB 偵錯」的 Android 裝置（Android 12 / API 31 以上）或啟動 AVD 模擬器。
* 點擊 Android Studio 工具列上的 **Run (綠色三角形)** 即可安裝並啟動 App。

---

## 💡 未來優化與擴充建議 (Roadmap)

- [ ] **相機 QR Code 掃描**：整合 ZXing 或 Google ML Kit，支援掃描店家 QR Code 自動解析場所代碼並儲存。
- [ ] **背景非同步處理**：調整資料庫查詢與操作，改由 `ExecutorService`、RxJava 或 Coroutine 於背景執行，避免主線程 (`allowMainThreadQueries`) 堵塞。
- [ ] **向下相容性改善**：評估調整 `minSdkVersion` 至 API 24+ (Android 7.0+)，讓更多舊型號行動裝置得以運行。
- [ ] **動態劑次擴充**：將疫苗施打資料轉為資料庫管理或 API 同步，支援第 4 劑、次世代追加劑等動態擴充。
- [ ] **數位證明匯入**：支援匯入並解析衛福部「數位新冠病毒健康證明」標準規格 (SHC / EU DCC QR Code)。
