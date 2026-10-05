# 開發者指南

TMD 不僅是為人類創作者設計的樂譜標記語言，也是現代化的音樂編譯與處理框架。

透過官方 npm 套件 **`tmdlang`**，可將 TMD 語法解析、小節檢查、AST 遍歷、多格式轉檔（MIDI / MusicXML / WAV / REAPER / VOCALOID）與 MCP AI 工具整合到 Node.js、TypeScript、Web 應用程式或自動化腳本。

## 安裝套件

```bash
npm install tmdlang
```

## 核心 API

### 解析樂譜（AST 與元數據）

使用 `TmdParser` 可將 TMD 原始字串解析為抽象語法樹（`Sheet` 物件）：

```typescript
import { TmdParser } from 'tmdlang';

const tmdCode = `
::SCORE::
name: My Prelude
speed: 120
key: C
?=0

intro:piano {
  <4*> 1 3 5 1' | 5 3 1 - |
}

-> intro ->#
`;

try {
  const sheet = TmdParser.parse(tmdCode);

  console.log(`曲名: ${sheet.name}`);
  console.log(`速度: ${sheet.speed} BPM`);
  console.log(`拍號: ${sheet.beat.count}/${sheet.beat.noteValue}`);
  console.log(`段落數: ${sheet.entries.length}`);
} catch (error) {
  console.error("解析失敗:", error);
}
```

### 檢查小節拍數完整度

使用 `TMDMeasureChecker` 可在不編譯的情況下，靜態校驗樂譜拍數：

```typescript
import { TMDMeasureChecker } from 'tmdlang';

const codeWithMistake = `
intro:piano {
  <4*> 1 2 3 |  # 4/4 拍中只有 3 拍
}
`;

const issues = TMDMeasureChecker.check(codeWithMistake);

if (issues.length > 0) {
  for (const issue of issues) {
    console.warn(`第 ${issue.lineNumber} 行 [${issue.paragraphName}]: ${issue.description}`);
  }
} else {
  console.log("所有小節拍數檢查皆通過！");
}
```

### 生成 Standard MIDI 二進位檔案（Uint8Array）

使用 `TMDMIDIGenerator` 可將 AST 轉換為標準 MIDI 格式的 `Uint8Array`，方便存檔或傳送給音訊引擎：

```typescript
import * as fs from 'node:fs';
import { TmdParser, TMDMIDIGenerator } from 'tmdlang';

const sheet = TmdParser.parse(tmdCode);

// 產出 MIDI 二進位資料（預設 480 ticks per quarter note）
const midiBytes: Uint8Array = TMDMIDIGenerator.generateMIDI(sheet);

// 寫入硬碟檔案
fs.writeFileSync('output.mid', Buffer.from(midiBytes));
console.log('成功生成 output.mid');
```

## 多格式生成器（Exporters）

`tmdlang` 內建豐富的匯出轉換器，可以直接將 AST 轉譯為各種主流音樂格式：

```typescript
import {
  TmdParser,
  TMDMusicXMLGenerator,
  TMDLilyPondGenerator,
  TMDABCGenerator,
  TMDReaperGenerator,
  TMDChordProGenerator,
  TMDWAVRenderer
} from 'tmdlang';

const sheet = TmdParser.parse(tmdCode);

// 1. MusicXML 4.0 (字串，可交給 MuseScore / Finale)
const xmlString = TMDMusicXMLGenerator.generateMusicXML(sheet);

// 2. LilyPond 原始碼 (字串，可排版高解析度向量樂譜)
const lilyPondSource = TMDLilyPondGenerator.generateLilyPond(sheet);

// 3. ABC Notation (字串)
const abcString = TMDABCGenerator.generateABC(sheet);

// 4. REAPER 專案檔 (.rpp，包含段落 Marker 與速度軌)
const rppProject = TMDReaperGenerator.generateRPP(sheet);

// 5. ChordPro 和弦導航譜
const chordProLeadSheet = TMDChordProGenerator.generateChordPro(sheet);

// 6. 16-bit 44.1kHz 雙聲道 WAV 音訊（內建軟體合成器直接算聲波）
const wavBytes: Uint8Array = TMDWAVRenderer.renderWAV(sheet);
```

## Model Context Protocol (MCP) 伺服器

`tmdlang` 原生實作了標準的 MCP 協定，可以讓任何支援 MCP 的 AI Agent（如 Claude Desktop、Cursor、Windsurf、Gemini CLI）直接獲得音樂編譯、語法校驗與格式轉換能力。

### 一鍵自動註冊

在命令列執行：

```bash
npx tmdlang --install-mcp
```

指令會自動偵測 Claude Desktop、Cursor 與 Gemini 設定檔，並寫入啟動配置。

### 手動設定範例（Claude Desktop / Cursor）

在你的 `claude_desktop_config.json` 或 Cursor `mcp.json` 中加入：

```json
{
  "mcpServers": {
    "tmd": {
      "command": "npx",
      "args": ["-y", "tmdlang", "--mcp"]
    }
  }
}
```

### 提供之 MCP Tools

MCP 伺服器掛載後，AI Agent 可呼叫以下 4 個工具：

| 工具名稱 | 說明 | 輸入參數 |
| :--- | :--- | :--- |
| `get_tmd_skill` | 取得 TMD 語法規範與 Prompt 編寫指引 | 無 |
| `parse_tmd` | 解析 TMD 樂譜，回傳速度、調性、拍號與段落元數據 | `text` 或 `filePath` |
| `check_tmd` | 校驗小節拍數、節奏完整度與播放流程，回報具體錯誤小節 | `text` 或 `filePath` |
| `convert_tmd` | 將樂譜轉換為指定目標格式（`midi`, `musicxml`, `wav`, `reaper` 等） | `text`, `format`, `outputPath` |

## 語言伺服器協定（LSP）支援

若要在自訂編輯器（如 Neovim、Sublime Text、Emacs）中使用 TMD 語法診斷與補全：

```bash
# 透過 stdio 啟動 JSON-RPC LSP 服務
npx tmdlang lsp
```
