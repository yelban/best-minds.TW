# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## 專案概述

這是一個 Claude Code Skill，實作「模擬器思維」方法論。核心理念來自 Andrej Karpathy：把 LLM 當成模擬器，而非實體。

**用法**：不問「你怎麼看」，而是問「世界上誰最懂這個？TA 會怎麼說？」

## 專案結構

```
best-minds.TW/
├── SKILL.md      # Skill 定義檔（核心）
└── LICENSE       # MIT License
```

## Skill 設計原則

1. **問題決定人數** — 一人夠就一人，需碰撞才多人
2. **找真正最懂的** — 不是「合適的」，是「最強的」
3. **基於真實** — 模擬要基於 TA 公開的思想、著作、言論
4. **引用原話** — 盡可能用 TA 說過的話

## 修改須知

修改 `SKILL.md` 時，須同步更新：
1. 檔案頭部的 YAML frontmatter（name, description）
2. 檔案內的 HTML 註解（input, output, pos）
