---
name: coze-workflow-builder
description: 通过 Playwright MCP 控制浏览器，在扣子平台（www.coze.cn）上完成低代码工作流从创建到发布的全流程。适用于需要自动化搭建扣子工作流的场景，包括添加节点、连接节点、配置变量、测试和发布。
user-invocable: true
---

# 扣子平台低代码工作流搭建

本 skill 用于通过 Playwright MCP 控制浏览器，自动化完成扣子平台低代码工作流的搭建、测试和发布。

> **重要**：本 skill 针对的是**低代码工作流**（Low-code Workflow），不是 AI 编程工作流。低代码工作流是通过可视化拖拽方式搭建的工作流。

## 官方文档参考

本 skill 内容基于扣子官方文档编写，官方文档地址：https://docs.coze.cn/guides/workflow

## 一、低代码工作流基础概念

### 1. 什么是低代码工作流

低代码工作流是一系列可执行指令的集合，用于实现业务逻辑或完成特定任务。它为应用/智能体的数据流动和任务处理提供了一个结构化框架。低代码工作流的核心在于将大模型的强大能力与特定的业务逻辑相结合，通过系统化、流程化的方法来实现高效、可扩展的 AI 应用开发。

### 2. 工作流与对话流

扣子编程提供两种类型的工作流：

| 类型 | 用途 | 适用场景 |
|------|------|----------|
| **工作流（Workflow）** | 处理功能类请求，通过顺序执行一系列节点实现某个功能 | 数据自动化处理，如生成报告、制作海报、翻译等 |
| **对话流（Chatflow）** | 基于对话场景的特殊工作流，通过对话方式与用户交互并完成复杂业务逻辑 | Chatbot、个人助手、智能客服等 |

### 3. 节点基础

**节点**是低代码工作流的核心，每个节点是一个具有特定功能的独立组件，代表一个独立的步骤或逻辑。节点负责处理数据、执行任务和运行算法，并且都具备输入和输出。

每个低代码工作流默认包含：
- **开始节点**：工作流的起始节点，定义启动工作流需要的输入参数
- **结束节点**：工作流的最终节点，返回工作流运行结果

### 4. 支持的变量类型

开始节点、结束节点、大模型节点、插件节点、子工作流节点、代码节点、SQL 自定义节点、新增数据节点、查询数据节点、更新数据节点、删除数据节点、问答节点、批处理节点、循环节点、变量聚合节点、变量节点、选择器节点均支持多种变量类型：

| 类型 | 说明 | 示例 |
|------|------|------|
| String | 字符串 | "你好" |
| Integer | 整数 | 25 |
| Number | 数字 | 3.14 |
| Boolean | 布尔值 | true/false |
| Object | 对象 | {"name": "张三"} |
| Array | 数组 | ["a", "b", "c"] |
| File | 文件 | 图片/视频等 |

---

## 二、节点类型详解

### 1. 开始节点

开始节点是工作流的起始节点，用于定义启动工作流需要的输入参数。

**参数配置说明**：

| 配置项 | 说明 |
|--------|------|
| 变量名 | 为输入参数指定一个有意义的名称 |
| 变量类型 | 输入参数的数据类型，支持 String、Number、Array<String>、Object 等 |
| 参数描述 | 参数的描述信息，帮助模型理解传入的参数含义 |
| 是否必选 | 参数是否必选。如果未指定必选参数，无法开始执行工作流 |

**配置方式**：
- 逐个手动添加：单击➕图标，逐个配置输入参数信息
- 从 JSON 批量导入：单击导入图标，在面板中输入 JSON 数据，系统会自动将 JSON 数据解析为对应的输入参数

**默认输入参数**：
- 默认情况下，开始节点已包含一个输入参数 `input`，表示用户在本轮对话中输入的原始内容

### 2. 结束节点

结束节点是工作流的最终节点，用于返回工作流运行的结果。结束节点支持两种返回方式：

