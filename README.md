# 淘到宝引擎（Taodaobao Engine）

面向销售与售前工作的 AI 辅助系统。项目围绕客户磋商流程，把资料整理、公开情报、招标响应、方案生成、异议演练和后续研究连接起来，并为生成内容保留来源与人工确认环节。

本仓库是 [TTJTR/taodaobao-engine](https://github.com/TTJTR/taodaobao-engine) 团队项目的个人展示版本，保留原有提交历史。王筱涵（[@wxh042](https://github.com/wxh042)）参与后端开发；产品、前端、AI 服务及其他模块由团队协作完成。项目参加 2026 年飞书 AI 人才大赛，进入北部地区 17 强。

## 功能概览

- **资料与知识底座：**接入飞书文档、妙记和文本资料，管理经验、原子能力及来源快照。
- **磋商前：**公开情报搜索、客户画像建议、招标要求拆解与响应矩阵。
- **磋商中：**可信资料检索、快速方案、客户异议演练与互动 HTML 展示。
- **磋商后：**多阶段 Deep Research、专家协作、事实审计与经验沉淀。
- **运行与权限：**异步任务、模型连接状态、飞书登录、邀请码及人工审核门禁。

详细的功能边界见 [V2.0 产品说明](docs/V2.0最终产品与真实运行PRD.md) 和 [当前能力审计](docs/真实性审计与当前能力边界_20260815.md)。部分历史验收文档记录了当时使用的 Mock 联调，不能作为当前线上能力证明。

## 技术组成

后端使用 Python、FastAPI、SQLAlchemy、Alembic、PostgreSQL/pgvector 和 Redis；异步 Worker 处理长任务。静态前端位于 `static/`。生产编排使用 Docker Compose，Caddy 提供 HTTPS 入口。AI 与飞书功能依赖外部服务凭据。

## 本地查看与运行

开发环境需要 Python 3.11+。安装依赖并启动 API：

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install -e ".[dev]"
Copy-Item .env.example .env
uvicorn main:app --reload
```

配置项说明见 `.env.example`。数据库相关功能需要 PostgreSQL 与 pgvector，并执行迁移；未配置模型、飞书或数据库的功能会明确报告不可用，不会自动返回模拟成功结果。不要将填有真实密钥的 `.env` 提交到仓库。

部署步骤、域名及服务依赖见 [生产部署说明](docs/生产部署说明.md)。本仓库公开代码不等于已提供可访问的线上实例。

## 仓库结构

| 路径 | 内容 |
| --- | --- |
| `app/` | API、业务服务、数据模型及外部服务适配 |
| `alembic/` | 数据库迁移 |
| `static/` | 前端页面与资源 |
| `sidecar/` | 可选扩展服务 |
| `tests/` | 自动化测试 |
| `docs/` | 产品、接口、联调及部署文档 |
| `fixtures/` | 明确标记为虚构的验收资料 |

第三方组件与素材的许可说明见 [THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md)。项目整体未单独声明开源许可证；复用代码前请联系相应权利人。
