# Git 速查手册

> 写给自己：看不懂的随时问我，别硬背。
> 位置：`C:\Users\12042\git-practice\notes.md`

---

## 一、先记住这一个模型

```
   工作区                    暂存区                  仓库
   你能看到的文件             待存档的篮子            永久历史
        │                        │                      │
        │───── git add ─────────>│                      │
        │                        │──── git commit ─────>│
```

**只有走完 add + commit，文件才进入历史。**

| 名词 | 大白话 |
|---|---|
| 仓库 repository | 一个被 Git 管理的文件夹（里面多了个隐藏的 `.git`） |
| 工作区 | 你正在编辑的文件 |
| 暂存区 staging | "待存档篮子"，先挑再存 |
| 提交 commit | 按一次"存档"按钮 |
| 分支 branch | 另起一份草稿，不影响主稿 |
| 合并 merge | 把草稿并进主稿 |
| 远程 remote | GitHub 上那份拷贝 |
| origin | 远程仓库的默认名字，只是个称呼 |

**Git 和 GitHub 是两回事**：Git 是本地管历史的工具，GitHub 是网上的存放处。

---

## 二、每天真正会用的命令（就这几条）

### 看状态 —— 敲得最多的

```bash
git status
```
问 Git："现在什么情况？" **不知道下一步干嘛时，先敲这个，它会告诉你。**

```bash
git status --short          # 紧凑版，一行一个文件
```

### 存一版（两步）

```bash
git add 文件名              # 单个文件放进篮子
git add .                   # 所有改动都放进篮子
git commit -m "说明这次改了啥"
```

> **提交说明怎么写**：写"为什么改"，不要写"改了啥"。
> ❌ `更新代码` ✅ `修复登录页密码框不显示错误提示`

### 看历史

```bash
git log --oneline           # 简洁列表，最常用
git log --oneline --graph   # 带分支图形，看分叉时用
git log --oneline -5        # 只看最近 5 条
```

### 提交前检查改了什么

```bash
git diff                    # 工作区 ↔ 暂存区（我改了但还没 add 的）
git diff --staged           # 暂存区 ↔ 仓库（我马上要提交的）
git diff HEAD               # 工作区 ↔ 上次提交（总共动了什么）
```

> **习惯**：每次 commit 前敲一次 `git diff --staged` 扫一眼。
> 提交前看一眼，比提交后补救省事一百倍（尤其防手滑提交密码）。

---

## 三、完整工作流（照着抄）

```bash
cd 你的项目文件夹

git status                          # 1. 先看看现在什么情况
git add .                           # 2. 改动放进篮子
git diff --staged                   # 3. 检查即将提交的内容
git commit -m "说明这次改了啥"        # 4. 存档
git push                            # 5. 推到 GitHub（首次要用 -u，见第五节）
```

---

## 四、分支

```bash
git branch                          # 列出所有分支（* 是当前所在）
git switch -c 新分支名               # 创建并切换过去
git switch main                     # 切换回主分支
git merge 分支名                     # 把某分支合并进【当前】分支
git branch -d 分支名                 # 删除已合并的分支
```

**切换分支时文件会变，这是正常的。** 因为 Git 存的是一份份完整快照，切换 = 把工作区换成那个快照的样子。不是你删了文件。

**典型用法**：
```bash
git switch -c 试试新功能      # 1. 开个草稿
# ...改代码、add、commit...
git switch main              # 2. 回主稿
git merge 试试新功能          # 3. 满意了，合并
git branch -d 试试新功能      # 4. 删掉草稿
```

> 老教程里写的是 `git checkout -b` / `git checkout 分支名`——**效果一样，只是新命令更清楚**。两个都会遇到，`switch` 管分支，`checkout` 是老的全能命令。

---

## 五、和 GitHub 打交道

```bash
git clone <仓库网址>                 # 把别人的仓库整个下载下来（含历史）
git remote add origin <仓库网址>     # 给本地仓库关联一个远程地址
git remote -v                       # 看当前关联的远程地址
git push -u origin main             # 首次推送（-u 记住关联，以后直接 git push）
git push                            # 以后推送
git pull                            # 把远程的新东西拉下来
```

**首次推送完整流程**：
```bash
git remote add origin git@github.com:wang-ye0/仓库名.git
git push -u origin main
```

> `-u` 的作用：把本地 `main` 和远程 `origin/main` 绑起来。**只在第一次需要**，之后 `git push` / `git pull` 就够。

**SSH 地址长得像**：`git@github.com:用户名/仓库名.git`
**HTTPS 地址长得像**：`https://github.com/用户名/仓库名.git`

你配的是 SSH，**用 SSH 地址**。

---

## 六、你的环境配置（已配好，忘了就查这里）

| 项目 | 值 | 在哪 |
|---|---|---|
| 用户名 | `wang-ye0` | `git config --global user.name` |
| 邮箱 | `1204211723@qq.com` | `git config --global user.email` |
| 默认分支名 | `main` | `git config --global init.defaultBranch` |
| SSH 私钥 | `C:\Users\12042\.ssh\id_ed25519` | **绝不能外传** |
| SSH 公钥 | 同目录 `.pub` 结尾 | 已贴到 GitHub |

