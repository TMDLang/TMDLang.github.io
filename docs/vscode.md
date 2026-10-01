# Visual Studio Code 以及 TMD 本機開發環境

除了網頁版 TMD Studio 之外，TMD 提供了完整的本機命令列工具（CLI）、VS Code 擴充套件、Model Context Protocol (MCP) Server 以及 AI Agent Skill，讓您可以將音樂創作無縫整合至現代開發者工作流與各類 AI 工具中。

---

## 1. 安裝 TMD TS (Node.js) 版本

TMD 的 TypeScript/JavaScript 實作可在任何安裝有 Node.js 的環境（macOS、Linux、Windows）下執行。

### 系統需求

- Node.js `v20.0.0` 或更新版本。

### 全域安裝 CLI 工具

透過 `npm` 全域安裝：

```bash
npm install -g tmd-ts
```

安裝完成後，您即可在終端機中直接使用 `tmd` 指令：

```bash
tmd --help
```

亦可透過 `npx` 免安裝直接執行：

```bash
npx tmd-ts score.tmd -m score.mid
```

---

## 2. 安裝 TMD Swift (原生命令列版本)

如果您使用 macOS 或偏好原生編譯的高效能工具，可以使用 Swift 實作版本。

### 透過 Homebrew 安裝 (macOS / Linux)

```bash
brew tap zonble/tmd
brew tap --trust zonble/tmd  # 允許第三方 Tap
brew install tmd
```

### 從原始碼編譯

```bash
git clone https://github.com/zonble/TmdSwift.git
cd TmdSwift
swift build -c release
# 執行檔產出於 .build/release/tmd
```

---

## 3. 其他必要與推薦套件

若要使用完整的格式輸出與樂譜排版功能，建議視需要安裝以下外部工具：

- **LilyPond**（產生高階五線譜 PDF 檔案）：

  ```bash
  brew install lilypond
  ```

- **MuseScore** 或 **Sibelius**（檢視與播放匯出的 `.musicxml` 檔案）。
- **FluidSynth / SoundFont**（在命令列直接試聽或離線合成音訊）。

---

## 4. 安裝 VS Code Extension

在 Visual Studio Code 中編輯 TMD 檔案可享有絕佳的記譜體驗：

1. 開啟 VS Code。
2. 進入擴充套件市集（Extensions，快速鍵 `Cmd+Shift+X` 或 `Ctrl+Shift+X`）。
3. 搜尋 `TMD` 或 `Timebase Mark Down`。
4. 點選 **Install** 安裝擴充套件。

### VS Code 提供的功能

- **語法高亮（Syntax Highlighting）**：精確標示標題、速度、拍號、唱名數字、升降八度與和弦符號。
- **即時診斷與小節檢查（Diagnostics）**：當小節拍數不足或超出拍號時，編輯器內直接出現紅色波浪線提示。
- **快捷自動補齊（Snippets & Completions）**：輸入 `sec` 或 `chord` 快速產生段落模板。
- **樂譜預覽與快捷編譯**：一鍵將當前檔案編譯為 MIDI 或呼叫本機播放。

---

## 5. 安裝 TMD SKILL 與 TMD MCP Server

為了讓您的本機 AI 助理（如 Claude Desktop、Cursor、Gemini CLI、Antigravity、Codex）理解 TMD 並協助您編曲，TMD 提供了一鍵安裝命令：

### 5.1 安裝 AI Agent Skill

執行以下指令，系統會自動將 TMD 記譜規範、動機發展原則與對位法技巧安裝到本機的 AI Agent 設定目錄中：

```bash
tmd --install-skills
```

支援自動偵測配置的 Agent 包括：

- Google Antigravity / Gemini CLI
- Claude Code
- OpenAI Codex

### 5.2 安裝 Model Context Protocol (MCP) Server

透過 MCP，AI 模型可以在交談過程中直接讀取、驗證、排錯或產生 TMD 樂譜：

```bash
# 自動註冊 TMD MCP 到 Claude Desktop、Cursor 與 Gemini 設定檔
tmd --install-mcp
```

您也可以手動以 stdio 啟動 MCP 伺服器：

```bash
tmd --mcp
```

---

## 6. 使用 Web MCP 呼叫 TMD

在支援 Web MCP 的現代瀏覽器或雲端 AI 介面中，TMD Studio 亦支援透過標準 Web MCP 端點進行遠端互動：

- **即時小節驗證（`checkTmd` / `check_tmd`）**：傳入 TMD 文字，工具回傳小節長度檢查結果與詳細差異清單。
- **樂譜結構解析（`parseTmd`）**：分析段落結構、樂器分配與播放流程。
- **音樂轉檔與匯出**：透過 API 直接取得編譯後的 MIDI 或 MusicXML 資料。

---

## 7. 在 CLI Agent 中與 TMD 互動（人機協同工作流）

在終端機或 AI Coding Agent 中，您可以直接以自然語言指揮 AI 與 TMD 工具進行雙向互動：

### 常見協作指令範例

#### 1. 小節檢查與語法自我修復

```bash
# 讓 AI 檢查小節長度
tmd check my_song.tmd
```

如果發現小節拍數不符，AI 會根據報錯訊息（例如第 12 行短少 1 拍）自動補足延音線 `-` 或休止符 `0`。

#### 2. 自動格式化排版

```bash
tmd format my_song.tmd -i
```

自動調整縮排與對齊，維持樂譜整潔。

#### 3. 歌曲結構與音域剖析

```bash
tmd inspect my_song.tmd
```

印出終端機 ASCII 報表，檢視最高音、最低音、跨越半音數與調性吻合度。

#### 4. 終端機即時試聽與快速轉檔

```bash
# 終端機播放試聽
tmd my_song.tmd --play

# 匯出 MIDI
tmd my_song.tmd -m my_song.mid

# 輸出 MusicXML
tmd my_song.tmd -x my_song.musicxml

# 渲染 WAV 音訊
tmd my_song.tmd -w preview.wav
```

透過這些本機工具鏈，創作者只需負責哼唱或編寫核心旋律，其餘繁瑣的多軌配器、和弦編排、小節校驗與檔案轉檔皆可由 AI 與 TMD CLI 高效完成！
