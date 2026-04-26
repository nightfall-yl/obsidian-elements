# Findings & Decisions

## Requirements
- 在不影响当前 UI 的前提下，review 并精简 `styles.css`
- 重点关注双重或多重补偿、重复定义、残留覆盖
- 对高风险区域采取保守策略，先审计后修改

## Research Findings
- 当前 `styles.css` 共有 1740 行，已明显进入“局部修补累积”状态。
- 当前 worktree 是脏的，除 `styles.css` 外还有 `main.js`、`src/elementCardBuilderModal.ts`、`src/view/form/GraphForm.tsx`、`src/view/icon/Icons.tsx` 等文件有未提交修改，清理时必须避免误伤。
- 已发现至少一处真实的双重补偿案例：`contribution-graph` 的“对齐方式”图标，在替换成专用 16x16 SVG 后，`styles.css` 中仍残留对同一组 SVG 的尺寸覆盖和 transform 处理。
- 已发现多处“同一 block 内重复声明并以后值覆盖前值”的模式，例如 `contribution-graph` 表单和“对齐方式”控件块中同时存在重复的 `width/min-width/max-width/height/flex` 定义。
- 已发现 `elementCard` 主题选择器属于高风险区域：常规规则、局部规则、移动端规则三层交错覆盖，不能在第一轮直接大删。
- 已发现 `contribution-graph` 表单区比其他区域更集中地包含补丁式覆盖，适合作为第一批安全精简样本。
- 第一批已验证的低风险精简模式：同一 block 内重复的尺寸/布局声明，且后写值完全覆盖前写值；已在“对齐方式”控件组试行清理。
- 第二批已验证的低风险精简模式：`contribution-graph` 表单基础布局块中，`form-item / label / form-description / form-content` 存在明显的历史残留前值，可在不改变最终值的前提下删除。
- 第三批已验证的低风险精简模式：同一控件 block 内新旧尺寸方案叠加，但后写方案已经完全主导，例如 `contribution-graph` 的 `.color-picker`。
- git 视图与月追踪视图都存在“普通模式与 fill 模式语义未完全分离”的问题：普通模式仍保留 `width: 100%` 或拉伸因子，导致字体变化时图表整体横向被放大。
- 当普通模式缩到单元格下限后，单靠内容宽度策略还不够，需要专用横向滚动容器承接溢出，否则图表只能继续被裁切或挤压。

## Technical Decisions
| Decision | Rationale |
|----------|-----------|
| 使用文件化计划追踪审计和清理批次 | 任务复杂，便于控制风险和记录发现 |
| 第一轮仅做样式审计，不急于批量删除 | 先厘清哪些覆盖是有意兼容，哪些是真冗余 |
| 第一批优先清理“语义不变”的重复声明 | 同一 block 内前值被后值覆盖，删除风险最低 |
| 第一批清理仅限 `contribution-graph` 对齐方式控件 | 范围小、重复明显、容易回归验证 |
| git 视图与月追踪视图统一采用“普通模式内容宽度、fill 模式才拉满” | 让两种视图在字体变化时行为一致，避免普通模式被横向拉伸 |
| 对 git 视图和月追踪视图增加 `.charts-scroll` 容器 | 让普通模式在到达最小单元格宽度后，通过横向滚动展示剩余内容 |

## Issues Encountered
| Issue | Resolution |
|-------|------------|
| 当前样式存在同一 selector 多段重复定义 | 计划按“组件/区域”分组审计，避免一次性全局改写 |
| 局部移动端补丁与通用桌面规则交错 | 审计时先标记为高风险，不纳入第一批自动清理 |

## Resources
- `/Users/yanfan/Documents/Codex/obsidian-elements/styles.css`
- `/Users/yanfan/Documents/Codex/obsidian-elements/src/view/form/GraphForm.tsx`
- `/Users/yanfan/Documents/Codex/obsidian-elements/src/view/icon/Icons.tsx`

## Visual/Browser Findings
- `contribution-graph` 的“对齐方式”在移动端仍有轻微视觉偏差，目前已从结构性问题收敛到移动端渲染差异。
- `contribution-graph` 主题选择器曾出现“触发器与弹出框宽度不一致”的问题，根因分别出现在代码里的宽度计算和 CSS 的宽度限制上。
- `contribution-graph` 表单样式呈现出明显的“多轮补丁叠加”痕迹，尤其是输入框、对齐方式胶囊、slider、checkbox 这几个控件族。
- git 视图的核心问题在于 `.charts.default .column` 在普通模式下仍然存在拉伸因子。
- 月追踪视图的核心问题在于 `.charts.month-track` 在普通模式下仍然吃 `width: 100%`，且日期 indicator 在普通模式下仍保留拉伸倾向。
