# GitHub Actions 自动更新配置说明

本项目已包含工作流 `.github/workflows/update-news.yml`，按以下步骤即可启用自动更新。

## 1. 将代码推送到你的 GitHub 仓库

若你是在本地克隆的 [LearnPrompt/ai-news-radar](https://github.com/LearnPrompt/ai-news-radar)，需要先创建**你自己的** GitHub 仓库并推送：

```bash
# 在项目根目录
git remote rename origin upstream   # 可选：保留上游为 upstream
git remote add origin https://github.com/你的用户名/你的仓库名.git
git push -u origin master
# 若默认分支是 main：
# git push -u origin main
```

或直接在 GitHub 上 Fork 本仓库，再在本地添加你的 fork 为 remote 并推送。

## 2. 开启 Workflow 写权限

1. 打开仓库 **Settings** → **Actions** → **General**
2. 在 **Workflow permissions** 中选择 **Read and write permissions**
3. 保存

这样定时任务才有权限把更新后的 `data/*` 提交回仓库。

## 3. 可选：使用私有 RSS OPML

若你希望 Actions 使用自己的 RSS 订阅（`feeds/follow.opml`），而该文件不提交到仓库：

1. 在本地生成 base64：  
   `base64 < feeds/follow.opml | pbcopy`（macOS）
2. 仓库 **Settings** → **Secrets and variables** → **Actions**
3. 新建 Secret，名称：`FOLLOW_OPML_B64`，值：粘贴刚才复制的 base64 内容

未设置时，工作流会使用内置网页源，不加载 OPML，仍可正常运行。

## 4. 触发方式

- **定时**：每 30 分钟自动运行（cron: `*/30 * * * *`）
- **手动**：仓库 **Actions** → 选择 “Update AI News Snapshot” → **Run workflow**

运行成功且数据有变化时，会自动提交并 push `data/latest-24h.json`、`data/archive.json`、`data/source-status.json`、`data/waytoagi-7d.json`、`data/title-zh-cache.json`。