**验证配置**：
```bash
git config --global --list        # 看所有配置
ssh -T git@github.com             # 测 GitHub 连接，应回 "Hi wang-ye0!"
```

---

## 七、⚠️ 出事了怎么办（撤销类命令）

**这些是救命命令，先记住位置，出事时回来查。**

| 我想… | 命令 | 危险度 |
|---|---|---|
| 放弃某个文件的改动（回到上次提交的样子） | `git restore 文件名` | 低（改动会没，但只影响这一个文件） |
| 把文件从暂存区拿出来（add 错了） | `git restore --staged 文件名` | 低（改动还在） |
| 改最后一次提交的**说明文字** | `git commit --amend -m "新说明"` | 低（别改已推送的） |
| 撤销最后一次提交，**改动保留** | `git reset --soft HEAD~1` | 中（最常用） |
| 撤销最后一次提交，**改动也丢掉** | `git reset --hard HEAD~1` | 🔴 **高，改动真没了** |
| 找回"好像删没了"的提交 | `git reflog` 然后 `git reset --hard <编号>` | 救命的 |

> **`git reflog` 是后悔药**：它记录你**所有**HEAD 的移动（包括被 reset 掉的提交），保留 90 天。以为把东西搞没了的时候，先敲 `git reflog`。

> 🔴 **`--hard` 会真的删掉你的改动，没有回收站。** 敲之前一定先 `git status` 看清楚。

---

## 八、报错对照表

| 报错信息 | 什么意思 | 怎么办 |
|---|---|---|
| `fatal: not a git repository` | 这个文件夹还没被 Git 管理 | `git init`；或你可能**不在项目目录里**，先 `cd` 过去 |
| `Author identity unknown` | 没配用户名/邮箱 | `git config --global user.name "名字"` 和 `user.email "邮箱"` |
| `nothing to commit, working tree clean` | 没有改动可提交 | **不是错误**，说明当前状态很干净 |
| `warning: LF will be replaced by CRLF` | Windows 和 Linux 换行符不同 | **不是错误，不用管** |
| `! [rejected] ... (fetch first)` | 远程有你本地没有的东西，直接推会被覆盖 | 先 `git pull`，解决可能的冲突，再 `git push` |
| `error: src refspec main does not match any` | 本地还没有任何提交（或分支名不对） | 先 `git add` + `git commit` 产生至少一个提交；查分支名 `git branch` |
| `fatal: remote origin already exists` | 已经关联过远程了 | 改地址用 `git remote set-url origin <新地址>` |
| `Permission denied (publickey)` | SSH 没配好 / 公钥没贴到 GitHub | 检查 `ssh -T git@github.com`；检查 GitHub 上有没有那把公钥 |
| `CONFLICT (content): Merge conflict in 文件名` | 两边改了同一处，Git 不知道听谁的 | 见下面第九节 |
| `fatal: refusing to merge unrelated histories` | 本地和远程是两套独立历史（常见于建仓库时勾了 README） | 加参数 `git pull --allow-unrelated-histories` |
| `Your branch is ahead of 'origin/main' by 1 commit` | 本地领先远程 1 个提交 | **不是错误**，`git push` 推上去就行 |
| 提示要输密码 / 一直卡住 | 用了 HTTPS 地址而不是 SSH | `git remote set-url origin git@github.com:用户/仓库.git` |

---

## 九、冲突（conflict）怎么解

合并时两边改了**同一行**，Git 无法自动决定，就在文件里标出来：

```
<<<<<<< HEAD
这是你当前分支的内容
=======
这是要合并过来的内容
>>>>>>> 分支名
```

**处理三步**：

1. 打开这个文件，找到 `<<<<<<<` 标记
2. **手动编辑成你最终想要的样子**——把三行标记本身全部删掉，只留正确内容
3. 保存，然后：
```bash
git add 文件名
git commit                  # 完成这次合并
```

**想放弃本次合并、回到操作前**：
```bash
git merge --abort
```

---

## 十、`.gitignore` —— 别把垃圾传上去

在项目根目录建一个 `.gitignore` 文件，写上**不想被 Git 管**的文件：

```
# 依赖文件夹
node_modules/
venv/

# 密钥、密码类（重要！）
.env
*.key
*.pem

# 系统生成的
.DS_Store
Thumbs.db

# 编译产物
build/
dist/
*.log
```

> **一定要把密钥类文件写进去。** 一旦推到 GitHub，就算删掉，历史记录里还能翻出来。

---

## 十一、记不住怎么办

**不需要背。** 日常高频的其实就四条：

```bash
git status                  # 我在哪、发生了什么
git add .                   # 装篮子
git commit -m "说明"         # 存档
git push                    # 传上去
```

其余的全部回来查这份文档。用上十天半月，肌肉记忆自然就有了。

---

*最后更新：2026-10-13*
