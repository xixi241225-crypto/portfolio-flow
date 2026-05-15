# 🌿 PortfolioFlow

> 用一句话,给任何领域的设计师做一个有生命力的作品集网站。
> 黑底景观绿 or 深蓝科技感 or 米白人文风——主题随选,内容由 AI 协同生成,部署一行命令。

[![EdgeOne Pages](https://img.shields.io/badge/Deployed_on-EdgeOne_Pages-blue)](https://edgeone.ai/products/pages)
[![Skill Version](https://img.shields.io/badge/Skill-v1.0-green)]()
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

---

## ✨ 这是什么

**PortfolioFlow 是一个 WorkBuddy Skill**——你只需要告诉它"我是 XX 领域的设计师,我的名字叫 XX",它会:

1. 选择最适合的视觉主题(5 套预设 + hex 自定义)
2. 生成符合你领域的专业作品集结构
3. 配置好 JWT 鉴权 + KV 存储 + 访问统计 + AI 助手
4. 输出一份可以一行命令部署到 EdgeOne Pages 的完整项目

**真实场景**:从"我想要一个作品集"到"网站上线",时间从原本的 1 周缩短到 **15 分钟**。

---

## 🎬 两个真实部署案例

### Demo 1 · 李博 · 景观设计作品集

- **URL**: https://lee-landscape-portfolio-bebzulxs.edgeone.cool
- 主题:`dark-forest`(黑底景观绿)
- Hero 叙事:`revolution`(三次生产力革命)
- 内容:6 个真实景观项目(滨海 / 儿童 / 口袋公园 / 商业街 / 居住区 / 湿地)

### Demo 2 · Nova Chen · UX 作品集

- **URL**: https://nova-ux-portfolio-d6xfgkjk.edgeone.cool
- 主题:`deep-ocean`(深蓝科技感)
- Hero 叙事:`plain`(极简)
- 内容:6 个 UX 项目(金融 App / SaaS / 健身追踪 / 跨境电商 / 设计系统 / 社区通知)

**两个 Demo 用的是同一个 Skill**——这就是 Skill 通用性的最强证据。

---

## 🎯 核心能力

| 能力 | 说明 |
|---|---|
| **🎨 5 套预设主题** | dark-forest / dawn-beige / deep-ocean / sunset-coral / pure-minimal,以及任意 hex 自定义 |
| **🌐 5 大设计领域** | 景观 / 平面 / 室内 / 工业 / UX——每个领域有专属词库 |
| **📖 3 种 Hero 叙事** | plain / revolution / custom,适配不同人设 |
| **🔐 JWT 鉴权** | Web Crypto API 实现,零依赖,HttpOnly Cookie |
| **💾 KV 存储** | 3 个命名空间分别管理用户 / 作品 / 统计 |
| **✨ 双引擎 AI 生成** | 默认模板引擎(零成本)+ 可选 DeepSeek API(增强模式) |
| **📊 访问统计看板** | PV/UV 去重 + 30 天趋势图 + Top 10 作品 |
| **💬 访客 AI 助手** | 前台浮动对话框,可问"擅长什么"、"最新作品"等 |

---

## 🏗️ 技术架构

```
浏览器
  ↓
EdgeOne Pages (CDN + Edge Functions)
  ├── 静态资源 (HTML / CSS / 图片)
  └── Edge Functions (V8 isolate)
      ├── _middleware.js     ← JWT 鉴权 + CORS
      ├── api/auth.js        ← 登录 / 改密 / 初始化
      ├── api/portfolios.js  ← 作品 CRUD + 种子数据
      ├── api/generate.js    ← AI 双引擎生成
      ├── api/stats.js       ← 访问统计 + 看板
      └── api/assistant.js   ← 访客 AI 对话
  ↓
KV Storage × 3
  ├── PORTFOLIO_USERS   ← 用户账号 (盐 + SHA-256)
  ├── PORTFOLIO_WORKS   ← 作品数据 + 索引
  └── PORTFOLIO_STATS   ← PV/UV + 来源 + 时间序列
```

**关键技术决策**:

- ✅ Edge Function 全部用 **Web Crypto API**——不引入任何 npm 包
- ✅ JWT 用 **HS256 + 8 字节随机盐**,zero npm dependency
- ✅ KV 索引设计:`index:all` / `index:featured` / `index:by-type:{type}`,3 倍查询效率
- ✅ UV 去重:IP + UA 哈希,每日重置集合,自动 trim 防超限
- ✅ 看板趋势图用**纯 SVG 自绘**——不引 chart.js / echarts

---

## 📁 Skill 结构

```
skills/portfolio-flow/
├── SKILL.md                      ← 主控决策树(204 行)
├── references/                   ← 7 份按需加载的知识库(3405 行)
│   ├── design-system.md          ← 5 套主题 + hex 算法
│   ├── portfolio-knowledge.md    ← 5 个领域词库
│   ├── auth-jwt.md               ← JWT 完整实现
│   ├── portfolio-crud.md         ← KV CRUD + 索引设计
│   ├── ai-generate.md            ← 双引擎 AI 实现
│   ├── stats-kv.md               ← UV 去重 + SVG 看板
│   └── deploy-workflow.md        ← 端到端部署流程
└── templates/                    ← 可直接复用的代码模板(4513 行)
    ├── index.html                ← 首页(Hero + 作品 + 三次革命)
    ├── portfolio-detail.html     ← 详情(Markdown + Lightbox)
    ├── admin.html                ← 后台(看板 + CRUD + AI + 设置)
    ├── login.html                ← 登录页
    ├── styles.css                ← 5 套主题 CSS 变量
    └── functions/                ← 6 个 Edge Function
        ├── _middleware.js
        └── api/{auth,portfolios,generate,stats,assistant}.js
```

**总行数:8122 行**——包含详尽注释和工程实践。

---

## 🚀 快速使用

### 在 WorkBuddy 中调用 Skill

```
我想做一个作品集网站,我是景观设计师,叫李博。
帮我用 PortfolioFlow Skill 生成一份。
```

Skill 会按 SKILL.md 决策树:

1. 询问关键信息(领域 / 名字 / 主题偏好)
2. 加载对应 references(landscape 词库 / dark-forest 主题等)
3. 从 templates 复制并替换占位符
4. 生成完整项目目录
5. 引导部署到 EdgeOne Pages

### 手动部署(从 GitHub clone 后)

```bash
cd ~/Desktop/my-portfolio

# 1. 部署到 EdgeOne Pages
edgeone pages deploy . -n my-portfolio

# 2. 控制台绑定 3 个 KV 命名空间(变量名:PORTFOLIO_USERS / WORKS / STATS)

# 3. 控制台配置环境变量 JWT_SECRET

# 4. 重新部署一次让 KV 和环境变量生效
edgeone pages deploy . -n my-portfolio

# 5. 浏览器 Console 初始化
#    fetch('/api/auth?init=true').then(r=>r.json()).then(d=>console.log(d))
#    fetch('/api/portfolios?init=true').then(r=>r.json()).then(d=>console.log(d))
```

---

## 🎨 主题展示

| 主题 | 适用场景 |
|---|---|
| **dark-forest** 🌿 黑底景观绿 | 景观 / 建筑 / 自然题材 |
| **dawn-beige** ☕ 米白人文 | 平面 / 文创 / 出版 |
| **deep-ocean** 🌊 深蓝科技 | UX / 互联网 / 数字产品 |
| **sunset-coral** 🌅 暖色生活 | 室内 / 软装 / 生活方式 |
| **pure-minimal** ⚪ 极简黑白 | 工业 / 设计师作品集 / 通用 |

每套主题封装在 CSS 变量中,**切换主题只需要改 `<html data-theme="xxx">` 一处**。

---

## 📊 灵感与致谢

PortfolioFlow 的设计思路受这些产品启发:

- **Tencent EdgeOne Pages**——边缘计算 + KV + Functions 全栈能力
- **Anthropic Claude Skills 协议**——SKILL.md 主控 + references 按需加载的分层架构
- **Cargo / Squarespace**——独立设计师作品集的视觉范式

---

## 🤝 贡献

欢迎 Issue / PR / 主题扩展。如果你用 PortfolioFlow 生成了自己的作品集,在 Issue 里贴链接,我会更新到 README 的"实战案例"板块。

---

## 📄 License

MIT
