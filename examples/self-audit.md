# 自查记录：用本 skill 的证据规则审查本 skill

> 审查日期：2026-08-17
> 审查对象：本 skill 的 `references/` 全部参考文件
> 依据条款：`evidence-rules.md` 第 8 节「外部资料」、第 9 节「成稿后事实抽查」，以及 `sample-deconstructions.md` 开头「页面失效或身份无法复核时立即降级，不沿用旧结论」
> 全过程命令可复现，见文末。

一个要求别人「每条事实都能回到原始来源」的 skill，自己的参考文件必须先过一遍同样的关。这份记录保留了真实的检查过程和检查结果，包括查出来的三个问题。

---

## 检查一：外部链接是否还活着

本 skill 的参考文件共引用 6 个外部来源。逐条实际访问：

| 来源 | 出现位置 | 结果 |
|---|---|---|
| 河长制下小微水体的治理（中国传媒大学） | sample-deconstructions §2、style-examples §1 | 200，正常 |
| 圆安居梦，筑幸福滩（黄河科技学院） | sample-deconstructions §5 | 200，正常 |
| OpenAI build-report SKILL.md | sample-deconstructions §7 | 200，正常 |
| 京东参与乡村振兴专题调研报告（人大 SARD） | sample-deconstructions §6、style-examples §2 | **404，已失效** |
| 无废城市（西南科技大学） | sample-deconstructions §4 | TLS 握手失败，**当前环境无法核实** |
| 谁来种粮（挑战杯官方项目库） | sample-deconstructions §3 | 连接超时，**当前环境无法核实** |

**这里有一个必须区分的判断。** 后三条都返回了空状态码，但性质完全不同：

- 人大 SARD 那条，服务器正常响应并明确返回了 nginx 404 页面，站点根目录本身是 200。这是**资源确实已被删除**。
- 西南科技大学那条是 TLS 握手在传输层就失败了，跳过证书校验仍然拿不到响应。
- 挑战杯官方项目库那条是 40 秒超时，没有收到任何字节。

后两条是**当前网络环境到该站点不通**，不是内容失效。按本 skill 的规则，这两条应标为「当前环境无法核实」，不得据此降级已经写入的判断，也不得当成死链删除。把连接失败当成页面失效，和把 404 当成「可能只是暂时打不开」，是同一类错误的两个方向。

## 检查二：引文能否回到原文

`style-examples.md` 第 2 节引用了京东报告的一句话，用引用块标成了短摘录：

> 「成本地板不断抬升，销售价格受到天花板封顶。」

由于原始链接已 404，按规则必须回查存档。Internet Archive 存有两份快照：2024-08-17（8.6 MB，完整可读）与 2025-09-05（5.0 MB，xref 表损坏，无法解析）。取前者提取全文核对，原文实际是分属两个自然段的两句话：

> ……而是生产效率远高于农业的城市部门顶推着农业生产经营的「成本地板」不断抬升。
>
> 另一方面，国内农产品的销售价格还受到国际市场价格「天花板封顶」的影响，农产品价格甚至一定程度上出现了「地板」高于「天花板」的倒挂情况。

**认定：这是一条不合规引用。** 它把跨段的两句话缩写拼接成一句，加引号呈现为直接引语。`evidence-rules.md` 第 7 节写明「引用转写前回到原始录音或原文核对；无法核对时标为转述」。原文的意思没有被歪曲，但形式上它是转述而不是直引，不该用引用块加引号呈现。

同时确认了报告身份可核实：课题组首席专家温铁军（中国人民大学教授、可持续发展高等研究院执行院长），主持人董筱丹（农业与农村发展学院副教授），2023 年发布，21 世纪经济报道 2023-09-21 有公开报道佐证。

## 检查三：访问日期是否随内容更新

`style-examples.md` 与 `sample-deconstructions.md` 都标注「访问日期：2026-07-30」。但 `style-examples.md` 的文件修改时间是 2026-08-14，内容改过而访问日期没动。

`evidence-rules.md` 第 8 节要求「记录页面标题、发布机构、发布日期、链接和访问日期」。访问日期的作用是让读者判断这条核验有多新；不随内容更新，这个字段就失去意义。

---

## 处理结论

按本 skill 自己的规则，三个问题的处理方式：

