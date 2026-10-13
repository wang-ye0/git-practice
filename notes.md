# Git 学习笔记

## 三个区域

工作区 → (git add) → 暂存区 → (git commit) → 仓库

## 常用命令

| 命令 | 作用 |
|---|---|
| `git status` | 看当前状态，最常用 |
| `git add <文件>` | 加入暂存区 |
| `git commit -m "说明"` | 提交 |
| `git log --oneline` | 看历史 |
| `git diff` | 看未暂存的改动 |
| `git diff --staged` | 看已暂存的改动 |

## 心得

暂存区的作用是让你能"挑着提交"，不是所有改动必须一次提完。
