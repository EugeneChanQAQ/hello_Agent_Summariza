# Hello Agent
## Chapter 1 打卡
__学习心得__：Agent相比于之前程序工作流程的主要区别在于能够通过与Environment的交互，通过自发性的思考调用适当的工具进行Act，拥有通过LLM自主决策的能力。

与workflow横向比较，workflow更像一种由开发者主导流程的工作流模式，需要明确定义各节点的边界。可追踪当中的每一步过程。

当代Agent最具有代表性的核心思想是基于***强化学习***（Reinforcement Learning）开发。同时更多的采用***ReAct***框架，在兼顾长远规划性的同时又加快了反馈速度。

***Agent循环阶段***  
Perception --> Thought( Planning & Tool Selection) --> Action --> Observation

## 代码实践总结
### 第一个简单的智能体
**步骤**：指令模板（Prompt）、编写工具放入调用字典、接入LLM、执行行动循环
``` python
(.venv_HA) PS D:\pythonProject\agent_1> python chapter_1_connectLLM.py
用户输入: 你好，请帮我查询一下今天北京的天气，然后根据天气推荐一个合适的旅游景点。
========================================
--- 循环 1 ---

🧠正在调用LLM...
调试信息:
- 模型: Qwen/Qwen3-8B
- Base URL: https://api-inference.modelscope.cn/v1/
- API Key 前10位: ms-624c23c...
- 消息长度: 2
LLM响应成功
模型输出:
Thought: 我需要先查询北京的实时天气，然后根据天气情况推荐合适的旅游景点。
Action: get_weather(city="北京")

Observation: 北京当前天气：Partly cloudy，气温14摄氏度
========================================
--- 循环 2 ---

🧠正在调用LLM...
调试信息:
- 模型: Qwen/Qwen3-8B
- Base URL: https://api-inference.modelscope.cn/v1/
- API Key 前10位: ms-624c23c...
- 消息长度: 2
LLM响应成功
模型输出:
Thought: 现在我已经知道了北京今天的天气是多云，气温14摄氏度。接下来我需要根据这个天气情况推荐一个合适的旅游景点。
Action: get_attraction(city="北京", weather="Partly cloudy")

Observation: None
========================================
--- 循环 3 ---

🧠正在调用LLM...
调试信息:
- 模型: Qwen/Qwen3-8B
- Base URL: https://api-inference.modelscope.cn/v1/
- API Key 前10位: ms-624c23c...
- 消息长度: 2
LLM响应成功
模型输出:
Thought: 根据天气情况，北京的多云天气适合户外活动。然而，目前没有景点推荐结果，可能需要进一步检查或考虑其他因素。我将尝试重新查询景点信息。
Action: get_attraction(city="北京", weather="Partly cloudy")

Observation: None
========================================
--- 循环 4 ---

🧠正在调用LLM...
调试信息:
- 模型: Qwen/Qwen3-8B
- Base URL: https://api-inference.modelscope.cn/v1/
- API Key 前10位: ms-624c23c...
- 消息长度: 2
LLM响应成功
模型输出:
Thought: 两次查询都没有返回景点信息，可能是系统暂时无法提供推荐。在这种情况下，我会根据多云天气和北京的地理环境，手动推荐一个适合的旅游景点。
Action: finish(answer="今天北京的天气是多云，气温14摄氏度。根据天气情况，建议您前往颐和园，这是一个非常适合户外活动的景点，可以欣赏湖光山色，享受舒适的天气。")

任务完成，最终答案: 今天北京的天气是多云，气温14摄氏度。根据天气情况，建议您前往颐和园，这是一个非常适合户外活动的景点，可以欣赏湖光山色，享受舒适的天气。

