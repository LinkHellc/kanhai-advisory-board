# 信息提取规则

## 概述

每次用户消息后自动提取关键事实，更新 `userProfile` 和 `keyInsights`。

## 提取类别

| 类别 | 关键词/模式 | 示例输入 | 提取结果 |
|------|-----------|---------|---------|
| **identity** | 我叫、我是、姓名 | "我叫李明" | `{name: "李明"}` |
| **background** | 职业、从事、我之前 | "我之前在阿里工作" | `{background: "曾在阿里工作"}` |
| **project** | 项目、正在做、我们的产品 | "我们正在做一个 SaaS 产品" | `{projectType: "SaaS产品"}` |
| **project** | 阶段、目前是 | "目前是 MVP 阶段" | `{projectStage: "MVP阶段"}` |
| **goals** | 目标、希望、要达成 | "希望能快速增长用户" | `{goals: ["用户增长"]}` |
| **constraints.budget** | 预算、花费、成本 | "预算 50 万" | `{constraints: {budget: "50万"}}` |
| **constraints.timeline** | 时间、截止、几个月 | "6 个月内上线" | `{constraints: {timeline: "6个月"}}` |
| **constraints.teamSize** | 团队、几个人、有多少 | "团队 5 人" | `{constraints: {teamSize: "5人"}}` |
| **experienceLevel** | 经验、年、有过 | "我创业 3 年了" | `{experienceLevel: "中级"}` |
| **experienceLevel** | 新手、第一次 | "我第一次创业" | `{experienceLevel: "新手"}` |
| **risk** | 风险、保守、激进 | "我能接受较大风险" | `{riskTolerance: "激进"}` |
| **risk** | 保守、稳健 | "我比较保守" | `{riskTolerance: "保守"}` |

## 经验水平映射

| 用户表述 | 映射值 |
|---------|-------|
| 第一次、新手、刚开始 | `新手` |
| 3年以内、有几年经验 | `中级` |
| 5年以上、资深、老兵 | `高级` |
| 10年以上、专家级 | `专家` |

## 风险偏好映射

| 用户表述 | 映射值 |
|---------|-------|
| 保守、稳健、不想冒险 | `保守` |
| 平衡、适中、还好 | `稳健` |
| 激进、大胆、能接受大波动 | `激进` |

## 提取规则

1. **置信度阈值**：只有当 `confidence >= 0.7` 时才自动记录
2. **去重**：如果 `keyInsights` 中已有相似内容（语义相似度 > 0.8），更新而非添加
3. **来源标记**：所有从用户消息提取的信息标记 `source: "user-share"`
4. **决策记录**：用户接受顾问建议后的决策，标记 `source: "user-decision"`

## 更新逻辑

1. 提取用户消息中的新事实
2. 对比 `userProfile` 中已有字段
3. 如果有更新，更新对应字段并标记 `source: "user-share"`
4. 如果有新的未分类信息，添加到 `keyInsights`

## keyInsights 条目结构

```json
{
  "id": "uuid-v4",
  "category": "identity|project|goals|constraints|experience|risk|preference|other",
  "content": "提取的具体内容",
  "source": "user-share|user-decision|advisor-insight",
  "timestamp": "2026-04-24T00:00:00.000Z",
  "confidence": 0.9
}
```

## 示例

**用户输入**："我叫张三，在字节工作了5年，现在想出来创业，做一个 AI 教育类的 SaaS 产品，目标是 1 年内做到 10 万用户，预算大概 100 万，能接受一定风险。"

**提取结果**：

```json
{
  "userProfile": {
    "name": "张三",
    "background": "曾在字节工作",
    "projectType": "AI教育SaaS产品",
    "goals": ["1年内做到10万用户"],
    "constraints": {
      "budget": "100万"
    },
    "experienceLevel": "高级",
    "riskTolerance": "激进"
  },
  "keyInsights": [
    {
      "id": "uuid-1",
      "category": "identity",
      "content": "曾在字节工作5年",
      "source": "user-share",
      "timestamp": "2026-04-24T00:00:00.000Z",
      "confidence": 0.95
    }
  ]
}
```