| 问题 | 严重度 | 处理 | 状态 |
|---|---|---|---|
| 京东报告链接 404 | 高 | 换成 Internet Archive 2024-08-17 存档链接，并注明原始链接已失效及失效核查日期 | 已修复 |
| 跨段拼接的假直引 | 高 | 改为分行呈现两句原文，并标注「分属两个自然段，非连续原句」 | 已修复 |
| 另两条链接无法核实 | 中 | 标注「2026-08-17 当前环境无法核实，非判定失效」，保留原判断不降级 | 已修复 |
| 访问日期未随内容更新 | 低 | 两份参考文件访问日期更新为 2026-08-17 | 已修复 |

### 修复后复验（2026-08-17）

```bash
grep -rn "2026-07-30" references/          # → 无输出，访问日期已同步
grep -n "web.archive.org" references/*.md  # → style-examples.md、sample-deconstructions.md 各一处存档链接
```

`sample-deconstructions.md` 同时新增了一条**链接核查口径**，把这次踩到的判断困境写成规则：`404`、`410` 或内容明确失效才算来源失效；连接超时、TLS 握手失败、DNS 失败只标「本次未能核实」，不得据此降级已有结论。

规则来自真实踩坑，不是预想出来的——这一条写进文件的时间，比发现它的时间晚了 40 秒（挑战杯官方库那次超时）。

## 这次自查说明了什么

第一，**死链是必然的，不是意外。** 6 个来源用了不到一年就死了 1 个、2 个无法核实。任何写进报告的网络来源都会走这条路，所以规则不能只写「记录链接」，得写「记录到什么程度才算可回溯」。

第二，**最难发现的不是死链，是形式上合规的引用。** 那条拼接引语在文件里躺了很久，读多少遍都读不出问题，只有真的把原文拉下来逐字比对才会露馅。这也是为什么 `evidence-rules.md` 要单列「成稿后事实抽查」一节：抽查的动作是打开原始材料，不是重读自己的稿子。

第三，**绿色不等于健康。** 三个链接返回 200 就以为没事，是这次检查一开始差点犯的错。真正的判断来自逐条看响应内容和失败原因。

---

## 复现命令

```bash
# 检查一：逐条访问外部链接
for u in \
  "https://xinwenxueyuan.cuc.edu.cn/2021/0609/c7465a182725/page.htm" \
  "https://xmk.tiaozhanbei.net/project/284910/" \
  "https://www.swust.edu.cn/2025/0603/c11237a217690/page.htm" \
  "https://triz.hhxy.edu.cn/info/1163/1713.htm" \
  "https://www.sard.ruc.edu.cn/docs/2023-12/45f0d3cfc90b4f5395ff45decdd77b71.pdf" \
  "https://github.com/openai/role-specific-plugins/blob/main/plugins/data-analytics/skills/build-report/SKILL.md"
do
  printf "%s  %s\n" "$(curl -s -o /dev/null -w '%{http_code}' -L --max-time 20 "$u")" "$u"
done

# 区分 404 与连接层失败：看 curl 退出码和返回体
curl -sk -L --max-time 25 "https://www.sard.ruc.edu.cn/docs/2023-12/45f0d3cfc90b4f5395ff45decdd77b71.pdf" | head -c 200
# → <html><head><title>404 Not Found</title></head>... nginx
curl -s -o /dev/null -L --max-time 25 "https://www.swust.edu.cn/2025/0603/c11237a217690/page.htm"; echo "exit=$?"
# → exit=35 (SSL/TLS handshake failed)，属于无法核实，不是失效

# 检查二：查存档、下载、回原文核对引文
curl -s "http://web.archive.org/cdx/search/cdx?url=sard.ruc.edu.cn/docs/2023-12/45f0d3cfc90b4f5395ff45decdd77b71.pdf"
curl -sL -o jd.pdf "https://web.archive.org/web/20240817173611if_/http://sard.ruc.edu.cn/docs/2023-12/45f0d3cfc90b4f5395ff45decdd77b71.pdf"
pdftotext -enc UTF-8 jd.pdf jd.txt
grep -n "成本地板\|天花板" jd.txt
```

存档链接（2024-08-17 完整版）：
https://web.archive.org/web/20240817173611if_/http://sard.ruc.edu.cn/docs/2023-12/45f0d3cfc90b4f5395ff45decdd77b71.pdf
