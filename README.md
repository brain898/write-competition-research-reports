# 竞赛调研报告写作 · write-competition-research-reports

> 让 AI 写调研报告时，不敢编数据，不敢把五个案例说成「八成企业」，不敢把一次试用写成长期成效。

![Agent Skill](https://img.shields.io/badge/Agent-Skill-000?style=flat-square)
![Claude Code](https://img.shields.io/badge/Claude%20Code-supported-blue?style=flat-square)
![Codex](https://img.shields.io/badge/Codex-supported-blue?style=flat-square)
![Language](https://img.shields.io/badge/language-简体中文-red?style=flat-square)
![License](https://img.shields.io/badge/license-MIT-green?style=flat-square)
[![skills.sh](https://img.shields.io/badge/skills.sh-listed-8A2BE2?style=flat-square)](https://skills.sh/s/brain898/write-competition-research-reports)

一个面向中文竞赛调研报告的 Agent Skill。它管的不只是让 AI 写得更对，是让 AI 把凭什么这么说一并交出来：材料范围、实名授权状态、外部链接核验结果写在正文旁边，字数账和材料台账在交付说明里报给你一眼，不占正文篇幅。

## 你什么时候需要它

**场景一：你手上有真材料，AI 却写出了政策综述。**
你走访了五家企业、录了八小时音、发了两百份问卷，让 AI 起草报告，它交回来一篇「随着乡村振兴战略深入推进……建议加大扶持力度、完善长效机制」。你的实地材料一句没进去。

**场景二：你不知道自己的结论已经越界。**
五家企业里有四家提到同一个问题，AI 写成「八成企业普遍面临」。评委答辩时问一句「你这个比例的分母是什么」，现场就下不来台。

**场景三：定稿前你不确定哪里会被追问。**
报告写完了，但你不知道哪些数字回不到原始材料，哪些企业真名还没拿到公开授权，哪些建议其实和前文发现对不上。

## 它会交付什么

不是更长的稿子，是一份**每句话都能回到原始材料**的稿子。

| 交给它之前 | 交给它之后 |
|---|---|
| 「80% 的受访企业缺乏电商运营能力」 | 「走访的五家企业中，有三家提到线上销售缺少固定运营人手」 |
| 「当地属于熟人社会，因此治理观念不易改变」 | 写清熟人关系具体如何改变了执法、反馈或合作行为 |
| 「建议加大扶持力度，完善长效机制」 | 谁、在什么条件下、做什么动作、用哪个指标验证、指标不动时何时停止 |
| 「项目已被当地采纳，成效显著」 | 「已发生试用」与「长期成效」分开写，后者标为待验证 |
| 引用了一个链接，没人知道它还在不在 | 死链、拼接引语、无年份的网络资料，逐条标为待确认或降级 |

完整对照见 [`examples/before-after.md`](examples/before-after.md)。

**它对自己也一样严。** [`examples/self-audit.md`](examples/self-audit.md) 记录了三轮真实自查，每一轮都是用这个 skill 自己的证据规则审查它自己的文件。第一轮审参考文件，查出一条 404 死链、一条跨段拼接的假直引、两条无法核实但不足以判为失效的链接、一处未随内容更新的访问日期。第二轮审 README，查出首屏挂着自己的死链、一笔回算不出来的字数账、一个没有分母的测试分数。第三轮审第二轮新补的基准报告，查出三处，全部是第二轮自己写进去的。每轮的发现、处理和复现命令都在文件里，未解决的标着未解决。

## 快速开始

一行安装：

```bash
npx skills add brain898/write-competition-research-reports
```

或者直接 clone 到对应的 skills 目录：

```bash
# Claude Code（全局）
git clone https://github.com/brain898/write-competition-research-reports.git \
  ~/.claude/skills/write-competition-research-reports

# Codex（全局）
git clone https://github.com/brain898/write-competition-research-reports.git \
  ~/.codex/skills/write-competition-research-reports

# NewMax（全局）
git clone https://github.com/brain898/write-competition-research-reports.git \
  ~/.newmax/skills/write-competition-research-reports

# 只在某个项目里用
git clone https://github.com/brain898/write-competition-research-reports.git \
  .claude/skills/write-competition-research-reports
```

装完新开一个会话，直接说人话即可，Agent 会按 `description` 自动匹配。

## 触发方式

以下说法都能触发：

- 「帮我起草挑战杯调研报告的第三章」
- 「这是我们三下乡的访谈记录和问卷，先帮我清点一下能支持什么结论」
- 「按评委视角审一下这份报告，答辩会被问什么」
- 「这份报告要定稿了，做一次全文体检」
- 「把这份两万字的报告压成八分钟路演稿，别改成宣传稿」
- 「帮我看看这段结论有没有超出样本能证明的范围」

**不要用它做**：纯活动总结、没有调研材料的政策综述、商业计划书、纯学术论文。

## 示例

**输入：** 五家农产品加工企业的访谈记录，用户要求写「比较与机制」一章。

**它会做的：** 先建证据清单（来源、时间、授权状态、能支持什么/不能支持什么），再为每个核心判断建证据卡并检查反例，然后建框架覆盖表，最后才动笔写正文。写完逐条检查本章是否只完成一个任务、每个判断是否紧邻证据、实际字数是否在预算内。

**它不会做的：** 用提纲、方法说明或工作计划代替正文；把五家企业写成五篇独立简介；在没有授权的情况下写出企业真名。

## 它和同类有什么不同

| | 本 skill | 学术论文写作类 skill | 质性研究方法类 skill |
|---|---|---|---|
| 语境 | 中文竞赛调研（挑战杯、三下乡、返乡实践） | 英文期刊/会议论文 | 通用质性研究方法学 |
| 核心约束 | 结论强度不超过证据强度 | 投稿格式与论证清晰度 | 抽样、编码、信效度 |
| 独有内容 | 实名公开授权登记、评委追问清单、青年视角进因果链、字数预算净减字核销 | 引用管理、venue 适配 | 编码工具适配 |
| 交付形态 | 中文段落式正文 | LaTeX / 结构化章节 | 研究设计与编码方案 |

同类做得好的地方不少，这里只说定位差异，不做优劣评判。

## 安全边界

它**不会**：

- 编造数据、案例、引文、授权或政策。查不到就写「未核实」，不补造看似合理的来源。
- 把没拿到实名公开授权的企业或个人真名写进终稿。
- 把「已试用」「已被采纳」写成「已产生长期成效」。
- 未经要求覆盖你的源材料，或擅自改动你给定的章节结构与表达公式。
- 把「尚无充分证据」「待确认」这类克制措辞，在润色时改成确定性说法。

它**会停下来问你**：

- 字数预算会实质影响内容取舍时。
- 核心问题完全无法用现有材料回答时。
- 比赛硬约束（字数、格式、匿名要求、AI 使用披露）本地查不到原文时，它会标「未核实」而不是按惯例猜。

## 文件结构

```
write-competition-research-reports/
├── SKILL.md                          # 主入口：七阶段流程、任务路由、表达纪律
├── README.md
├── LICENSE
├── .claude-plugin/
│   └── marketplace.json              # Claude Code plugin marketplace 清单
├── agents/
│   └── openai.yaml                   # Codex 展示元数据
├── examples/
│   ├── before-after.md               # 注水段落 → 合规段落，逐条标注改动依据
│   ├── benchmark.md                  # 对抗性测试的设计、判分口径、结果与限制说明
│   └── self-audit.md                 # 用本 skill 的规则审查本 skill 自己的参考文件
└── references/
    ├── core-method.md                # 论证主链、调研反转、案例比较、机制、对策、青年视角
    ├── evidence-rules.md             # 证据等级、证据卡、结论强度表、授权与匿名、事实抽查
    ├── quality-rubric.md             # 评分优先级、审稿七问、三轮删改、评委视角
    ├── report-shapes.md              # 五章默认结构与三类变体、章节边界、篇幅分配
    ├── sample-deconstructions.md     # 竞赛报告样本拆解，分 S1-S4 来源等级；含外部链接核查注记表
    └── style-examples.md             # 正文文风示范：事实、机制、匿名、比较、对策怎么写
```

参考文件按需加载，不会一次全读进上下文。

## 它经过什么验证

发布前跑过五条对抗性测试，每条单开干净会话、不提示 Agent 使用本 skill、第一次输出即被测对象：

- **不装 skill 的对照组：0/5。** 这五次判分来自两轮：其中三条探针首轮在无 skill 条件下也通过了，说明它们测的是模型通用能力，于是各加严一条红线后重跑，三条全部失败，且只触碰新增的那条红线
- **装了之后：判分规则 v2 下 5/5，v1 下 4/5。** 差别只在字数那条红线，它当天从「自报值与实测偏差超 3% 即失败」降级为「只判净减方向」。改尺子会抬高历史分数，所以两个版本都留着，引用哪个都得写版本
- **改动内核后跑了三轮回归。** 其中「外部链接核验到什么程度就停手」这条规则，前两轮因为测试链接恰好都命中缓存注记而根本没被走到，两轮绿灯都是假的，换成注记表外的链接第三轮才验实

**这个分数能支持什么、不能支持什么，单列在 [`examples/benchmark.md`](examples/benchmark.md)。** 简单说：n=5 是定性探针不是有效性度量，判分方虽与跑测方分离但非盲评，单次采样，模型版本未记录因而未跨模型验证，测试用例不入库因而第三方无法独立复现。按本 skill 自己的结论强度表，它只能支持「在这五条探针上，装与不装的第一次输出有稳定差异」，不能支持「这个 skill 必然有效」。一个要求别人交出分母的 skill，自己的分母也得摆出来。

对照组的失败点落在同一模式上：内容大多写对了，但没有一条留下可供第三方核对的痕迹，不说材料范围、不登记授权状态、不报字数变化、不区分「链接 404」与「本机连不上」。这个 skill 管的就是这一层。

基线不需要满分，需要真实。首轮跑出 3/5 比跑出 5/5 更有用。

## 开发

改完 `SKILL.md` 或任何参考文件，重新 clone 覆盖安装目录即可。

对抗性测试用例、判分规则和历次运行记录保留在开发者本地，不入库。原因是 Agent 会顺手翻 skill 目录，而测试用例写着全部红线和期望行为，被测方读到就是开卷考，跑出来的绿灯是假的。改动内核后要验证有没有改坏，自建一套用例即可，关键是每条单开干净会话、不点名 skill、只认第一次输出。

## License

MIT
