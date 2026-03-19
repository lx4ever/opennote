---
date: 2026-03-19
created: "01:51"
summary: Openclaw AI agent 经常不按约定流程执行，分析了常见原因。
---

# Openclaw 不按流程执行的问题

## 现象

Openclaw（一个 AI agent）经常不能按照 agreed upon 的流程执行，表现得"很笨"。

## 常见原因分析

1. **System prompt 太模糊** — 流程描述不够精确，模型自己"发挥"
2. **上下文丢失** — 长对话后早期的约定被遗忘
3. **没有强制检查点** — 没有机制让 agent 在关键步骤前确认
4. **工具定义不清** — tool description 模糊，模型猜测调用时机
