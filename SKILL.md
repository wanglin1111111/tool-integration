---
name: tool-integration
version: 1.0.0
author: wanglin1111111
description: |
  Tool Integration 工具集成技能集合，包含 GitHub CLI 操作、GitHub Stars 增长策略等工具集成方法。支持使用 gh CLI 进行 Issue、PR、CI 管理，以及 14 天 GitHub Stars 增长冲刺计划。
---

# Tool Integration

> Merged from 20 skills

## Skill: github

---
name: tool-integration
description: "使用 gh CLI 与 GitHub 交互。使用 gh issue、gh pr、gh run 和 gh api 进行 Issue、PR、CI 运行和高级查询。"
---

# GitHub Skill

使用 `gh` CLI 与 GitHub 交互。在非 git 目录时始终指定 `--repo owner/repo`，或直接使用 URL。

## Pull Requests

检查 PR 的 CI 状态：
```bash
gh pr checks 55 --repo owner/repo
```

列出最近的 workflow 运行：
```bash
gh run list --repo owner/repo --limit 10
```

查看运行并查看哪些步骤失败：
```bash
gh run view <run-id> --repo owner/repo
```

仅查看失败步骤的日志：
```bash
gh run view <run-id> --repo owner/repo --log-failed
```

## API for Advanced Queries

`gh api` 命令可用于访问其他子命令不可用的数据。

获取特定字段的 PR：
```bash
gh api repos/owner/repo/pulls/55 --jq '.title, .state, .user.login'
```

## JSON Output

大多数命令支持 `--json` 进行结构化输出，可使用 `--jq` 过滤：

```bash
gh issue list --repo owner/repo --json number,title --jq '.[] | "\(.number): \(.title)"'
```

---

## Skill: github-stars-playbook

---
name: github-stars-playbook
description: |
  14 天冲刺快速为你的仓库增长 GitHub Stars。涵盖 Show HN、Reddit、Twitter/X 线程和 GitHub Trending 机制的每日战术计划。
---

# 快速获取 GitHub Stars — 14 天增长冲刺

> 🌍 **Language / 语言**: [中文](#中文版) | [English](references/en/README.md) | [日本語](references/ja/README.md) | [한국어](references/ko/README.md)

## 📦 Install

```bash
clawhub install github-stars-playbook
```

**安装后你将获得：**
- 月增长 300+ stars 的系统方法
- Show HN、Reddit、Twitter/X 的每日战术
- GitHub Trending 机制深度解析

---

## 许可证

MIT License
