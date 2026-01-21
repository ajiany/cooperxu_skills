# 三技能组合产品工作流

本文档介绍如何组合使用 `product-reverse-analysis`、`business-analyst`、`prd-generator` 三个技能，完成从竞品分析到产品需求文档的完整流程。

---

## 技能概述

| 技能 | 来源 | 作用 |
|------|------|------|
| **product-reverse-analysis** | cooperxu_skills (本仓库) | 竞品逆向分析，提取功能、用户故事、商业模式 |
| **business-analyst** | @aj-geddes/claude-code-bmad-skills | 产品发现与需求分析，创建产品简报 |
| **prd-generator** | @aj-geddes/claude-code-bmad-skills | 从产品计划生成完整 PRD 文档 |

---

## 完整工作流图

```
┌─────────────────────────────────────────────────────────────────────┐
│                    完整产品开发工作流                                  │
└─────────────────────────────────────────────────────────────────────┘

      竞品分析                  产品发现                 需求文档
    (学习借鉴)               (原创开发)              (规格落地)
          │                      │                       │
          ▼                      ▼                       ▼
  ┌───────────────┐      ┌───────────────┐      ┌───────────────┐
  │ product-      │      │ business-     │      │ prd-          │
  │ reverse-      │──▶   │ analyst       │──▶   │ generator     │
  │ analysis      │      │               │      │               │
  └───────────────┘      └───────────────┘      └───────────────┘
          │                      │                       │
          │                      │                       │
          ▼                      ▼                       ▼
     竞品PRD              产品简报/问题分析           完整PRD
```

---

## 阶段详解

### 阶段1: 竞品逆向分析

**技能**: `product-reverse-analysis`

**输入**: 竞品产品 URL

**输出**: 竞品分析文档（包含功能、用户故事、商业模式）

**使用方式**:
```
帮我分析 https://news.aibase.com 这个产品
```

**分析内容**:
- 第1层（What）: UI组件、可见功能、导航结构
- 第2层（How）: 用户流程、状态变化、API调用
- 第3层（Why）: 业务问题、用户画像、设计决策

**工具**:
- `mcp__web_reader__webReader` - 内容概览
- `browser_snapshot` - 页面结构分析
- `browser_click/type` - 交互测试
- `browser_network_requests` - API分析

---

### 阶段2: 差异化产品发现

**技能**: `business-analyst`

**输入**: 竞品分析文档 + 你的产品想法

**输出**: 产品简报、问题分析、MVP范围

**使用方式**:
```
基于 AIBase 的分析，帮我做差异化产品分析
```

**分析内容**:
- 利益相关者访谈框架
- 市场研究
- 问题发现 (5 Whys, JTBD)
- 产品简报生成

---

### 阶段3: PRD 文档生成

**技能**: `prd-generator`

**输入**: 产品简报 + 竞品分析

**输出**: `docs/PRD.md` 完整产品需求文档

**使用方式**:
```
基于分析结果，生成我的产品 PRD
```

**文档结构**:
1. 产品概述 - 愿景、目标受众、成功指标
2. 用户画像 - 人口统计、痛点、目标
3. 功能需求 - 用户故事、验收标准
4. 用户流程 - 注册、核心功能、错误状态
5. 非功能需求 - 性能、安全、无障碍
6. 技术考虑 - 数据模型、API要求
7. 竞品背景 - 竞争对手、差异化策略

---

## 实际案例: AI资讯平台

### 案例: 开发一个AI资讯聚合平台

#### 步骤1: 分析 AIBase 竞品

```
帮我分析 https://news.aibase.com 这个产品
```

**输出**: `AIBase产品需求文档.md`

| 维度 | AIBase 现状 | 关键数据 |
|------|-------------|----------|
| 产品定位 | AI行业一站式信息枢纽 | 850,000+ 用户 |
| 核心功能 | 资讯聚合、工具库、模型广场 | 95,000+ 资讯 |
| 商业模式 | 免费→订阅→企业服务 | 28,000+ 订阅 |

#### 步骤2: 差异化分析

```
基于 AIBase 的分析，帮我做差异化产品分析
我想做一个社区驱动的 AI 资讯平台
```

**识别的差异化机会**:
- 社区互动 (评论、讨论、UGC)
- 个性化推荐 (AI驱动内容流)
- 深度分析 (行业报告、趋势预测)
- 工具评测 (真实用户评分)
- C端知识付费 (会员体系)

#### 步骤3: 生成 PRD

```
基于分析结果，生成我的产品 PRD
```

**输出**: `docs/PRD.md`

---

## 快速开始

### 前置条件: 安装技能

```bash
# 1. product-reverse-analysis (本仓库)
# 手动复制到 ~/.claude/skills/
cp -r cooperxu_skills/product-reverse-analysis ~/.claude/skills/

# 2. business-analyst
npx skills-installer install @aj-geddes/claude-code-bmad-skills/business-analyst

# 3. prd-generator
npx skills-installer install @aj-geddes/claude-code-bmad-skills/prd-generator

# 安装到多个客户端
npx skills-installer install <skill-name> --client claude-code
npx skills-installer install <skill-name> --client opencode
```

### 使用流程

```bash
# 步骤1: 竞品分析
> 帮我分析 https://example.com 这个产品

# 步骤2: 差异化分析
> 基于上面的分析，帮我做差异化产品分析
> 我的目标用户是 XXX，核心差异点是 XXX

# 步骤3: 生成 PRD
> 基于分析结果，生成我的产品 PRD
```

---

## 常见问题

**Q: 三个技能可以单独使用吗？**

A: 可以。每个技能都是独立的，可以单独使用：
- 只做竞品分析 → `product-reverse-analysis`
- 只做产品发现 → `business-analyst`
- 只生成PRD → `prd-generator` (需要先有产品计划)

**Q: 必须按顺序使用吗？**

A: 推荐按顺序，但不是强制：
- 如果已有竞品分析，可以从 `business-analyst` 开始
- 如果已有产品简报，可以直接用 `prd-generator`

**Q: 三个技能的输入输出是什么？**

| 技能 | 输入 | 输出 |
|------|------|------|
| product-reverse-analysis | URL | 竞品PRD |
| business-analyst | 竞品PRD + 想法 | 产品简报 |
| prd-generator | 产品简报 | 完整PRD |

---

## 相关资源

- [product-reverse-analysis 主文档](../SKILL.md)
- [business-analyst 文档](https://github.com/aj-geddes/claude-code-bmad-skills)
- [prd-generator 文档](https://github.com/aj-geddes/claude-code-bmad-skills)
