# 演算法生成

在生成式 AI（LLM、Diffusion）大行其道之前，電腦音樂領域早在數十年前就發展出極為嚴謹的**演算法音樂（Algorithmic Composition）**。

從巴哈時代的數學對位法、莫札特的「音樂擲骰子遊戲（Musikalisches Würfelspiel）」，到現代的細胞自動機（Cellular Automata）與馬可夫鏈（Markov Chain），音樂本質上就蘊含著高度的數學結構。

TMD 是一套純文字的音樂記譜語法，這意味著：**你不一定要依賴 AI 大模型，只要用傳統的程式語言（Python、JavaScript、Swift）或是 TMD 內建的 S-Expression 巨集，就能寫出完全確定、可控、且富有數學美感的生成音樂！**

---

## 1. 為什麼要用「傳統演算法」生成音樂？

對比當今的黑盒子 AI，傳統演算法具有三大無法取代的優勢：

1. **100% 確定性與可重現（Deterministic）**：給定相同的種子碼（Seed）或數學公式，每次產出的音符與節奏完全一致，沒有隨機幻覺。
2. **極致輕量與零成本（Zero Token Cost）**：不需要昂貴的 GPU 算力、不需要聯網、不需要呼叫任何 API，幾行程式碼就能瞬間生成幾千個小節。
3. **探索複雜的數學對位美學**：卡農（Canon）、賦格（Fugue）、碎形幾何（Fractals）、費氏數列等嚴密的音樂邏輯，用演算法實作遠比讓機率模型猜測更加精確。

---

## 2. TMD 內建的演算法引擎：S-Expression 巨集

TMD 本身就內建了一套圖靈完備的函數式宏語言（S-Expression Macro），讓你在**不寫任何 Python/JS 腳本**的情況下，直接用代數轉換語法實現古典對位法！

### 2.1 嚴格卡農對位（Canon）
在古典音樂中，卡農是指多個聲部演奏同一個主題，但各聲部在時間上依序錯開進場（如著名的帕海貝爾《D 大調卡農》）：

```tmd
::SCORE::
** Algorithmic Canon in D **
!= 60
?= D
<4/4>

/* 定義 4 小節抽象卡農主題（不綁定樂器） */
Theme {
    <4*>
    3^ 2^ 1^ 7 | 6 5 6 7 | 1^ 7 6 5 | 4 3 4 2 |
}

/* 一行指令：三把小提琴各間隔 2 小節嚴格進場，編譯器自動計算小節對齊 */
-> (canon Theme (Violin1 Violin2 Violin3) 2) ->#
```

### 2.2 數學變形運算（Inversion, Retrograde, Transpose）
TMD 巨集支援古典巴洛克與十二音技法（Serialism）的核心數學變換：

| 運算子 | 語法範例 | 數學意義 |
| :--- | :--- | :--- |
| **`transpose`（移調）** | `(transpose Theme +7)` | 所有音符音高加上 7 個半音（純五度平移）。 |
| **`reverse`（逆行）** | `(reverse Theme)` | 將旋律在時間軸上**完全倒著演奏**（倒帶）。 |
| **`flip`（倒影 / 鏡射）** | `(flip Theme)` | 以第一個音為對稱軸，向上跳音改為向下跳音（垂直鏡射）。 |
| **`minor` / `major`** | `(minor Theme)` | 將原本大調音階在自然音級上映射為平行小調。 |
| **`vary`（複合管線）** | `(vary Theme +7 reverse minor)` | 函數管線組合：移調 ➔ 逆行 ➔ 小調化。 |

### 2.3 聲部層疊與固定低音循環（Layer & Loop）
你可以將數學變形後的主題與固定低音（Ground Bass）在時間軸上重疊：
```tmd
/* 大提琴循環低音 8 次，小提琴演奏變形主題 */
-> (layer
     (loop Bass Cello 8)
     (play (vary Theme +12 reverse) Violin1)
   ) ->#
```

---

## 3. 用 Python 寫演算法生成 TMD

由於 TMD 是最純粹的文字格式，任何程式語言都可以用字串格式化直接輸出 `.tmd` 檔案，並透過 `tmd` CLI 即時編譯成 MIDI 或 WAV！

### 範例 A：費氏數列旋律生成器（Fibonacci Melody）
利用費氏數列對 7 取餘數（模除），映射到簡譜的 `1` 至 `7`，創造自然界碎形美感的旋律：

