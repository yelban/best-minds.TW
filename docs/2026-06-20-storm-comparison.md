# STORM 對照研究：推文是劣化轉述，best-minds 比想像中更接近正源

> 「我建議提取提示詞組合做成適合自己的 SKILL.md 讓 agent 執行，這點原作者沒有提到。」
> — LinearUncle，轉推 Nav Toor 的 STORM 推文

2026-06-20，吹吹丟來一篇爆紅推文（Nav Toor 的「Stanford STORM Method：4 個 prompt 讓 Claude 像博士做研究」）、一個別人據此推文做的 skill（`multi-lens-research`），以及 STORM 的 Stanford 原始倉庫，問：有沒有可以增強 best-minds 的高價值知識？

本文記錄這次對照研究的完整發現。一句話結論：**那篇推文是 STORM 的劣化轉述，砍掉了 STORM 真正的兩個創新；而 best-minds 的「選人」其實已經比照推文做的 multi-lens 更接近真正的 STORM。** 但 STORM 與 Co-STORM 的原始碼裡，仍有三個 best-minds 缺的高價值機制值得借鏡。

## 一、四個對照對象

| 對象 | 是什麼 | 性質 |
|---|---|---|
| Nav Toor 推文 | 「STORM = 5 個固定角色 + 4 個 prompt」 | 二手轉述，爆紅 |
| `multi-lens-research` | 把推文工程化成 Coordinator + 5 並行 subagent + 4 phase | 忠實實作推文版 |
| STORM 原始倉庫 | NAACL 2024 Stanford OVAL，`knowledge_storm` Python 套件 | 一手正源 |
| `best-minds.TW` | Karpathy 模擬器思維的 skill | 本專案 |

## 二、核心發現：推文砍掉了 STORM 真正的創新

讀 `knowledge_storm/storm_wiki/modules/persona_generator.py` 與 `knowledge_curation.py` 後確認，真正的 STORM 有兩個推文完全沒提的機制：

### 機制一：視角是「探勘」出來的，不是固定清單

推文版說 STORM = 5 個寫死角色（實踐者／學者／懷疑者／經濟學家／歷史學家）。**錯。**

真正的 STORM（`persona_generator.py` 的 `CreateWriterWithPersona`）：

1. `FindRelatedTopic` — 先找與主題相似的維基頁面
2. 讀那些頁面的**目錄結構**（table of contents）——看「這類主題通常從哪些維度被組織」
3. `GenPersona` — 據此生成視角，每個視角綁定一個真實存在的內容維度

換言之，**視角從主題鄰域探勘而來**，隨題目變化。固定 5 角是推文的簡化。

### 機制二：模擬對話有檢索接地，且「查不到不准編」

推文版是 5 個角色各說一段，純參數記憶。真正的 STORM（`knowledge_curation.py`）是多輪 writer↔expert 對話：

- `AskQuestionWithPersona` → `QuestionToQuery` → 真的去檢索 → `AnswerQuestion`
- 關鍵註解：`"When no information is found, the expert shouldn't hallucinate."`
- writer 會根據 expert 的回答**追問**（follow-up），不是單輪

retrieval grounding + 追問，是 STORM 論文聲稱「比直接問問題更深更廣」的真正來源。推文把這塊也砍了。

### Co-STORM（EMNLP 2024）再加三件

讀 `collaborative_storm/modules/grounded_question_generation.py`：

- **Moderator**：用 **unused_snippets**（檢索到但對話中沒被引用的資訊）生成下一個問題——系統性地把「圓桌的集體盲區」逼出來。論文 Section 3.5 的核心。
- **動態 mind map**：把蒐集的資訊組織成階層概念結構，降低長對話的心智負荷
- **turn management policy**：管理多 agent 發言順序

## 三、四方機制對照

| 機制 | 真 STORM / Co-STORM | 推文 / multi-lens | best-minds（改版前） |
|---|---|---|---|
| 視角從哪來 | **探勘**相似主題維度 | 5 個**寫死**角色 | 依問題**選人** ✅ 接近正源 |
| 接地 | 每輪**檢索接地**，查不到不編 | 純參數記憶 ❌ | 引言查證（部分接地） |
| 對話結構 | 多輪追問 + 交鋒 | 單輪各說各話 ❌ | 兩階段（獨立→交鋒）✅ |
| 盲區挖掘 | Moderator 用未用資訊挖盲區 | Phase 2 第 5 條（靜態問一次） | **無** ❌ |
| 自我批判 | （STORM 已知弱點，論文自陳） | Phase 4 peer review ✅ | 已知失效模式 + 歸屬誠實 |

## 四、兩個反直覺的對照結論

