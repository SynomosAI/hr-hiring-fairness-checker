# hr-hiring-fairness-checker · 招聘公平性与用工合规核验域

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Domain: hr](https://img.shields.io/badge/domain-hr-9cf.svg)](domain.manifest.json)
[![Profile: domain/1.0](https://img.shields.io/badge/profile-domain%2F1.0-blue.svg)](domain.manifest.json)
[![Status: RESERVED](https://img.shields.io/badge/status-RESERVED-orange.svg)](domain.manifest.json)
[![Gate levels](https://img.shields.io/badge/gate-5%20levels-lightgrey.svg)](gate-map.json)

> **Status: RESERVED (placeholder · open for adoption)** — This repo has scaffolded the
> industry-access four-file skeleton per the platform profile, and awaits a qualified
> operator in this domain to claim it and fill in real rules.

把「AI 参与招聘到什么程度、有没有歧视风险」拆成可核验的属性，让 AI 只做提示，不做录用结论。

> Turns "how much does AI participate in hiring, and is there discrimination risk" into
> verifiable attributes: the AI only surfaces prerequisites, it never makes a hiring decision.

**Keywords / 关键词**：AI 招聘公平性 · 算法歧视 · 受保护特征排除 · 知情权披露 · 人工复核 · 用工合规 · 劳动合同 · 属性核验 · 红线 · gate-map · privacy-by-default · AI hiring fairness · bias audit · protected attributes · disclosure · human review · employment compliance · MCP checker

---

## ✨ 这个域管什么 / What this domain covers

招聘场景 AI 应用公平性、披露与用工合规核验。

AI 用于简历筛选、面试评估时，歧视风险与知情权是核心问题。AI 能做的是把披露、特征排除、人工复核等要件摆清楚，录用什么人永远由人决定。

This domain governs **fairness, disclosure and employment compliance of AI used in hiring**.
When AI screens resumes or assesses interviews, discrimination risk and the candidate's right
to know are the central issues. The checker surfaces disclosure, protected-feature exclusion
and human-review prerequisites — who gets hired is always decided by a human.

---

## 📇 域标识 / Domain identity

| 项 / Item | 值 / Value |
|---|---|
| 域 ID / Domain ID | `hr`（全局唯一，一经分配不复用 / globally unique, never reused） |
| 域名称 / Domain name | 人力 · 招聘公平性与用工合规 |
| Profile 版本 / Profile version | `domain/1.0` |
| 当前状态 / Status | `RESERVED` |
| 占位时间 / Reserved since | 2026-09-12 |
| Checker 端点 / Checker endpoint | `mcp://pending-claim/hr-checker`（待认领） |

---

## 🧾 属性清单 / Attribute catalogue

| 属性键 / Key | 类型 / Type | 说明 / Meaning |
|---|---|---|
| `hr.ai_screening_disclosure` | boolean | 是否向候选人披露 AI 参与筛选或评估 |
| `hr.protected_attribute_exclusion` | enum | 是否排除性别/年龄/地域/健康/婚育等受保护特征，须用人单位留痕 · excluded/partially_excluded/not_excluded/unverified |
| `hr.human_review_available` | boolean | 是否为候选人提供人工复核与申诉渠道 |
| `hr.job_qualification` | enum | 岗位依法要求的职业资格持证情况 · license_valid/license_missing/not_required |
| `hr.labor_contract_status` | enum | 劳动合同订立状态 · signed/pending/none/dispatched |

---

## 🚫 本域红线（不可逾越，机器可读）/ Red lines

1. 是否排除受保护特征须由用人单位核验并留痕，AI 不得自行认定无歧视
2. 不得输出对具体候选人的录用、淘汰建议或排名结论
3. 不得输出涉及性别、年龄、地域、健康状况、婚育状况等受保护特征的筛选判断

> 红线在 `gate-map.json` 中均有对应阻断规则。平台校验器会检查「每条红线都有规则覆盖」，
> 缺失即校验失败——**制度与系统不允许不同步**。

---

## 📦 行业接入四件套 / The four-file access kit

| 文件 / File | 作用 / Role |
|---|---|
| `domain.manifest.json` | 本域声明：属性清单、签发方要求、有效期、红线 |
| `gate-map.json` | 本域「什么动作要多少摩擦」：silent / warn / confirm / block / require-owner |
| `privacy.json` | 本域隐私声明：默认关闭、最小必要、可撤回、可删除 |
| `checker.io.json` | 本域核验器（MCP 工具，**只出示核验，不下判定**） |

---

## 🚀 快速开始 / Quick Start

> 前置：本仓是**声明式配置仓**，无可执行二进制；你只需要 git 与 Python 3（用于 JSON 自检）。

```bash
# 1. 克隆本仓
git clone https://github.com/SynomosAI/hr-hiring-fairness-checker.git
cd hr-hiring-fairness-checker

# 2. 自检四件套 JSON 是否合法（Python 标准库即可）
python -m json.tool domain.manifest.json > /dev/null
python -m json.tool gate-map.json > /dev/null
python -m json.tool privacy.json > /dev/null
python -m json.tool checker.io.json > /dev/null
```

跑通后，用自然语言提问即可，例如「**这个招聘流程有没有向候选人披露 AI？**」
「**受保护特征排除有没有留痕？**」。

---

## 📖 示例 / Examples

### 示例 1：列出本域声明的全部属性键

```bash
python -c "import json;m=json.load(open('domain.manifest.json',encoding='utf-8'));[print(a['key'],a['type']) for a in m['attributes']]"
```

### 示例 2：checker 工具的一次调用契约（见 checker.io.json）

```json
{
  "subject_ref": "mpp:cn:9f2c1a7b",
  "attribute_keys": ["hr.ai_screening_disclosure", "hr.human_review_available"],
  "evidence": ["tsa:2026-09-12#sha256:3b1f..."],
  "request_id": "req-20260912-0001"
}
```

checker 只回 `shown / missing / expired / unverifiable` 四种状态与出处链，
**永远不回**「录用谁 / 淘汰谁 / 排名 / 是否歧视」。

---

## 🗂 仓库结构 / Repository layout

```text
hr-hiring-fairness-checker/
├── README.md              # 本文件
├── README.en.md           # 英文版说明
├── domain.manifest.json   # 域声明（属性 / 签发方 / 红线）
├── gate-map.json          # 动作摩擦分级
├── privacy.json           # 隐私声明（默认关闭 / 最小必要）
├── checker.io.json        # checker MCP 工具 I/O 契约
├── CODEOWNERS             # 代码所有者 @SynomosAI/synomosai-gov
├── LICENSE                # MIT License（© 2026 SynomosAI）
├── SECURITY.md            # 安全策略
├── AGENTS.md              # 给 AI Agent 的工作指引
├── llms.txt               # 给 AI 的最小索引
└── llms-full.txt          # 给 AI 的完整文本
```

---

## 🧭 核心原则 / Core principles

**平台只当擂台，不当货架。** 本域的核验器只回答「这条声明是否可核验、缺什么要件」，
不回答「这件事是否合规、该不该做」。判定权在本域的资质方、监管方与人。

**Privacy is a gating condition, not a remediation item.** Missing `privacy.json` or any
required field mismatch fails conformance outright — it is a rejection, not a warning.

---

## 📚 参考依据 / References

- 中华人民共和国劳动法
- 中华人民共和国劳动合同法
- 中华人民共和国就业促进法
- 生成式人工智能服务管理暂行办法

> 上列依据仅用于说明本域属性的来源与口径，不构成法律意见。具体适用以现行有效文本与主管部门解释为准。

---

## 📮 认领方式 / How to adopt

本域面向具备相应资质的机构开放。认领后请：

1. Fork 本仓，填注 `operator` 与 `checker_endpoint`
2. 按本域现行有效规则校准属性取值与红线表述
3. 跑平台侧校验器自测（五项判据全过方可提交）
4. 提 PR，附资质证明与规则依据

This domain is open to qualified operators. Fork the repo, fill in `operator` and
`checker_endpoint`, align attribute values and red lines with current rules, run the
platform self-checker (five criteria must all pass), then open a PR with credentials.

---

## 🔒 安全 / Security

如发现安全问题（例如配置泄露敏感信息、checker 契约被用于越权下录用或歧视结论），请按
[`SECURITY.md`](SECURITY.md) 的方式私下报告，不要在公开 issue 中披露。

## 🤝 贡献 / Contributing

欢迎提 Issue / PR。本仓 main 分支受组织 ruleset 保护，请从个人分支发起 Pull Request，
等待 lgd-ci-gate 通过后合并。改动属性 / 红线时须同步 `gate-map.json` 并重新跑校验。

---

## 许可与署名

代码与配置按 MIT 许可使用。文档的知识版权归 SynomosAI 所有。

© 2026 SynomosAI. All rights reserved.

完整条款见根目录 [LICENSE](LICENSE)（MIT License）。
