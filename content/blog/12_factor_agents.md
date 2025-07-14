---
date: '2025-07-14T08:46:25+08:00'
draft: true
title: '12 Factor Agents Reading Notes'
math: true
---

# 序

Agent 是软件，软件的设计的历史是

* 60年代：Directed Graphs (DGs)

![010-software-dag](https://github.com/humanlayer/12-factor-agents/raw/main/img/010-software-dag.png)

* 20年前：DAG 编排器

![015-dag-orchestrators.png](https://github.com/humanlayer/12-factor-agents/raw/main/img/015-dag-orchestrators.png?raw=true)

* 10-15年前：当 ML 模型开始变得足够有用时，我们开始看到 DAG 中加入了 ML 模型。您可能会想到 "将这一列中的文本总结到新的一列中 "或 "根据严重性或情感对支持问题进行分类 "这样的步骤。

![020-dags-with-ml.png](https://github.com/humanlayer/12-factor-agents/blob/main/img/020-dags-with-ml.png?raw=true)

## The promise of agents

![](https://github.com/humanlayer/12-factor-agents/raw/main/img/026-agent-dag-lines.png?raw=true)

这里的承诺是，你可以编写更少的软件，只需将图的“边缘”提供给 LLM，让它自行找出节点。你可以从错误中恢复，可以编写更少的代码，而且你可能会发现 LLM 会为问题找到新颖的解决方案。

## Agents as loops

一个常见的Agent搭建方式是：

```python
initial_event = {"message": "..."}
context = [initial_event]
while True:
  next_step = await llm.determine_next_step(context)
  context.append(next_step)

  if (next_step.intent === "done"):
    return next_step.final_answer

  result = await execute_step(next_step)
  context.append(result)
```

换句话说，你有一个由 3 个步骤组成的循环：

* LLM 确定工作流中的下一步，输出结构化的 JSON（“工具调用”）
* 确定性代码执行工具调用
* 结果将被附加到上下文窗口
* 重复此过程，直到确定下一步是“完成”

这个模式最大的问题是：

* 当上下文窗口过长时，代理会迷失方向——它们会陷入重复尝试相同错误方法的循环。
* "字面意思就是这样"，但这就足以让这个方法无法发挥作用了。

即使您没有亲自编写过代理，您可能在与代理编码工具的交互中也遇到过这种长上下文问题。它们用着用着就迷失了方向，您需要开始一个新的聊天。

即使模型支持越来越长的上下文窗口，但**使用小而集中的提示和上下文始终会取得更好的效果。**

大多数我交流过的 Agent 们在超过 10-20 个回合后就会变得一团糟，当乱到LLM 无法恢复时，LLM 可能会没有“工具调用循环”的想法了。即使代理能有 90% 的时间做对，也远远达不到“足够好可以交付给客户”的标准。你能想象一个有 10% 概率会在加载 WEB页面时崩溃的 Web 应用吗？

## 真正有效的方法--微型代理

![](https://github.com/humanlayer/12-factor-agents/raw/main/img/028-micro-agent-dag.png?raw=true)

让 LLM 管理好范围限定的任务集合，可以轻松地纳入实时的人类反馈,而不是陷入上下文错误循环。

## 一个微型代理的例子

![](https://github.com/humanlayer/12-factor-agents/raw/main/img/029-deploybot-high-level.png?raw=true)

我们没有给这个代理很多工具或任务。LLM 的主要价值在于*解析人类的纯文本反馈并提出更新的操作计划*。我们将任务和上下文尽可能地隔离，以使 LLM 专注于一个较小的、5-10 步的工作流程。

我们通过拥有控制流和上下文累积获得了几个好处：

* 在我们的switch语句和for循环中，我们可以劫持控制流，以暂停等待人类输入，或等待长时间运行的任务完成。
* 我们可以轻松地序列化上下文窗口以实现暂停+恢复。
* 在我们的提示中，我们可以极大地优化向LLM传递指令和“迄今为止发生了什么”的方式。

# Own Your Context Window

我们可以使用更高效的格式管理上下文窗口

example:

```python

class Thread:
  events: List[Event]

class Event:
  # could just use string, or could be explicit - up to you
  type: Literal["list_git_tags", "deploy_backend", "deploy_frontend", "request_more_information", "done_for_now", "list_git_tags_result", "deploy_backend_result", "deploy_frontend_result", "request_more_information_result", "done_for_now_result", "error"]
  data: ListGitTags | DeployBackend | DeployFrontend | RequestMoreInformation |  
        ListGitTagsResult | DeployBackendResult | DeployFrontendResult | RequestMoreInformationResult | string

def event_to_prompt(event: Event) -> str:
    data = event.data if isinstance(event.data, str) \
           else stringifyToYaml(event.data)

    return f"<{event.type}>\n{data}\n</{event.type}>"


def thread_to_prompt(thread: Thread) -> str:
  return '\n\n'.join(event_to_prompt(event) for event in thread.events)
```

Example：XML 格式的上下文窗口

```xml
<slack_message>
    From: @alex
    Channel: #deployments
    Text: Can you deploy the latest backend to production?
    Thread: []
</slack_message>

<deploy_backend>
    intent: "deploy_backend"
    tag: "v1.2.3"
    environment: "production"
</deploy_backend>

<error>
    error running deploy_backend: Failed to connect to deployment service
</error>

<request_more_information>
    intent: "request_more_information_from_human"
    question: "I had trouble connecting to the deployment service, can you provide more details and/or check on the status of the service?"
</request_more_information>

<human_response>
    data:
      response: "I'm not sure what's going on, can you check on the status of the latest workflow?"
</human_response>
```

## Tools are just structured outputs