#### 返回变量模式
- 工作流运行结束后以 JSON 格式输出所有返回参数
- 适用于工作流绑定卡片或作为子工作流的场景
- 如果工作流直接绑定了智能体，对话中触发了工作流时，大模型会自动总结 JSON 格式的内容，并以自然语言回复用户

#### 返回文本模式
- 工作流运行结束后，智能体中的模型将直接使用指定的内容回复对话
- 回答内容中支持引用输出参数，引用方式为 `{{变量名}}`
- 支持开启流式输出，回复内容中由大模型生成的内容将逐字显示在对话中

### 3. 大模型节点

大模型节点可以调用大型语言模型，根据输入参数和提示词生成回复，通常用于执行文本生成任务。

**主要配置项**：

| 配置项 | 说明 |
|--------|------|
| 模型 | 选择要使用的模型，可选范围取决于账号类型 |
| 技能 | 可添加插件、工作流或知识库，扩展模型能力的边界 |
| 输入 | 需要添加到提示词中的动态内容 |
| 系统提示词 | 模型的系统提示词，用于指定人设和回复风格 |
| 用户提示词 | 模型的用户提示词，是用户在本轮对话中的输入 |
| 输出格式 | 支持文本、Markdown、JSON 三种格式 |

**关于技能配置**：
- 大模型节点配置技能时，模型会根据用户的 Query 自动判断调用技能的时机与方式
- 直接使用插件等技能节点时，调用技能的时机和方式是固定的

### 4. 插件节点

插件节点用于在低代码工作流中调用插件运行指定工具。

**主要配置项**：

| 配置项 | 说明 |
|--------|------|
| 输入与输出 | 取决于插件工具定义的输入输出结构，不支持自定义设置 |
| 模拟集 | 试运行时可选择使用模拟集的数据，而非真实输出 |
| 异常处理 | 支持设置超时时间、重试次数、异常处理方式 |

**异常处理方式**：
- 中断流程：工作流执行中断，不再运行后续节点
- 返回设定内容：发生异常后，工作流运行不会中断，可自定义设置返回内容
- 执行异常流程：发生异常后，转而执行异常流程分析

### 5. 代码节点

代码节点支持通过编写代码来生成返回值，支持 JavaScript 和 Python 两种语言。

**主要配置项**：

| 配置项 | 说明 |
|--------|------|
| 输入 | 声明代码中需要使用的变量 |
| 代码 | 需要执行的代码片段 |
| 输出 | 代码运行成功后输出的参数 |

**代码限制**：
- 单请求限制 60s
- 每个低代码工作流中最多添加 50 个代码节点
- JavaScript：仅内置 dayjs 和 lodash
- Python：仅内置 requests_async 和 numpy

---

## 三、全流程操作指南

### 第一步：创建低代码工作流

1. 登录扣子编程（https://code.coze.cn）
2. 在页面顶部选择目标工作空间，然后在左侧导航栏中单击**资源库**
3. 在资源库页面右上角，选择 **+资源 > 工作流**
4. 设置工作流的名称与描述，并单击确认
5. 创建后页面会自动跳转至工作流的编辑页面

**说明**：清晰明确的工作流名称和描述，有助于大语言模型更好地理解工作流的功能。

### 第二步：添加节点

#### 节点面板位置
- 节点面板位于页面**底部**，包含各类节点分类（输入、逻辑、大模型等）
- 需要先展开分类才能看到具体节点列表

#### 正确且快捷的添加方式

**方式一：双击节点（推荐）**

1. 在底部面板中找到目标节点类型（如"大模型"）
2. **双击**该节点条目
3. 节点会自动添加到画布中央
4. 如节点出现在意外位置，后续点击"优化布局"即可整理

```javascript
// 双击添加节点的示例
await page.locator('.node-item', { hasText: '大模型' }).dblclick();
await page.waitForTimeout(1000);
```

**方式二：单击拖拽**

