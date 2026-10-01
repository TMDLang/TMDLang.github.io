# 後期製作

當你使用 TMD 與 AI 完成了樂曲的旋律、動機變奏、和弦編配與結構設計後，你在 TMD Studio 或終端機裡聽到的聲音，只是用來確認音高與小節節奏的**「草稿預聽音色」**（通常是輕量的 General MIDI 或 SoundFont）。

要讓你的作品蛻變為戴上耳機讓人起雞皮疙瘩、具備電影配樂質感或商業發行水準的真正成品，就必須將 TMD 產出的檔案送入現代數位音樂製作工業鏈中。

本章將為你梳理從 TMD 匯出到**打譜軟體（MuseScore）**、**數位音樂工作站（DAW：GarageBand / Logic / REAPER）**以及**頂級虛擬樂器音色庫**的完整後期製作流程。

## 跨平台打譜與專業排版：MuseScore 4（免費開源首選）

如果你需要印出精美絕倫的五線譜給樂手演奏、樂團排練，或者交給合唱團看譜，**MuseScore 4** 是目前的最佳選擇。

### 匯出與匯入流程

1. 在 TMD Studio 點擊 `Export` ➔ 選擇 **MusicXML 4.0 (`.musicxml`)**；或在命令列執行：

   ```bash
   tmd my_song.tmd -x my_song.musicxml
   ```

2. 開啟 MuseScore 4，點擊 `檔案` ➔ `開啟`，選取該 `.musicxml` 檔案。
3. MuseScore 會自動將 TMD 的多軌道、拍號、小節線、速度、動態強弱記號（`{p}`, `{f}`）與和弦代號完美排版為標準總譜與分譜。

### 下載 Muse Sounds 音色庫

以往免費打譜軟體的內建聲音都很機械化，但 MuseScore 4 推出了革命性的 **Muse Sounds** 模組：

- **完全免費下載**：透過 Muse Hub 可以免費取得管弦樂團全套音色（Muse Strings, Muse Brass, Muse Woodwinds, Muse Percussion, Muse Choir）。
- **驚人的真人真實感**：連圓滑奏（Legato）、連弓跳弓（Spiccato）的連貫性都處理得極度細膩，在 MuseScore 裡按下播放，立刻就能聽見逼真的交響樂團演奏。

## 蘋果生態系

如果你使用 iPhone、iPad 或 Mac，Apple 生態系提供了無縫且免費的音樂製作工作流。只要一隻手機，在通勤或沙發上就能把 TMD 轉成高品質音樂！

### 行動端：iOS / iPadOS GarageBand

在手機上打開 TMD Studio 匯出 MIDI 後，可以直接丟進 iOS 版 GarageBand：

1. **從手機瀏覽器匯出**：
   - 用 iPhone 或 iPad 打開 TMD Web Studio。
   - 點擊 `Export` ➔ **MIDI (.mid)**，檔案會存入 iOS「檔案（Files）」App 中。
2. **匯入 iOS GarageBand**：
   - 打開 iOS GarageBand，新建一個歌曲（選擇任意軟體樂器軌，例如 Keyboard）。
   - 點擊右上角的 **迴圈圖示（Loop Browser）** ➔ 切換到 **「檔案（Files）」** 分頁。
   - 點擊「從檔案 App 瀏覽項目」，選取剛才下載的 `.mid` 檔案。
   - 將該 MIDI 檔案按住不放，**直接拖曳到時間軸軌道區域**。
3. **享受 Apple 的行動聲音庫**：
   - GarageBand 會自動將多軌拆分開來。
   - 點擊軌道樂器圖示，免費下載 Apple 官方的「聲音資源庫（Sound Library）」：包含 *Toy Box*、*K-Pop*、*Flex & Flow*、*Vintage Mellotron* 等世界級音色包。
   - 搭配觸控螢幕的 **Touch Instruments** 或 **Smart Strings**，用手指輕輕一滑就能即時加上浪漫的弦樂群掃弦！

### 行動進階：Logic Pro for iPad

如果你使用 iPad，Apple 官方的 **Logic Pro for iPad** 更是全功能的觸控旗艦工作站：

- 完整繼承 Mac 版 Logic 的採樣合成器（Alchemy、Retro Synth）與專業混音台。
- 支援直接開啟 GarageBand 專案，躺在沙發上就能用 Apple Pencil 繪製表情自動化曲線與做母帶處理。

### 桌面端：macOS GarageBand ➔ Logic Pro

