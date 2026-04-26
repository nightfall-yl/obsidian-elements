# Task Plan: styles.css 审计与无感精简

## Goal
在不影响当前 UI 表现的前提下，系统审计并精简 `styles.css` 中的重复定义、双重或多重补偿、残留覆盖和无效样式，分批做可验证、可回退的清理。

## Current Phase
Phase 2

## Phases

### Phase 1: Requirements & Discovery
- [x] Understand user intent
- [x] Identify constraints and requirements
- [x] Document findings in findings.md
- **Status:** complete

### Phase 2: Audit & Risk Mapping
- [x] Inventory duplicated selectors and conflicting overrides
- [x] Identify UI-critical areas and high-risk sections
- [x] Group findings by safe cleanup batches
- **Status:** in_progress

### Phase 3: Safe Cleanup Batches
- [x] Remove or consolidate dead/duplicate rules
- [ ] Eliminate double-compensation patterns where safe
- [ ] Preserve known-good UI behaviors
- **Status:** in_progress

### Phase 4: Verification
- [ ] Review diffs for unintended scope creep
- [ ] Verify critical UI areas manually via code inspection/build where needed
- [ ] Log residual risks and intentionally preserved quirks
- **Status:** pending

### Phase 5: Delivery
- [ ] Summarize changes and remaining risks
- [ ] Call out any areas intentionally left untouched
- [ ] Deliver concise review + cleanup outcome
- **Status:** pending

## Key Questions
1. 哪些重复样式只是冗余，哪些实际上承担了兼容不同端的补偿作用？
2. 哪些区域属于高风险 UI，不应在第一轮精简中触碰？
3. 哪些规则已经被后写规则完全覆盖，可以安全删除？

## Decisions Made
| Decision | Rationale |
|----------|-----------|
| 先做只读审计，再分批清理 | 当前 worktree 已有多处修改，直接大改风险高 |
| 以 `styles.css` 为主审计对象 | 用户明确指出样式优化历史较多，双重补偿主要集中在这里 |
| 保守处理近期刚调通的控件 | 避免把已验证好的移动端/桌面端修正重新带歪 |
| 第一批只动“语义不变”的冗余声明 | 先清理低风险重复，再考虑跨 selector 合并 |
| Phase 3 先从 `contribution-graph` 对齐方式控件开始 | 该区域重复声明清晰且风险相对最低 |

## Errors Encountered
| Error | Attempt | Resolution |
|-------|---------|------------|
|       | 1       |            |