```python
# fibonacci_tmd.py
def generate_fibonacci_tmd(n_notes=32):
    a, b = 1, 1
    notes = []
    for _ in range(n_notes):
        # 費氏數列 mod 7 映射到 1-7
        degree = (a % 7) + 1
        notes.append(str(degree))
        a, b = b, a + b

    # 將音符按 4 拍一小節排版
    bars = [" ".join(notes[i:i+4]) for i in range(0, len(notes), 4)]
    score_body = " | ".join(bars)

    tmd_content = f"""\
::SCORE::
** Fibonacci Algorithmic Music **
!= 120
?= C
<4/4>

melody:Marimba@|0|{{
    <4*>
    | {score_body} |
}}

-> melody ->#
"""
    with open("fibonacci.tmd", "w", encoding="utf-8") as f:
        f.write(tmd_content)
    print("fibonacci.tmd 生成成功！")

generate_fibonacci_tmd()
```

執行後一鍵試聽或匯出：
```bash
python fibonacci_tmd.py
tmd fibonacci.tmd --play        # 終端機即時試聽
tmd fibonacci.tmd -m fib.mid     # 匯出標準 MIDI
```

---

### 範例 B：馬可夫鏈和弦進行生成（Markov Chain Harmonies）
如果你想要創作具有某種風格（如爵士或 J-Pop）、但每次都有不同可能性的和弦，可以使用**一階馬可夫轉移矩陣**：

```python
import random

# 定義流行音樂中常見的和弦轉移機率
transitions = {
    "[1]":   ["[6m]", "[4]", "[2m7]"],
    "[6m]":  ["[4]", "[2m7]", "[5]"],
    "[4]":   ["[5]", "[1]", "[2m7]"],
    "[5]":   ["[1]", "[6m]"],
    "[2m7]": ["[5]", "[4]"]
}

def generate_markov_chords(n_bars=8):
    current = "[1]"
    chords = [current]
    for _ in range(n_bars - 1):
        current = random.choice(transitions[current])
        chords.append(current)
    
    tmd_bars = "\n    ".join([f"{c} - - -" for c in chords])
    return f"""\
::SCORE::
** Markov Chain Chords **
!= 90
?= F
<4/4>

harmony:Piano@|0|{{
    <4*>
    {tmd_bars}
}}

-> harmony ->#
"""

with open("markov.tmd", "w") as f:
    f.write(generate_markov_chords(16))
```

---

### 範例 C：康威生命遊戲生成節奏打擊樂（Cellular Automata Drums）
將康威生命遊戲（Conway's Game of Life）的二維網格細胞生滅狀態，投影成 TMD 第 10 軌的爵士鼓打擊節奏：
- 細胞存活 ➔ 輸出大鼓 `B` 或小鼓 `S`。
- 細胞死亡 ➔ 輸出閉合鈸 `X` 或休止符 `0`。
- 每一代細胞演化 ➔ 生成下一小節的切分節奏，產生永不重複、卻又具備自相似有機律動的極致實驗電子樂！

---

## 4. 演算法音樂與 AI 的雙劍合璧

最頂級的現代音樂製作，往往不是「非此即彼」，而是將**演算法的精準度**與**AI 的感性理解**相互融合：

```text
┌─────────────────────────────────┐
│ 數學演算法 (Python / S-Expr)    │ ───► 產生結構嚴密的對位骨架、碎形動機
└─────────────────────────────────┘
                 │
                 ▼
┌─────────────────────────────────┐
│ 大語言模型 (LLM / Chat AI)      │ ───► 潤飾人性化過門、調整配器、修飾表情
└─────────────────────────────────┘
                 │
                 ▼
┌─────────────────────────────────┐
│ TMD 編譯器與 DAW 後期混音       │ ───► 驗證小節拍數、上高階音色庫完成製作
└─────────────────────────────────┘
```

1. **用演算法生骨架**：用費氏數列或馬可夫矩陣算出 64 小節複雜的抽象卡農或和弦框架。
2. **交給 AI 做情緒渲染**：將產生的 TMD 文字丟給 Claude / ChatGPT：「這是我用演算法生成的對位骨架，請幫我加上浪漫主義風格的力度記號 `{p}`、`{f}`，並在空隙補上弦樂長音襯底。」
3. **品管驗收**：用 `tmd check` 確認小節長度，最後匯入 Logic / Cubase 混音。

這展現了純文字記譜的終極魅力：**無論是寫程式的駭客、研究數學的學者、還是手握 Prompt 的創作者，TMD 都是你們最通用的音樂母語！**