當你回到電腦桌前：

1. 將 TMD 匯出的 `my_song.mid` 拖進 Mac 版 GarageBand。
2. 更換高品質錄音室音色（如 *Steinway Grand Piano*、*Studio Strings*）。
3. 當需要商業級精修時，點擊選單 `檔案 ➔ 在 Logic Pro 中打開`，直接解鎖好萊塢級的空間殘響（Space Designer）與智慧母帶助手（Mastering Assistant）。

## Android：BandLab 與 FL Studio Mobile

如果你使用的是 Android 手機或平板，雖然 Android 系統沒有預載類似 GarageBand 的系統內建工具，但 Google Play 上有兩款非常成熟強大的行動音樂 App，同樣能直接吃 TMD 的 MIDI 檔案：

### 免費雲端首選：BandLab

**BandLab** 是目前 Android 上最受歡迎的免費雲端數位音樂工作站，零門檻且功能極為強大：

1. **匯入 MIDI**：在 Android 瀏覽器下載 TMD 匯出的 `.mid` 檔案 ➔ 打開 BandLab 建立新專案 ➔ 點擊 `+` 選擇 `Import Audio/MIDI` 選取檔案。
2. **自動分軌與豐富音色庫**：BandLab 會自動解析多軌，並內建超過 300 種虛擬樂器（鋼琴、吉他、打擊樂、管弦樂）與龐大的免費效果器。
3. **雲端跨平台同步**：你在 Android 手機上編排好的一半半成品，回家打開電腦瀏覽器登入 BandLab 網頁版就能無縫繼續做，甚至能一鍵分享到社群給朋友合唱或加吉他。

### 專業付費旗艦：FL Studio Mobile

如果你追求更極致的離線編曲、音源採樣與混音精準度，**FL Studio Mobile** 是 Android 生態圈的工業標準：

- **完整支援多軌 MIDI 拖曳匯入**：直接把 TMD 產出的 MIDI 檔載入音軌。
- **強大的合成器引擎（DirectWave / GMS）**：具備高傳真音質的採樣播放器與合成器，支援載入外部 SoundFont（`.sf2`）。
- **可回傳電腦端**：手機版的專案可以一鍵導入 Windows / Mac 上的 FL Studio 完整版進行最終發行級混音。

## 跨平台輕量王者：REAPER

如果你使用 Windows、Linux，或是追求極致輕量與客製化的 Mac 用戶，**REAPER** 是目前全球影視與遊戲配樂界最推崇的 DAW 之一。

### TMD 原生支援 REAPER 工程檔（`.rpp`）

TMD 具備專門針對 REAPER 的編譯器輸出：

```bash
tmd my_song.tmd -r my_song.rpp
```

或在 TMD Studio 匯出選單中點選 **REAPER Project (.rpp)**。

### 為什麼這個整合如此強大？

一般軟體匯出 MIDI 後，進 DAW 還要手動重新建立段落標記（Markers）與速度軌（Tempo Map）。但 TMD 產出的 `.rpp` 已經為你做好了所有繁瑣雜事：

- **段落標記自動就位**：TMD 裡的 `intro`、`verse`、`chorus` 在 REAPER 時間軸上方會自動轉化為彩色的段落 Marker。
- **速度與拍號地圖對齊**：樂譜中所有中途變更的速度 `{!=140}`、拍號變更全都自動寫進 REAPER 速度軌中。
- **軌道命名與 MIDI 嵌入**：打開專案，你只需要做一件事——**在軌道上掛載你喜歡的音色插件（VSTi）**！

### REAPER 播放 MIDI 的「無聲痛點」

許多人覺得 REAPER 處理 MIDI「很麻煩」，主要原因在於：**REAPER 追求極度精簡純粹，因此出廠預設「沒有內建 General MIDI (GM) 軟音源」**。在 GarageBand 或 Logic 裡把 MIDI 丟進去立刻有鋼琴、木吉他與爵士鼓發聲，但在未設定的 REAPER 裡打開，預設是完全靜音的。

要讓 REAPER 像 GarageBand 一樣一開就響，最推薦的兩種極速配搭方式：

1. **極速試聽法（掛載免費 SoundFont 播放器）**：
   - 下載免費的 SoundFont 讀取插件（例如 **Plogue sforzando** 或 **TX16Wx**）。
   - 下載高品質通用音色檔（例如免費的 *GeneralUser GS* 或 *FluidR3 GM* SoundFont）。
   - 在 REAPER 第一軌掛上 Sforzando，載入 SoundFont，此時所有 MIDI 通道就會自動依據標準 GM 樂器發出正確的聲音。
