---
name: 自学编程
description: 开发与迭代「代码翻译官」零基础自学编程 web 应用（让完全没接触过代码的小白，从看懂到会写）。单文件 index.html，纯前端 + localStorage，含七大板块：代码是啥（概念打比方）、逐行翻译（点代码看人话）、黑话词典（36 关键词分类速查）、自己动手写（带半成品的填空练习 + 内置迷你 Python 解释器 v2 + 变量盒子可视化）、小测验、自由练习场（任意写代码即时运行）、听学电台（Web Speech 把全部课程读出来，开车跑步戴耳机免提学，14 集节目单 + 断点续听 + 语速调节 + 屏幕常亮防锁屏）。触发场景：用户想新增/优化这个学编程应用的功能、加例子、加练习题、扩展迷你解释器、改电台/朗读、改文案视觉、做进度记忆、或部署分享。每次改完都要：①更新本地 skill 文件 ②commit 并 push 到 GitHub。
---

# 自学编程 · 代码翻译官

一个**给编程小白**的自学网站，愿景是**让从没碰过代码的人，从「看不懂天书」到「能读会写」**。本 skill 既是产品代码，也是迭代手册。

## 当前形态

- **单文件** `index.html`，纯前端，无构建步骤、无后端，用浏览器 `localStorage` 存学习进度。
- 这个文件就是 skill 仓库的根，也是 GitHub Pages 的入口（在线体验即下载链接）。
- 全中文、移动端友好、宣纸/朱砂红的国风配色（变量在 `:root`）。
- **GitHub**：https://github.com/lovezhujunjie-sys/zixue-coding-translator （public）
- **在线体验（GitHub Pages）**：https://lovezhujunjie-sys.github.io/zixue-coding-translator/

## 六大板块（顶部 6 个 tab，JS 在 `.tab` 切换逻辑）

1. **① 代码是啥**（`#intro`）— 9 张概念卡片，每个编程概念配一个生活打比方（代码=菜谱、变量=贴标签的盒子、函数=咖啡机按钮、循环=跑5圈、列表=鸡蛋托盘…）。最后一张是「看懂代码三板斧」。纯静态。
2. **② 逐行翻译**（`#translate`）— 数据数组 `examples`（7 个真实 Python 小程序：点单/成绩/循环/问名字/自定义函数/列表清单/while倒计时）。每行代码可点击（也可键盘 Tab+Enter，已加 a11y），右侧把它「翻译成大白话」（`label`/`human`/`detail` 三段）。`runBtn` 模拟逐行打印 `output`（**注意：这里的输出是写死的演示数据，新增例子时要和真实 Python 行为一致**）。逻辑 `loadExample()`。
3. **③ 黑话词典**（`#glossary`）— 数据 `glossaryGroups`，36 个关键词按 5 类分组（常见动作 / 判断与重复 / 盒子与数据 / 函数相关 / 符号与格式），卡片速查。
4. **④ 自己动手写**（`#write`）— 数据 `writeTasks`（10 个练习：打招呼/变量/算术/if/if-else/for循环/累加/单双数%/while倒计时/列表）。每题给「半成品」`starter` + 提示 `hint` + 通关判定 `check(out)` + 鼓励 `win`。编辑器 Tab 键插入 4 空格。运行调内置解释器。**进度记忆**：通关写入 `localStorage('zxbc_write_done')`，卡片标 ✓，顶部 `#writeProgress` 显示「已完成 X/10」。
5. **⑤ 小测验**（`#quiz`）— 数据 `quizData`（8 题选择题），逐题反馈 + 讲解。最好成绩存 `localStorage('zxbc_quiz_best')`，结算页显示历史最佳；评语阈值按 `quizData.length` 比例算，加题不用改。逻辑 `renderQuiz()`。
6. **⑥ 自由练习场**（`#sandbox`）— 一个大编辑器，随便写随便跑，没有题目。内容自动存 `localStorage('zxbc_sandbox')`，可「清空」「放个例子」。
7. **⑦ 听学电台 🎧**（`#radio`）— 免提音频学习模式（开车/跑步/做家务戴耳机听）。详见下面「听学电台」一节。

## 听学电台（免提学习，`Radio` 模块）

用浏览器自带 `speechSynthesis`（Web Speech）朗读，**零音频文件、零依赖、离线可用**：

- **节目单自动生成**（`build()`）：内容只维护一份，电台从现有数据现场生成 14 集——第一站 9 张概念卡（读 DOM `#intro .card` 的 textContent）+ 词典 5 类（`glossaryGroups`）+ 逐行翻译 7 例（`examples` 的 label/human/detail，**不读原始代码读人话翻译**）+ 听力问答（`quizData`：读题和选项 → 留 3.2 秒作答 → 公布答案讲解）。**新增卡片/词条/例子/题目会自动进电台，不用改电台代码**。
- **代码转口语** `codeSpeech()`：`==`→"双等号"、`>=`→"大于等于"、`*`→"乘" 等，符号代码才能听懂。
- **播放器**：大圆钮 ▶/⏸、⏮⏭ 上一段/下一段（跨集自动衔接）、节目单点选任意集、语速 0.8/1/1.2/1.5、连播开关；正在朗读的文本同步显示（瞄一眼跟上）。
- **断点续听**：`localStorage('zxbc_radio')` 存 {ep, seg, rate, autoNext, keepAwake}，下次打开从上次位置继续。
- **屏幕常亮**：播放时 `navigator.wakeLock` 防手机锁屏打断（移动端锁屏会杀掉 speechSynthesis），可关；`visibilitychange` 回来自动重新请求。
- **引擎细节**：pause 用 `cancel()` + 记段落位置实现（`speechSynthesis.pause()` 在安卓上不可靠），恢复时整段重读；段落都是短句，避开 Chrome 长朗读 15 秒掐断的老 bug；`onerror` 也会推进不卡死；选声音优先 zh 高质量人声（Tingting/Xiaoxiao 等）。
- **诚实边界**：锁屏/切 App 会被系统暂停（所以才做了屏幕常亮）；耳机线控按键控制不了 speechSynthesis（Media Session 只认真实 audio 流，要支持得改用预生成音频文件，那就破坏单文件原则了——除非用户要求）。

