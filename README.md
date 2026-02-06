<div align="center">

# TaiGong (太公)

**Next-Generation Red Team Social Engineering Infrastructure**

**下一代红队社会工程学基础设施平台**

> **"姜太公钓鱼,愿者上钩。"**  
> _"In the silence of the deep, the willing bite."_

![License](https://img.shields.io/badge/license-MIT-blue.svg)
![Go Version](https://img.shields.io/badge/go-1.20+-lightgrey.svg)
![React](https://img.shields.io/badge/react-18-61DAFB.svg)
![Platform](https://img.shields.io/badge/platform-Linux%20%7C%20Windows%20%7C%20macOS-lightgrey.svg)
![Status](https://img.shields.io/badge/status-Pre--Release-orange.svg)

[中文文档](#中文文档) | [English](#english-documentation)

</div>

<p align="center">
  <img src="doc/screenshots/21_dashboard_active.png" alt="TaiGong Dashboard" width="100%">
</p>

---

## 中文文档

### 项目状态

**本项目目前处于预发布(Pre-Release)阶段。**

- **内部测试版本** - 当前版本为内部开发与红队实战验证阶段
- **暂不公开发布** - 短期内不会发布面向公众的通用版本
- **持续迭代中** - 核心功能与架构仍在快速演进

如需获取访问权限或了解发布计划,请联系项目维护团队。

**联系方式:**
- **GitHub Issues**: [提交 Issue](https://github.com/your-org/taigong/issues)
- **微信交流 (贡献/交流)**:
  
  <img src="doc/screenshots/wechat_contact.png" alt="WeChat QR" width="400">

---

### 项目背景

过去十年,**Gophish** 一直是开源钓鱼平台的标杆。简单好用是它的优点,但在 2025 年面对高度敏感的 EDR、普及的零信任架构,以及能读懂语义的 AI 邮件网关时,传统的 "Gophish + 静态模板" 打法已经不太够用了。

具体来说,现在碰到的主要问题是:

- **特征显著**: 默认指纹(RID 参数、Header、404 页面)已被所有威胁情报(TI)厂商标记
- **交互单一**: 缺乏对 MFA(多因素认证)场景的对抗能力,缺乏多步交互的支持
- **缺乏智能**: 依赖人工编写话术和模板,难以规模化生成高质量的鱼叉邮件
- **运维困难**: 单体架构,难以适应大规模、多租户的演练需求

**TaiGong (太公)** 就是为了解决这些问题而生的。它不只是一个工具,而是基于现代红队作战流程打造的**企业级基础设施平台**。就像 APT 行动需要的不只是一把枪,而是一个能统筹全局的作战指挥中心。

---

### 核心特性

#### 行动安全 (OpSec)

**指纹重写 (Silent by Default)**
- 编译级去指纹 - 移除所有默认 HTTP Header、TLS 特征和 X-Gophish 头
- 透明重定向 - 拒绝 404,所有非法访问流量被立即 302 重定向至目标官网
- 盲搜 RID - 不依赖固定参数名,自动扫描 URL 中的 7 位随机字符串
- 动态参数混淆 - 支持随机化参数名(`?token=`, `?q=`, `?sid=`)
- 三层架构原生支持 - 推荐 Nginx/CDN 前置,TaiGong 仅监听本地环回地址

> ![Weaponization Console](doc/screenshots/11_weaponization.png)

#### 武器化增强 (Weaponization)

**智能对抗体系**
- 魔术页面 - 用 JS 探针识别环境:安全沙箱看到 404,真实用户才能看到钓鱼表单
- 地理围栏 - 根据 IP 定位控制访问范围,只让特定国家/地区的人访问
- 反沙箱检测 - 识别并屏蔽安全沙箱、云安全平台探测
- 云服务黑名单 - 自动拦截来自 AWS、阿里云、腾讯云等数据中心的探测请求
- JA4/TLS 指纹识别 - 自动识别并屏蔽安全厂商指纹

#### 智能化能力 (Intelligence)

**AI 驱动的攻击编排**
- AI 话术生成 - 接入 OpenAI/智谱 GLM,一键生成高转化率的钓鱼邮件内容
- SOAR 自动化引擎 - 事件驱动编排(比如点击链接后自动发钓鱼邮件)
- 多阶段攻击链 - 支持条件触发的多步骤演练场景
- 智能评分系统 - 风险等级自动分类,员工安全意识评分
- 惊慌按钮 (Panic Button) - 目标点击链接时立即触发第二封"异地登录警报"

<img src="doc/screenshots/12_ai_assistant.png" alt="AI Assistant" width="45%"> <img src="doc/screenshots/17_ai_generation.png" alt="AI Generation" width="45%">

#### 多渠道投递 (Multi-Channel Delivery)

**全频道触达能力**
- 增强型邮件 - CID 内嵌图片、伪装 Outlook/Zoho 客户端、智能 MIME 降级
- 短信钓鱼 - 集成阿里云、腾讯云、Twilio 短信网关
- 即时通讯 - 支持钉钉、飞书、企业微信 Bot/Webhook 投递
- 二维码钓鱼 - 原生 CID 内嵌,离线可见
- 回复链攻击 (Reply-Chain) - 伪造邮件回复,折叠进受害者历史邮件流

![Sending Profiles](doc/screenshots/09_sending_email.png)

#### 企业级架构 (Enterprise-Grade)

**商业级管理平台**
- 完整 RBAC 权限系统 - 66 个细粒度权限,5 个系统角色
- 多租户隔离 - 支持子公司/演练批次独立 Tenant,内置计费逻辑
- 全量审计日志 - 所有配置改动、活动下发详细记录
- WebSocket 实时推送 - Dashboard 无需刷新实时更新
- 热配置加载 - 无需重启即可实时调整拦截策略

![Risk Dashboard](doc/screenshots/18_risk_dashboard.png)

#### 现代化前端 (Modern UI/UX)

**驾驶舱级交互体验**
- React 18 + Ant Design 5 - 现代化响应式管理界面
- TinyMCE 模板编辑器 - 变量智能提示、实时预览(桌面/移动双视图)
- 数据可视化 - ECharts 漏斗分析、地理热力图、雷达图、时间轴
- PDF 报告导出 - 一键生成符合企业审计要求的总结报告
- 国际化支持 - 完整支持中文/英文/日文三语切换
- 实时态势感知 - 武器化控制台,仪表盘形式展示防御状态

![Landing Page Editor](doc/screenshots/05_landing_page_editor.png)

#### 专业模板库 (Template Library)

**64 个实战级模板**
- 邮件登录类: 163/126/Sina/iCloud/Yahoo/ProtonMail/Zoho (10 个)
- VPN/认证类: Cisco/Fortinet/Pulse/Palo Alto/OpenVPN (5 个)
- 金融支付类: 微信支付/中行/工行/建行/招行/云闪付/PayPal (8 个)
- 物流快递类: 京东/菜鸟/圆通/中通/EMS (5 个)
- 社交平台类: QQ/微博/知乎/B站/小红书 (5 个)
- IT 通知类: 密码过期/系统升级/安全补丁/VPN 证书 (10 个)
- HR 类: 年终奖/绩效/请假/福利调查 (7 个)
- 其他场景: 14 个通用模板

![Template Library](doc/screenshots/16_preview_list.png)

---

### 功能截图 (Screenshots)

#### 1. 核心指挥舱 (Dashboard & Analytics)

| 全局态势感知 | 风险分析仪表盘 |
| :---: | :---: |
| ![Dashboard](doc/screenshots/21_dashboard_active.png) | ![Risk Dashboard](doc/screenshots/18_risk_dashboard.png) |
| **用户风险画像** | **风险趋势分析** |
| ![User Risk](doc/screenshots/20_user_risk.png) | ![Risk Charts](doc/screenshots/19_risk_charts.png) |
| **智能诊断中心** | |
| ![Diagnosis](doc/screenshots/07_smart_diagnosis.png) | |

#### 2. 武器化与AI (Weaponization & AI)

| 武器化控制台 | AI 助手对话 |
| :---: | :---: |
| ![Weaponization](doc/screenshots/11_weaponization.png) | ![AI Assistant](doc/screenshots/12_ai_assistant.png) |
| **AI 内容生成** | |
| ![AI Generation](doc/screenshots/17_ai_generation.png) | |

#### 3. 演练编排 (Campaign & Delivery)

| 新建演练任务 | 邮件投递配置 |
| :---: | :---: |
| ![New Campaign](doc/screenshots/13_new_campaign.png) | ![Sending Email](doc/screenshots/09_sending_email.png) |
| **短信投递 (SMS)** | **交互响应跟踪** |
| ![SMS](doc/screenshots/10_sending_sms.png) | ![Button Response](doc/screenshots/06_button_response.png) |

#### 4. 模板与资源 (Templates & Landing Pages)

| 落地页编辑器 | 落地页管理 |
| :---: | :---: |
| ![Editor](doc/screenshots/05_landing_page_editor.png) | ![Landing Page Mgmt](doc/screenshots/08_landing_page_mgmt.png) |
| **模板市场** | **模板预览 (163)** |
| ![Market](doc/screenshots/14_template_market.png) | ![Preview](doc/screenshots/15_preview_163.png) |
| **场景商店** | **用户组管理** |
| ![Scenario](doc/screenshots/03_scenario_store.png) | ![Users](doc/screenshots/04_users_groups.png) |

#### 5. 更多截屏 (More Screenshots)

| 运行详情 | 环境特征 |
| :---: | :---: |
| ![Detail](doc/screenshots/22.png) | ![Environment](doc/screenshots/23.png) |

#### 6. 帮助与列表 (Help & Lists)

| 落地页列表 | 模板帮助文档 |
| :---: | :---: |
| ![Landing List](doc/screenshots/01_landing_page_list.png) | ![Template Help](doc/screenshots/02_template_help.png) |

---

### 技术架构

```
┌─────────────────────────────────────────────────────┐
│            CDN / Nginx 反向代理层                     │
│         (TLS 终止, Domain Fronting)                  │
└────────────────┬────────────────────────────────────┘
                 │
┌────────────────▼────────────────────────────────────┐
│               TaiGong 核心服务                        │
│  ┌──────────────────┐  ┌──────────────────┐         │
│  │  Admin Server    │  │  Phish Server    │         │
│  │  (Port 3333)     │  │  (Port 80)       │         │
│  │  - React SPA     │  │  - Landing Pages │         │
│  │  - REST API      │  │  - Evasion Proxy │         │
│  │  - WebSocket     │  │  - Fingerprinting│         │
│  └──────────────────┘  └──────────────────┘         │
│                                                       │
│  ┌───────────────────────────────────────┐          │
│  │        Worker 调度引擎                 │          │
│  │  - Email/SMS/IM Sender                │          │
│  │  - SOAR Automation                    │          │
│  │  - AI Assistant                       │          │
│  └───────────────────────────────────────┘          │
└────────────────┬────────────────────────────────────┘
                 │
┌────────────────▼────────────────────────────────────┐
│          数据持久化层                                 │
│  - SQLite / MySQL (主数据库)                         │
│  - Redis (缓存 + 实时消息队列)                       │
└─────────────────────────────────────────────────────┘
```

**技术栈**

| 组件 | 技术 | 说明 |
|:---|:---|:---|
| **后端语言** | Go 1.20 | 高并发性能,单文件部署 |
| **前端框架** | React 18 + Ant Design 5 | 现代化 SPA,前后端分离 |
| **数据库** | SQLite / MySQL | 默认 SQLite,支持 MySQL 集群 |
| **缓存层** | Redis | 高性能缓存 + 实时消息 |
| **通信层** | WebSocket + REST API | 实时推送 + 标准接口 |
| **安全层** | JA4 + TLS Fingerprint | 底层对抗模块 |
| **任务调度** | Cron + Worker Pool | 自动化编排引擎 |

---

### 版本规划 (Licensing)

> [!NOTE]
> **快速迭代说明**  
> 本项目处于快速开发阶段，下表所列的功能分级可能与实际代码存在差异。但请放心：
> - ✅ **核心功能完整**：所有基础功能模块均已完整实现
> - ✅ **社区版可用**：Community 版本并非阉割版，可完整使用所有基础功能
> - 🚀 **持续更新**：我们正在快速完善各项高级特性

为保证项目的长期可持续发展与维护,TaiGong 分为**社区版**与**赞助版**。

| 功能模块 | 社区版 (Community) | 赞助版 (Sponsor) |
|:---|:---|:---|
| **【基础功能】** | | |
| 邮件钓鱼 | 完整支持 | 完整支持 |
| 模板编辑器 | 完整支持 (TinyMCE) | 完整支持 (TinyMCE) |
| 钓鱼页面 | 完整支持 | 完整支持 |
| 目标组管理 | 完整支持 | 完整支持 |
| 基础统计 | 完整支持 | 完整支持 |
| **【OpSec 对抗】** | | |
| 编译级去指纹 | 支持 | 支持 |
| 透明重定向 | 支持 | 支持 |
| 盲搜 RID | 支持 | 支持 |
| 地理围栏 | 基础版 (国家级) | **高级版 (省市级精准)** |
| 反沙箱检测 | 基础规则 | **高级特征库 + 自动更新** |
| 云服务黑名单 | 内置规则 | **自定义规则 + 实时情报** |
| JA4 指纹识别 | 支持 | **支持 + 自定义指纹库** |
| **【多渠道投递】** | | |
| 邮件投递 (SMTP) | 支持 | 支持 |
| 短信钓鱼 (SMS) | 不支持 | **支持 (阿里云/腾讯云/Twilio)** |
| 即时通讯 (IM) | 不支持 | **支持 (钉钉/飞书/企微)** |
| 二维码钓鱼 | 支持 | 支持 |
| 回复链攻击 | 不支持 | **支持** |
|  **【AI 与自动化】** | | |
| AI 话术生成 | BYOK (自带 API Key) | **内置服务 + 预置 Prompt 库** |
| SOAR 自动化引擎 | 基础规则 (Click/Submit 触发) | **高级编排 + Webhook 集成** |
| 多阶段攻击链 | 不支持 | **支持 (最多 5 阶段)** |
| Cron 定时调度 | 不支持 | **支持** |
| CobaltStrike 联动 | 不支持 | **支持** |
| **【企业级架构】** | | |
| RBAC 权限系统 | 不支持 | **支持 (66 权限 + 5 角色)** |
| 多租户隔离 | 不支持 | **支持 (完整数据隔离)** |
| 审计日志 | 基础日志 | **完整审计 + 合规导出** |
| WebSocket 实时推送 | 支持 | 支持 |
| 热配置加载 | 支持 | 支持 |
| **【数据分析】** | | |
| 基础统计图表 | 支持 | 支持 |
| ECharts 可视化 | 基础版 | **完整版 (漏斗/热力图/雷达图)** |
| 智能评分系统 | 不支持 | **支持 (风险分级 + 高危识别)** |
| PDF 报告导出 | 不支持 | **支持 (自定义模板)** |
| 数据对比分析 | 不支持 | **支持** |
| **【威胁情报】** | | |
| 域名黑名单检查 | 不支持 | **支持 (VirusTotal/Google Safe Browsing)** |
| IP 信誉检测 | 不支持 | **支持** |
| 自动暂停机制 | 不支持 | **支持 (域名被标记自动暂停)** |
| **【模板与资源】** | | |
| 内置模板数量 | 64 个专业模板 |
| 模板市场 | 不支持 | **支持 (访问官方模板库)** |
| 自定义模板 | 支持 | 支持 |
| **【使用限制】** | | |
| 用户配额 | 最多 10 个目标用户 | **无限制** |
| Campaign 配额 | 最多 5 个并发活动 | **无限制** |
| 租户配额 | 不支持多租户 | **无限制** |
| **【适用场景】** | | |
| 目标人群 | 个人研究者,学生,小型安全团队 | 专业红队,企业安全部,渗透测试公司 |
| 典型规模 | 小规模演练 (10-50 人) | 大规模演练 (50-10000+ 人) |
| **【定价】** | | |
| 价格 | 完全免费 | **暂未定价** |

> **Q: 社区版是"阉割版"吗?**  
> **A: 绝对不是。** 社区版包含所有核心架构改进(静默、去指纹、React 重构、64 个模板、完整的邮件钓鱼能力)。它完全足以应对中小规模的常规演练。赞助版是为需要极高隐蔽性(对抗顶级 EDR)、多渠道投递(SMS/IM)、企业级管理(RBAC/多租户)和自动化效率的专业团队设计。

---

### 快速开始

#### 环境要求

- Go 1.20+
- Node.js 18+ (前端构建)
- SQLite 3 / MySQL 5.7+
- Redis (可选,用于缓存和实时推送)

#### 从源码构建

```bash
# 1. 克隆仓库
git clone https://github.com/your-org/taigong.git
cd taigong

# 2. 构建后端
go mod download
go build -o taigong

# 3. 构建前端
cd admin-frontend
npm install
npm run build
cd ..

# 4. 配置文件
cp config.json.example config.json
# 编辑 config.json,配置端口、数据库、redirect_url 等

# 5. 启动服务
./taigong

# 6. 访问管理界面
# https://localhost:3333
# 默认账号: admin (密码查看启动日志)
```

#### Docker 快速部署

```bash
# 构建镜像
docker build -t taigong:latest .

# 运行容器
docker run -d \
  --name taigong \
  -p 3333:3333 \
  -p 80:80 \
  -v $(pwd)/taigong.db:/app/taigong.db \
  -v $(pwd)/config.json:/app/config.json \
  taigong:latest
```

---

### 文档与手册

**完整知识库** (30,000+ 字红队作战手册)

- [第 1 章: 基础设施与行动安全](doc/taigong-book/01_infrastructure.md)
- [第 2 章: 武器化与智能](doc/taigong-book/02_weaponization.md)
- [第 3 章: 投递与协议](doc/taigong-book/03_delivery.md)
- [第 4 章: 自动化与编排](doc/taigong-book/04_automation.md)
- [第 5 章: 态势感知与反侦察](doc/taigong-book/05_analytics.md)
- [第 6 章: 高级识别](doc/taigong-book/06_advanced_identification.md)
- [第 7 章: 信任劫持](doc/taigong-book/07_trust_hijacking.md)
- [第 8 章: 载荷工程](doc/taigong-book/08_artifact_engineering.md)


---

### 开发路线 (Roadmap)

#### 已完成 (v0.12.1 - Current)

- **第一阶段**: 编译级去指纹,透明重定向
- **第二阶段**: React 18 前端重构,64 个专业模板
- **第三阶段**: AI 辅助生成,SOAR 自动化,多租户架构
- **第四阶段**: RBAC 权限,审计日志,国际化(67%)

#### 近期计划 (Q1-Q2 2026)

**Phase 5: 高级对抗能力**
- HTML5 Smuggling 生成器 - 绕过邮件网关文件类型检测
- 一次性链接 (Burn-on-View) - 链接访问一次后立即失效
- 环境感知 Payload - 根据指纹动态选择下发 HTA/VBS/EXE
- Domain Front支持 - CDN 回源流量验证

**Phase 6: 凭证对抗**
- 2FA/MFA 中继模块 - 集成 Evilginx 风格的实时中继
- Session Token 捕获 - 反向代理目标登录页,提取 Cookie
- OAuth 劫持链 - 支持对接 SSO 单点登录场景

**Phase 7: 情报与运营**
- 威胁情报集成 - 自动检查域名 VirusTotal/Google Safe Browsing 状态
- 健康度监控 - 域名被标记时自动暂停 Campaign 或切换备用域名
- 团队协同增强 - 操作审计,多人协作锁机制

#### 长期愿景

- 构建完整的 Red Team Infrastructure 工具链(C2 联动、云原生部署)
- 开放插件生态,支持第三方模块扩展
- 持续对抗最新的 AI 邮件网关和行为分析系统


---

### 贡献指南

**当前阶段暂不接受公开贡献。**

本项目目前处于内部开发阶段,暂未开放外部 Pull Request。待正式发布后,我们将发布详细的贡献指南。
。

---

### 免责声明

**本工具仅供合法授权的红队演练及安全研究使用。**

使用本软件即表示您同意:
- 仅在获得明确书面授权的环境中使用
- 遵守所在国家/地区的网络安全法律法规
- 不将本工具用于任何恶意或非法目的
- 妥善保管捕获的敏感数据,不得泄露或滥用
- 开发者不对任何非法滥用行为承担法律责任

**Hack the Planet, Legally.**

---

### 开源协议

本项目基于 [MIT License](LICENSE) 开源。


---


### 联系方式

- **GitHub Issues**: [提交 Issue](https://github.com/your-org/taigong/issues)
- **微信交流 (贡献/交流)**:
  
  <img src="doc/screenshots/wechat_contact.png" alt="WeChat QR" width="200">

## English Documentation

### Project Status

**This project is currently in Pre-Release stage.**

- **Internal Testing** - Current version for internal development and red team validation
- **Not Publicly Available** - No immediate plans for public release
- **Professional Teams Only** - Access granted only to vetted professional red teams/security organizations
- **Active Development** - Core features and architecture still under rapid iteration

For access requests or release schedule inquiries, please contact the project maintainers.

---

### Background

Over the past decade, **Gophish** defined the standard for open-source phishing platforms. Simple and effective, but gradually aging. Facing 2025's defense systems — highly sensitive EDRs, ubiquitous Zero Trust architectures, and AI-powered email gateways capable of semantic understanding — the traditional "Gophish + static templates" approach has become increasingly inadequate.

The challenges we face:

- **Obvious Signatures**: Default fingerprints (RID parameters, headers, 404 pages) flagged by all threat intelligence platforms
- **Limited Interaction**: Lack of capability against MFA scenarios and multi-step interaction flows
- **No Intelligence**: Relies on manual content creation, difficult to scale spear-phishing campaigns  
- **Operational Burden**: Monolithic architecture unsuitable for large-scale, multi-tenant deployments

**TaiGong** was born to address these challenges. Not just a tool, but an **enterprise-grade infrastructure platform** based on modern red team operations. In APT campaigns, you need not a gun, but a command center orchestrating entire operations.

---

### Core Features

#### Operational Security (OpSec)

**Fingerprint Rewrite (Silent by Default)**
- Compile-time De-fingerprinting - Remove all default HTTP headers, TLS signatures, X-Gophish identifiers
- Transparent Redirect - No 404s; all unauthorized traffic redirected to legitimate sites
- Blind RID Search - Parameter-name agnostic, scans for 7-character tracking IDs
- Dynamic Parameter Obfuscation - Randomized parameter names (`?token=`, `?q=`, `?sid=`)
- 3-Tier Architecture Native - Designed for Nginx/CDN fronting, local-only listening

#### Weaponization

**Intelligent Adversarial System**
- Magic Page - JS probes: headless browsers see 404, real users see phishing forms
- Geo-Fencing - IP-based access restriction to specific countries/regions
- Anti-Sandbox Detection - Identify and block security sandbox and cloud security probes
- Cloud Datacenter Blacklist - Auto-block requests from AWS, Alibaba Cloud, Tencent Cloud
- JA4/TLS Fingerprinting - Identify and block security vendor fingerprints

#### Intelligence

**AI-Driven Attack Orchestration**
- AI Content Generation - Integrated OpenAI/GLM API for one-click phishing copy
- SOAR Automation Engine - Event-driven orchestration (If-Click-Then-Phish)
- Multi-Stage Campaigns - Conditional triggering for complex scenarios
- Smart Scoring System - Auto risk classification, employee awareness scoring
- Panic Button - Auto-trigger "suspicious login alert" email on link click

#### Multi-Channel Delivery

**Omni-Channel Reach**
- Enhanced Email - CID-embedded images, Outlook/Zoho mimicry, smart MIME fallback
- SMS Phishing - Alibaba Cloud, Tencent Cloud, Twilio gateway integration
- Instant Messaging - DingTalk, Feishu, WeChat Work bot/webhook delivery
- QR Code Phishing - Native CID embedding, offline-visible
- Reply-Chain Attack - Forge email replies, fold into victim's message history

#### Enterprise Architecture

**Commercial-Grade Platform**
- Complete RBAC - 66 fine-grained permissions, 5 system roles
- Multi-Tenancy - Isolated tenants for subsidiaries/batches, built-in billing
- Full Audit Logs - All config changes and campaign launches recorded
- WebSocket Real-Time - Dashboard updates without refresh
- Hot Configuration - Real-time policy adjustments without restart

#### Modern Frontend

**Cockpit-Level UX**
- React 18 + Ant Design 5 - Modern responsive admin interface
- TinyMCE Template Editor - Smart variable hints, live preview (desktop/mobile)
- Data Visualization - ECharts funnel, geo heatmap, radar chart, timeline
- PDF Export - One-click compliance-ready summary reports
- Internationalization - Full CN/EN/JP support
- Real-Time Situational Awareness - Weaponization console with dashboard metrics

#### Professional Templates

**64 Battle-Tested Templates**
- Email Services: 163/126/Sina/iCloud/Yahoo/ProtonMail/Zoho (10)
- VPN/Auth: Cisco/Fortinet/Pulse/Palo Alto/OpenVPN (5)
- Finance: WeChat Pay/BOC/ICBC/CCB/CMB/UnionPay/PayPal (8)
- Logistics: JD/Cainiao/YTO/ZTO/EMS (5)
- Social: QQ/Weibo/Zhihu/Bilibili/Xiaohongshu (5)
- IT Alerts: Password expiry/System upgrade/Security patch (10)
- HR: Bonus/Performance/Leave/Benefits (7)
- Misc: 14 general-purpose templates

---

### Architecture

```
┌─────────────────────────────────────────────────────┐
│            CDN / Nginx Reverse Proxy Layer           │
│         (TLS Termination, Domain Fronting)           │
└────────────────┬────────────────────────────────────┘
                 │
┌────────────────▼────────────────────────────────────┐
│               TaiGong Core Services                  │
│  ┌──────────────────┐  ┌──────────────────┐         │
│  │  Admin Server    │  │  Phish Server    │         │
│  │  (Port 3333)     │  │  (Port 80)       │         │
│  │  - React SPA     │  │  - Landing Pages │         │
│  │  - REST API      │  │  - Evasion Proxy │         │
│  │  - WebSocket     │  │  - Fingerprinting│         │
│  └──────────────────┘  └──────────────────┘         │
│                                                       │
│  ┌───────────────────────────────────────┐          │
│  │        Worker Scheduler                │          │
│  │  - Email/SMS/IM Sender                │          │
│  │  - SOAR Automation                    │          │
│  │  - AI Assistant                       │          │
│  └───────────────────────────────────────┘          │
└────────────────┬────────────────────────────────────┘
                 │
┌────────────────▼────────────────────────────────────┐
│          Data Persistence Layer                      │
│  - SQLite / MySQL (Primary Database)                │
│  - Redis (Cache + Real-time Message Queue)          │
└─────────────────────────────────────────────────────┘
```

**Technology Stack**

| Component | Technology | Description |
|:---|:---|:---|
| **Backend** | Go 1.20 | High concurrency, single binary deployment |
| **Frontend** | React 18 + Ant Design 5 | Modern SPA, decoupled architecture |
| **Database** | SQLite / MySQL | SQLite default, MySQL for scale |
| **Cache** | Redis | High-performance cache + real-time messaging |
| **Communication** | WebSocket + REST API | Live push + standard interfaces |
| **Security** | JA4 + TLS Fingerprint | Adversarial foundation |
| **Scheduler** | Cron + Worker Pool | Automation orchestration engine |

---

### Licensing Tiers

To ensure sustainable development, TaiGong offers **Community** and **Sponsor** editions.

| Feature | Community | Sponsor |
|:---|:---|:---|
| **【Core Features】** | | |
| Email Phishing | Full Support | Full Support |
| Template Editor | Full Support (TinyMCE) | Full Support (TinyMCE) |
| Landing Pages | Full Support | Full Support |
| Target Group Management | Full Support | Full Support |
| Basic Statistics | Full Support | Full Support |
| **【OpSec】** | | |
| Compile-time De-fingerprinting | Supported | Supported |
| Transparent Redirect | Supported | Supported |
| Blind RID Search | Supported | Supported |
| Geo-Fencing | Basic (Country-level) | **Advanced (Province/City-level)** |
| Anti-Sandbox Detection | Basic Rules | **Advanced Signature Library + Auto-update** |
| Cloud Datacenter Blacklist | Built-in Rules | **Custom Rules + Real-time Intel** |
| JA4 Fingerprinting | Supported | **Supported + Custom Fingerprint DB** |
| Code Obfuscation | Not Supported | **Supported (Anti-reversing)** |
| **【Multi-Channel Delivery】** | | |
| Email Delivery (SMTP) | Supported | Supported |
| SMS Phishing | Not Supported | **Supported (Aliyun/Tencent/Twilio)** |
| Instant Messaging (IM) | Not Supported | **Supported (DingTalk/Feishu/WeChat)** |
| QR Code Phishing | Supported | Supported |
| Reply-Chain Attack | Not Supported | **Supported** |
| **【AI & Automation】** | | |
| AI Content Generation | BYOK (Bring Your Own Key) | **Built-in Service + Preset Prompt Library** |
| SOAR Automation Engine | Basic Rules (Click/Submit triggers) | **Advanced Orchestration + Webhook Integration** |
| Multi-Stage Campaigns | Not Supported | **Supported (Up to 5 stages)** |
| Cron Scheduler | Not Supported | **Supported** |
| CobaltStrike Integration | Not Supported | **Supported** |
| **【Enterprise Architecture】** | | |
| RBAC Permission System | Not Supported | **Supported (66 permissions + 5 roles)** |
| Multi-Tenancy | Not Supported | **Supported (Full data isolation)** |
| Audit Logs | Basic Logging | **Complete Audit + Compliance Export** |
| WebSocket Real-Time Push | Supported | Supported |
| Hot Configuration Reload | Supported | Supported |
| **【Data Analytics】** | | |
| Basic Statistics Charts | Supported | Supported |
| ECharts Visualization | Basic Edition | **Full Edition (Funnel/Heatmap/Radar)** |
| Smart Scoring System | Not Supported | **Supported (Risk Grading + High-risk ID)** |
| PDF Report Export | Not Supported | **Supported (Custom Templates)** |
| Data Comparison Analysis | Not Supported | **Supported** |
| **【Threat Intelligence】** | | |
| Domain Blacklist Check | Not Supported | **Supported (VirusTotal/Google Safe Browsing)** |
| IP Reputation Detection | Not Supported | **Supported** |
| Auto-Pause Mechanism | Not Supported | **Supported (Auto-pause when flagged)** |
| **【Templates & Resources】** | | |
| Built-in Templates | 64 Professional Templates | 64 + **Custom Template Service** |
| Template Marketplace | Not Supported | **Supported (Access official library)** |
| Custom Templates | Supported | Supported |
| **【Operations & Support】** | | |
| Documentation | Full Docs + Community Issues | Full Docs + Community Issues |
| Technical Support | Community Support | **Priority Tickets + Dedicated Red Team Group** |
| Custom Development | Not Supported | **Supported (Paid custom development)** |
| SLA Guarantee | None | **99% Uptime Commitment** |
| **【Usage Limits】** | | |
| User Quota | Max 10 targets | **Unlimited** |
| Campaign Quota | Max 5 concurrent campaigns | **Unlimited** |
| Tenant Quota | Multi-tenancy not supported | **Unlimited** |
| **【Target Audience】** | | |
| Target Users | Individual researchers, students, small security teams | Professional red teams, enterprise security, pentest firms |
| Typical Scale | Small-scale exercises (10-50 people) | Large-scale exercises (50-10000+ people) |
| **【Pricing】** | | |
| Price | Completely Free | **Pricing TBD** |

> **Q: Is Community edition crippled?**  
> **A: Absolutely not.** Community includes all core architecture improvements (stealth, de-fingerprinting, React rebuild, 64 templates, full email phishing capabilities). It's fully capable for most standard engagements of small to medium scale. Sponsor is designed for teams requiring extreme stealth (top-tier EDR evasion), multi-channel delivery (SMS/IM), enterprise-grade management (RBAC/multi-tenancy), and automation efficiency.

---

### Quick Start

#### Requirements

- Go 1.20+
- Node.js 18+ (frontend build)
- SQLite 3 / MySQL 5.7+
- Redis (optional, for caching and real-time)

#### Build from Source

```bash
# 1. Clone repository
git clone https://github.com/your-org/taigong.git
cd taigong

# 2. Build backend
go mod download
go build -o taigong

# 3. Build frontend
cd admin-frontend
npm install
npm run build
cd ..

# 4. Configure
cp config.json.example config.json
# Edit config.json for ports, database, redirect_url, etc.

# 5. Run
./taigong

# 6. Access admin panel
# https://localhost:3333
# Default: admin (password in startup logs)
```

#### Docker Deployment

```bash
# Build image
docker build -t taigong:latest .

# Run container
docker run -d \
  --name taigong \
  -p 3333:3333 \
  -p 80:80 \
  -v $(pwd)/taigong.db:/app/taigong.db \
  -v $(pwd)/config.json:/app/config.json \
  taigong:latest
```

---

### Documentation

**Complete Knowledge Base** (30,000+ word red team playbook)

- [Chapter 1: Infrastructure & OpSec](doc/taigong-book/01_infrastructure.md)
- [Chapter 2: Weaponization & Intelligence](doc/taigong-book/02_weaponization.md)
- [Chapter 3: Delivery & Protocols](doc/taigong-book/03_delivery.md)  
- [Chapter 4: Automation & Orchestration](doc/taigong-book/04_automation.md)
- [Chapter 5: Analytics & Counter-Reconnaissance](doc/taigong-book/05_analytics.md)
- [Chapter 6: Advanced Identification](doc/taigong-book/06_advanced_identification.md)
- [Chapter 7: Trust Hijacking](doc/taigong-book/07_trust_hijacking.md)
- [Chapter 8: Artifact Engineering](doc/taigong-book/08_artifact_engineering.md)

---

### Roadmap

#### Completed (v0.12.1 - Present)

- **Phase 1**: Compile-time de-fingerprinting, transparent redirect
- **Phase 2**: React 18 rebuild, 64 professional templates
- **Phase 3**: AI generation, SOAR automation, multi-tenancy
- **Phase 4**: RBAC permissions, audit logs, i18n (67%)

#### Upcoming (Q1-Q2 2026)

**Phase 5: Advanced Evasion**
- HTML5 Smuggling Generator - Bypass email gateway file detection
- One-Time Links (Burn-on-View) - Self-destructing links
- Environment-Aware Payloads - Dynamic HTA/VBS/EXE delivery based on fingerprints
- Domain Fronting Support - CDN origin verification

**Phase 6: Credential Warfare**
- 2FA/MFA Relay Module - Evilginx-style real-time relay
- Session Token Capture - Reverse proxy target login, extract cookies
- OAuth Hijack Chains - SSO single sign-on scenarios

**Phase 7: Intelligence & Operations**
- Threat Intel Integration - Auto-check VirusTotal/Google Safe Browsing
- Health Monitoring - Auto-pause campaigns when domains flagged
- Team Collaboration - Operation audits, multi-user locking

#### Long-Term Vision

- Complete Red Team Infrastructure toolchain (C2 integration, cloud-native)
- Open plugin ecosystem for third-party extensions
- Continuous adversarial updates against latest AI gateways and behavioral analysis

---

### Contributing

**Public contributions not yet accepted.**

This project is currently in internal development. External pull requests are not open at this time. Upon official release, detailed contribution guidelines will be published.

---

### Disclaimer

**This tool is intended ONLY for authorized red team engagements and security research.**

By using this software, you agree to:
- Use only in explicitly authorized environments with written permission
- Comply with cybersecurity laws in your jurisdiction
- Not use for any malicious or illegal purposes
- Properly safeguard captured sensitive data, no leakage or abuse
- Developers bear no legal responsibility for unauthorized misuse

**Hack the Planet, Legally.**

---

### License

Licensed under [MIT License](LICENSE).

---

### Contact

- **GitHub Issues**: [Submit Issue](https://github.com/your-org/taigong/issues)
- **WeChat (Contribution/Discussion)**:

  <img src="doc/screenshots/wechat_contact.png" alt="WeChat QR" width="200">

---

<div align="center">

**© 2026 TaiGong Security Lab | 太公安全实验室**

**Made with care for the Red Team Community**

</div>
