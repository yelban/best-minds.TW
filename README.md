# Best Minds

> "Don't think of LLMs as entities but as simulators."
> — Andrej Karpathy

一個 Claude Code Skill，實作「模擬器思維」方法論。

## 核心理念

不要問 AI「你怎麼看」。

要問：**這個問題，世界上誰最懂？TA 會怎麼說？**

然後讓 AI 模擬那個人。

| 傳統問法 | Best Minds 問法 |
|---------|----------------|
| 「你怎麼看？」 | 「世界上誰最懂這個？TA 會怎麼說？」 |
| AI 用自己視角回答 | AI 模擬頂級專家回答 |

## 使用方式

在對話中使用觸發詞：

- 最強大腦
- 頂級專家
- 世界級
- best minds
- 誰最懂這個

### 範例

```
「關於量子計算的未來，誰最懂這個？」
→ AI 會找出該領域最強專家（如 John Preskill），模擬其觀點回答

「我想創業，請用頂級專家的視角給建議」
→ AI 可能模擬 Paul Graham、Marc Andreessen 等人
```

## 安裝

```bash
npx skills add yelban/best-minds.TW
```

或手動安裝：

```bash
cd ~/.claude/skills/
git clone https://github.com/yelban/best-minds.TW.git best-minds
```

## 原則

1. **問題決定人數** — 一人夠就一人，需碰撞才多人
2. **找真正最懂的** — 不是「合適的」，是「最強的」
3. **基於真實** — 模擬要基於 TA 公開的思想、著作、言論
4. **引用原話** — 儘可能用 TA 說過的話

## 發想來源

源自 [Andrej Karpathy 的推文](https://x.com/karpathy/status/1816531576228053133)，詳見 [docs/origin.md](docs/origin.md)。

## 授權

MIT License
