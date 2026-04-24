# 状态文件 JSON Schema

## 概述

`state.json` 是看海专家顾问团的永久记忆存储文件，位于 `~/.claude/skills/kanhai-advisory-board/state.json`。

## 完整 Schema

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "type": "object",
  "properties": {
    "version": {
      "type": "string",
      "description": "状态文件版本，用于迁移",
      "example": "1.0.0"
    },
    "advisors": {
      "type": "object",
      "properties": {
        "active": {
          "type": "array",
          "items": { "type": "string" },
          "description": "当前正式顾问名单",
          "example": ["Paul Graham", "张一鸣", "Karpathy"]
        },
        "candidates": {
          "type": "array",
          "items": { "type": "string" },
          "description": "候选顾问名单（可添加）",
          "example": []
        }
      }
    },
    "userProfile": {
      "type": "object",
      "properties": {
        "name": {
          "type": "string",
          "description": "用户姓名/昵称",
          "example": "李明"
        },
        "background": {
          "type": "string",
          "description": "用户背景/职业",
          "example": "互联网创业者"
        },
        "projectType": {
          "type": "string",
          "description": "当前项目类型",
          "example": "AI SaaS产品"
        },
        "projectStage": {
          "type": "string",
          "description": "项目阶段",
          "example": "MVP阶段"
        },
        "goals": {
          "type": "array",
          "items": { "type": "string" },
          "description": "用户目标",
          "example": ["用户增长", "盈利"]
        },
        "constraints": {
          "type": "object",
          "properties": {
            "budget": { "type": "string", "example": "50万" },
            "timeline": { "type": "string", "example": "6个月" },
            "teamSize": { "type": "string", "example": "5人" },
            "technical": { "type": "string", "example": "需要支持移动端" }
          }
        },
        "experienceLevel": {
          "type": "string",
          "description": "经验水平",
          "enum": ["新手", "中级", "高级", "专家"],
          "example": "中级"
        },
        "riskTolerance": {
          "type": "string",
          "description": "风险承受能力",
          "enum": ["保守", "稳健", "激进"],
          "example": "激进"
        },
        "preferences": {
          "type": "object",
          "properties": {
            "preferredAdvisors": {
              "type": "array",
              "items": { "type": "string" },
              "description": "用户偏好的顾问"
            },
            "responseStyle": {
              "type": "string",
              "enum": ["简洁", "详细", "全面"],
              "default": "详细"
            }
          }
        }
      }
    },
    "conversationHistory": {
      "type": "array",
      "items": {
        "type": "object",
        "properties": {
          "role": {
            "type": "string",
            "enum": ["user", "advisor", "system"]
          },
          "content": { "type": "string" },
          "timestamp": { "type": "string", "format": "date-time" },
          "advisorUsed": {
            "type": "array",
            "items": { "type": "string" }
          }
        },
        "required": ["role", "content", "timestamp"]
      },
      "maxItems": 20
    },
    "keyInsights": {
      "type": "array",
      "items": {
        "type": "object",
        "properties": {
          "id": { "type": "string" },
          "category": {
            "type": "string",
            "enum": ["identity", "project", "goals", "constraints", "experience", "risk", "preference", "other"]
          },
          "content": { "type": "string" },
          "source": { "type": "string" },
          "timestamp": { "type": "string", "format": "date-time" },
          "confidence": {
            "type": "number",
            "minimum": 0,
            "maximum": 1,
            "default": 0.9
          }
        },
        "required": ["id", "category", "content", "timestamp"]
      },
      "maxItems": 50
    },
    "preferences": {
      "type": "object",
      "properties": {
        "autoConsult": {
          "type": "boolean",
          "description": "是否自动咨询多位相关专家",
          "default": true
        },
        "elicitThreshold": {
          "type": "string",
          "description": "触发主动询问的阈值",
          "enum": ["strict", "moderate", "loose"],
          "default": "moderate"
        },
        "rememberFacts": {
          "type": "boolean",
          "description": "是否自动记录用户分享的事实",
          "default": true
        }
      }
    },
    "metadata": {
      "type": "object",
      "properties": {
        "createdAt": { "type": "string", "format": "date-time" },
        "updatedAt": { "type": "string", "format": "date-time" },
        "sessionCount": { "type": "integer", "default": 0 }
      }
    }
  },
  "required": ["version", "advisors", "userProfile", "conversationHistory", "keyInsights", "preferences", "metadata"]
}
```

## 字段说明

| 字段 | 类型 | 说明 |
|------|------|------|
| `version` | string | 状态文件版本，用于未来兼容迁移 |
| `advisors.active` | string[] | 当前正式顾问名单 |
| `advisors.candidates` | string[] | 候选顾问名单 |
| `userProfile` | object | 用户画像，包含背景、项目、目标等 |
| `conversationHistory` | array | 最近20条对话历史 |
| `keyInsights` | array | 从对话中提取的关键洞察，最多50条 |
| `preferences` | object | 用户偏好设置 |
| `metadata` | object | 元数据，包含创建/更新时间、会话计数 |

## 存储位置

```
~/.claude/skills/kanhai-advisory-board/state.json
```

## 读写时机

- **读取**：skill 激活时加载
- **写入**：每次交互后自动保存

## 兼容性

如果 `state.json` 文件损坏或版本不兼容：
1. 自动备份为 `state.json.bak`
2. 使用默认状态重新初始化