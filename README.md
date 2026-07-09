# AI Virtual Phone

一个基于 Next.js 的 AI 虚拟手机项目。

## 分支选择

本仓库长期保留两个设备兼容版本：

- `main`：正常设备版。
- `test`：兼容设备版，用于部分设备全屏或显示异常时部署。

自部署用户可以按设备情况选择：

```bash
git clone -b main <repo-url>
# 或
git clone -b test <repo-url>
```

## 自部署

默认自部署不会连接作者的 Supabase，也不会使用作者的账号密码系统。

```bash
npm install
cp .env.example .env.local
npm run dev
```

`.env.example` 默认包含：

```env
NEXT_PUBLIC_SELF_HOSTED_MODE=true
```

这个模式会跳过账号/激活码门禁，使用本地单机账号进入应用。适合个人部署和本地体验。

如果你直接部署到 Netlify、Vercel 或其它平台，也需要在平台后台添加：

```env
NEXT_PUBLIC_SELF_HOSTED_MODE=true
```

平台不会自动读取仓库里的 `.env.example`。

## 启用自己的 Supabase

如果你想启用账号、激活码、成年审核、便签墙、游戏大厅、应用市场或黑市等云端功能，请创建自己的 Supabase 项目，并在 Supabase SQL Editor 执行 `docs/` 目录下对应的 SQL：

- `docs/account-supabase.sql`：账号、会话、激活码。
- `docs/verify-supabase.sql`：成年审核与审核图片桶。
- `docs/notewall-supabase.sql`：便签墙。
- `docs/game-hall-supabase.sql`：游戏大厅。
- `docs/custom-app-market-supabase.sql`：应用市场。
- `docs/black-market-supabase.sql`：黑市。

然后在 `.env.local` 中关闭自部署模式，并填写你自己的服务端密钥：

```env
NEXT_PUBLIC_SELF_HOSTED_MODE=false
SUPABASE_URL=https://your-project.supabase.co
SUPABASE_SERVICE_ROLE_KEY=your-service-role-key
ACCOUNT_GATE_SECRET=your-random-long-secret
```

不要把 `.env.local` 提交到 Git。

如需使用通用生图代理，请部署自己的代理服务后再配置：

```env
NEXT_PUBLIC_IMAGE_GEN_PROXY_URL=https://your-image-proxy.example.com
```

默认留空时不会连接作者的代理服务。

## 常用命令

```bash
npm run dev
npm run build
npm run start
npm run lint
```

## License

本项目采用 GNU Affero General Public License v3.0 only（AGPL-3.0-only）开源。详见 [LICENSE](./LICENSE)。

## Credits

本项目为独立实现，但部分产品设计和系统抽象受 SillyTavern 启发，包括预设、正则处理、世界书 / lorebook / WorldInfo 等概念。

- SillyTavern: https://github.com/SillyTavern/SillyTavern
- SillyTavern 使用 AGPL-3.0 许可证。

更多说明见 [NOTICE](./NOTICE)。

## 备注

`NEXT_PUBLIC_*` 变量会公开到浏览器。不要把 Supabase `service_role`、后台管理密钥、第三方 API 私钥写进任何 `NEXT_PUBLIC_*` 变量。
