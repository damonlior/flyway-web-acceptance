# 设计前端还原度验收

仅在设计稿对比、界面还原或响应式验收模式下读取。

## 事实来源

按以下优先级取证：指定 Figma 节点的实际属性 > 当前 Component / Variables / Styles > 项目设计治理文档 > 用户提供的静态截图。必须使用用户指定的版本和 `node-id`，不能用相似页面代替。

飞来汇 B 端页面必须同时使用 `$flyway-b-design`，完成其规定的治理文件读取、Pattern、组件、Token、状态与响应式核对。用户明确要求忽略的项目作为当前任务的 `Approved Exception`，不写入缺陷表。

## 基准

1. 从 Figma 获取 Frame 尺寸、布局方向、约束、组件实例、文本样式、颜色变量、间距、圆角、阴影和关键状态。
2. 将浏览器设置为与设计 Frame 完全一致的 CSS 视口；记录设备像素比、缩放比例和字体环境。
3. 获取页面基线截图、DOM 快照、Console 和 Network 基线。
4. 设计稿与实现无法对齐同一视口或设计数据无法读取时，不宣称像素级一致；将相关结论标记为待验证。

## 桌面还原检查

- Application Shell、导航、顶栏、内容容器和模块顺序。
- 元素位置、宽高、对齐、Grid、padding、margin、gap和留白。
- 字体族、字号、字重、行高、颜色、截断及换行。
- 背景、边框、圆角、阴影、渐变、图标和图片裁切。
- Button、Form、Table、Tabs、Dropdown、Modal、Drawer、Badge 等组件的 Variant 和结构。
- Default、Hover、Active、Focus、Disabled、Loading、Error、Empty、Permission 及业务状态。
- 主滚动容器、Sticky / Fixed 元素、浮层层级、遮挡和点击热区。

## 数值取证

使用 `evaluate_script` 获取相关节点的 `getBoundingClientRect()` 和 `getComputedStyle()`。重点记录：

- `x/y/width/height`
- `display/grid/flex` 及列定义
- `padding/margin/gap`
- `fontFamily/fontSize/fontWeight/lineHeight`
- `color/background/border/borderRadius/boxShadow`
- `position/inset/zIndex/overflow`

截图用于呈现问题，实际数值用于证明差异。不要只凭肉眼使用“略有偏差”“感觉不一致”等描述。

## 响应式检查

优先使用设计稿明确的断点；未给出时，至少检查与任务相关的 Desktop、Compact Desktop 和 Mobile。常用检查视口可选：`1440×900`、`1280×800`、`1024×768`、`768×1024`、`390×844`、`375×812`。

每个视口检查：

- 横向和纵向溢出。
- 元素重叠、遮挡、裁切和异常空白。
- 文本截断、换行、数字和长文案增长。
- 菜单、筛选、卡片、表格和操作区的换行或结构转换。
- 导航、Dropdown、Modal、Drawer、Date Picker 的移动形态。
- 主任务、状态、金额、对象和风险信息是否仍可见。
- Fixed / Sticky、滚动容器和安全区是否正确。

移动端不是桌面等比缩小。设计未提供移动方案且业务范围没有要求时，风险应标为“待设计确认”，不要自行创造设计标准。

## 质量检查

- 使用 `list_console_messages` 和 `list_network_requests` 区分运行错误与视觉差异。
- 按需运行 `lighthouse_audit`；性能问题使用 `performance_start_trace` / `performance_stop_trace`，不要把性能建议误写成像素还原问题。
- 检查可见 Focus、图标按钮可访问名称、色彩以外的状态表达和文本缩放。

## 缺陷描述要求

“问题描述（截图）/期望效果”单元格依次包含：

1. 所在页面和组件/区域。
2. 对应视口与触发状态。
3. 实际表现及测量数值。
4. 设计稿节点和设计预期数值。
5. 差值与用户影响。
6. 建议调整方向。
7. 问题截图。

“备注”放置实现 URL、Figma URL / `node-id`、DOM 选择器、浏览器环境、Console / Network 关联证据和验证状态。
