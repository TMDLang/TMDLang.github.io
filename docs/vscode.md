# VS Code 套件

TMD 官方為 **Visual Studio Code**（以及 Cursor、VSCodium 等相容編輯器）提供了專屬擴充套件，將專業音樂工作站的各項功能直接搬進程式碼編輯器中，讓音樂創作享有與寫程式一樣流暢的體驗。

---

## 1. 安裝與設定

### 方式 A：從市集安裝

1. 開啟 VS Code。
2. 切換至延伸模組面板（快速鍵 `Cmd+Shift+X` 或 `Ctrl+Shift+X`）。
3. 搜尋 `TMD` 或 `Timebase Mark Down`。
4. 點選 **安裝（Install）**。

### 方式 B：本地安裝指令碼（開發／免市集）

如果您已經 clone 了 [TmdSwift](https://github.com/zonble/TmdSwift) 儲存庫，可直接使用內建指令碼安裝：

```bash
# 建立 Symlink 連結（推薦開發者使用）
./scripts/install-vscode-extension.sh

# 或以獨立複製模式安裝
./scripts/install-vscode-extension.sh --copy
```

安裝後在 VS Code 按下 `Cmd+Shift+P` ➔ 選擇 **Developer: Reload Window** 即可生效。

### 本機 CLI 依賴

擴充套件的進階匯出與分析功能會自動偵測本機的 `tmd` 執行檔（預設搜尋 `/usr/local/bin/tmd`、`/opt/homebrew/bin/tmd`、`~/.local/bin/tmd` 或系統 `PATH`）。如果使用自訂路徑，可在 VS Code 設定搜尋 `tmd.executablePath` 進行指定。

---

## 2. 語法高亮與智慧程式碼片段（Snippets）

打開任何 `.tmd` 檔案，擴充套件會提供專屬的語法突顯與結構摺疊：

- **精確色彩高亮**：
    - 樂譜標頭 `::SCORE::` 與標題 `** 標題 **`。
    - 拍號 `<4/4>`、速度 `!= 120`、基準音高 `?= C` 與調性 `key= Bm`。
    - 唱名音符數字 `1`–`7`、升降記號 `'` / `,` 與八度標記 `^` / `_`。
    - 和弦標記 `[Cmaj7]`、`[1]`、`[6m]`。
    - 演奏流程 `-> intro -> verse ->#`。
- **程式碼區塊摺疊**：支援以段落 `{ ... }` 為單位進行展開與摺疊。
- **快速程式碼片段（Snippets）**：
    - 輸入 `score` ➔ 按 Tab 快速產生完整樂譜範本。
    - 輸入 `para` ➔ 產生樂器軌道段落區塊（`name:Instrument@|0|{ ... }`）。
    - 輸入 `sec` ➔ 插入節奏網格細分（`<4*>`）。
    - 輸入 `tup` ➔ 插入三連音等節奏群組（`%(---)`）。
    - 輸入 `ch` ➔ 快速插入和弦語法（`[...]`）。

---

## 3. 即時小節檢查與診斷（Live Diagnostics）

VS Code 延伸模組內建了即時節奏編譯器，在您輸入或儲存檔案時自動檢查小節拍數：

- **紅色波浪線提示**：若某個小節的拍數多出或短少（例如 `<4/4>` 拍號下只寫了 3 拍），編輯器會立即在該小節下方繪製警告波浪線。
- **問題面板整合（Problems View）**：在 VS Code 底部的 **Problems（問題）** 面板中，會條列出所有出錯的段落名稱、小節序號、預期拍數與實際拍數差距，點擊即可跳轉至出錯行。

---

## 4. 側邊欄專屬工作區（TMD Studio 活動列）

點擊 VS Code 左側活動列的 **TMD 圖示**，即可展開專屬的側邊欄面板：

### 1. 大綱與段落導覽（TMD Outline）

自動解析目前樂譜中的所有段落（`intro`、`verse`、`chorus` 等）與各軌道樂器清單。

- 點擊段落直接跳轉至對應程式碼位置。
- 提供直覺的快捷按鈕，可直接單獨播放選定段落或軌道。

### 2. 歌曲分析器（Song Inspector）

內建視覺化 Webview 儀表板，即時呈現：

- **歌手音域分析（Vocal Tessitura）**：顯示主旋律最高音、最低音、跨越半音數與聲部難易度評估（Soprano / Tenor 等）。
- **調性推論與五度圈軌跡**：利用 K-S 演算法分析調性吻合度與離調轉調走向。
- **編曲密度統計**：全曲總小節數、演奏時間與同時發聲的音符密度分布。

### 3. 虛擬鋼琴鍵盤（Virtual Keyboard）

提供可點擊試聽的互動式鋼琴鍵盤，可將音符直接插入至編輯器游標所在位置。

### 4. 哼唱轉譜（Hum to TMD）

透過側邊面板啟動麥克風錄音，利用 Basic Pitch 演算法進行即時人聲哼唱辨識，自動轉換為簡譜段落並填入編輯區。

---

## 5. 互動式播放器與多格式匯出

在編輯器右上角按鈕、右鍵選單或按 `Cmd+Shift+P` 叫出命令面板（輸入 `TMD:`）即可使用豐富的功能：

### 試聽與播放

- **TMD: Open Web MIDI Player**：開啟內建的 Web MIDI 合成器播放器面板，支援 FluidR3 平台鋼琴音色、多軌 GM 音色與 Chiptune 8-bit 音效。
- **TMD: Play Audio Preview in Terminal**：直接在終端機背景播放即時音訊。
- **TMD: Play Current Section / Track**：單獨試聽游標所在的段落或單軌。

### 格式匯出（Export）

- **匯出為 MIDI** (`.mid`)：供 Logic Pro / Cubase / FL Studio 載入。
- **匯出為 MusicXML** (`.musicxml`)：供 MuseScore / Sibelius 排版五線譜。
- **匯出為 LilyPond** (`.ly`) 或**繪製成 PDF** (`.pdf`)：高水準出版印刷樂譜。
- **匯出為 ABC 記譜法** (`.abc`)。
- **渲染為 WAV 音訊** (`.wav`)。
- **虛擬歌手格式**：匯出為 VOCALOID (`.vsq`, `.vsqx`) 或 UTAU (`.ust`) 專案檔。

---

## 6. 編曲重構工具（Refactoring）

選取樂譜片段或直接在文件中點擊右鍵，可使用一系列快速重構指令：

- **TMD: Format Document**（快速鍵 `Shift+Option+F` / `Shift+Alt+F`）：自動縮排、整理小節線與空格。
- **網格解析度調整**：
    - **Double Grid**：`<4*>` ➔ `<8*>`（解析度加倍）。
    - **Halve Grid**：`<8*>` ➔ `<4*>`（解析度減半）。
    - **Optimize Grid**：自動計算全曲最小冗餘網格。
- **移調**：整首歌曲或選取區域平移指定半音數。
- **軌道操作**：複製軌道、自動生成平行三度／六度合音軌道、抽出單一樂器。
- **展開演奏順序（Inline Orders）**：將包含反覆與轉調的流程展開為單一線性樂譜。

---

## 7. GitHub Copilot 與 AI Chat 深度整合

TMD VS Code 擴充套件原生支援 GitHub Copilot Chat 與語言模型工具（Language Model Tools）：

### Copilot Chat Participant（`@tmd`）

在 VS Code Copilot Chat 視窗中輸入 `@tmd` 即可呼叫專屬音樂助理：

- `@tmd /compose`：描述風格與旋律動機，讓 Copilot 協助你撰寫多軌 TMD 段落。
- `@tmd /check`：檢查當前檔案中的小節長度、拍數計算與語法問題。
- `@tmd /fix`：自動修復小節拍數不符或遺漏的延音線。
- `@tmd /explain`：解說樂譜結構、和弦進行與首調唱名關係。

### Language Model Tools

擴充套件向 VS Code 註冊了 `tmd_check`、`tmd_format` 與 `tmd_get_specification` 等工具，其他相容的 AI Agent（如 Claude、Cursor Agent）可直接呼叫這些工具來驗證與排版樂譜。
