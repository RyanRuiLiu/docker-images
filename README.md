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

- 触发：修改镜像目录仅构建该镜像（路径过滤）；标签 `caddy-<版本>` / `sops-<版本>` 产生对应版本 tag
- 发布：ACR
- 引用：服务器与应用一律引用 ACR 路径

## 依赖的 Actions Secrets

| Secret | 说明 |
| --- | --- |
| ACR_REGISTRY / ACR_NAMESPACE / ACR_USERNAME / ACR_PASSWORD | ACR 公网端点、命名空间与凭据 |