1. 单击目标节点并保持按住
2. 将节点拖拽到画布上
3. 松开鼠标

```javascript
// 拖拽添加节点（适用于方式二）
const nodeItem = page.locator('.node-item', { hasText: '大模型' });
const canvas = page.locator('.workflow-canvas');

await nodeItem.dragTo(canvas);
await page.waitForTimeout(1000);
```

**方式三：单击+单击（备选）**

1. 单击底部面板中的目标节点（选中高亮）
2. 单击画布空白区域
3. 节点会被添加到点击位置附近

```javascript
// 单击选中节点
await page.locator('.node-item', { hasText: '大模型' }).click();
await page.waitForTimeout(300);

// 单击画布添加
await page.locator('.workflow-canvas').click({ position: { x: 400, y: 300 } });
await page.waitForTimeout(1000);
```

#### 添加失败时的处理

| 问题 | 原因 | 解决方案 |
|------|------|----------|
| 双击无反应 | 节点分类未展开 | 先点击分类名称展开列表 |
| 拖拽后节点消失 | 放置位置超出画布 | 拖拽到画布内部再释放 |
| 节点添加位置混乱 | 正常现象 | 点击"优化布局"按钮整理 |

#### 优化布局

添加完所有节点后，点击"优化布局"按钮整理节点位置：

```javascript
await page.getByTestId('workflow.detail.toolbar.auto-layout').click();
await page.waitForTimeout(1500);
```

**核心节点**：
- 开始节点：定义输入变量
- 大模型节点：处理业务逻辑
- 结束节点：定义输出变量

### 第三步：连接节点

**重要**：节点连接必须通过鼠标拖拽，不能简单点击。

#### 标准连接流程

**第一步：点击"优化布局"**

在开始连第一个连接之前，必须点击"优化布局"按钮，确保节点位置标准化、端口坐标准确。

```javascript
await page.getByTestId('workflow.detail.toolbar.auto-layout').click();
await page.waitForTimeout(1500); // 等待布局完成
```

**第二步：获取所有端口的精确坐标**

```javascript
const ports = await page.evaluate(() => {
  const nodes = document.querySelectorAll('.gedit-flow-activity-node');
  const result = [];
  nodes.forEach(node => {
    const nodeName = node.querySelector('p')?.textContent || '';
    if (!nodeName) return;

    const nodeRect = node.getBoundingClientRect();
    const ports = node.querySelectorAll('.workflow-port-bg');
    ports.forEach(p => {
      const rect = p.getBoundingClientRect();
      const isOutput = rect.x > nodeRect.x + nodeRect.width / 2;
      result.push({
        nodeName,
        portX: Math.round(rect.x + rect.width / 2),
        portY: Math.round(rect.y + rect.height / 2),
        side: isOutput ? 'output' : 'input'
      });
    });
  });
  return result.sort((a, b) => a.portY - b.portY);
});
```

**第三步：执行拖拽连接**

```javascript
const fromPort = ports.find(p => p.nodeName === '开始' && p.side === 'output');
const toPort = ports.find(p => p.nodeName === '大模型' && p.side === 'input');

await page.mouse.move(fromPort.portX, fromPort.portY);
await page.waitForTimeout(300);
await page.mouse.down();
await page.waitForTimeout(100);

// 分50步缓慢拖拽，每步等待40ms
for (let i = 1; i <= 50; i++) {
  const x = fromPort.portX + (toPort.portX - fromPort.portX) * (i / 50);
  const y = fromPort.portY + (toPort.portY - fromPort.portY) * (i / 50);
  await page.mouse.move(x, y);
  await page.waitForTimeout(40);
}

await page.waitForTimeout(100);
await page.mouse.up();
await page.waitForTimeout(1000);
```

**第四步：再次点击"优化布局"**

所有连接完成后，再次点击"优化布局"整理连接线。

#### 连接判断规则

