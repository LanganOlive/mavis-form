# Mavis-form

Trading-system knowledge graph (mavis-form). Parallel to yaoguform but schema for **Mavis trading system** data: decisions, errors, case studies, monitoring pool, trading rules — not 妖股复盘.

## 目录

```
mavis-form/                       # graph 产物
├── graph.json                    # 知识图谱
├── graph.html                    # pyvis 可视化
├── graphrag.json                 # RAG-friendly export
├── GRAPH_REPORT.md               # markdown 报告
└── manifest.json                 # 源文件清单

mavis-corpus/                     # 源语料
└── Mavis产出/                    # Mavis 系统的 .md 产出

we-mp-rss/                        # pipeline 脚本
├── _rebuild_for_mavis.py         # 重建脚本 (跟 _rebuild_for_yaogu.py 对称)
└── _mavis_subagent_prompt_template.md
```

## 与 yaoguform 的差异

| | yaogu-form | mavis-form |
|---|---|---|
| 数据视角 | 妖股历史复盘 (启动→见顶) | Mavis 交易系统日志 (决策/错误/案例/规则) |
| 节点类型 | stock / concept / policy / sector / event | decision / error / rule / case_study / stock / sector / indicator |
| 边关系 | 受益于 / 触发 / 关联 / 接力 | 修正 / 引用 / 触发 / 适用于 / 触犯 |
| 源语料 | yaogu-corpus/妖股档案/*.md | mavis-corpus/Mavis产出/*.md |
| 缓存位置 | {grainform_dir}/graphify-out/cache/ | {mavisform_dir}/graphify-out/cache/ |
| GitHub repo | LanganOlive/yaoguform | (待定) |

## Schema 详情

节点 type 枚举:
- `decision` — 一次决策 (买入/卖出/加减仓/空仓/...)
- `error` — 一个错误编号 (#001-#067)
- `rule` — 一条交易规则 (V2.2 系统的若干条铁律)
- `case_study` — 一个案例库条目 (利好出尽/中国西电涨停板出货/...)
- `stock` — 关注的股票
- `sector` — 板块
- `indicator` — 指标 (MA5/RSI/MACD/涨停板战法/...)
- `event` — 关键事件 (盘前/盘中/盘后)
- `person` — 关键人物 (Mavis 维护者)

## Pipeline

跟 yaogu-form 完全对称:

```
mavis-corpus/Mavis产出/*.md
    ↓ subagent 抽取 (per-chunk)
.graphify-out/.graphify_chunk_<id>.json
    ↓ merge + dedupe by node id
graph.json
    ↓ graphify.build_from_json
NetworkX Graph
    ↓ Leiden community detection
graph.html + GRAPH_REPORT.md
```

## 当前状态

- 1 个源文件 (Mavis 7/16-7/25 所有产出汇总)
- 还没跑过 extraction / build
- 还没 git init 也没 push

下一步: 写 `_rebuild_for_mavis.py` (从 yaogu 复制改路径), 写 subagent prompt template, 跑第一次 extract + build.