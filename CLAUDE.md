# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## 專案概述

這是一個 Claude Code Skill / Plugin，實作「模擬器思維」方法論。核心理念來自 Andrej Karpathy（2025）：把 LLM 當成模擬器，而非實體。

**用法**：不問「你怎麼看」，改問「世界上哪群人最適合探討這個？他們會怎麼說？」

**護欄**：這不是 expert roleplay。Karpathy 本人已澄清不是在建議「你是專家」式的舊提示技巧；本 skill 的價值在視角多元、反諂媚、定位專家分歧。

**語言策略（v2.2.0 起）**：`SKILL.md` 正本以**英文**撰寫，並在開頭明令「以使用者提問的語言回覆」。理由：(1) A/B 實測顯示護欄遵循度與指令語言無可靠差異；(2) 英文為國際推廣與第三語言中立性的較佳基底；(3) 顯式語言規則確保跟隨使用者語言。**注意**：「英文指令更能跟隨使用者語言」的初步實測受環境污染（subagent 繼承強制 `zh-TW` 輸出設定），C1/C2 再測未能複現，此主張**待中立環境驗證**（計畫見 docs 第九節）。維護者讀英文正本；繁中說明保留在 `docs/`。詳見 `docs/2026-06-20-language-ab-and-english-canonical.md`。

## 專案結構

```
best-minds.TW/
├── .claude-plugin/
│   ├── marketplace.json          # Marketplace 清單（source: "./"，單一 plugin repo 模式）
│   └── plugin.json               # Plugin 清單
├── skills/
│   └── best-minds/
│       └── SKILL.md              # Skill 定義檔（唯一正本，英文撰寫 + 跟隨使用者語言規則）
├── SKILL.md                      # → skills/best-minds/SKILL.md 的 symlink（舊式安裝相容）
├── evals/
│   └── evals.json                # 行為驗收契約（實測固化的護欄回歸測試）
├── docs/
│   ├── origin.md                 # 方法論源流（2023 演講 → 2025 推文 → 2026 後續 → STORM 交叉驗證）
│   ├── usage.md                  # 使用指南
│   ├── 2026-06-12-v2-revision.md # v2.0.0 修訂記錄（反思過程、Grok 查證發現、兩輪修正）
│   ├── 2026-06-20-storm-comparison.md # STORM 對照研究（推文是劣化轉述、三機制借鏡）
│   └── 2026-06-20-language-ab-and-english-canonical.md # 指令語言 A/B 實測 + 轉英文正本決策
├── README.md                     # 英文版（國際推廣，含 mermaid 流程圖）
├── README_zh-TW.md               # 繁體中文版
└── LICENSE                       # MIT License
```

## Skill 設計原則

1. **問題決定人數** — 一人夠就一人；需碰撞才組圓桌，且至少放一位可能唱反調的
2. **選探討組合，不是選頭銜** — 目標是能把問題照亮的視角組合，不是頭銜最大的人
3. **基於真實** — 模擬要基於當事人公開的思想、著作、言論
4. **引言查證** — 引用原話前先查證；查不到就明標轉述，不可捏造名言

## 修改須知

修改 `skills/best-minds/SKILL.md`（唯一正本）時，須同步更新：
1. 檔案頭部的 YAML frontmatter（name, description）
2. 檔案內的 HTML 註解（input, output, pos）
3. `.claude-plugin/marketplace.json` 與 `.claude-plugin/plugin.json` 的 version（兩處需一致）
4. 內容若涉及方法論變動，同步檢查 `README.md`（英文）、`README_zh-TW.md`（繁中）與 `docs/`
5. 改動護欄後，對照 `evals/evals.json` 的 expectations 確認行為契約未被破壞

根目錄 `SKILL.md` 是 symlink，不要直接編輯或以實體檔案覆蓋。
