# Page Layout Batch Task List

## 0.任务状态

<table>
  <tr><td>任务ID</td><td></td></tr>
  <tr><td>任务类型</td><td>pm-04-page-layout-batch</td></tr>
  <tr><td>创建日期</td><td></td></tr>
  <tr><td>更新时间</td><td></td></tr>
  <tr><td>执行状态</td><td>Pending</td></tr>
  <tr><td>生成模式</td><td>Development</td></tr>
  <tr><td>Sitemap来源模式</td><td></td></tr>
  <tr><td>当前恢复位置</td><td></td></tr>
</table>

## 1.源Sitemap清单

| Sitemap ID | Sitemap路径 | 产品端与形态 | 处理状态 | Eligible数量 | Skipped节点数量 | Done数量 | Failed数量 | 备注 |
|---|---|---|---|---:|---:|---:|---:|---|

## 2.页面生成任务清单

| Task ID | Sitemap ID | Sitemap路径 | 页面ID | 父级ID | 层级 | 页面/模块 | 页面类型 | 状态组 | 输出路径 | 通用模板污染检查 | 状态 | 开始时间 | 完成时间 | 失败原因/恢复说明 |
|---|---|---|---|---|---:|---|---|---|---|---|---|---|---|---|

## 3.中断恢复说明

- `Pending`：尚未执行，可继续执行。
- `In Progress`：上次中断时可能正在执行，恢复时应重新检查输出路径；若输出文件完整则改为 `Done`，否则重新执行。
- `Done`：已完成，不重复执行，除非用户明确要求重跑。
- `Planned`：历史测试状态；恢复时应按当前规则重新检查并创建空 Markdown 占位文件。
- `Directory Created`：历史目录测试状态；恢复时应按当前规则重新检查并创建空 Markdown 占位文件。
- `Placeholder Created`：测试/预演/目录结构模式下已创建规范目录与 0 bytes 空 Markdown 占位文件，未生成页面内容。
- `Placeholder Exists`：测试/预演/目录结构模式下目标 Markdown 文件已存在且被保留，未覆盖、未清空。
- `Skipped`：页面类型为 `节点` 或不在用户筛选范围内，不传给 `pm-03-page-layout`。
- `Failed`：执行失败；恢复时可优先重试，失败原因保留在表格最后一列。

## 4.执行日志

| 时间 | Task ID | 操作 | 结果 | 说明 |
|---|---|---|---|---|
