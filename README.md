# GPT-5 模型对接教程（基于ablai API）

## 1. 概述
GPT-5 是 OpenAI 推出的最新旗舰模型，在代码生成、错误修复、长上下文理解及工具调用等领域具备行业领先能力。其配套 API 新增多项控制功能，包括冗长性调节、推理工作强度设置、自定义工具支持等。

本教程基于 **ablai API 网关**（官网：[`api.ablai.top`](https://api.ablai.top)），详细说明 GPT-5 系列模型的对接方法，帮助开发者快速集成并使用模型功能。


## 2. 模型版本与支持说明
### 2.1 支持的模型分组
ablai API 已全面兼容 GPT-5 系列模型，具体包括：
- `gpt-5-2025-08-07`：官转 OpenAI 通道（含优质官转节点）
- `gpt-5-chat-latest`：无思考模式，专注高效响应
- `gpt-5-mini-2025-08-07`：限时特价，支持 `default` / 纯 `az` 分组
- `gpt-5-nano-2025-08-07`：限时特价，支持 `default` / 纯 `az` 分组

### 2.2 参数限制说明
- 不支持 `max_tokens` 参数（无需配置最大输出长度）
- 仅支持 `temperature=1`（固定温度参数，保证输出多样性）


## 3. 模型特性与适用场景
GPT-5 系列包含三款模型，各有侧重，选型参考如下：

| 模型变体       | 核心优势                  | 适用场景                                  |
|----------------|---------------------------|-------------------------------------------|
| gpt-5          | 复杂推理、全量世界知识    | 多步骤代理任务、代码开发、深度逻辑推理    |
| gpt-5-mini     | 平衡速度、成本与功能      | 日常聊天、中等复杂度问答、通用推理        |
| gpt-5-nano     | 低延迟、高吞吐量、低成本  | 简单指令执行、数据分类、批量处理任务      |


## 4. 新 API 功能解析
### 4.1 推理工作强度（`reasoning.effort`）
控制模型生成响应前的思考深度，可选值：
- `low`：优先速度，推理步骤少（适合简单查询）
- `medium`：默认值，平衡质量与速度
- `high`：深度推理（适合复杂问题，如数学证明）
- `minimal`：推理标记极少，首令牌响应最快（适合编码场景）

### 4.2 冗长性调节
控制输出内容详尽程度：
- **高冗长性**：输出详细，适合文档解析、代码重构
- **低冗长性**：输出简洁，适合快速问答、生成短句代码


## 5. 对接步骤（基于ablai API）
### 5.1 准备工作
1. 访问 ablai API 官网（[`api.ablai.top`](https://api.ablai.top)）注册账号
2. 获取 API 密钥（`API Key`）
3. baseURL：`https://api.ablai.top`


### 5.2 对接方式示例
#### 5.2.1 Responses API（支持思维链传递）
适用于需要展示推理过程的场景（如教学、复杂问题解析），支持回合间传递思维链（CoT）。

```bash
curl --request POST \
--url https://api.ablai.top/v1/responses \
--header "Authorization: Bearer $YOUR_API_KEY" \
--header 'Content-type: application/json' \
--data '{
  "model": "gpt-5",
  "input": "计算给自由女神像镀1毫米厚的金层需要多少黄金？",
  "reasoning": {
    "effort": "high"
  }
}'
```

#### 5.2.2 Chat Completions API（直接获取答案）
适用于无需推理过程、追求高效响应的场景（如日常聊天）。

```bash
curl --request POST \
--url https://api.ablai.top/v1/chat/completions \
--header "Authorization: Bearer $YOUR_API_KEY" \
--header 'Content-type: application/json' \
--data '{
  "model": "gpt-5-mini",
  "messages": [
    {
      "role": "user",
      "content": "推荐3部经典科幻电影"
    }
  ],
  "reasoning_effort": "low"
}'
```


## 6. 迁移指南
从传统 Chat Completions API 迁移至 GPT-5 接口的核心差异：
- **Responses API 新增优势**：支持思维链传递，提升推理透明度、降低成本、减少延迟
- **参数调整**：将 `reasoning_effort` 迁移至 `reasoning` 对象中（仅 Responses API 适用）


## 7. 参考资源
- 官方文档：访问 [ablai API 官网](https://api.ablai.top) 查看完整接口说明
- 调试工具：通过官网提供的 API 调试界面快速测试请求格式
- 技术支持：联系 ablai API 客服获取对接协助


通过 ablai API 网关对接 GPT-5 模型，可跳过复杂部署流程，快速体验前沿 AI 能力，适配从简单查询到复杂推理的全场景需求。