2. **終極解法：建立你的專屬「TMD 專案範本」（Project Template）**：
   - 在 REAPER 中預先建好常用的軌道架構（例如：Track 1 鋼琴掛好 Pianoteq、Track 2 弦樂掛好 BBC SO Discover、Track 3 鼓組掛好 EZdrummer）。
   - 將它存成 `File ➔ Project templates ➔ Save project as template`。
   - 未來 TMD 匯出 MIDI 時，只要在你的模板專案中直接拖入 MIDI 檔案，音色、EQ、空間殘響全部即插即用，完全不需要重複設定！

## 虛擬樂器音色庫（VSTi / Audio Units）

在 DAW 中，替換掉預設 MIDI 音色的核心武器叫做「虛擬採樣樂器（Sample Libraries）」。以下是配樂師與製作人最常用的幾款夢幻逸品：

### 管弦交響樂首選：Spitfire Audio

- **BBC Symphony Orchestra (BBC SO)**：
    - **BBC SO Discover（免費版）**：只要在官網註冊填寫問卷即可免費取得！包含倫敦 BBC 交響樂團整套編制的弦樂、木管、銅管與打擊樂，檔案極小但音質高雅乾淨，新手必備。
    - **BBC SO Core / Professional**：好萊塢與 BBC 紀錄片標準採樣，包含各麥克風擺位與全套演奏法。
- **Albion ONE**：
    - 電影預告片、動作大片與史詩配樂的工業標準，專門提供「一按鍵下去就震撼山河」的交響齊奏、重裝銅管與雷霆戰鼓。

### 鋼琴與鍵盤樂器

- **Native Instruments - The Giant / Alicia's Keys**：世界最大直立鋼琴的超低頻震顫，或是溫暖細膩的現代流行抒情平台鋼琴。
- **Modartt - Pianoteq**：極致輕量的物理建模鋼琴，不需要幾十 GB 的採樣硬碟空間，泛音共鳴極度靈動。

### 吉他與貝斯

- **Ample Sound（Ample Guitar / Ample Bass）**：精準還原木吉他掃弦律動、電吉他推弦滑音與電貝斯的放克打弦（Slap）。
- **Spectrasonics - Trilian**：全球低音貝斯的終極聖杯，包含全世界最深沉溫暖的低音大提琴與合成貝斯。

### 真實鼓組與打擊樂

- **Toontrack - Superior Drummer 3 / EZdrummer 3**：
    - 將 TMD 第 10 軌的鼓點換上世界頂級錄音室採樣的爵士鼓，包含鼓皮泛音、鼓棒敲擊邊框（Rimshot）與真實空間殘響麥克風。

## 製作人的「最後兩道工序」（The Polish）

把 MIDI 放進 DAW、換上好音色之後，為什麼有時候聽起來還是有點像「機器人在彈昂貴的樂器」？只要完成最後這兩步，音樂就會徹底活過來：

### 繪製表情控制器（MIDI CC Automation）

真實管弦樂手在演奏長音時，音量和音色會隨著呼吸有起伏，絕對不會是一條筆直的水平線。在 DAW 裡打開控制器曲線：

- **CC1 (Modulation Wheel)**：控制音色動態（從柔和變為激昂明亮）。
- **CC11 (Expression)**：控制聲部整體音量表情。
- **操作技巧**：用滑鼠在長音小節畫一個「慢起 ➔ 漸強 ➔ 漸弱」的平滑波浪，小提琴和小號立刻就會像真人一樣「在呼吸」。

### 聲場定位與立體聲空間（Panning & Reverb）

不要讓所有樂器都擠在正中央喇叭發聲：

- **依照管弦樂團真實站位調整 Panning（左右方位）**：
    - 第一小提琴在最左邊（偏左 30%～40%）。
    - 大提琴與低音大提琴在右側（偏右 30%～50%）。
    - 木管樂器置中偏左、法國號在左後方、小號長號在右後方。
    - 定音鼓在正後方。
- **掛上空間殘響（Send Reverb）**：
  在 Bus 軌道掛上一個大音樂廳（Concert Hall）殘響，把所有軌道發送一點聲音進去，讓所有樂器彷彿身處同一座空間，聲音立刻融合成渾然一體的史詩聽感！
