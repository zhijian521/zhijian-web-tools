# 知简 Web 工具集 · zhijian-web-tools

集中管理四个日常 Web 工具，每个应用保留自己的技术栈、依赖、锁文件和本地开发配置。

| 应用 | 目录 | 技术栈 |
| --- | --- | --- |
| Simple Home · 导航与书签 | [apps/home](apps/home/README.md) | Vue 3 + TypeScript + Vite |
| Sticky Note · 便签画板 | [apps/sticky-note](apps/sticky-note/README.md) | React + TypeScript + Vite + PWA |
| Resource Hub · 图片资源管理 | [apps/resource-hub](apps/resource-hub/README.md) | Nuxt 3 + GitHub API |
| Simple Chat · 私密匿名聊天 | [apps/chat](apps/chat/README.md) | Next.js 16 + React 19 + SSE |

## 开始

建议使用 Node.js 22.12 或更高的受支持版本。克隆后按需安装对应应用；不使用 npm workspaces，也不在根目录集中安装应用依赖。

```sh
git clone https://github.com/zhijian521/zhijian-web-tools.git
cd zhijian-web-tools

npm run setup:home
npm run dev:home
```

| 应用 | 安装 | 启动 | 默认开发地址 | 构建 |
| --- | --- | --- | --- | --- |
| 导航 | `npm run setup:home` | `npm run dev:home` | `http://localhost:5173` | `npm run build:home` |
| 便签 | `npm run setup:sticky-note` | `npm run dev:sticky-note` | `http://localhost:3000` | `npm run build:sticky-note` |
| 资源管理 | `npm run setup:resource-hub` | `npm run dev:resource-hub` | `http://localhost:3001` | `npm run build:resource-hub` |
| 聊天 | `npm run setup:chat` | `npm run dev:chat` | `http://localhost:3002` | `npm run build:chat` |

根命令委托给各应用的 npm 脚本。资源管理的根启动命令指定 3001 端口，便于与便签同时运行；直接在资源管理目录运行时仍沿用 Nuxt 的默认端口。应用可独立 `cd apps/<应用>` 后运行 `npm ci`、`npm run dev`。

聊天的根启动命令使用 3002 端口；`preview:chat` 调用 Next.js 的 `start`，在完成聊天应用构建后于本地 3002 端口预览。直接在聊天应用目录启动时仍使用 Next.js 默认端口。

导航和资源管理的 `.env.example` 保存在各自应用目录；将副本 `.env` 放在同一目录。资源管理的 GitHub 存储仓库由 `NUXT_GITHUB_REPO` 决定，与本代码仓库名称无关。

## 目录

```text
zhijian-web-tools/
├─ apps/
│  ├─ home/               导航应用、独立 package.json 和锁文件
│  ├─ sticky-note/        便签应用、独立 package.json 和锁文件
│  ├─ resource-hub/       资源管理、独立 package.json 和锁文件
│  └─ chat/               匿名聊天、独立 package.json 和锁文件
├─ package.json           启动和构建入口，无共享应用依赖
└─ README.md
```

## 维护与历史

业务代码和样式在应用内部维护。各应用的 lint、测试与类型检查仍由各自脚本提供；根命令不会连带检查其他应用。便签不再在依赖安装时初始化仓库级 Git hooks。

本仓库由 `simple-home` 更名，合并 `sticky-note`、`simple-repository` 与 `SimpleChat`。四个项目的原提交均作为 `master` 的祖先保留，原提交 SHA、作者和时间未重写。`archive/home-v6.0.6` 保存导航原有的历史分支，原 `v2.0.5` 至 `v6.0.6` 标签均属于导航项目。

`archive/chat-master` 保存聊天应用合并前的主分支，原 `v1.0.0` 标签以 `chat/v1.0.0` 保留。聊天使用单进程内存会话，不接数据库、不保存聊天记录。

各应用保留自己的许可与声明；便签的 MIT 许可证位于 [apps/sticky-note/LICENSE](apps/sticky-note/LICENSE)。本次合并不新增跨应用的统一授权。
