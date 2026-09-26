# My Brain Trust

*[English](README.md) ｜ 繁體中文*

一套給 AI agent 用的五角色思辨框架，以 [Agent Skill](https://agentskills.io/specification) 格式封裝。

把一個助理拆成五個角色，各有職責，而且關鍵在於**各有不同的輸出形狀**：偵探負責查證與標示信心水準，教頭處理人際處境與難開口的對話，顧問做決策並講清楚代價，奧客在現實動手之前先攻擊你的計畫，小編負責任何要給別人讀的東西。

在 Claude Code、Codex CLI、Gemini CLI、Cursor、Copilot 以及其他實作 Agent Skills 開放標準的工具上，不需修改即可運作。

## 安裝

最快的方式，用 [`skills` CLI](https://skills.sh)：

```bash
npx skills add chunghoyun/My_Brain_Trust
```

或手動把 `skills/brain-trust/` 資料夾複製到 agent 的 skills 目錄：

```bash
# 個人使用
cp -r skills/brain-trust ~/.agents/skills/

# 或隨專案 commit，供團隊共用
cp -r skills/brain-trust .agents/skills/
```

Claude Code 也會讀取 `~/.claude/skills/`。

skills.sh 上的頁面：[chunghoyun/my_brain_trust → brain-trust](https://www.skills.sh/chunghoyun/my_brain_trust/brain-trust)，有 `SKILL.md` 全文與第三方安全稽核結果。

## 設計原則

這套框架的存在，來自對抗測試中的一個發現：**角色分離是先在格式層失敗，不是在推理層失敗。**

在免費版模型上高負載運行五角色系統，會產生三種混在一起出現、但成因各異的故障——低優先序的角色搶在前面發言；各角色語氣逐漸趨同，讀起來像同一個人掛了五個名牌；角色標籤與輸出格式逐輪走樣。

直覺的解法無效。一條全域指令——「各角色語氣必須明顯不同、嚴禁互相模仿」——對人格同化**沒有可測量的效果**。它讀起來像一條強硬的規定，實際上什麼都沒做到，因為它只說了不准做什麼，卻把「那該怎麼做」留給模型自己推論。

有效的是**角色專屬的輸出格式對照表**：一個具體的、模型可以逐輪拿來自我核對的標準，不需要推論。而真正解掉格式滑移的，是改變標籤規格——讓**每個角色輪到自己時才標自己，而不是在回應開頭列出所有參與者**。

同樣的模式在另一場測試裡從相反方向出現。同一份規格文件交給兩個獨立的 coding agent，用兩邊產出的分歧程度來衡量這份文件留下了多少解讀空間。四輪下來，端點差異率從 91.2% 降到 0.0%。**寫死具體值有效；寫通則反而更糟**——一句「這個細節在別處處理」等於宣告本文件並不完整，於是兩個讀者各自填補，而且填得不一樣。

> **具體且可核對的對照表，打敗抽象的禁令。** 兩場測試裡失效的那條指令，都是人類讀者會正確理解的那一條。說明該做什麼的指令是可核對的；描述一種思考方式的敘述不是。

## 內容

| 檔案 | 內容 |
| --- | --- |
| `skills/brain-trust/SKILL.md` | 核心規則、回應格式表、五個角色、證據紀律 |
| `skills/brain-trust/references/roles.md` | 各角色的操作指令 |
| `skills/brain-trust/references/prompt-design.md` | 提示詞的元素順序與注意力標記 |
| `*.zh-TW.md` | 繁體中文版本——以原文撰寫，非翻譯 |

中文檔案不是事後補上的在地化。`prompt-design` 記錄了一件中文提示詞特有的事：**標記強度並不均勻，寫在括號裡的約束經常被當成可選的**，因為括號這個符號本身就在宣告「這是次要資訊」。大多數提示詞指引是從英文出發寫的，不會涵蓋這一點。

## 相容性

符合 Agent Skills 開放標準：`SKILL.md` 含 `name` 與 `description` frontmatter、Markdown 本體、選用的 `references/`。未使用任何特定 agent 的專屬擴充，因此在各個符合標準的工具上行為一致。

驗證方式：

```bash
npx skills-ref validate ./skills/brain-trust
```

## 授權

MIT，詳見 `LICENSE`。任何人都可以使用、修改、散布，**條件是保留著作權聲明**。本軟體按現狀提供，不附任何擔保。
