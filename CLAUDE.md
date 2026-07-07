# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## 專案概述

這是一個 Claude Code Skill / Plugin，實作「模擬器思維」方法論。核心理念來自 Andrej Karpathy（2025）：把 LLM 當成模擬器，而非實體。

**用法**：不問「你怎麼看」，改問「世界上哪群人最適合探討這個？他們會怎麼說？」

**護欄**：這不是 expert roleplay。Karpathy 本人已澄清不是在建議「你是專家」式的舊提示技巧；本 skill 的價值在視角多元、反諂媚、定位專家分歧。

**語言策略（v2.2.0 起）**：`SKILL.md` 正本以**英文**撰寫，並在開頭明令「以使用者提問的語言回覆」。理由：(1) A/B 實測顯示護欄遵循度與指令語言無可靠差異；(2) 英文為國際推廣與第三語言中立性的較佳基底；(3) 顯式語言規則確保跟隨使用者語言。**驗證狀態**：「英文指令更能跟隨使用者語言」初步實測受環境污染，後於中立環境（移除 zh-TW 輸出設定 + global 繁中偏好）**完整驗證**：英文正本在中／英／日三語下均正確跟隨；先前的英→繁中為環境污染所致，非規則失效。維護者讀英文正本；繁中說明保留在 `docs/`。詳見 `docs/2026-06-20-language-ab-and-english-canonical.md` 第八～十節。

## 專案結構

```
best-minds.TW/
├── .claude-plugin/
│   ├── marketplace.json          # Marketplace 清單（source: "./"，單一 plugin repo 模式）
│   └── plugin.json               # Plugin 清單
├── skills/
│   └── best-minds/
│       └── SKILL.md              # Skill 定義檔（唯一正本，英文撰寫 + 跟隨使用者語言規則）
├── .agents/
│   └── plugins/
│       └── marketplace.json      # Codex marketplace 清單（指向 ./plugins/best-minds）
├── plugins/
│   └── best-minds/
│       ├── .codex-plugin/
│       │   └── plugin.json       # Codex CLI plugin 清單（雙平台發佈）
│       └── skills/
│           └── best-minds/
│               └── SKILL.md      # Codex 平台變體（僅平台機制不同，方法論跟隨正本）
├── SKILL.md                      # → skills/best-minds/SKILL.md 的 symlink（舊式安裝相容）
├── evals/
│   └── evals.json                # 行為驗收契約（實測固化的護欄回歸測試）
├── docs/
│   ├── origin.md / origin_zh-TW.md  # 方法論源流（英／繁；2023→2025→2026→STORM）
│   ├── usage.md / usage_zh-TW.md    # 使用指南（英／繁）
│   ├── 2026-06-12-v2-revision.md # v2.0.0 修訂記錄（繁中過程記錄）
│   ├── 2026-06-20-storm-comparison.md # STORM 對照研究（繁中）
│   └── 2026-06-20-language-ab-and-english-canonical.md # 指令語言 A/B 實測 + 轉英文正本決策（繁中）
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
3. `.claude-plugin/marketplace.json`、`.claude-plugin/plugin.json` 與 `plugins/best-minds/.codex-plugin/plugin.json` 的 version（三處需一致）
3b. 方法論變動需移植到 Codex 平台變體 `plugins/best-minds/skills/best-minds/SKILL.md`（該檔僅平台機制與正本不同：無 subagent 工具、一般提問取代 AskUserQuestion、泛稱 web search）
4. 內容若涉及方法論變動，同步檢查 `README.md`（英）／`README_zh-TW.md`（繁）與 `docs/`（`origin.md`／`usage.md` 為英文版，`*_zh-TW.md` 為繁中版，兩者並行維護；其餘 `docs/` 過程記錄維持繁中）
5. 改動護欄後，對照 `evals/evals.json` 的 expectations 確認行為契約未被破壞；可跑 `evals/run.sh` 自動回歸（`--case 3` 為最便宜的快篩）

根目錄 `SKILL.md` 是 symlink，不要直接編輯或以實體檔案覆蓋。
