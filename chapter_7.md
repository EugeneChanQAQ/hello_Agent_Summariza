## Agent框架开发
#### Step 1 明确设计边界与核心价值
- 回答关键问题
- 定义最小可行产品
```python
MVP_FEATURES = {
    "must_have": [
        "BaseAgent基类", 
        "统一LLM接口", 
        "工具系统", 
        "简单记忆管理"
    ],
    "nice_to_have": [
        "多Agent协作", 
        "高级记忆系统", 
        "可视化监控"
    ],
    "future": [
        "分布式Agent", 
        "自动优化"
    ]
}
```
#### Step 2 核心抽象设计
- Agent抽象
- Message抽象
- Tool抽象

### 本地模型调用（vLLM、Ollama）
#### vLLM