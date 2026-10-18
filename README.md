# Escort Console 2.12.07 Traditional Chinese Translation

[English](#english) | [繁體中文](#繁體中文)

## English

Escort Console is the Windows software for ESCORT iMiniPlus PDF Data Logger temperature recorders. The vendor ships English, French, German, Italian, Spanish and Polish interfaces only. This project is the **Traditional Chinese (Taiwan) translation** prepared by [Dayuan Refrigeration](https://www.start.com.tw/) for its [vaccine refrigerator](https://www.start.com.tw/vaccine-refrigerator/) customers, released for anyone who needs it.

This repository contains the translation and a description of the method only. **It contains no vendor binaries and no modified DLLs.**
To use the Traditional Chinese edition directly, download the one-click installer from our website.

### Links

- Company website (Dayuan Refrigeration, Taipei): <https://www.start.com.tw/>
- Vaccine refrigerators: <https://www.start.com.tw/vaccine-refrigerator/>
- Escort Console Traditional Chinese download page: <https://www.start.com.tw/data-logger-download/>
- Installation guide: <https://www.start.com.tw/escort-console-software-install/>
- iMiniPlus PDF Data Logger setup guide: <https://www.start.com.tw/iminiplus-pdf-data-logger-setup-guide/>

### Contents

| File | Description |
|---|---|
| `translation/zh-TW.csv` | All 2,036 UI strings, columns `English` and `繁體中文`. The English column is the original resource text, including accelerator marks (`&`) and format specifiers (`%s`, `%lu`), which are preserved in the translation. Line breaks inside a string are written as the two characters `\n`. |
| `translation/glossary.csv` | Terminology used to keep the translation consistent. |
| `extras/escort0809.tod` | Traditional Chinese "Tip of the Day" file (Big5 plain text, one tip per line). |

CSV files are UTF-8 with BOM and open correctly in Excel.

### Translation principles

- All 2,036 strings were checked one by one against the English source, with the vendor's French and German editions used as a second reference.
- Wording follows Windows Traditional Chinese conventions (dialog box, port, log on, print preview, wizard).
- Fixed terms: 讀數 (reading), 行程 (trip), 程式設定 (program), 組態設定 (configure), 重新待命 (rearm), 平均動力溫度 (MKT), 生長度日 (GDD).
- Sentences containing `%s` are re-ordered for natural Chinese while keeping the parameter order of the source, e.g. 「不支援 %s（韌體版本 %s）」.

### How to apply it to the program

All interface text lives in a language resource DLL named `Escort<LANGID>.dll`; the main executable is not modified. The translation is written back into one of the language DLLs and the display language is selected through the registry.

1. Open the vendor's `Escort0809.dll` (the English UK slot) in [Resource Hacker](http://www.angusj.com/resourcehacker/) and export the MENU, DIALOG and STRINGTABLE resources as `.rc` files.
2. Replace the English strings with the Chinese ones from `zh-TW.csv`; change every dialog `FONT` to `9, "PMingLiU"`.
3. Compile the `.rc` files with Resource Hacker and write them back with `addoverwrite`, in the order STRINGTABLE, MENU, DIALOG.
4. In the program choose English (UK) under Edit → Options → Languages and restart. (The same can be done by setting the DWORD `DisplayLanguage` to `0x809` under `HKEY_CURRENT_USER\Software\Escort DLS\ESCORT Console\Settings`.)

The language list in the program is read from the DLL string table (IDs from 10010), so changing that entry to 「中文(台灣)」 makes the drop-down show Chinese.

#### Exceptions you must handle

Escort Console is an ANSI (MBCS) application and different UI elements take different code-page paths. Writing everything as Unicode leaves a few places showing `??`:

- **Top-level menu bar** (File, Edit, Action, Window, Help), **property-sheet captions** (string IDs 5942 and 9001), the **One Page / Two Page buttons in print preview** (61505, 61506) and the **Send Mail error messages** (61840–61842) are converted by the program through Windows-1252. For these, encode the Chinese text as Big5 bytes first and store those bytes as Windows-1252 characters in the resource, i.e. `text.encode('cp950').decode('cp1252')`.
- Sub-menus, dialogs and the rest of the string table can be written as plain Unicode; they render correctly on a Traditional Chinese system (code page 950).
- **Do not translate** IDs 21057–21074 (the VBScript keyword list of the script editor) or 53546–53553 (graph marker shape names); translating them breaks functionality. The same English word can appear under both translatable and non-translatable IDs (for example "Step" is both the graph type 「階梯」 and a keyword), so decide by ID, not by text.

#### Known limitations

- Help → Help Topics opens WinHelp (.hlp), which Windows 10/11 no longer supports. Unrelated to the translation.
- The URL in the About dialog is hard-coded in the executable and cannot be changed.
- Sensor names shown in the readings grid and graph legend come from a string table inside the communication DLL (esapi32a.dll), not from the language DLL. That table is Simplified Chinese (GB2312) only and shows garbled text on Traditional Chinese systems. Our website installer handles this separately; it is not part of this repository.
- A few standard Windows dialog strings are supplied by the operating system.

### License

The translation and this document are licensed under [CC BY 4.0](LICENSE). Please credit 「大元冷凍 https://www.start.com.tw/ 」.
Escort Console and its resource files are copyright of Escort Data Logging Systems. This project is not affiliated with the vendor.
Produced by Dayuan Refrigeration Engineering Co., Ltd., Taipei, Taiwan. Questions about [vaccine refrigerators](https://www.start.com.tw/vaccine-refrigerator/), data loggers and the software: <https://www.start.com.tw/data-logger-download/>.

---

## 繁體中文

Escort Console 是 ESCORT iMiniPlus PDF Data Logger 溫度記錄器的 Windows 管理軟體，原廠只提供英、法、德、義、西、波蘭文介面。本專案是[大元冷凍](https://www.start.com.tw/)為[疫苗冰箱](https://www.start.com.tw/vaccine-refrigerator/)客戶整理的**繁體中文翻譯**，開放給有需要的人取用。

這裡只放翻譯內容與做法說明，**不含任何原廠程式或修改後的 DLL**。
要直接使用繁體中文版，請到官網下載一鍵安裝程式。

### 相關連結

- 公司網站（大元冷凍）：<https://www.start.com.tw/>
- 疫苗冰箱：<https://www.start.com.tw/vaccine-refrigerator/>
- Escort Console 繁體中文版下載頁：<https://www.start.com.tw/data-logger-download/>
- 軟體安裝教學：<https://www.start.com.tw/escort-console-software-install/>
- iMiniPlus PDF Data Logger 設定教學：<https://www.start.com.tw/iminiplus-pdf-data-logger-setup-guide/>

### 內容

| 檔案 | 說明 |
|---|---|
| `translation/zh-TW.csv` | 全部介面字串的翻譯，2,036 條，欄位為 `English`、`繁體中文`。英文是原廠資源裡的原文，含快速鍵標記 `&` 與格式符號 `%s`、`%lu`，翻譯時保留。字串內的換行以 `\n` 兩個字元表示。 |
| `translation/glossary.csv` | 術語表，翻譯時統一用語的依據。 |
| `extras/escort0809.tod` | 「每日提示」的繁體中文版（Big5 編碼純文字，一行一則）。 |

CSV 為 UTF-8（含 BOM），Excel 直接開啟不會亂碼。

### 翻譯原則

- 2,036 條全部逐條對照英文原文，並參考原廠法文、德文版確認語意。
- 用語對齊 Windows 繁體中文慣例：對話方塊、連接埠、登入、預覽列印、精靈。
- 專有名詞：讀數 (reading)、行程 (trip)、程式設定 (program)、組態設定 (configure)、重新待命 (rearm)、平均動力溫度 (MKT)、生長度日 (GDD)。
- 帶 `%s` 的句子依中文語序調整，參數順序以原文為準，例如「不支援 %s（韌體版本 %s）」。

### 怎麼套用到程式

Escort Console 的介面文字全部放在語言資源 DLL `Escort<語言代碼>.dll`，主程式不必修改。做法是把翻譯寫回其中一個語言 DLL 的資源，再用登錄檔指定顯示語言。

1. 用 [Resource Hacker](http://www.angusj.com/resourcehacker/) 開啟原廠的 `Escort0809.dll`（English UK 槽位），匯出 MENU、DIALOG、STRINGTABLE 三種資源的 `.rc`。
2. 依 `zh-TW.csv` 把英文字串換成中文；對話框的 `FONT` 改為 `9, "PMingLiU"`。
3. 用 Resource Hacker 編譯 `.rc` 並以 `addoverwrite` 寫回 DLL，順序 STRINGTABLE → MENU → DIALOG。
4. 在程式內 Edit → Options → Languages 選擇 English (UK) 後重開。（等效做法：把登錄檔 `HKEY_CURRENT_USER\Software\Escort DLS\ESCORT Console\Settings` 下的 DWORD `DisplayLanguage` 設為 `0x809`。）

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

### 授權

翻譯內容與本說明以 [CC BY 4.0](LICENSE) 授權，使用時請註明「大元冷凍 https://www.start.com.tw/ 」。
製作：大元冷凍工程有限公司，台北。[疫苗冰箱](https://www.start.com.tw/vaccine-refrigerator/)、溫度記錄器與軟體的相關問題可至 <https://www.start.com.tw/data-logger-download/> 查詢。
Escort Console 及其資源檔的著作權屬 Escort Data Logging Systems，本專案與原廠無關。
