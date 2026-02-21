# ALMS - Amos Literature Management System
# 仓颉文献管理系统 - 版本控制

## 🏛️ 命名致敬

**ALMS** = **A**mos **L**iterature **M**anagement **S**ystem

**中文名**: 仓颉文献管理系统

**命名来源**: [仓颉 (Cangjie)](https://en.wikipedia.org/wiki/Cangjie) —— 中国神话中创造汉字的始祖，黄帝时期的史官。相传他观察鸟兽足迹、山川走势，首创文字，使人类知识得以记录传承。

**寓意**: 像仓颉造字一样，帮助Amos管理现代学术文献，从海量信息中提炼精华，构建知识体系。

---

## 当前版本
- **v1.0** (2026-02-21) - 初始稳定版本
  - 功能：SAGE分析展示、关键词词云、搜索筛选、详情页
  - 命名：ALMS (原Prometheus)
  - 文件：`web/versions/index_v1.0.html`
  - 备份：`web/index.html` (当前运行版本)

## 版本历史
```
v1.0 - 2026-02-21
  ├─ 命名确立: ALMS (Amos Literature Management System)
  ├─ 中文名: 仓颉文献管理系统
  ├─ 致敬: 仓颉 (Cangjie) - 中国文字之神
  ├─ SAGE中文分析展示（8个字段）
  ├─ 关键词词云（80个关键词，彩色散在分布）
  ├─ 分级/IF统计卡片
  ├─ 搜索+筛选功能
  └─ 详情页弹窗

v0.x - 开发迭代 (原Prometheus)
  ├─ index.html.a - 基础版本 (221KB)
  ├─ index.html.b - SAGE版无摘要 (263KB)
  ├─ index.html.c - 完整SAGE中文版 (446KB)
  ├─ index.html.d - 带分级筛选版 (458KB)
  └─ index.html.prometheus_backup - 最终Prometheus版本
```

## 更新规范

### 1. 新增文献流程
当通过auto-harvest或手动添加新文献时：

```
1. 新文献加入数据库 (harvested_papers_YYYY-MM-DD.json)
2. 触发SAGE分析（如果新文献无分析）
3. 运行 rebuild_webui.py 重新生成HTML
4. 自动继承v1.0结构
5. 测试验证
6. 发布更新
```

### 2. 版本升级流程
当需要修改页面结构时：

```
1. 创建新版本号 (v1.1, v1.2, v2.0...)
2. 复制 v1.0 → versions/index_v{new}.html
3. 修改 rebuild_webui.py 模板
4. 充分测试
5. 更新 VERSION.md
6. 部署新版本
```

### 3. 文件命名规范
```
web/
  index.html              # 当前运行版本
  versions/
    index_v1.0.html      # 稳定版本归档
    index_v1.1.html      # 未来版本
    ...
```

## 数据结构契约

### 文献数据结构 (Paper Schema)
```javascript
{
  id: string,              // PMID_xxx 或 DOI_xxx
  pmid: string,
  title: string,
  authors: [string],
  journal: string,
  year: number,
  impact_factor: number,
  grade: 'P1'|'P2'|'P3',
  categories: [string],    // 分类标签
  keywords: [string],      // 关键词列表
  tags: [string],
  sage: {
    tldr_lightning: string,
    tldr_brief: string,
    tldr_detailed: string,
    core_findings: [string],
    clinical_significance: string,
    methodology: {
      study_type: string,
      key_techniques: [string],
      sample_size: string,
      data_analysis: string
    },
    innovation_score: number,
    innovation_rationale: string,
    limitations: [string],
    research_gaps: [string]
  },
  links: {
    pubmed: string,
    doi: string,
    google_scholar: string
  }
}
```

### 全局关键词数据
```javascript
keywordsData = [
  {word: string, count: number},
  ...
]
```

## SubAgent 接口

### 新增文献Agent
- **触发**: auto-harvest完成后 / 手动添加
- **输入**: 新论文列表（PubMed ID或DOI列表）
- **输出**: 更新后的数据库 + 重新生成的HTML
- **工具**: fetch_papers.py → analyze_sage.py → rebuild_webui.py

### SAGE分析Agent
- **触发**: 新论文无sage_analysis字段时
- **输入**: 论文摘要、标题、元数据
- **输出**: 完整的sage_analysis对象
- **模型**: Kimi K2.5 (kimi-coding/k2p5)

### 版本发布Agent
- **触发**: 需要发布新版本时
- **输入**: 版本号、更新说明
- **输出**: 版本归档、更新日志

## 质量保证清单

每次更新后检查：
- [ ] 所有文献正确加载
- [ ] 关键词词云正常显示
- [ ] 点击筛选功能正常
- [ ] 详情页SAGE分析完整
- [ ] 搜索功能正常
- [ ] 无JavaScript错误
- [ ] 文件大小合理 (300KB-600KB)

## 回滚机制

如果发现严重问题：
```bash
# 回滚到v1.0
cp web/versions/index_v1.0.html web/index.html
# 重启web服务器
```

## 命名致敬

> "昔者仓颉作书，而天雨粟，鬼夜哭" ——《淮南子》
> 
> *When Cangjie created writing, the sky rained grain and ghosts cried at night.*

**仓颉 (Cangjie)**
- 中国神话中的文字之神
- 黄帝时期的史官
- 传说他有"双瞳四目"，观察鸟兽足迹首创文字
- 造字成功时"天雨粟，鬼夜哭"
- 被历代文人、书法家祭拜

**ALMS 致敬仓颉**：
- 仓颉创造了记录人类知识的系统
- ALMS 帮助Amos管理现代学术文献
- 两者都在做同一件事：**让知识有序传承**

---
📚 ALMS - Amos Literature Management System
像仓颉造字一样管理文献
最后更新: 2026-02-21
版本: v1.0
