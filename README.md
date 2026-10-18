# Escort Console 2.12.07 繁體中文翻譯

[Escort Console](https://www.escortdls.com/) 是 ESCORT iMiniPlus PDF Data Logger 溫度記錄器的 Windows 管理軟體，原廠只提供英、法、德、義、西、波蘭文介面。本專案是[大元冷凍](https://www.start.com.tw/)為疫苗冰箱客戶整理的**繁體中文翻譯**，開放給有需要的人取用。

這裡只放翻譯內容與做法說明，**不含任何原廠程式或修改後的 DLL**。
要直接使用繁體中文版，請到官網下載一鍵安裝程式。

## 相關連結

- 公司網站（大元冷凍）：<https://www.start.com.tw/>
- Escort Console 繁體中文版下載頁：<https://www.start.com.tw/data-logger-download/>
- 軟體安裝教學：<https://www.start.com.tw/escort-console-software-install/>
- iMiniPlus PDF Data Logger 設定教學：<https://www.start.com.tw/iminiplus-pdf-data-logger-setup-guide/>

## 內容

| 檔案 | 說明 |
|---|---|
| `translation/zh-TW.csv` | 全部介面字串的翻譯，2,036 條，欄位為 `English`、`繁體中文`。英文是原廠資源裡的原文，含快速鍵標記 `&` 與格式符號 `%s`、`%lu`，翻譯時保留。字串內的換行以 `\n` 兩個字元表示。 |
| `translation/glossary.csv` | 術語表，翻譯時統一用語的依據。 |
| `extras/escort0809.tod` | 「每日提示」的繁體中文版（Big5 編碼純文字，一行一則）。 |
| `extras/set-display-language-zh.reg` | 把顯示語言設為 0x0809 的登錄檔，裝好中文 DLL 後匯入即可免手動切換。 |

CSV 為 UTF-8（含 BOM），Excel 直接開啟不會亂碼。

## 翻譯原則

- 2,036 條全部逐條對照英文原文，並參考原廠法文、德文版確認語意。
- 用語對齊 Windows 繁體中文慣例：對話方塊、連接埠、登入、預覽列印、精靈。
- 專有名詞：讀數 (reading)、行程 (trip)、程式設定 (program)、組態設定 (configure)、重新待命 (rearm)、平均動力溫度 (MKT)、生長度日 (GDD)。
- 帶 `%s` 的句子依中文語序調整，參數順序以原文為準，例如「不支援 %s（韌體版本 %s）」。

## 怎麼套用到程式

Escort Console 的介面文字全部放在語言資源 DLL `Escort<語言代碼>.dll`，主程式不必修改。做法是把翻譯寫回其中一個語言 DLL 的資源，再用登錄檔指定顯示語言。

1. 用 [Resource Hacker](http://www.angusj.com/resourcehacker/) 開啟原廠的 `Escort0809.dll`（English UK 槽位），匯出 MENU、DIALOG、STRINGTABLE 三種資源的 `.rc`。
2. 依 `zh-TW.csv` 把英文字串換成中文；對話框的 `FONT` 改為 `9, "PMingLiU"`。
3. 用 Resource Hacker 編譯 `.rc` 並以 `addoverwrite` 寫回 DLL，順序 STRINGTABLE → MENU → DIALOG。
4. 匯入 `set-display-language-zh.reg`，或在程式內 Edit → Options → Languages 選擇 English (UK)。

程式語言清單是從 DLL 的字串表（ID 10010 起）讀取的，把該條改成「中文(台灣)」，下拉選單就會顯示中文。

### 必須注意的例外

這是一支 ANSI（MBCS）程式，不同介面元素走不同編碼路徑，直接全部寫 Unicode 會有幾處顯示「??」：

- **頂層功能表列**（檔案、編輯、動作、視窗、說明）、**屬性表標題**（字串 ID 5942、9001）、**預覽列印的單頁／兩頁按鈕**（61505、61506）、**傳送郵件錯誤訊息**（61840–61842）：程式用 Windows-1252 轉換這些字串，所以必須先把中文編成 Big5 位元組，再把這些位元組當作 Windows-1252 字元寫進資源。寫法等同 `text.encode('cp950').decode('cp1252')`。
- 其餘子功能表、對話框、字串表：直接寫 Unicode 即可，系統為繁體中文（代碼頁 950）時正常顯示。
- **不可翻譯的字串**：ID 21057–21074 是指令碼編輯器的 VBScript 關鍵字清單，53546–53553 是圖表標記形狀名稱，翻了功能會壞。同一個英文字可能同時出現在可翻與不可翻的 ID（例如 Step 既是圖表類型「階梯」也是關鍵字），要以 ID 判斷。

### 已知限制

- 「說明 → 說明主題」開的是 WinHelp (.hlp)，Windows 10／11 已不支援，與翻譯無關。
- 「關於」對話框的網址寫死在主程式內，無法改。
- 讀數表格與圖表的感測器名稱來自通訊 DLL（esapi32a.dll）內建的字串表，不在語言 DLL 內；該表只有簡體中文（GB2312），在繁體系統會顯示亂碼。官網安裝程式已另行處理，本專案不含該部分。
- 少數 Windows 標準對話框文字由系統提供。

## 授權

翻譯內容與本說明以 [CC BY 4.0](LICENSE) 授權，使用時請註明「大元冷凍 https://www.start.com.tw/ 」。
製作：大元冷凍工程有限公司，台北。疫苗冰箱、溫度記錄器與軟體的相關問題可至 <https://www.start.com.tw/data-logger-download/> 查詢。
Escort Console 及其資源檔的著作權屬 Escort Data Logging Systems，本專案與原廠無關。
