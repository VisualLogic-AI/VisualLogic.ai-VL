# DAG Harness 与上下文补丁的区别

## 当前 Harness Engineering 的主要模式

现在很多 Harness Engineering 的工作，主要是在处理症状：

- 防止上下文腐烂
- 增强记忆
- 增加检索
- 保存任务状态
- 保留中间笔记
- 让 agent 记住已经发生过什么

这些手段有价值。它们可以降低长任务失败率，但没有解决核心问题。它们本质上仍然把 agent 当成一个“点”系统：模型接收上下文，输出结果，然后由人或者外层 wrapper 决定下一步该怎么走。

真正的问题不只是记忆不足。真正的问题是：整个过程没有被明确地规划和编程下来。

## 根本问题

单个 AI 节点可以吃进很多输入，然后输出一个很强的结果：

```text
prompt + docs + files + memory + tools -> AI -> output
```

这个 output 可能很强，但对复杂系统来说会带来几个问题：

- 下一步是隐式的
- 操作者必须很懂怎么驾驭 AI
- 每次运行输出可能不稳定
- 校验不一定发生
- 失败后的恢复是临时的
- 不同的人接手同一个 output，可能会走向完全不同的结果

所以单点 agent 很难真正用于严肃的软件交付。它能产出一个“点”，但很难稳定地守住一条完整的“线”。

## 为什么 DAG 才是核心解决方案

DAG harness 会把整个过程显式化：

```text
Intent
  -> Manifest
  -> Contract
  -> Generate
  -> Validate
  -> Repair
  -> Preview
  -> Package
```

每个节点都有：

- 预期输入
- 预期输出
- 能力类型
- 校验规则
- 下游依赖
- 可恢复的失败路径

这会改变 AI 的角色。AI 不再是整个系统本身。AI 变成更大程序里的某一种节点。

## DAG 本质上也是一个程序

DAG 不只是图。DAG 本质上也是一个程序。

它可以表达：

- 执行步骤
- 数据流方向
- 依赖顺序
- AI 节点
- 工具节点
- 校验节点
- 人工确认节点
- 重试和修复回路
- 产物生成
- 部署或打包步骤

从这个角度看，一个 DAG 应用和传统应用程序没有本质差异。它有结构、输入、输出、逻辑、控制流和运行时行为。区别在于，它是一种专门用来 harness AI 工作过程的程序。

## DAG 与上下文补丁的对比

| 问题 | 上下文 / 记忆补丁 | DAG harness |
| --- | --- | --- |
| 上下文腐烂 | 增加 memory 和 summary | 通过显式步骤降低歧义 |
| 长任务 | 试图让 agent 记住上下文 | 把过程拆成类型化节点 |
| 输出漂移 | 给 prompt 加更多约束 | 每个节点输出先校验再进入下一步 |
| 人的负担 | 需要高手持续驾驭 | 把驾驭方式编码进图里 |
| 失败恢复 | 多数依赖人工处理 | 可以进入 repair、retry 或 human gate |
| 复用 | 复用 prompt pattern | 可以复用整个 workflow |

上下文和记忆仍然有用，但它们是支撑基础设施。DAG 才是主结构。

## Agent Application

这会引出一种新的产品形态：Agent Application。

Agent Application 不是普通聊天机器人，也不是简单 prompt chain。它是一个可以运行的包，由这些部分组成：

- DAG
- manifest
- 节点定义
- input / output contract
- 可选 UI 面板或弹框
- 人机交互点
- runtime 配置
- 打包元数据

manifest 描述这个 agent app 应该如何运行：需要什么输入、使用哪些节点、能展示什么 UI、需要什么权限、会产出什么 artifact。

DAG 则定义真正的工作过程。

两者合在一起，就是一个可以和人交互、调用 AI、调用工具、校验结果并产出最终结果的真实应用。

## AAS: Agent Application Studio

AAS，也就是 Agent Application Studio，是 VisualLogic 用来生产 Agent Application 的系统。

它的目标是让人们像构建结构化程序一样构建 agent app，而不是只写松散的 prompt chain。一个 agent app 可以独立运行，也可以打包后在本地或远端运行。

在这个模型里：

- DAG 是可执行过程
- manifest 是应用 contract
- AI 节点负责推理和生成
- tool 节点负责确定性动作
- UI 节点负责人机交互
- validation 节点保证执行不越界

这个能力现在已经实现：agent app 可以被表示为 DAG + manifest，可以作为结构化 workflow 运行，可以和人交互，也可以被打包到本地或远端执行。

## 核心观点

Agent Engineering 的未来，不只是更好的 memory。

未来是可编程的 AI 过程。

VisualLogic 的判断很简单：

- VL 给 AI 正确的结构化表示层。
- DAG 给 AI 正确的结构化工作过程。
- AAS 把这个过程打包成可运行的 Agent Application。

这就是“让 AI 输出一个结果”和“用 AI 完成真实工作”之间的区别。