1. **输出端口在右侧**（x值较大）
2. **输入端口在左侧**（x值较小）
3. **连接从输出端口 → 输入端口**
4. **必须通过鼠标拖拽**

### 第四步：配置节点变量

#### 连接与配置的区分

**重要经验**：节点连接成功≠变量配置完成。连接只是建立了数据传递的通道，但每个节点的输入输出变量仍需单独配置。

#### 变量配置的核心原则

**变量引用必须通过六边形按钮选择，绝对不能直接输入文本。**

#### 六边形按钮的正确区分

在变量配置面板中，有**两个六边形按钮**：

1. **左侧六边形按钮**（靠近变量类型标识）→ 选择变量类型（String, Integer 等）
2. **右侧六边形按钮**（带向下箭头）→ **打开变量选择对话框（正确方式）**

#### 变量选择对话框操作流程

1. 点击变量值输入框**右侧的六边形按钮**（带向下箭头）
2. 在弹出的对话框中，会显示节点列表（用户变量、应用变量、系统变量、上游节点）
3. 点击目标节点
4. 会展开该节点的输出变量列表
5. 选择所需的输出变量
6. 正确配置后，变量应显示为按钮形式（如 "大模型 - output"），而不是纯文本

#### 开始节点配置

1. 点击开始节点
2. 添加输入变量（如 `input`，类型选择"字符串"）
3. 如有需要，可从 JSON 批量导入参数

#### 大模型节点配置

**输入配置**：
1. 点击大模型节点
2. 找到输入框
3. 点击输入框**右侧的六边形按钮**
4. 在弹出面板中选择目标节点（如"开始"）
5. 选择对应的变量

**提示词配置**：
- 清晰描述任务要求
- 使用 `{{变量名}}` 引用输入变量
- 示例：`请将下面的中文文本翻译成英文，只输出翻译结果：{{input}}`

**模型选择**：
- 根据任务需求选择合适的模型
- 个人版用户可使用默认的几类模型
- 企业版用户还可以使用火山引擎方舟平台的模型

#### 结束节点配置

1. 点击结束节点
2. 选择返回模式（返回变量 或 返回文本）
3. 如选择返回变量，点击右侧六边形按钮选择变量
4. 如选择返回文本，在回答内容中引用输出参数

### 第五步：测试工作流

1. 单击**试运行**
2. 填写输入参数的测试值
3. 单击执行
4. 检查输出是否符合预期

**测试结果判断**：
- 运行成功的节点边框会显示绿色
- 在各节点的右上角可查看节点的输入和输出

**测试不通过时的排查**：
- 检查节点连接是否正确
- 检查变量引用是否配置正确
- 检查大模型提示词是否清晰
- 查看错误信息提示

### 第六步：发布工作流

1. 确认所有配置正确
2. 单击**发布**
3. 填写版本号（通常从 v0.0.1 开始）
4. 填写版本描述
5. 单击确认发布

---

## 四、节点连接代码模板

### 获取所有端口的精确坐标
```javascript
async (page) => {
  return await page.evaluate(() => {
    const nodes = document.querySelectorAll('.gedit-flow-activity-node');
    const result = [];
    nodes.forEach(node => {
      const nodeName = node.querySelector('p')?.textContent || '';
      if (!nodeName) return;

      const nodeRect = node.getBoundingClientRect();
      const ports = node.querySelectorAll('.workflow-port-bg');
      ports.forEach(p => {
        const rect = p.getBoundingClientRect();
        const isOutput = rect.x > nodeRect.x + nodeRect.width / 2;
        result.push({
          nodeName,
          portX: Math.round(rect.x + rect.width / 2),
          portY: Math.round(rect.y + rect.height / 2),
          side: isOutput ? 'output' : 'input'
        });
      });
    });
    return result.sort((a, b) => a.portY - b.portY);
  });
}
```

