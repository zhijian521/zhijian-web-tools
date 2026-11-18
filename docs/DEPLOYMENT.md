# 独立部署与仓库迁移

三个应用分别部署到独立站点。代码集中在 `zhijian521/zhijian-web-tools`，线上域名和用户数据不因目录合并而迁移。

## Vercel 配置

分别为每个应用配置一个 Vercel 项目，生产分支均选择 `master`，命令相对于 Root Directory 执行。

| 设置 | 导航 | 便签 | 资源管理 |
| --- | --- | --- | --- |
| Git 仓库 | `zhijian521/zhijian-web-tools` | `zhijian521/zhijian-web-tools` | `zhijian521/zhijian-web-tools` |
| Root Directory | `apps/home` | `apps/sticky-note` | `apps/resource-hub` |
| Framework Preset | Vite | Vite | Nuxt.js |
| Install Command | `npm ci` | `npm ci` | `npm ci` |
| Build Command | `npm run build` | `npm run build` | `npm run build` |
| Output Directory | `dist` | `dist` | 使用 Nuxt 预设默认值 |

资源管理包含服务端 API，不能只上传静态页面代替 Nuxt 服务。开发时的 3001 端口不用于生产部署。

## 原部署项目迁移

本次 Git 仓库整理未修改 Vercel 控制台、DNS 或线上环境变量。已发布站点应继续使用原域名及原环境变量：

1. 导航原仓库更名为 `zhijian-web-tools`；在原部署项目核对 Git 关联，并将 Root Directory 改为 `apps/home`。
2. 便签、资源管理的原部署项目重新连接 `zhijian521/zhijian-web-tools`，分别设置上表的 Root Directory 与生产分支。
3. 保留各项目的环境变量、域名与部署保护配置，再从新仓库触发部署并确认页面和 API 可用。
4. 若原项目配置了固定文件路径、构建脚本或忽略构建规则，同步调整到新目录。仅改无关应用时是否跳过构建，由部署平台的项目设置决定；本仓库未自动启用跳过策略。
5. 两个来源仓库的代码和历史已合入主分支；删除 `sticky-note`、`simple-repository` 前，先完成其线上部署项目的仓库切换。不要删除更名后的 `zhijian-web-tools`。

已有 GitHub App 的仓库授权是否覆盖更名后的仓库，需要在部署平台核对。没有登录部署平台时，单靠 Git 推送不能确认关联已完成。

## 环境与存储

- 导航：保留 `VITE_API_BASE_URL`，构建时注入。线上 API 地址不能是开发机的 `127.0.0.1`。
- 便签：继续使用原域名，避免浏览器的本地存储和 PWA 缓存因更换源而被隔离。仓库搬迁不修改用户浏览器内的数据。
- 资源管理：使用 `.env.example` 中的 `NUXT_` 前缀变量，对应 `nuxt.config.ts` 的 runtimeConfig。保留原 `NUXT_GITHUB_OWNER`、`NUXT_GITHUB_REPO`、`NUXT_GITHUB_BRANCH` 与认证配置，存储仓库不必改成代码集合仓库。
- 本次合并只包含代码仓库里的资料，不迁移部署平台设置或 GitHub 数据存储仓库中的图片。

## 自建部署

Vite 两个应用分别在其目录构建，发布对应 `dist/`，并按原有站点配置处理 SPA 路由。资源管理按 Nuxt/Nitro 服务部署，在其应用目录安装和构建。每个服务的工作目录使用对应的 `apps/<应用>`，环境配置也分别维护。
