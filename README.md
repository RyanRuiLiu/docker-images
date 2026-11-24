# docker-images

自建容器镜像的构建定义仓库；镜像统一发布至阿里云容器镜像服务（ACR）。

## 镜像

| 镜像 | 目录 | 版本 | 用途 |
| --- | --- | --- | --- |
| caddy-cloudflare | caddy-cloudflare/ | 2.11.9 | Caddy + Cloudflare DNS 插件（caddy-dns/cloudflare）；中心反向代理 |
| sops | sops/ | 3.13.3 | 官方 sops 二进制（官方 checksums 校验）的工具镜像（实例初始化提取二进制） |

## 仓库结构

| 路径 | 说明 |
| --- | --- |
| caddy-cloudflare/Dockerfile | caddy-cloudflare 镜像构建定义（官方 builder 多阶段模式） |
| sops/Dockerfile | sops 镜像构建定义（构建时自官方 release 下载二进制并以官方 checksums 校验，二进制不入库） |
| .github/workflows/ | 各镜像的构建工作流（复用 .github 仓库共享 docker.yml） |

## 构建与发布

- 触发：`push` 到 main 且改动对应镜像目录时构建并推送；`pull_request` 改动同样路径时仅构建校验
- 标签：推送 main 时产生 `<版本>`（自 Dockerfile 提取）、`main` 与 `sha-<提交短哈希>`；PR 构建不推送镜像
- 发布：ACR
- 引用：服务器与应用一律引用 ACR 路径
- Caddy 自动同步：`caddy-cloudflare` 在 main 构建成功后会调用服务器同步钩子，重建线上 Caddy 容器

## 依赖的 Actions Secrets

| Secret | 说明 |
| --- | --- |
| ACR_REGISTRY / ACR_NAMESPACE / ACR_USERNAME / ACR_PASSWORD | ACR 公网端点、命名空间与凭据 |
| WEBHOOK_SECRET | 调用服务器同步钩子的 HMAC 密钥，与 infra 侧同一值 |
