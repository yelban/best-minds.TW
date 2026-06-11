# Best Minds

> "Don't think of LLMs as entities but as simulators."
> — Andrej Karpathy

一個 Claude Code Skill / Plugin，實作「模擬器思維」方法論：不問 AI「你怎麼看」，改問「**世界上哪群人最適合探討這個？他們會怎麼說？**」

## 核心理念

LLM 沒有「自己的看法」。當你問「你怎麼看」，得到的是 finetuning 資料統計拼出的預設助理人格——中庸、傾向附和的共識答案。模擬器思維改為指定真實人物的視角組合，分聲部模擬，最後收斂成建議。

| 傳統問法 | Best Minds 問法 |
|---------|----------------|
| 「你怎麼看？」 | 「世界上哪群人最適合探討這個？他們會怎麼說？」 |
| 預設助理人格的共識答案 | 多位真實人物的視角碰撞 + 收斂綜合 |

它的價值不在「答得更好」，而在：

1. **視角多元** — 取出模型內部有稜角、彼此衝突的真實觀點
2. **反諂媚** — 預設助理人格傾向附和；模擬的真實人物批評起來毫無顧忌
3. **定位分歧** — 專家們意見相左之處，正是資訊量所在

## 這不是 expert roleplay

Karpathy 在原 thread 特別澄清：

> "I am not suggesting people use the old style prompting techniques of 'you are an expert swift programmer' or etc. it's ok."

「你是世界級專家」那種舊式提示在現代前沿模型上已無品質增益。本 skill 不是在答案前貼專家頭銜，而是讓每個聲部呈現那個人獨有的思考框架與立場——包括反對你前提的地方。

## 安裝

### 方式一：Claude Code Plugin（建議）

```
/plugin marketplace add yelban/best-minds.TW
/plugin install best-minds@best-minds
```

### 方式二：skills CLI

```bash
npx skills add yelban/best-minds.TW
```

### 方式三：手動

```bash
git clone https://github.com/yelban/best-minds.TW.git
ln -s "$(pwd)/best-minds.TW/skills/best-minds" ~/.claude/skills/best-minds
```

## 使用方式

在對話中使用觸發詞：

- 最強大腦
- 頂級專家
- 世界級
- best minds
- 誰最懂這個

### 範例

```
「LLM agent 的記憶系統該怎麼設計？誰最懂這個？」
→ 組一個視角面板（如 Andrej Karpathy、Richard Sutton、Michael Stonebraker），
  分聲部模擬各自的主張與分歧，最後收斂成建議

「我想辭職創業，請用頂級專家的視角給建議」
→ 可能模擬 Paul Graham，並刻意放一位會唱反調的風險視角（如 Nassim Taleb），
  而不是一面倒的鼓勵
```

人選一律用有公開言論記錄的真實人物（extract，不 invent）——捏造的「一位資深╳╳」會向刻板印象與正向偏誤漂移。

## 方法論源流

源自 [Andrej Karpathy 2025 年的推文](https://x.com/karpathy/status/1997731268969304070)及其[澄清推文](https://x.com/karpathy/status/1998245684521353664)，上溯至 2023 年《[State of GPT](https://www.youtube.com/watch?v=bZQun8Y4L2A)》演講，下接 2026 年的 population simulation 發展與社群實證檢驗（假共識、身分扁平化等失效模式，已內建為 skill 護欄）。完整演化線詳見 [docs/origin.md](docs/origin.md)。

## 授權

MIT License
