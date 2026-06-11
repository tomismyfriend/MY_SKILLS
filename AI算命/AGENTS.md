# AGENTS.md

## Overview

Reference corpus of classical Chinese Bazi (八字/four pillars) texts for an AI fortune-telling project. No application code exists yet.

## Files

| File | Description | Lines |
|------|-------------|-------|
| `《三命通会》作者：明·万民英（完结）.txt` | San Ming Tong Hui — comprehensive Ming-dynasty Bazi encyclopedia (complete) | ~8800 |
| `滴天髓.txt` | Di Tian Sui — core Bazi theory classic | ~2350 |
| `穷通宝鉴.txt` | Qiong Tong Bao Jian — seasonal elemental reference | ~4580 |
| `子平真诠评注/zzbj_zipingzqpz01.txt`–`10.txt` | Zi Ping Zhen Quan Ping Zhu — annotated Bazi method, split into 10 chapter files | ~39 each |

## Critical: File Encoding

All `.txt` files are **GBK/GB2312 encoded**, not UTF-8. Reading them as UTF-8 produces garbled output. Use `encoding='gbk'` in Python or equivalent when processing.

## Language

All content is **classical Chinese** (文言文). Agents should not attempt to "fix" or modernize the text.

## Bazi Fortune-Telling Skill

The `bazi-fortune` skill (`.opencode/skills/bazi-fortune/SKILL.md`) contains a synthesized 8-step analysis methodology derived from all four classical texts, plus a **complete search index** with line-number lookup tables for all 120 sections of 穷通宝鉴, all 63 chapters of 滴天髓, all 48 sections of 子平真诠, and all 120 day×hour entries of 三命通会. Load it when performing any Bazi reading.

### Book Roles in Analysis

| Book | Primary Use |
|------|------------|
| 穷通宝鉴 | **First lookup**: seasonal 用神 by day-stem × month-branch |
| 滴天髓 | Core theory: 天干 properties, 体用, 顺逆, 衰旺判断 |
| 子平真诠 | 格局 determination, 成败救应, 刑冲会合 rules |
| 三命通会 | 日柱断法, 时辰断法, 纳音, 神煞 reference |
