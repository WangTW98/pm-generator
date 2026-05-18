# Figma 还原报告

## 基本信息

- 源 JSON：
- 目标 Figma：
- fileKey：
- nodeId：
- 写入模式：
- consistencyMode：profile-locked / legacy / existing-figma-baseline
- 渲染模式：recursive / component-adapter / update / append
- 根画板：
- 根节点 ID：
- pageIntent：
- layoutKey：
- pageIntentKey：
- stateGroupKey：
- designSpecKey：
- styleFingerprint：
- semanticValidation：
- renderSpec：
- consistency：
- 执行时间：

## 还原结果

- 状态：
- 创建节点数：
- 更新节点数：
- 跳过节点数：
- 组件适配数量：
- 组件降级数量：
- token 命中数量：
- token 缺失数量：
- profile 命中数量：
- profile 缺失数量：
- fingerprint 匹配：
- driftCheck：
- 关键语义区校验：
- 截图 QA：

## RenderPlan 校验

| 项目 | 结果 | 说明 |
| --- | --- | --- |
| 使用 figmaDocument.children 作为主来源 |  |  |
| RenderPlan 节点总数 |  |  |
| 保留原始节点顺序 |  |  |
| 保留 sourceNodeId metadata |  |  |
| 合并 renderSpec.componentInstances |  |  |
| 合并 renderSpec.textPolicies |  |  |
| 合并 renderSpec.constraints |  |  |
| 合并 consistency profiles |  |  |
| 应用 locked properties |  |  |
| pageIntent 仅用于校验 |  |  |
| excludedStates 未进入主节点树 |  |  |

## 一致性 Profile 校验

| 项目 | 结果 | 说明 |
| --- | --- | --- |
| layoutProfile |  |  |
| intentProfile |  |  |
| stateGroupProfile |  |  |
| layoutProfileHash |  |  |
| intentProfileHash |  |  |
| stateGroupProfileHash |  |  |
| styleFingerprint |  |  |
| driftCheck |  |  |
| existing Figma fingerprint |  |  |
| metadata 写入 |  |  |

## 页面结构

| 区域 | 节点类型 | 还原结果 | 备注 |
| --- | --- | --- | --- |
|  |  |  |  |

## 非默认状态排除记录

| 状态 | 来源 | 处理方式 |
| --- | --- | --- |
|  |  | 仅记录，未写入 Figma |

## Token 处理

| Token | 来源文件 | 命中/缺失 | Figma 映射或 fallback |
| --- | --- | --- | --- |
|  |  |  |  |

## Token 文件读取

| 文件 | 是否读取 | 说明 |
| --- | --- | --- |
| tokens.json |  |  |
| figma-tokens.json |  |  |
| component-rules.json |  |  |
| component-blueprints.json |  |  |
| figma-render-rules.json |  |  |
| ai-generation-rules.json |  |  |
| consistency-rules.json |  |  |
| layout-profile-schema.json |  |  |
| page-intent-patterns.json |  |  |
| layout-profile.json |  |  |
| intent-profile.json |  |  |
| state-group-profile.json |  |  |

## 组件处理

| 组件 | 节点 | 处理方式 | 备注 |
| --- | --- | --- | --- |
|  |  | adapter / imported / fallback |  |

## RenderSpec 命中情况

| 项目 | 命中数量 | 降级数量 | 说明 |
| --- | ---: | ---: | --- |
| componentInstances |  |  |  |
| textPolicies |  |  |  |
| constraints |  |  |  |
| qaExpectations |  |  |  |
| consistencyProfiles |  |  |  |
| lockedProperties |  |  |  |

## 两阶段布局 QA

| 项目 | 结果 | 说明 |
| --- | --- | --- |
| 根画板尺寸正确 |  |  |
| 第一阶段尺寸计算完成 |  |  |
| 第二阶段 append 后 fill/hug 设置完成 |  |  |
| 主要容器高度正常 |  |  |
| 文本未明显挤压或溢出 |  |  |
| 组件最小尺寸与文本策略已应用 |  |  |
| Profile locked 尺寸/间距已应用 |  |  |
| 主要容器无明显堆叠 |  |  |
| 空白位置追加且未覆盖原内容 |  |  |

## 关键语义区校验

| 项目 | 结果 | 说明 |
| --- | --- | --- |
| pageIntent |  |  |
| 必备区域命中 |  |  |
| 源 JSON 是否包含关键区 |  |  |
| Figma 结果是否包含关键区 |  |  |
| 是否发生 renderer loss |  |  |
| 是否使用固定模板替代 |  |  |

## 截图/结构 QA

| 项目 | 结果 | 说明 |
| --- | --- | --- |
| 截图 URL |  |  |
| 直接子节点 |  |  |
| 总节点数 |  |  |
| Auto Layout Frame 数量 |  |  |
| 关键文本样本 |  |  |
| 不应出现的状态页/表格/截图占位 |  |  |

## 警告

- 

## 人工检查项

- 
