# 多通道下载站 · Multi-Channel Download

> 粘贴一个下载链接，自动生成多条加速通道地址，一键复制或下载。
> Paste a download link, get multiple accelerated URLs, copy or download in one click.

本项目是一个**纯前端 + Cloudflare 边缘函数**的公益下载工具站，面向网络访问受限地区的普通用户。本站不托管任何文件，只负责把原始链接拼接成各家公益加速通道的地址，真正的下载由各通道提供。

A free, non-profit download helper built with a static front-end and Cloudflare Workers. It hosts **no files** — it only rewrites your link into URLs for public acceleration channels; the actual transfer is provided by those channels.

---

## 功能特性 · Features

| 功能 Feature | 说明 Description |
| --- | --- |
| 🔗 一键提取 | 支持 GitHub / GitLab / HuggingFace / 任意直链；自动解析 `latest` 跳转链接为真实直链，自动把 GitHub blob 网页地址转成 raw 文件直链。<br/>Accepts GitHub / GitLab / HuggingFace / any direct link. Resolves `latest` redirects and converts GitHub blob pages to raw file URLs. |
| ⚡ 多通道测速 | 打开页面自动测速，之后每 5 分钟重测；按真实吞吐排序，自动选中推荐通道中最快的一条。<br/>Auto speed test on load, refreshed every 5 minutes. Channels sort by real throughput and the fastest recommended one is selected. |
| 🎨 通道分类 | 蓝 = GitHub 专用，绿 = 通用直链，紫 = 多平台，灰 = 自定义前缀。提取后非推荐通道会淡化下沉，不参与排序。<br/>Blue = GitHub only, Green = general, Purple = multi-platform, Gray = custom. After extraction, non-recommended channels dim and sink to the bottom. |
| 🧩 智能回退 | GitHub 专用通道遇到非 GitHub 链接、Xget 遇到不支持的站点时，自动改用通用通道，不会出现"点了跳网页却不下载"。<br/>GitHub-only channels fall back to a general channel for non-GitHub links; Xget falls back for unsupported sites. |
| ✂️ 复制 / 下载 | 复制拼接好的加速地址，或在新标签页直接打开下载。<br/>Copy the accelerated URL or open it in a new tab to download. |
| 🛠 自定义前缀 | 可填入自己搭建或购买的加速前缀，作为备用通道。<br/>Bring your own accelerator prefix as a backup channel. |
| 🌐 中英双语 | 首次打开按浏览器系统语言自动显示中文或英文；左侧「加速通道」旁的 `EN` / `中` 按钮可随时手动切换，选择会被记住。<br/>Language follows your browser on first visit; toggle with the `EN` / `中` button next to "Channels". Your choice is remembered. |

---

## 使用方法 · How to use

1. **粘贴链接** — 把下载直链、GitHub blob / release 页面地址，或官方 `latest` 跳转链接粘进输入框。
   **Paste a link** — a direct file URL, a GitHub blob / release page, or an official `latest` redirect URL.
2. **点击「提取」/ "Extract"** — 页面会解析出真实直链并列出推荐通道。
   Click **Extract** — the real direct link is resolved and recommended channels are listed.
3. **选择通道** — 左侧栏按速度排序，点亮的为推荐通道；也可点结果卡里的通道标签快速选中。
   **Pick a channel** — the sidebar is sorted by speed; highlighted ones are recommended. You can also click a chip in the result card.
4. **复制或下载** — 点「复制地址」拿到加速链接，或点「下载」直接打开。
   **Copy or download** — click "Copy URL" to get the accelerated link, or "Download" to open it directly.

> 💡 通道不可用时换一条即可；免费公益通道可能限速或临时失效。
> 💡 If a channel fails, just switch to another one. Free public channels may be rate-limited or temporarily down.

---

## 常见问题 · FAQ

**Q: 只差一点就下完却失败 / 始终下不了？**
多为浏览器拦截。换 Chrome / Edge / Firefox 重试即可。
Usually browser blocking. Try Chrome / Edge / Firefox.

**Q: 通道失效或跑路了怎么办？**
本站提供多家通路，换一条即可；仍不行可通过帮助弹窗里的 UP 按钮反馈。
We provide multiple routes — just switch. Still stuck? Use the UP button in the help dialog to send feedback.

**Q: 想用自己的加速地址？**
选「自定义前缀」通道，按格式 `https://你的加速前缀/` 填写。
Select the "Custom prefix" channel and enter `https://your-prefix/`.

**Q: 为什么要登录 B 站才能私信？**
私信是账号对账号的行为，网页和客户端都必须先登录自己的 B 站账号，这一点无法绕过。手机端点 UP 按钮会优先唤起 B 站 App，电脑端打开网页私信。
Messages require a logged-in Bilibili account on both web and app. On mobile the UP button opens the Bilibili app first; on desktop it opens the web message page.

---

## 技术说明 · Technical notes

### 目录结构 · Structure

```
.
├── index.html                 # 单文件前端：界面、i18n、测速与排序逻辑
├── functions/
│   └── api/
│       ├── extract.js         # 解析目标页图标，返回预览与可用直链
│       ├── icon.js            # 同域图片转发代理，规避防盗链与跨域
│       └── resolve.js         # 跟随重定向，解析 latest 跳转链接为真实直链
├── static/
│   ├── icon/favicon.ico
│   ├── img/background*.webp
│   └── svg/snake-Light.svg
└── _headers                   # Cloudflare Pages 缓存规则
```

### 部署 · Deploy

1. Fork / 克隆本仓库到自己的 GitHub。
   Fork or clone this repo to your GitHub.
2. 在 Cloudflare Pages 连接该仓库，构建命令留空（纯静态），根目录为 `/`。
   Connect the repo to Cloudflare Pages; leave the build command empty, root `/`.
3. 部署完成后，`functions/api/*` 会自动作为边缘函数生效。
   After deployment, `functions/api/*` work as edge functions automatically.

### 隐私与免责 · Privacy & disclaimer

- 本站为非盈利站，只为方便你我这样的普通用户。
  This is a non-profit site, made to help everyday users like you and me.
- 本站不收集用户信息，不托管或分发任何文件，仅做链接拼接与图标代理。
  No user data is collected, no files are hosted — only URL rewriting and icon proxying.
- 加速通道由第三方公益服务提供，可能随时调整或停服；使用前请自行判断。
  Acceleration channels are third-party public services and may change or shut down at any time.

---

## 反馈 · Feedback

有问题或建议，欢迎通过帮助弹窗里的 **UP** 按钮联系作者（B 站 UID：1476255996）。
For issues or suggestions, contact the author via the **UP** button in the help dialog (Bilibili UID: 1476255996).
