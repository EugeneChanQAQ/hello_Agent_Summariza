## Chapter 4 智能体经典范式结构
- ReAct
- Plan-and-solve
- Reflection  

开始前准备工作：安装依赖库、配置API key到.env文件中，配置config.py文件加载、封装LLM调用函数（初始化client、大模型think函数：response和处理流式响应）

### ReAct
ReAct诞生之前范式分为两种：纯思考（CoT）和纯行动，一种没有与外界交互的能力容易产生幻觉，一种直接输出缺乏规划和纠错能力。

___--> ReAct___：Thought -> Action -> Observation  
具体来说：在每个时间步t，agent的策略（llm）会根据***初始问题q***和之前的所有步骤的“***行动-观察***”历史轨迹((a1,o1), (a2,02),...)来生成当前的思考th和行动a。

#### 工具的定义与实现
- 名称
- 描述
- 执行逻辑

##### 构建通用的工具执行器
使用多种工具时，构建统一的管理器来注册与调度。主要能包括以下基本部分。（注册新工具、根据名称获取执行函数、获取可用工具列表等）

ReAct demo的实现需要：系统提示词、封装好的llm client、tool_executor、最大循环步数、history。  
还需要输出解析器解析llm结果和tool使用  
_parse_output：解析llm输出，提取Thought和Action  
_parse_action：解析Action字符串，提取工具名和输入

### Plan-and-Solve
为了解决CoT在处理多步骤、复杂问题时容易偏离轨道的问题。  
主要分为两个和新阶段：Planning和Solving
- Planning：分解问题，制定一个清晰、分步骤的行动计划（llm调用产物）
- Solving：按计划注意执行，直到所有步骤完成，得出答案。

两部分都需要prompt，在planning后，我们定义一个Executor来完成任务，需要进行状态管理。

**_对于Executor，Prompt需要包含以下关键信息：原始问题、完整计划、历史步骤与结果、当前步骤。_**

最后整合两部分到一个Agent中。

### Reflection
核心思想：在Agent用任意方法（ReAct、Plan-and-Solve等）尝试完成任务，生成一个初步方案后，引入一种事后（post-hoc）的自我校正循环，迭代优化。  
Execution --> Reflection --> Refinement（每一步都需要不同的Prompt

__*学习心得*__: 
- ReAct侧重充满不确定性的、需要与外部交互的任务，能够通过实时反馈动态调整路径。
- Plan-and-Solve逻辑清晰，侧重内部推理和步骤分解任务。
- Reflection侧重对结果质量有高要求的任务。
