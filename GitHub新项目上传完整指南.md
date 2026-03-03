# 新项目上传到 GitHub 完整指南

本文档描述如何将本地一个新项目首次上传到 GitHub，精确到每个选项与终端命令。适用于从未初始化过 Git 的文件夹。

---

## 前置条件

- 已安装 Git（终端执行 `git --version` 可检查）
- 已注册 GitHub 账号
- 项目文件夹路径已知（例如：`/Users/harvey/Documents/Cursor/RWA_AI`）

**注意：** 路径中若包含 `&` 等特殊字符，在终端里需用英文双引号包裹，如 `"/Users/harvey/Documents/Cursor/RWA&AI"`。建议项目文件夹名使用字母、数字、下划线（如 `RWA_AI`），避免符号带来问题。

---

## 第一部分：在本地初始化 Git 并首次提交

### 步骤 1.1：打开终端并进入项目目录

1. 打开终端（macOS：应用程序 → 实用工具 → 终端，或 Spotlight 搜索「终端」）。
2. 执行以下命令（**将路径替换为你的项目实际路径**）：

```bash
cd "/你的/项目/路径"
```

示例：

```bash
cd "/Users/harvey/Documents/Cursor/RWA_AI"
```

3. 确认当前目录无误（可选）：

```bash
pwd
```

终端应输出你的项目路径。

### 步骤 1.2：初始化 Git 仓库

在项目目录下执行：

```bash
git init
```

终端会提示：`Initialized empty Git repository in .../.git/`

### 步骤 1.3：添加所有文件到暂存区

```bash
git add .
```

- `.` 表示当前目录下所有文件（受 `.gitignore` 规则影响，被忽略的文件不会加入）。

### 步骤 1.4：创建首次提交

```bash
git commit -m "初始提交：项目文档与结构"
```

- `-m` 后面引号内是本次提交的说明，可按项目情况修改。
- 执行后会显示类似 `X files changed, Y insertions(+)`，表示首次提交成功。

**第一部分小结：** 到这一步，本地已经有了一个带一次提交的 Git 仓库。

---

## 第二部分：在 GitHub 上创建空仓库

### 步骤 2.1：打开创建仓库页面

- 浏览器打开：**https://github.com/new**
- 或：登录 GitHub → 右上角 **+** → 选择 **New repository**

### 步骤 2.2：填写仓库信息（逐项说明）

| 页面上的项 | 操作 / 选项 |
|-----------|-------------|
| **Owner** | 下拉选择你的 GitHub 账号（一般默认即为当前登录账号，无需改） |
| **Repository name** | 输入仓库名，例如：`RWA_AI` 或 `my-project`。仅建议使用字母、数字、连字符、下划线，避免 `&` 等符号 |
| **Description**（可选） | 简短描述项目，如：`RWA 与 AI 相关产品网站 demo 项目`。可不填 |
| **Public** | 选此项：仓库对所有人可见，便于分享与协作 |
| **Private** | 选此项：仅你与所添加的协作者可见 |

### 步骤 2.3：初始化选项（重要：全部不勾选）

以下选项**一律不勾选、不添加**，保持空仓库：

- **Add a README file** — 不勾选  
- **Add .gitignore** — 不勾选（本地若已有 `.gitignore` 会冲突）  
- **Choose a license** — 选择 **None**（或保持默认不选）

原因：本地已有提交历史，若远程带 README 等会与首次 push 产生合并分歧，需多一步合并。

### 步骤 2.4：创建仓库

- 点击页面下方的绿色按钮 **Create repository**。
- 创建完成后会进入仓库页面，并显示「Quick setup」或「…or push an existing repository from the command line」的说明。

---

## 第三部分：将本地仓库推送到 GitHub

### 步骤 3.1：添加远程仓库地址

在终端中确保仍在项目目录下，执行（**将用户名和仓库名替换为你自己的**）：

```bash
git remote add origin https://github.com/你的GitHub用户名/你的仓库名.git
```

示例（用户名为 HarveyLLI，仓库名为 RWA_AI）：

```bash
git remote add origin https://github.com/HarveyLLI/RWA_AI.git
```

- 若提示 `remote origin already exists`，说明已添加过，可执行 `git remote remove origin` 后再执行上述命令，或直接进行步骤 3.2。

### 步骤 3.2：确认分支名为 main（可选）

GitHub 默认主分支名为 `main`。若本地分支是其他名称，可统一为 main：

```bash
git branch -M main
```

### 步骤 3.3：推送到 GitHub

```bash
git push -u origin main
```

- `-u origin main` 表示将本地 `main` 与远程 `origin/main` 关联，之后在该分支上可直接使用 `git push`。

### 步骤 3.4：输入 GitHub 凭据

执行 `git push` 后，终端会提示：

1. **Username for 'https://github.com':**  
   - 输入你的 **GitHub 用户名**（如 `HarveyLLI`），回车。

2. **Password for 'https://用户名@github.com':**  
   - 此处必须使用 **Personal Access Token（个人访问令牌）**，不能使用 GitHub 登录密码。  
   - 若尚未创建 Token，见下方「附录 A：创建 Personal Access Token」。  
   - 将 Token 粘贴到终端（粘贴时不会显示字符），回车。

推送成功时，终端会显示类似：

```text
Writing objects: 100% (xx/xx), done.
To https://github.com/你的用户名/仓库名.git
 * [new branch]      main -> main
branch 'main' set up to track 'origin/main'.
```

---

## 附录 A：创建 Personal Access Token（用于 Password 提示）

1. 打开：**https://github.com/settings/tokens**
2. 点击 **Generate new token** → **Generate new token (classic)**。
3. **Note**：填写用途说明，如 `Mac 本地推送`。
4. **Expiration**：选择有效期（如 90 天）或 No expiration。
5. **Select scopes**：在页面上方找到 **repo**，勾选 **repo**（Full control of private repositories）。  
   - 仅推送/拉取代码只需勾选 **repo**，无需勾选 workflow、packages、org 等其它权限。
6. 滚动到页面底部，点击 **Generate token**。
7. **复制生成的 Token**（形如 `ghp_xxxx...`），只显示一次，请妥善保存。  
   - 在终端提示 **Password** 时粘贴此 Token。

**安全提醒：** Token 等同于密码，不要提交到仓库、不要发到聊天或邮件。若已泄露，请立即在 https://github.com/settings/tokens 中撤销并重新生成。

---

## 附录 B：避免每次 push 都输入密码（可选）

在终端执行一次（对本机所有 Git 仓库生效）：

```bash
git config --global credential.helper osxkeychain
```

之后在第一次 `git push` 时输入用户名和 Token，macOS 会将凭据保存到钥匙串，后续 push 一般不再提示。

---

## 检查清单（新项目上传）

- [ ] 本地已 `git init` 并完成首次 `git add .` 和 `git commit`
- [ ] 在 GitHub 上创建了**空**仓库（未勾选 README / .gitignore / license）
- [ ] 已执行 `git remote add origin https://github.com/用户名/仓库名.git`
- [ ] 已执行 `git branch -M main`（如需）
- [ ] 已执行 `git push -u origin main` 并在提示时输入用户名与 Token
- [ ] 在浏览器打开 `https://github.com/你的用户名/你的仓库名` 能看到项目文件

完成以上步骤，即完成新项目首次上传到 GitHub 的全流程。

---

*文档版本：1.0 | 适用于首次上传新项目到 GitHub*