### 连接两个节点
```javascript
async (page, fromPort, toPort) => {
  await page.mouse.move(fromPort.portX, fromPort.portY);
  await page.waitForTimeout(300);
  await page.mouse.down();
  await page.waitForTimeout(100);

  // 分50步缓慢拖拽，每步等待40ms
  for (let i = 1; i <= 50; i++) {
    const x = fromPort.portX + (toPort.portX - fromPort.portX) * (i / 50);
    const y = fromPort.portY + (toPort.portY - fromPort.portY) * (i / 50);
    await page.mouse.move(x, y);
    await page.waitForTimeout(40);
  }

  await page.waitForTimeout(100);
  await page.mouse.up();
  await page.waitForTimeout(1000);
}
```

### 优化布局按钮
```javascript
await page.getByTestId('workflow.detail.toolbar.auto-layout').click();
await page.waitForTimeout(1000); // 等待布局完成
```

---

## 五、常见问题与解决方案

| 问题 | 可能原因 | 解决方案 |
|------|----------|----------|
| 端口连接无反应 | 拖拽太快 | 减慢拖拽速度，增加等待时间 |
| 变量显示为纯文本 | 直接输入而非选择 | 删除文本，点击六边形按钮重新选择 |
| 节点无法添加 | 页面未完全加载 | 等待页面加载完成后再操作 |
| 测试失败 | 连接或配置错误 | 逐一检查每个节点的配置 |
| 发布失败 | 存在未完成的配置 | 确保所有必填项都已填写 |
| 连接线不消失 | 连接失败残留 | 刷新页面重新连接 |
| 节点重叠无法连接 | 端口坐标被遮挡 | 点击"优化布局"按钮后再连接 |
| 端口坐标每次不同 | 节点位置不固定 | 每次连接前重新获取端口坐标 |

---

## 六、注意事项

### 核心要点

1. **耐心等待**：页面加载和操作都需要等待响应
2. **鼠标拖拽是唯一可靠方式**：不能偷懒用点击代替，必须模拟完整的拖拽动作
3. **变量必须通过 UI 选择**：六边形按钮是正确方式，直接输入文本无效
4. **慢速操作**：快速操作可能导致失败，慢速反而更高效
5. **每步验证**：完成一步验证一步，不要等到最后才发现问题
6. **优化布局保安全**：连接前点击优化布局，防止节点重叠导致连接失败
7. **坐标每次需获取**：优化布局后节点位置会变，连接前必须重新获取端口坐标
8. **连接≠配置**：连接成功后仍需单独配置每个节点的输入输出变量

### 自动化操作 vs 人工操作

- 人为操作可以快速点击和拖拽
- 自动化操作需要模拟人类的操作方式，且速度要慢
- 端口连接、变量选择都必须通过 UI 交互，不能用输入文本代替

---

## 七、工作流类型参考

### 1. 翻译工作流

- 开始节点：定义输入文本变量（String 类型）
- 大模型节点：翻译提示词，模型选择合适的翻译模型
- 结束节点：返回翻译结果

### 2. 问答工作流

- 开始节点：定义问题变量
- 大模型节点：问答提示词
- 结束节点：输出答案

### 3. 内容处理工作流

- 开始节点：定义内容变量
- 大模型节点：内容处理提示词
- 结束节点：输出处理结果

---

## 八、相关资源链接

- 扣子官方文档中心：https://docs.coze.cn/
- 低代码工作流介绍：https://docs.coze.cn/guides/workflow
- 使用低代码工作流：https://docs.coze.cn/guides/use_workflow
- 开始和结束节点：https://docs.coze.cn/guides/start_end_node
- 大模型节点：https://docs.coze.cn/guides/llm_node
- 插件节点：https://docs.coze.cn/guides/plugin_node
- 代码节点：https://docs.coze.cn/guides/code_node

---

**提示**：如遇到本 skill 未覆盖的特殊情况，请结合实际情况灵活处理，必要时可通过 `browser_snapshot` 工具深入分析页面结构。