### best-minds 已經比 multi-lens 更接近真 STORM

這是最諷刺的發現。multi-lens 忠實實作了推文，結果繼承了推文的劣化——退回 Karpathy 明確警告的「固定角色 roleplay」。而 best-minds 的「依問題選人」對應 STORM 的 perspective discovery，「兩階段交鋒」對應 STORM 的 simulated conversation。**best-minds 走的是正源的路，multi-lens 走的是二手簡化的路。**

### 「圓桌」是 STORM 的原生術語

上一輪（v2.0.4）把「面板」改「圓桌」純粹基於台灣語感。對照原始碼才發現：Co-STORM 程式碼**通篇用 `roundtable conversation`**（`KnowledgeBaseSummmary`、`ConvertUtteranceStyle`、`GroundedQuestionGeneration` 的 docstring）。獨立選到了 Stanford 的原生詞。

## 五、採納的三個高價值機制（→ v2.1.0）

### 1. 盲區掃描（借 Co-STORM Moderator）— 最高價值

best-minds 改版前只找「被選中的人之間」的分歧，**完全沒有機制問「所有人都沒談到什麼」**。這正是 Co-STORM Moderator 的全部存在理由，也和 best-minds 的反諂媚主軸同源——圓桌的集體共識也可能是集體盲點。

落實：
- 流程第 5 步「收斂綜合」新增**必含盲區掃描**：「這群人共同沒談到什麼？選人本身暴露了哪個全體預設？」並區分「假共識」（抹平存在的分歧）與「集體盲區」（全體真的都沒看到）
- 進階圓桌新增**可選盲區輪**：交鋒後派一個不替任何人發言的「盲區 agent」專挑全體盲點

### 2. 檢索接地（借 STORM grounded conversation）

best-minds 改版前的「查證」只用於引言。強化為：爭議／時效題先檢索當事人**實際近期立場**再模擬，鐵則「查不到就不准編」。直接對抗已列的身分扁平化與正向漂移失效模式。落實：流程第 3 步從「查證（可選）」改為「檢索接地（爭議／時效題必做）」。

### 3. 測繪對立軸（借 STORM perspective discovery）

best-minds 改版前直接點名人，風險是選到共享同一盲點的人（如前測中 Packer + Chase 都是「賣記憶方案的人」）。落實：流程第 2 步選人前先測繪「這問題有哪些對立軸」，再一軸配一人。

## 六、採納的工程實踐：evals 持久化（借 multi-lens）

multi-lens 有 `evals/evals.json`——4 個場景 prompt + 每個一張 expectations 清單，讓行為可回歸檢查。best-minds 之前的驗收是即興的（TDD 測試、客服記憶測試）。本次把這兩次實測固化成 `evals/evals.json`，把「跑真實案例 → 對照護欄」的循環持久化。

## 七、刻意不採用的

| 不採用 | 原因 |
|---|---|
| 固定 5 角色 | 推文的劣化，違背 Karpathy 本意；best-minds 的開放選人更正確 |
| 強制永遠開 subagent | multi-lens 強制；best-minds「進階才用、可降級」對思維方法論更合適 |
| 4 phase 強制串行 + 每步使用者確認 | 對「研究報告產出」合理，但對「對話中的判斷輔助」太重；best-minds 保持輕量 |
| 場景角色模板（Paper/Code/Direction Review） | 可當可選捷徑，但優先級低——硬塞固定角色正是 best-minds 刻意避開的反模式 |

## 八、meta 教訓（已收進 origin.md）

從 STORM 借東西，要回到**一手來源（論文＋原始碼）**，不是二手推文——因為推文本身就是劣化轉述，砍掉了 grounding 與 discovery、只留角色空殼。這恰好又是 best-minds 自己的原則：**extract from real, not invent**。

整件事是 best-minds 方法論的一次自我印證：如果當初照推文做（像 multi-lens 那樣），就會把 Karpathy 警告的固定角色 roleplay 抄進來；回到正源，才看清自己原來走對了路，並補上正源裡真正值錢的三塊。

## 九、留給未來的問題

- **mind map / 階層知識庫**：Co-STORM 的動態 mind map 降低長對話心智負荷——best-minds 多輪交鋒若變長，是否需要類似的結構化記憶？目前未採用，記錄待觀察
- **盲區掃描的真實增益**：盲區輪是新增機制，實際能挖出多少「全體盲點」值得在使用中驗證，比照 Roemmele 交鋒輪的待驗證狀態
- **檢索接地的成本**：每個聲部都檢索會顯著增加 token 與延遲；「爭議／時效題必做、其餘可選」的分界是否恰當，待用例累積
