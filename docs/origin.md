# 發想來源

## Andrej Karpathy 的 LLM 模擬器觀點（2025）

這個 Skill 的核心理念源自 Andrej Karpathy 2025 年 12 月在 X 上的一串推文。

### 原始推文

> "Don't think of LLMs as entities but as simulators. For example, when exploring a topic, don't ask:
>
> 'What do you think about xyz'?
>
> There is no 'you'. Next time try:
>
> 'What would be a good group of people to explore xyz? What would they say?'"
>
> — [@karpathy, 2025-12](https://x.com/karpathy/status/1997731268969304070)

核心論點：

1. **LLM 是模擬器，不是實體** — 它沒有像人類那樣長期思考一個主題、逐漸形成自己觀點的過程
2. **沒有真正的「你」** — 問「你怎麼看」，是強迫它採用 finetuning 資料統計所暗示的人格向量，得到的是某種「平均觀點」
3. **正確用法是指定視角組合** — 問「哪群人最適合探討這個？他們會怎麼說？」，讓模擬器發揮多視角能力

### 澄清推文（同串 thread）

> "A good chunk of people misunderstood this tweet btw, which is my bad. I am not suggesting people use the old style prompting techniques of 'you are an expert swift programmer' or etc. it's ok."
>
> — [@karpathy](https://x.com/karpathy/status/1998245684521353664)

這則澄清是本 skill 的關鍵護欄：**模擬器思維不是 expert roleplay**。重點是視角的多元探索，不是給答案掛專家頭銜。

## 上溯：2023《State of GPT》的演化線

Karpathy 在 2023 年 Microsoft Build 的《[State of GPT](https://www.youtube.com/watch?v=bZQun8Y4L2A)》演講中說過：

> "LLMs don't want to succeed, they want to imitate. You want to succeed, and you should ask for it."
> "Say something like, you are a leading expert on this topic. Pretend you have IQ 120..."

當年的邏輯：訓練資料裡好壞解答混雜，模型預設模仿全部，所以用「你是專家」把它調到分布的高品質區段——這是**品質增益技巧**。

到了 2025 年，前沿模型預設輸出已在高品質區段，這個技巧失效（"it's ok"）。但「模擬器」的本體論依然成立，框架轉為**認識論工具**：

| | 2023 expert prompting | 2025 模擬器思維 |
|---|---|---|
| 目的 | 提升單一答案品質 | 探索多元真實視角 |
| 形式 | 「你是專家」頭銜 | 「哪群人最適合探討？」圓桌 |
| 現狀 | 已無增益，被本人撤回 | 本 skill 實作的方法論 |

## 2026 的後續發展

### Karpathy 本人的延伸

**本體論修正**：在原 thread 的[一則回覆](https://x.com/karpathy/status/1997759548543947249)中，Karpathy 承認 RLHF/SFT 確實造出了一個工程化的「you」（複合人格），但它是後天拼裝（bolt-on）的，不是像人一樣隨時間建構的心智；在不可驗證的領域（觀點、價值判斷）尤其難以判斷該給它多少可信度。所以「沒有那個你」不是絕對陳述，而是「那個你不是你以為的那種你」。

**從圓桌到母體**：2026 年 2 月，Karpathy [投資並公開支持](https://x.com/karpathy/status/2022041235188580788) population simulator 新創 Simile：

> "Usually, the LLMs you talk to have a single, specific, crafted personality. But in principle, the native, primordial form of a pretrained LLM is that it is a simulation engine trained over the text of a highly diverse population of people on the internet. Why not lean into that statistical power: Why simulate one 'person' when you could try to simulate a population? ... How do you manage its entropy? How faithful is it?"

模擬器思維的「正史」方向：從單一人物 → 視角圓桌 → 母體模擬，而 entropy 管理與保真度是公認的未解問題。

### 理論限定：janus 的澄清

《Simulators》原作者 janus（@repligate）在 [2026 年 4 月澄清](https://x.com/repligate/status/2039857049312612571)：Simulators 理論針對的是 **base model**；RL 後訓練會打破天真的模擬器性質，產出的 character「常無法化約為訓練資料的線性總和」。對本 skill 的意義：你操作的是「對齊後的模擬器」，預設助理人格是真實存在的引力井，這正是需要明確指定視角組合來脫離它的原因。

### 社群檢驗：支持與反例

- **persona 不提升準確率**（Ethan Mollick 等，[SSRN 論文](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=5879722)）：「你是偉大物理學家」對事實準確率幾乎無顯著提升——佐證本 skill「價值不在答得更好」的護欄
- **衝突勝過中性圓桌**（[Brian Roemmele 自述實驗](https://x.com/BrianRoemmele/status/1998068828295877011)，12,400 條推理 trace、六個模型，未經同儕審查）：盲評中「5–7 個立場強烈衝突的人格鏈式辯論」（8.72/10）顯著勝過「中性專家圓桌」（6.81）與「無人格」（5.94）。乾淨圓桌易落入低能量、表面平衡的盆地——本 skill「兩階段圓桌」的交鋒輪即源於此
- **模擬社會的失效模式**（NeurIPS 2025 相關研究，經 [@koylanai 整理](https://x.com/koylanai/status/1999192104850133146)）：
  1. **假共識** — 多 agent 模擬收斂到訓練資料中位數，少數派觀點被平均掉
  2. **身分扁平化** — 模擬退化成刻板印象，「真實利害關係人豐富的立場知識被換成單體化、去脈絡的模擬」；應追求 reasoning fidelity 而非 speech simulation
  3. **合成人格系統性偏移** — 憑空捏造的人格有正向漂移與政治傾向漂移；「extract from real，不要 invent」

  本 skill 的「已知失效模式」與「基於真實，extract 不 invent」原則即來自這組發現
- **倫理警示**：2025–2026 多起關於 AI 模擬真實人物（尤其已故者）的爭議——同意權、哀悼操縱、冒充背書。本 skill 限定為「模擬公眾人物的公開思想立場、全程標明模擬」，與 likeness 複製、griefbot 劃清界線
