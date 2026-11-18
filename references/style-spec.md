# doodle-card-ui · 组件完整规格与实战踩坑

> 数值全部来自 pm-os 工作台实际落地代码（非理论值）。token 定义见 `assets/doodle.css`。

## 一、设计 token

| token | 值 | 说明 |
|---|---|---|
| 墨线 | `#1a1a1a` | 一级容器 2px；小元素（按钮/输入/tab/notice）1.6px；涂鸦外轮廓 3px、内部 2px |
| 硬投影 | 大卡 `2.5px 2.5px 0 #1a1a1a`（实心）；小件 `1.5–2px 2px 0 rgba(26,26,26,.75~.8)`；标题卡 `3px 3px 0`；弹窗 `5px 5px 0` | **实心/高不透明度**，贴纸感的关键；模糊阴影（`0 8px 30px` 类）只许弹窗底层配一道 |
| 荧光黄 | `#ffe28a`（底色）/ `#f5e042`（星形填充） | 仅三种语义：选中态 / 待办计数 / 人工闸门 |
| 奶油底 | `#f9f0d8` | hover 预览、涂鸦内部件 |
| 灰阶 | `#e3e1da` | 涂鸦内的"次要文本条"等灰件 |
| 圆角 | 卡 12px / 标题卡 14px / 弹窗 18px / 按钮 8–11px / seg 9px / chip 999px | |

## 二、组件配方

### 标题卡（页头）
```css
.title-card{background:#fff;border:2px solid #1a1a1a;border-radius:14px;
  box-shadow:3px 3px 0 #1a1a1a;padding:14px 20px;position:relative;font-weight:700}
.title-card::after{content:'';position:absolute;left:26px;bottom:-8px;width:52px;height:9px;
  border:2px solid #1a1a1a;border-top:0;border-radius:0 0 20px 20px/0 0 10px 10px}
```
弧线是"每幅必配点缀"的 CSS 版。子页 h1 用同一配方（小号 15px + 2.5px 投影）。

### 按钮交互三态（风格灵魂）
```css
box-shadow:2px 2px 0 rgba(26,26,26,.8);           /* 静止：贴纸 */
:hover{transform:translate(-1px,-1px);box-shadow:3px 3px 0 ...;background:#f9f0d8}  /* 翘起+奶油预览 */
:active{transform:translate(1px,1px);box-shadow:1px 1px 0 ...}                       /* 按下 */
```
primary 保留品牌色底，但描边仍是墨线（蓝底黑边白字）。

### 主导航贴纸组
按钮加 `nth-of-type(odd/even)` 交替 `rotate(∓0.45deg)`；选中态荧光黄底 + 投影加大 + 抬起 1px（不是压暗）；旧的下划线指示条必须 `content:none` 清掉。图标用 `currentColor` 继承墨线。

### tab 三形态
- 主导航：大贴纸（见上）
- 次级 tab：小贴纸 1.6px + 1.5px 投影，选中荧光黄
- seg 连体分段：外框墨线 + 内按钮 `border:0`，分隔用 `border-left:1px solid 墨线`，选中荧光黄

### 提示条
墨线描边 + 硬投影，**语义底色保留**（黄警示/蓝信息/红错误），可保留 3px 左侧语义标记。覆盖时**必须连变体选择器一起写**（见踩坑 1）。

### 统计/环节卡
白底墨线卡 + 硬投影；卡内大数字 700 字重是主角；底部量表条 `height:7px;border:1.5px solid 墨线;border-radius:4px` 内部彩色分段。选中筛选用荧光黄底（不用光圈 ring）。

### 空态公式
**涂鸦 SVG（见 assets/doodle-library.html）+ 一句话 + 一个动作按钮**，居中。禁止纯灰字，也禁止堆三个按钮。

### 弹窗
本体 `2.5px 墨线 + 18px 圆角 + 5px 实心投影 + 一道大模糊阴影`。**内部组件也要进语言**：左导航贴纸+荧光黄选中、步号圆圈墨线、关闭钮贴纸、完成标志墨线 chip——只化外壳不化内部会被用户看出"不完整"。

## 三、浅色主题约束
- 全部为浅色设计：白卡 + 深墨线。深色主题不可直接套（墨线改浅色描边、荧光黄对比度需重调）。
- 荧光黄上文字用深色 `#1b2233`，禁止白字。

## 四、实战踩坑（按代价排序）

1. **变体选择器特异性陷阱**：`.notice.info` 比 `.notice` 优先级高。覆盖块里只写 `.notice{border:1.6px...}` 时，`.notice.info` 的 `border-color` 不会被覆盖。→ 覆盖时把 `.notice,.notice.info,.notice.bad` 写成一组。
2. **内联 style 优先级最高**：元素上 `style="border:1px solid..."` 会让任何 CSS 覆盖失效。逐页排查 `style="...border` 的内联残留。
3. **本地样式表压过 shared**：多页面项目把覆盖放 shared css 时，页面本地 `<style>` 的同名规则（后加载）会赢。→ 本地有同名规则就必须逐页改本地。
4. **Chromium 取整假阳性**：1.6px 边框的 computed style 返回 `1px`。写样式断言时不要用 `borderTopWidth==='1.6px'` 判断，改判颜色或 box-shadow。
5. **静态 HTML 区 vs JS 模板串**：`'+SVGLIB+'` 写进静态 HTML 会字面输出。静态区直接内联 `<svg>`，JS 拼串才用常量。
6. **删块类 Edit 逐字核对**：new_string 写错会引入重复标签。删完 Read 复查 ±15 行。
7. **删组件先盘点独占功能**：状态条里的按钮可能是唯一入口（如"现在采一次"），删条必须先搬家。
8. **fc 观感依赖对比度**：退出登录等低调件**不要**贴纸化、数据密集区保持素净——风格的存在感来自对比，全站都粗等于没有粗。
9. **覆盖块必须后加载**：追加在页面样式末尾或独立文件最后引入；块首加日期注释便于回溯与回滚。
10. **每屏星 ≤2、荧光黄三语义**是守风格的最后一道闸——铺开时最容易破这两条。

## 五、验收清单

- [ ] 一级容器卡全部墨线+硬投影，卡内数据区素净
- [ ] 按钮三态齐全（静止/翘起/按下），primary 品牌底墨线边
- [ ] 荧光黄只出现在选中态/待办/闸门三种位置
- [ ] 四角星全屏 ≤2 颗
- [ ] 弹窗外壳和内部组件都进语言
- [ ] 提示条带语义底色不变
- [ ] 表格/日志/代码区保持素净可读
- [ ] DOM id / 函数签名 / 数据流零改动（纯样式覆盖）