## 变量盒子可视化（教学特色）

每次运行代码后，`renderVars()` 把所有变量画成「📦 贴标签的盒子」chip（`.wvarbox`/`.vchip`），呼应第一站「变量=盒子」的比方；文字值带引号、数字不带，强化 `"18"` 与 `18` 的区别。「动手写」每张卡和「自由练习场」都有。格式化用全局 `showVal()`。

## 内置迷你 Python 解释器 v2 `runMiniPython(code)`（核心资产）

纯 JS 手写，**零依赖、离线可用**，给「动手写」和「自由练习场」共用。返回 `{out: 输出行数组, vars: 结束时的变量表}`。架构：`tokenize` 分词 → 递归下降表达式求值（`pOr→pAnd→pNot→pCmp→pAdd→pMul→pUnary→pPow→pPostfix→pPrimary`，**真正的运算优先级和括号**）→ `execBlock` 按缩进递归执行语句。当前支持：

- 变量赋值（**变量名可用中文**）、`+= -= *= /=`、下标赋值 `lst[0] = x`；保留字不许当变量名
- 数字 / 文字 / `True/False/None` / 列表 `[ ]`（含负数序号 `[-1]`、`.append()`）
- 算术 `+ - * / % // **`（优先级正确、可加括号）；`文字*整数` 重复；**数字+文字会温柔报错并教用 `str()`**（和真 Python 一致）；除以 0 报错
- 比较 `>= <= == != > <`（**`"18" == 18` 是 False**，强化文字≠数字）、`and / or / not`
- 内置函数：`print input len int float str abs round range`；文字方法 `.upper() .lower()`
- `if / elif / else`、`while`、`for x in (range/列表/文字)`、`break / continue`
- `#` 行内注释（避开字符串里的 `#`）；`guard()` 超 20 万步防死循环
- **报错带行号**（「第 N 行：…」）且全是温柔的教学语言；常见笔误有专门提示（print 忘括号、if 忘冒号、单 = 误当 ==、def 未支持）

**扩展时注意**：这是教学级实现，不追求完整 Python。已知边界：`and/or` 是急算（不短路）、不支持比较链 `1<x<5`、字典/def 自定义函数/字符串切片未实现。加新语法就在 `pPrimary`/`callBuiltin`/`callMethod`/`execBlock`/`execSimple` 里加分支，**并务必跑一遍下面的测试**。

## 改完必测（防回归）

JS 语法检查（块注释里别出现 `*/`，上过当）：
```bash
cd ~/.claude/skills/自学编程 && python3 -c "
import re;html=open('index.html',encoding='utf-8').read()
open('/tmp/zx.js','w',encoding='utf-8').write(re.search(r'<script>(.*)</script>',html,re.S).group(1))" && node --check /tmp/zx.js && echo OK
```
然后在浏览器（或 preview_eval）里对 `runMiniPython` 跑一组断言：优先级 `2+3*4=14`、左结合 `10-2-3=5`、while 倒计时、break/continue、列表 append/取项/负索引、中文变量名、`"18"==18` 为 False、报错带行号。改完 UI 还要点一遍六个 tab 确认无 console 报错。

## 本地预览

```bash
python3 -m http.server --directory ~/.claude/skills/自学编程 8138
# 浏览器打开 http://localhost:8138/index.html
# (Claude 的 launch.json 里已有配置名 zixue-coding)
```

## 迭代铁律（每次修改优化都要做）

> 用户的明确要求：每次改完都执行这两个动作。

1. **更新本地 skill**：直接编辑 `~/.claude/skills/自学编程/index.html`（及本 SKILL.md），这是唯一真源。
2. **推到 GitHub**：
   ```bash
   git -C ~/.claude/skills/自学编程 add -A
   git -C ~/.claude/skills/自学编程 commit -m "描述本次优化"
   git -C ~/.claude/skills/自学编程 push
   ```
   推送后 GitHub Pages 自动更新，在线链接即最新版（也是给用户的下载/分享链接）。

## 设计原则与注意

- 一切面向**零基础小白**：每个概念先打生活比方，多鼓励、少术语，报错文案要温柔（「报错是正常的，改改再试」）。
- 所有展示用户输入 / 运行输出的地方都已 `escapeHtml()` 转义，新增处务必沿用，防 XSS。
- 保持**单文件、零依赖**的轻量原则，除非用户同意引入后端/框架。
- a11y：逐行翻译的代码行已可键盘聚焦（`tabIndex`+Enter/Space）。新增交互沿用。

## 后续可做（路线）

- 解释器扩展：`def` 自定义函数、字典 `{}`、字符串切片 `s[1:3]`、短路求值、比较链。
- 内容扩展：更多逐行翻译例子、按难度分级的练习、错题本。
- 学习路径与成就系统（连续打卡，参考「身心灵」skill 的 streak/热力图做法）。
- 「单步执行」可视化：一行一行走，实时看变量盒子变化（解释器已能拿到 vars，差一个逐步驱动）。
- 出第二门语言（如 JavaScript）或换主题皮肤；改微信小程序便于国内传播。
