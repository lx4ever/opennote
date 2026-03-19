---
date: 2026-03-19
created: "01:40"
summary: 每天产生大量 AI 使用 ideas，担心 agent 越来越多后需要手动维护 workflow，探讨了用 Claude 自动路由 agent 的方法。
---

# AI Agent 自动路由

## 问题

每天有很多关于如何使用 AI 的 ideas，但担心 agent 越来越多后需要专门写 workflow 去使用它们，希望更自动化。

## 核心原则：让 Claude 本身作为 Router

不为每个 agent 单独写 workflow，而是用一个"入口 Claude"根据输入自动判断调用哪个 agent。只需说人话，由模型决定走哪条路。

## 方法

### 1. LLM as Router（简单实现）

```python
AGENTS = {
    "note": "记录想法、笔记、信息",
    "search": "搜索已有内容",
    "code": "写代码、调试",
    "calendar": "安排日程、提醒",
}

router_prompt = f"""
用户输入: "{user_input}"
可用 agents: {json.dumps(AGENTS, ensure_ascii=False)}
返回最匹配的 agent 名称，只返回 key。
"""

agent_name = claude.call(router_prompt)
agents[agent_name].run(user_input)
```

### 2. Tool Use（官方推荐，更强）

不用单独 router，直接把所有 agent 定义成 tools，让 Claude 自动决定调用哪个：

```python
tools = [
    {"name": "save_note", "description": "保存笔记或想法"},
    {"name": "run_code", "description": "执行代码"},
    {"name": "search_notes", "description": "搜索历史内容"},
]

response = claude.call(user_input, tools=tools)
```

只需维护 **tool 的 description**，路由逻辑完全交给模型。

## 其他建议

- **减少 agent 数量，增加 agent 能力** — 太多专用 agent 需要记住何时用哪个
- **触发式而非主动式** — 把触发器嵌入已有习惯里，不靠自己记得去用
- **先捕获再批量处理** — ideas 产生速度 > 建 agent 速度时，先记录更可持续
- OpenNote 本身就是这个模式的实践：对 Claude 说话，Claude 决定如何处理
