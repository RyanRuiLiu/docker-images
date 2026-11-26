# docker-images

自建容器镜像的构建定义，镜像发布到阿里云容器镜像服务（ACR）。

## 镜像

| 镜像 | 目录 | 当前版本 | 用途 |
| --- | --- | --- | --- |
| caddy-cloudflare | `caddy-cloudflare/` | 2.11.9 | Caddy + Cloudflare DNS 插件；中心反向代理 |
| sops | `sops/` | 3.13.3 | sops 二进制（官方 checksums 校验）；解密密钥 |
| litestream | `litestream/` | 0.5.18 | Litestream 官方镜像；SQLite 持续备份 |

## 目录结构

| 路径 | 说明 |
| --- | --- |
| `caddy-cloudflare/Dockerfile` | caddy-cloudflare 镜像构建定义 |
| `sops/Dockerfile` | sops 镜像构建定义 |
| `litestream/Dockerfile` | litestream 镜像构建定义 |
| `.github/workflows/` | 各镜像的构建工作流 |

## 构建与发布

- 触发：`push` 到 main 且改动镜像目录或工作流文件；`pull_request` 只构建，不推送。
- 标签：只推送 `main`，不产生版本与 sha 标签；ACR 个人版不支持自动清理。
- 发布：ACR；服务器与应用通过 VPC 端点引用。
- 自动化：`caddy-cloudflare` 构建成功后调用 `/hooks/sync-infra`，并轮询状态。

## 配置

### Actions Secrets

| Secret | 说明 |
| --- | --- |
| `ACR_REGISTRY` / `ACR_NAMESPACE` / `ACR_USERNAME` / `ACR_PASSWORD` | ACR 公网端点、命名空间与凭据 |
| `WEBHOOK_SECRET` | 调用服务器同步钩子的 HMAC 密钥，与 infra 侧一致 |
