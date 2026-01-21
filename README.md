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

## 组合工作流

`product-reverse-analysis` + `business-analyst` + `prd-generator` 完整产品开发流程

```
竞品分析 → 产品发现 → PRD生成
(product-reverse-analysis) → (business-analyst) → (prd-generator)
```

[查看三技能组合工作流文档](product-reverse-analysis/reference/three-skill-product-workflow.md)
