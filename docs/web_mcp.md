# Web MCP 設定

Model Context Protocol（MCP）是由 Anthropic 發起的開放標準協議，允許外部 AI 代理程式（如 Claude Desktop、Cursor、Gemini CLI 等）掛載並呼叫各種專屬工具。

除了命令列本機的 stdio MCP 外，**TMD Studio 網頁版**（[https://tmdlang.github.io/Tmd-TS/](https://tmdlang.github.io/Tmd-TS/)）內建了 **Web MCP** 服務。透過 WebMCP 連線，外部的 AI 代理程式可以直接與瀏覽器中的 TMD Studio 編輯器連線，讓 AI 不僅能檢驗與轉檔樂譜，還能**直接讀取與寫入網頁編輯器並觸發即時播放**！

## Web MCP 開放之工具清單

TMD Studio 在瀏覽器端註冊了以下 6 項標準工具：

| 工具名稱 | 說明 | 輸入參數 | 回傳內容 |
| :--- | :--- | :--- | :--- |
| `getTmdSkill` | 取得 TMD 語法規格、樂理記譜指引與 Prompt 編寫手冊 | 無 | 完整的繁體中文/英文 Prompt 提示詞指引（Markdown） |
| `parseTmd` | 解析 TMD 樂譜，回傳曲名、BPM、調性、拍號、樂軌摘要與語法錯誤 | `text` (樂譜字串) | 結構化 JSON 元數據或語法診斷訊息 |
| `checkTmd` | 靜態校驗每一小節的時值與拍數一致性 | `text` (樂譜字串) | 驗證通過狀態（`valid: true`）或具體出錯的小節、行號與相差拍數 |
| `convertTmd` | 將樂譜轉換為指定目標格式 | `text`, `format` (`midi`, `musicxml`, `lilypond`, `abc`, `reaper`, `chordpro`) | 轉換後的字串或 Base64 編碼（MIDI） |
| `getCurrentScore` | 取得目前 TMD Studio 編輯器中開啟的樂譜內容 | 無 | 編輯器中的完整 TMD 純文字 |
| `loadScoreToEditor` | 將樂譜文字推入 TMD Studio 編輯器中，並可選擇是否立即播放 | `text` (樂譜字串), `play` (布林值，可選) | 載入成功訊息 |

## 連線方式與操作步驟

WebMCP 的架構是由外部的 **AI 客戶端 / 瀏覽器外掛（如 [Model Context Tool Inspector](https://chromewebstore.google.com/detail/webmcp-model-context-tool/gbpdfapgefenggkahomfgkhfehlcenpd) 或 [WebMCP Gateway](https://github.com/salespeak-ai/webmcp-gateway)）** 與網頁端的 **WebMCP Widget** 透過 WebSocket 建立端對端安全連線。

因此在連線前，您通常需要先在瀏覽器安裝支援 WebMCP 規範的擴充功能（或啟動本機的 WebMCP Proxy），以取得配對用的連線憑證（Connection Token）。

![WebMCP 連線小工具](./img_zh/tmd_studio.png)

### 步驟 1：取得連線憑證（Connection Token）

1. 在您的瀏覽器擴充功能（如 WebMCP Inspector）或本機 AI 代理客戶端中開啟 WebMCP 連線頁面。
2. 系統會產生一組專屬的配對 Token（通常為一串 Base64 或 JSON 格式的連線字串，包含 Gateway WebSocket 伺服器網址與一次性密鑰）。
3. 複製該組 Token。

### 步驟 2：開啟 TMD Studio 的 WebMCP 面板

1. 在瀏覽器中打開 TMD Studio（[https://tmdlang.github.io/Tmd-TS/](https://tmdlang.github.io/Tmd-TS/)）。
2. 點擊畫面右下角的 **WebMCP 藍色圖示** 展開連線設定面板。面板中會列出已就緒的 6 個工具（`getTmdSkill`、`parseTmd`、`checkTmd`、`convertTmd`、`getCurrentScore`、`loadScoreToEditor`）。

### 步驟 3：貼上 Token 並連線

1. 在面板的 **Paste connection token** 輸入框中貼上剛才複製的 Token。
2. 點擊 **Connect**。

連線建立後，Widget 狀態燈號將轉為綠色，外部 AI Agent 即可透過 WebSocket 通道直接呼叫上述工具，與當前網頁編輯器進行即時互動。

> [!TIP]
> 為了兼顧安全與瀏覽器資源，連線在閒置 5 分鐘後會自動中斷。若需繼續使用，點擊 Connect 即可重新連線。

## 與 Claude Desktop / Cursor 比較

TMD 提供了兩種 MCP 運作型態，您可以依照工作情境自由選擇：

| 比較維度 | 本機命令列 MCP (`tmd --mcp`) | 瀏覽器 Web MCP (TMD Studio) |
| :--- | :--- | :--- |
| **運作環境** | 本機 Node.js / CLI (stdio 管道) | 瀏覽器 JavaScript (WebSocket / DOM) |
| **適用客戶端** | Claude Desktop、Cursor、Windsurf、Gemini CLI | 網頁 AI 助手、瀏覽器代理人、WebMCP Client |
| **安裝門檻** | 需要本機安裝 `npm install -g tmdlang` 或 `brew` | **零安裝**，只要打開瀏覽器網址即可使用 |
| **編輯器互動** | 讀寫本機硬碟檔案（`.tmd`, `.mid` 等） | **直接讀寫網頁編輯器畫面**，支援網頁端即時播放 |
| **音訊預覽** | 依賴本機音訊渲染為 `.wav` 檔案 | 瀏覽器內建 Web Audio 合成器直接發聲試聽 |

## 實戰範例：AI 代理人透過 Web MCP 協同創作

當 AI Agent 連上 TMD Studio 的 Web MCP 後，典型的對話流程如下：

```
人類：「請幫我看看目前編輯器裡的歌，幫我檢查有沒有拍數錯誤，然後把和弦換成爵士風格並試聽。」
```

AI Agent 在背後的運作：

1. 呼叫 `getCurrentScore()` 讀取網頁目前內容。
2. 呼叫 `checkTmd({ text })` 檢驗現有小節拍數。
3. 根據音樂理論修改 `CHORD` 軌道，換上副屬和弦與九和弦（如 `[Cmaj9]`、`[Am7]`、`[2m7-5]`）。
4. 再次呼叫 `checkTmd({ text: newText })` 確認修改後的拍數完全吻合。
5. 呼叫 `loadScoreToEditor({ text: newText, play: true })` 將新譜即時寫入網頁並自動開起試聽！
