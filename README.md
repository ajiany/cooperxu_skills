# cooperxu_skills

个人 Claude Code Skills 仓库

## 技能发现

> 推荐：访问 [skillsmp.com](https://skillsmp.com/) 发现更多优质技能

```bash
# skills-discovery - 技能发现与搜索
npx skills-installer install @Kamalnrf/claude-plugins/skills-discovery
```

---

## 安装

### 本仓库技能

```bash
# product-reverse-analysis - 产品逆向分析
cp -r product-reverse-analysis ~/.claude/skills/
```

### 依赖技能（组合工作流必需）

```bash
# business-analyst - 产品发现与需求分析
npx skills-installer install @aj-geddes/claude-code-bmad-skills/business-analyst

# prd-generator - 产品需求文档生成
npx skills-installer install @rshankras/claude-code-apple-skills/prd-generator
```

### 推荐技能（开发阶段）

```bash
# Superpowers - 完整软件开发工作流
# Claude Code 内置插件市场安装
/plugin marketplace add obra/superpowers-marketplace
/plugin install superpowers@superpowers-marketplace
```

### 多客户端安装

```bash
# 同时安装到多个客户端
npx skills-installer install <skill-name> --client claude-code
npx skills-installer install <skill-name> --client opencode
```

---

## 本仓库技能

### product-reverse-analysis
**产品逆向分析**

当需要分析现有产品以提取需求、用户故事和业务场景时使用。适用于竞品分析、跨领域功能迁移、产品设计学习，或通过 Playwright 等自动化工具"复制"产品功能。

- **竞品分析** - 深度分析竞品功能背后的业务逻辑和设计意图
- **跨赛道功能迁移** - 将其他领域的成熟功能迁移到新领域
- **产品设计学习** - 理解优秀产品设计的本质和权衡
- **PRD/用户故事提取** - 从现有产品反推结构化需求文档

---

## 完整工作流

### 产品阶段

```
竞品分析 → 产品发现 → PRD生成
(product-reverse-analysis) → (business-analyst) → (prd-generator)
```

[查看三技能组合工作流文档](product-reverse-analysis/reference/three-skill-product-workflow.md)

### 开发阶段

```
brainstorming → writing-plans → development → TDD → code-review
    设计细化        实现计划        开发执行      测试      代码审查
```

**Superpowers** 提供完整的软件开发工作流，包含 20+ 战斗验证的技能：

- **brainstorming** - 设计细化与头脑风暴
- **writing-plans** - 创建详细实现计划
- **test-driven-development** - TDD 红绿重构循环
- **systematic-debugging** - 系统化根因分析
- **requesting-code-review** - 代码审查流程

[了解更多：obra/superpowers](https://github.com/obra/superpowers)

### 端到端流程

| 阶段 | 技能 | 输出 |
|------|------|------|
| 竞品分析 | product-reverse-analysis | 竞品PRD |
| 产品发现 | business-analyst | 产品简报 |
| 需求文档 | prd-generator | 完整PRD |
| 设计细化 | superpowers:brainstorming | 设计文档 |
| 实现计划 | superpowers:writing-plans | 开发计划 |
| 开发执行 | superpowers:subagent-driven-development | 代码实现 |
| 测试驱动 | superpowers:test-driven-development | 测试+代码 |
| 代码审查 | superpowers:requesting-code-review | 质量保证 |
