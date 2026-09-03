# FLOW 官网（GitHub Pages）

给摄影师的 macOS 选片工具官网。纯静态部署，下载计数用免费的 Upstash Redis 实现。

## 目录

- `index.html` — 单页官网
- `FLOW.dmg` — macOS 安装包（下载链接指向它）
- `.nojekyll` — 关掉 GitHub Pages 的 Jekyll 处理

## 一、部署到 GitHub Pages

1. 在 GitHub 新建一个仓库（例如 `flow`），**公开或私有都行**。
2. 本地把这个目录推上去：

   ```bash
   cd /Users/wanghaoyu/DS/flow-website
   git init
   git add .
   git commit -m "FLOW 官网"
   git branch -M main
   git remote add origin https://github.com/你的用户名/flow.git
   git push -u origin main
   ```

3. 仓库 → **Settings → Pages**：
   - Source 选 **Deploy from a branch**
   - Branch 选 **main**，目录选 **/ (root)**，保存。
4. 等一两分钟，访问 `https://你的用户名.github.io/flow/` 即可。

> 注：下载链接用相对路径 `FLOW.dmg`，放在根目录能直接下载，无需改动。

## 二、下载计数（可选）

GitHub Pages 没有后端，所以计数用一个免费的 Redis 服务 **Upstash**：

1. 到 [upstash.com](https://upstash.com) 注册（免费额度足够）。
2. 创建一个 Redis 数据库，选 **REST API** 模式。
3. 复制它给你的 **REST URL** 和 **token**，填进 `index.html` 底部脚本里：

   ```js
   var UPSTASH_URL = "https://xxxx.upstash.io";
   var UPSTASH_TOKEN = "xxxx";
   ```

4. 提交推送即可。之后每次点「下载」，`downloads` 这个 key 会 +1。

### 后门查询下载量

浏览器或命令行访问（带上你自己的 token）：

```bash
curl -H "Authorization: Bearer 你的token" https://xxxx.upstash.io/get/downloads
```

返回类似 `{"result": 37}`，`37` 就是下载次数。

> ⚠️ 说明：因为是纯静态，token 会出现在前端 JS 里（查看源码能看到）。对下载计数这种低敏感数据通常可接受；如果你想要「真正保密的查询」，需要再加一个极小的 serverless 后端（Cloudflare Worker / Vercel / Netlify），我可以再帮你改。
