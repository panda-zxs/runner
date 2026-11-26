# goRunner

GitHub Actions 爬虫执行模板仓库（配合 `iachinaCrawl` 的 `dispatcher` 使用）。

## 使用方式

1. 在目标 GitHub 仓库的 `Settings -> Environments -> INS -> Secrets` 中配置 `REPO`（指向 `iachinaCrawl` 仓库的带凭据 HTTPS 地址，默认分支为 `main` 或 `master`）。
2. 将本仓库推送至 `REPO_LIST` 中配置的各个执行仓库。
3. RN-01 上 `iachinaCrawl` 内置的调度器检测到 Redis 任务队列非空后，会自动更新 `1.txt` 并推送触发 `.github/workflows/go-runner.yml`。
4. Workflow 使用 `actions/setup-go@v5` + `actions/cache@v4` 稀疏检出 `iachinaCrawl` 的 Go 源码（跳过 `web/` 前端目录），通过 `go run ./cmd/iachina worker` 现场编译运行，无需 Node.js、Python 或预编译二进制。
