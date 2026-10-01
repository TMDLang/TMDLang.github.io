# CLI 工具

TMD 提供了功能完整的命令列工具（CLI），支援在終端機中進行樂譜解析、小節校驗、多種音樂格式匯出、音訊即時預覽、樂譜重構，以及 AI Agent 技能與 MCP 伺服器的整合。

TMD CLI 目前提供兩種語言實作版本：

- **TmdSwift**：macOS / Linux 原生編譯高效能版本（可透過 Homebrew 安裝）。
- **tmdlang (Node.js)**：跨平台版本（支援 macOS、Linux 與 Windows，透過 npm 發行）。


## 安裝方式

### macOS / Linux（推薦透過 Homebrew 安裝 TmdSwift）

```bash
brew tap zonble/tmd
brew tap --trust zonble/tmd  # 允許第三方 Tap
brew install tmd
```

### Node.js 環境（全域安裝 tmdlang）

```bash
npm install -g tmdlang
```

安裝完成後，即可在終端機中執行 `tmd` 指令：

```bash
tmd --help
```

## 核心功能與基本指令

### 解析樂譜結構（`-p, --parse-only`）

僅解析樂譜並在終端機中印出全曲標題、速度、基準音高、小節數與各軌道結構摘要，不產生輸出檔：

```bash
tmd score.tmd -p
```

### 檢查小節長度一致性（`check`）

檢驗每一小節的時值是否符合拍號（`<4/4>`、`<3/4>` 等）。若有拍數過多或不足，會精確指出出錯的段落、軌道、行號與相差拍數：

```bash
tmd check score.tmd
```

> **提示**：在匯出 MIDI 或其他格式時，TMD 預設會先執行小節檢查。若要忽略檢查強制匯出，可加上 `-f, --force` 參數。

### 樂譜自動排版（`format`）

將樂譜程式碼自動縮排、整理小節線與間距，並完整保留原本的註解內容：

```bash
# 印出格式化後的結果
tmd format score.tmd

# 直接就地更新覆寫原檔案（In-place）
tmd format score.tmd -i
```


## 音樂格式匯出與音訊渲染

TMD 可以編譯並匯出至主流數位音樂工作站（DAW）、打譜軟體與人聲合成器支援的格式：

| 匯出目標 | 指令範例 | 說明 |
| :--- | :--- | :--- |
| **Standard MIDI** | `tmd score.tmd -m score.mid` | 匯入 Logic Pro、Cubase、FL Studio、Ableton Live |
| **REAPER Project** | `tmd score.tmd -r score.rpp` | 生成完整的 REAPER 多軌專案檔 |
| **MusicXML 4.0** | `tmd score.tmd -x score.musicxml` | 匯入 MuseScore、Sibelius、Finale 印製五線譜 |
| **LilyPond 樂譜** | `tmd score.tmd -l score.ly` | 匯出高階出版級排版原始碼 |
| **PDF 樂譜** | `tmd score.tmd --pdf-output score.pdf` | 透過本機 LilyPond 編譯器直接繪製成 PDF |
| **ABC Notation** | `tmd score.tmd -a score.abc` | 網頁樂譜渲染常用的 ABC 格式 |
| **ChordPro** | `tmd score.tmd -c score.cho` | 產生吉他彈唱與主導譜（Lead Sheet） |
| **WAV 音訊** | `tmd score.tmd -w song.wav` | 離線合成 16-bit 立體聲 WAV 音訊 |
| **VOCALOID** | `tmd score.tmd --vsqx-output vocal.vsqx` | 匯出虛擬歌手音軌（可搭配 `--singer Miku`） |
| **UTAU** | `tmd score.tmd -u vocal.ust` | 匯出 UTAU / OpenUtau 工程檔 |

### 終端機即時試聽播放（`--play`）

無須打開任何播放軟體，直接在終端機即時合成並試聽全曲：

```bash
# 播放全曲
tmd score.tmd --play

# 只播放特定段落或特定樂器
tmd score.tmd --play --section chorus --instrument Piano

# 指定自訂 SoundFont 音色庫 (.sf2)
tmd score.tmd --play --soundfont FluidR3_GM.sf2
```


## 歌曲分析與剖析（`inspect`）

`tmd inspect` 提供類似效能分析器（Profiler）的音樂特徵診斷：

```bash
tmd inspect score.tmd
```

終端機會印出完整的 ASCII 報表，包含：

- **歌手音域分析（Vocal Tessitura）**：計算主旋律軌道最高音、最低音、跨越半音數，並評估演唱難易度與建議聲部（女高音、男低音等）。
- **調性推論（Tonality）**：以 Krumhansl-Schmuckler (K-S) 演算法推斷全曲調性吻合度與離調轉調。
- **編曲特徵**：全曲總小節數、秒數長度、使用和弦種類與最高發聲密度（Peak Concurrency）。
- **CI/CD 自動化支援**：加上 `--json` 參數輸出結構化資料：

  ```bash
  tmd inspect score.tmd --json
  ```


## 樂譜重構工具（`refactor`）

TMD CLI 內建了多項常用的編曲重構工具：

### 重新命名樂器或段落

```bash
# 全域重命名樂器（例如將 Piano 改為 GrandPiano）
tmd refactor rename-instrument score.tmd Piano GrandPiano -i

# 全域重命名段落（例如將 A 改為 verse）
tmd refactor rename-section score.tmd A verse -i
```

### 擷取特定樂器為獨立檔案

```bash
# 將 Vocal 旋律軌道單獨抽出儲存為 lead.tmd
tmd refactor extract-instrument score.tmd Vocal -o lead.tmd
```

### 展開演奏順序為線性樂譜（Inline Orders）

將包含重複、轉調（`-> {?+1}`）的播放順序展開為平鋪直述的單一長樂譜：

```bash
tmd refactor inline-orders score.tmd -o linear.tmd
```


## AI Agent 與本機工作流整合

TMD CLI 專為人機協同設計，提供了一鍵配置命令，讓本機的 AI 助理具備音樂編寫與診斷能力：

### 安裝 AI Agent 技能（`--install-skills`）

```bash
tmd --install-skills
```

自動偵測並安裝 TMD 語言規範、動機發展技巧與對位法手冊到本機 AI 工具目錄：

- **Claude Code**：`~/.claude/skills/tmd/SKILL.md`
- **Google Antigravity / Gemini CLI**：`~/.gemini/config/skills/tmd/SKILL.md`
- **OpenAI Codex**：`~/.codex/skills/tmd/SKILL.md`

### 註冊 Model Context Protocol 伺服器（`--install-mcp` / `--mcp`）

```bash
# 自動註冊 TMD MCP 到 Claude Desktop、Cursor 與 Gemini 設定檔中
tmd --install-mcp

# 或以 stdio 方式獨立啟動 MCP 服務
tmd --mcp
```

### 啟動 Language Server Protocol（`--lsp`）

提供標準的 LSP 服務，支援透過 JSON-RPC 與任何支援 LSP 的現代編輯器介接，提供即時語法檢查與補齊。

```bash
tmd --lsp
```
