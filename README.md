# 看海专家顾问团 (Kanhai Advisory Board)

集成13位智者于一身的决策参谋系统。根据问题内容自动选择最合适的专家回答，同时支持灵活增减顾问团成员。

## 专家阵容

| 专家 | 核心领域 |
|------|---------|
| Paul Graham | 创业/写作/产品/人生哲学 |
| 张一鸣 | 产品/组织/全球化/人才 |
| Karpathy | AI工程/教育/开源 |
| Ilya Sutskever | AI安全/scaling/研究品味 |
| MrBeast | 内容创作/YouTube方法论 |
| 特朗普 | 谈判/权力/传播/行为预判 |
| 乔布斯 | 产品/设计/战略 |
| 马斯克 | 工程/成本/第一性原理 |
| 芒格 | 投资/多元思维/逆向思考 |
| 费曼 | 学习/教学/科学思维 |
| 纳瓦尔 | 财富/杠杆/人生哲学 |
| 塔勒布 | 风险/反脆弱/不确定性 |
| 张雪峰 | 教育/职业规划/阶层流动 |

## 安装使用

### 一键安装（推荐）
```bash
npx skills add alchaincyf/kanhai-advisory-board
```

### 从GitHub直接安装
```bash
npx skills add https://github.com/alchaincyf/kanhai-advisory-board
```

### 本地路径安装
```bash
npx skills add ./kanhai-advisory-board
```

## 功能

- **智能路由** - 根据问题自动选择最匹配的专家回答
- **成员管理** - 添加/辞退顾问团成员（`加入[专家名]` / `辞退[专家名]`）
- **团队展示** - 查看当前顾问团阵容（`顾问团阵容` / `专家团详情`）
- **连续对话** - 跨多轮保持上下文，理解用户长期目标和背景
- **主动询问** - 在关键信息缺失时主动询问用户
- **永久记忆** - 记录用户分享的关键事实，跨会话持久化

## 使用示例

```
# 咨询顾问团
我想创业做AI产品，请各位顾问给点建议

# 查看阵容
顾问团阵容

# 管理成员
加入芒格
辞退特朗普
```

## 项目结构

```
kanhai-advisory-board/
├── SKILL.md                    # 主技能文件
├── README.md
├── state.json                  # 永久状态存储
├── references/                 # 参考文档
│   ├── state-schema.md         # 状态文件 JSON Schema
│   ├── elicitation-rules.md    # 主动询问规则
│   └── information-extraction.md # 信息提取模式
├── paul-graham-perspective/     # 各专家skill
├── zhang-yiming-skill/
├── karpathy-skill/
├── ilya-sutskever-skill/
├── mrbeast-skill/
├── trump-skill/
├── steve-jobs-skill/
├── elon-musk-skill/
├── munger-skill/
├── feynman-skill/
├── naval-skill/
├── taleb-skill/
└── zhangxuefeng-skill/
```

## License

MIT
