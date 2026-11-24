---
name: wangyi-memory
description: "Sync Claude Code memory files to a GitHub repository with auto-pull on session start and auto-push on memory writes. Created by wangyi-debug. Use when user asks to sync memory to GitHub, backup memory, or set up cross-device memory sync."
allowed-tools: Bash(git *), Bash(gh *), Read, Write, Edit
---

# Memory Sync

将 Claude Code 记忆目录同步到 GitHub 仓库，实现跨设备记忆共享。

## 前提

- GitHub 账号 + gh CLI 已认证（`gh auth status`）
- Git 已安装
- 已有 Claude Code 记忆目录（通常是 `~/.claude/projects/<project>/memory/`）

## 一键设置

```bash
# 1. 进入记忆目录
cd ~/.claude/projects/C--Users-89430/memory

# 2. 初始化 git（如果还没有）
git init
git add -A
git commit -m "Initial memory snapshot"

# 3. 创建 GitHub 仓库并推送
gh repo create claude-memory --private --push --source . --remote origin
git branch -M master
git push -u origin master
```

## 配置自动同步 Hooks

在 `~/.claude/settings.json` 中添加 hooks：

```json
{
  "hooks": {
    "SessionStart": [
      {
        "command": "cd ~/.claude/projects/C--Users-89430/memory && git pull origin master 2>/dev/null; exit 0",
        "description": "Pull latest memory from GitHub on session start"
      }
    ],
    "PostToolUse": [
      {
        "matcher": "Write|Edit",
        "hooks": [
          {
            "command": "cd ~/.claude/projects/C--Users-89430/memory && git add -A && git diff --cached --quiet || git commit -m \"auto: memory update $(date -Iseconds)\" && git push origin master 2>/dev/null; exit 0",
            "description": "Auto commit and push memory changes to GitHub"
          }
        ]
      }
    ]
  }
}
```

> 如果已有其他 hooks，合并而不是覆盖。

## 工作原理

| Hook | 触发时机 | 动作 |
|------|----------|------|
| SessionStart | 每次新会话开始 | `git pull` 拉取远程最新记忆 |
| PostToolUse (Write\|Edit) | 每次写入/编辑记忆文件后 | `git add -A && git commit && git push` |

## 新设备接入

在新电脑上：

```bash
# 1. 确保记忆目录的父目录存在
mkdir -p ~/.claude/projects/C--Users-89430

# 2. Clone 记忆仓库
gh repo clone wangyi-debug/claude-memory ~/.claude/projects/C--Users-89430/memory

# 3. 复制 hooks 配置到 settings.json（同上）
```
