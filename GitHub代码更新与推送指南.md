# GitHub 代码更新与推送指南

本文档描述在日常开发中如何把本地修改同步到 GitHub，精确到终端命令与推荐习惯。适用于已完成「新项目上传」、本地已与远程 `origin` 关联的仓库。

---

## 前置条件

- 项目已按《新项目上传到 GitHub 完整指南》完成首次推送，即已执行过 `git remote add origin ...` 和 `git push -u origin main`。
- 终端可正常使用 Git，推送时若需认证，已准备好 GitHub 用户名与 Personal Access Token（或已配置 `credential.helper`）。

---

## 第一部分：日常更新代码并推送到 GitHub（标准流程）

每次你修改了项目里的文件（文档、代码、配置等），希望把这些改动同步到 GitHub 时，按以下步骤操作。

### 步骤 1.1：打开终端并进入项目目录

```bash
cd "/你的/项目/路径"
```

示例：

```bash
cd "/Users/harvey/Documents/Cursor/RWA_AI"
```

确认当前在项目根目录（可执行 `pwd` 查看）。

### 步骤 1.2：查看当前修改（可选）

查看哪些文件被修改、新增或删除：

```bash
git status
```

- 红色表示未暂存改动，绿色表示已暂存未提交。

### 步骤 1.3：拉取远程最新内容（推荐先拉再推）

若多人协作或你在多台设备上改过同一仓库，先拉取再推送可减少冲突：

```bash
git pull
```

- 若当前分支已设置 `upstream`（首次 push 时用了 `-u origin main`），直接 `git pull` 即可。
- 若提示「无 upstream 分支」，使用：`git pull origin main`

### 步骤 1.4：将修改加入暂存区

**方式 A：添加所有改动**

```bash
git add .
```

- 会添加当前目录下所有已修改、新增的文件（受 `.gitignore` 限制）。

**方式 B：只添加指定文件**

```bash
git add 文件或文件夹路径
```

示例：

```bash
git add 需求文档/新需求.md
git add "原型&UI/官网/index.html"
```

### 步骤 1.5：提交到本地仓库

```bash
git commit -m "简短描述本次修改内容"
```

- `-m` 后的引号内写**本次修改的说明**，便于日后查看历史。  
- 示例：`git commit -m "更新官网首页文案"`、`git commit -m "新增竞品调研文档"`

若未执行 `git add` 或没有改动，会提示 `nothing added to commit` 或 `no changes added to commit`，需先完成步骤 1.4。

### 步骤 1.6：推送到 GitHub

```bash
git push
```

- 若首次 push 时已执行过 `git push -u origin main`，之后在该分支上只需 `git push` 即可。
- 若未设置过 upstream，使用：`git push origin main`

若需要输入用户名和 Password，在 **Password** 处粘贴 Personal Access Token（不是 GitHub 登录密码）。

---

## 第二部分：常用场景与命令速查

### 场景 1：只改了几个文件，一次性提交并推送

```bash
cd "/你的/项目/路径"
git add .
git commit -m "描述修改内容"
git push
```

### 场景 2：多人协作，先拉再改再推

```bash
cd "/你的/项目/路径"
git pull
# 在此进行编辑...
git add .
git commit -m "描述修改内容"
git push
```

### 场景 3：查看提交历史

```bash
git log --oneline
```

- 可看到最近提交的简短列表，便于确认是否已提交、是否已推送。

### 场景 4：查看远程仓库地址

```bash
git remote -v
```

- 确认 `origin` 指向的 GitHub 地址是否正确。

### 场景 5：放弃未提交的修改（慎用）

- 放弃**所有**未提交的修改（不可恢复）：

```bash
git checkout -- .
```

- 或使用较新语法：

```bash
git restore .
```

仅当确定不需要这些修改时使用。

---

## 第三部分：推送时可能遇到的问题与处理

### 问题 1：`git push` 提示 `Permission denied` 或认证失败

- **Username** 填 GitHub 用户名。  
- **Password** 必须填 **Personal Access Token**，不能填登录密码。  
- 若 Token 已过期或被撤销，需到 https://github.com/settings/tokens 重新生成并更新本地使用（或更新钥匙串中的凭据）。

### 问题 2：`git pull` 或 `git push` 提示「分支没有 upstream」

执行一次（将 `main` 改为你的主分支名若不同）：

```bash
git push -u origin main
```

之后可直接使用 `git push` 和 `git pull`。

### 问题 3：`git pull` 提示合并冲突（merge conflict）

1. 终端会列出冲突文件。  
2. 用编辑器打开这些文件，搜索 `<<<<<<<`、`=======`、`>>>>>>>` 标记，按需保留/合并内容。  
3. 保存后执行：

```bash
git add 冲突文件路径
git commit -m "解决合并冲突"
git push
```

### 问题 4：误将不该提交的文件加入了提交

- 若尚未 `git push`：可用 `git reset --soft HEAD~1` 撤销最近一次 commit，保留工作区修改，再修正 `.gitignore` 或 `git add` 后重新 commit。  
- 若已 push：需在后续提交中删除该文件，并从仓库历史中移除（涉及 `git filter-branch` 或 `git filter-repo`，建议查文档或谨慎操作）。  
- 预防：把敏感或本地专属文件加入 `.gitignore`（如 `github-token.local`、`.env`）。

---

## 第四部分：推荐协作习惯

1. **改前先 `git pull`**：减少与远程的冲突。  
2. **小步提交**：完成一个小功能或一类修改就 commit 一次，备注写清楚。  
3. **按目录分工**：多人时尽量不同人负责不同目录（如一人需求文档、一人原型），减少同文件同时修改。  
4. **不提交敏感信息**：Token、密码、本地配置等放入 `.gitignore`，不放入仓库。

---

## 检查清单（每次更新代码后）

- [ ] 已在项目目录下执行 `git pull`（多人协作时）
- [ ] 已用 `git add` 添加要提交的修改
- [ ] 已用 `git commit -m "说明"` 提交
- [ ] 已用 `git push` 推送到 GitHub
- [ ] 在 GitHub 仓库页面刷新后能看到最新提交与文件

按以上流程操作，即可持续将本地代码与文档更新到 GitHub。

---

*文档版本：1.0 | 适用于日常更新并推送代码到 GitHub*
