# Scientific Agent Skills 项目详解文档

> **版本**: v2.39.0 | **技能数量**: 138个 | **维护方**: K-Dense Inc.

---

## 一、项目简介

**Scientific Agent Skills** 是一个面向 AI 智能体（AI Agent）的综合性科学技能库。它将复杂的科学研究工作流封装为模块化的 "技能"（Skill），让任何支持 [Agent Skills](https://agentskills.io/) 开放标准的 AI 编码助手（如 Cursor、Claude Code、Codex、Gemini CLI 等）都能执行专业的科学研究任务。

### 1.1 核心理念

传统上，科研人员需要花费大量时间查阅文档、配置环境、编写和调试代码。Scientific Agent Skills 的核心理念是：

- **即装即用**：每个技能都包含完整的文档、代码示例和最佳实践
- **跨学科融合**：打破学科壁垒，将基因组学、化学信息学、临床医学、机器学习等领域的工具无缝整合
- **自然语言驱动**：科研人员用日常语言描述需求，AI Agent 自动调用相应技能完成复杂任务

### 1.2 覆盖领域

| 领域 | 技能数量 | 典型工具/数据库 |
|------|---------|---------------|
| 🧬 生物信息学与基因组学 | 21+ | Scanpy, BioPython, scvi-tools, gget |
| 🧪 化学信息学与药物发现 | 10+ | RDKit, DeepChem, DiffDock, MedChem |
| 🔬 蛋白质组学与质谱 | 2 | matchms, pyOpenMS |
| 🏥 临床研究与精准医学 | 8+ | ClinicalTrials.gov, DepMap, ClinVar |
| 🖼️ 医学影像与数字病理 | 3 | pydicom, histolab, PathML |
| 🤖 机器学习与AI | 16+ | PyTorch Lightning, Transformers, PyMC |
| 🔮 材料科学与化学 | 7 | Pymatgen, COBRApy, Qiskit |
| ⚙️ 工程与仿真 | 4 | MATLAB, SimPy, FluidSim |
| 📊 数据分析与可视化 | 16+ | Matplotlib, GeoPandas, Dask |
| 🧪 实验室自动化 | 4 | PyLabRobot, Opentrons, Benchling |
| 📚 科学传播 | 20+ | 文献检索、论文写作、海报制作 |
| 🔬 多组学与系统生物学 | 4+ | PrimeKG, LaminDB, HypoGeniC |
| 🎓 研究方法论 | 12+ | 头脑风暴、假设生成、基金申请 |

---

## 二、整体架构

### 2.1 目录结构

```
scientific-agent-skills-main/
├── .github/workflows/          # CI/CD 自动化工作流
│   ├── security-scan.yml       # 每周安全扫描
│   ├── pr-skill-scan.yml       # PR 技能安全审查
│   └── release.yml             # 自动版本发布
├── docs/                       # 项目级文档
│   ├── examples.md             # 23个跨学科实际案例
│   ├── scientific-skills.md    # 所有技能的详细说明
│   └── open-source-sponsors.md # 开源依赖致谢
├── scientific-skills/          # 🎯 138个技能目录（核心）
│   ├── scanpy/                 # 单细胞分析技能
│   │   ├── SKILL.md            # 技能定义文档（必须）
│   │   ├── references/         # API参考、快速指南
│   │   ├── scripts/            # 可执行辅助脚本
│   │   └── assets/             # 图片、模板等资源
│   ├── rdkit/                  # 化学信息学技能
│   ├── database-lookup/        # 78个数据库统一查询
│   └── ... (共138个技能目录)
├── pyproject.toml              # Python 项目配置
├── scan_skills.py              # 全量安全扫描脚本
├── scan_pr_skills.py           # PR 增量扫描脚本
└── uv.lock                     # 依赖锁定文件
```

### 2.2 三层架构模型

```
┌─────────────────────────────────────────────────────────────┐
│  第一层：AI Agent 宿主层                                       │
│  (Cursor / Claude Code / Codex / Gemini CLI / K-Dense Web)   │
│  • 解析用户自然语言指令                                         │
│  • 自动发现和匹配相关技能                                       │
│  • 协调多技能组合执行                                           │
└──────────────────────────┬──────────────────────────────────┘
                           │ 读取 SKILL.md
                           ▼
┌─────────────────────────────────────────────────────────────┐
│  第二层：Skill 技能层                                          │
│  (138个科学技能 + 自主生成技能的 autoskill)                     │
│  • 每个技能 = 结构化文档 + 参考材料 + 辅助脚本                   │
│  • YAML Frontmatter 元数据（名称、描述、许可、作者）              │
│  • Markdown 正文（使用场景、快速入门、API示例、工作流）           │
└──────────────────────────┬──────────────────────────────────┘
                           │ 调用 Python包 / REST API
                           ▼
┌─────────────────────────────────────────────────────────────┐
│  第三层：科学工具生态层                                         │
│  (Python包 / 数据库API / 云平台 / 实验室设备)                   │
│  • 70+ Python科学包（RDKit, Scanpy, PyTorch等）                 │
│  • 100+ 科学数据库（PubChem, ChEMBL, UniProt等）                │
│  • 云平台（Modal, DNAnexus, LatchBio）                         │
│  • 实验室自动化设备（Opentrons, PyLabRobot）                    │
└─────────────────────────────────────────────────────────────┘
```

---

## 三、Skill 技能的结构与规范

### 3.1 技能目录结构

每个技能都是一个自包含的目录，最小结构如下：

```
skill-name/
├── SKILL.md              # 【必需】技能定义文档
├── references/           # 【可选】参考文档子目录
│   ├── api_reference.md
│   └── workflow_guide.md
├── scripts/              # 【可选】可执行脚本
│   └── helper_script.py
└── assets/               # 【可选】静态资源
    └── diagram.png
```

### 3.2 SKILL.md 文档格式

`SKILL.md` 是技能的灵魂，采用 **YAML Frontmatter + Markdown正文** 的标准格式：

```yaml
---
name: scanpy                                    # 技能唯一标识名
description: Standard single-cell RNA-seq...    # 一句话描述（Agent用此匹配用户请求）
license: BSD-3-Clause                           # 许可证
metadata:
    skill-author: K-Dense Inc.                  # 作者信息
    requires: screenpipe                        # 【可选】特殊依赖
---

# Skill 标题

## Overview
技能概述，说明这个技能是什么、能做什么。

## When to Use This Skill
明确的使用场景列表。Agent 根据此部分判断何时调用该技能。

## Quick Start
快速入门代码示例。

## Standard Analysis Workflow
标准工作流步骤，通常包含：
1. 步骤一（含代码块）
2. 步骤二（含代码块）
...

## Best Practices
最佳实践和注意事项。

## References
参考资料链接。
```

### 3.3 Agent 如何匹配技能

AI Agent 的技能匹配逻辑：

1. **关键词匹配**：Agent 读取所有 `SKILL.md` 的 `description` 字段
2. **场景触发**：用户输入后，Agent 分析意图并与 `When to Use This Skill` 匹配
3. **多技能协调**：复杂任务可能同时触发多个技能，Agent 自动编排执行顺序
4. **显式调用**：用户也可直接在提示词中指定技能名（如 "使用 RDKit 技能分析分子"）

---

## 四、执行任务的逻辑与流程

### 4.1 单技能执行流程

以 "用 Scanpy 分析单细胞数据" 为例：

```
用户输入：
"分析我的 10x Genomics 单细胞数据，做质控、聚类和可视化"

        │
        ▼
┌──────────────────┐
│ Agent 解析意图    │ → 识别关键词：10x Genomics, 单细胞, 质控, 聚类, 可视化
│ 匹配技能          │ → 命中 scanpy 技能（描述含 single-cell RNA-seq, QC, clustering）
└────────┬─────────┘
         │
         ▼
┌──────────────────┐
│ Agent 读取        │ → 加载 scientific-skills/scanpy/SKILL.md
│ SKILL.md          │ → 提取 Quick Start 和 Standard Workflow
└────────┬─────────┘
         │
         ▼
┌──────────────────┐
│ Agent 生成        │ → 按 SKILL.md 中的标准工作流生成完整代码
│ 执行代码          │ → 调用 scanpy.read_10x_mtx() → sc.pp.calculate_qc_metrics()
│                   │ → sc.pp.normalize_total() → sc.pp.log1p() → sc.pp.neighbors()
│                   │ → sc.tl.leiden() → sc.pl.umap()
└────────┬─────────┘
         │
         ▼
┌──────────────────┐
│ 结果返回          │ → 输出 UMAP 图、聚类统计、质控报告
│ 用户确认          │ → 用户可要求进一步分析（如 marker gene 鉴定）
└──────────────────┘
```

### 4.2 多技能组合执行流程

以 "EGFR 抑制剂药物发现" 为例，展示跨 10+ 技能的复杂工作流：

```
用户输入：
"找到针对 EGFR 的新型肺癌抑制剂，需要做结构活性分析、分子对接和虚拟筛选"

        │
        ▼
┌──────────────────────────────────────────────────────────────┐
│ 阶段 1：数据收集                                              │
├──────────────────────────────────────────────────────────────┤
│ • database-lookup 技能 → 查询 ChEMBL (EGFR抑制剂 IC50<50nM)   │
│ • database-lookup 技能 → 查询 COSMIC (EGFR耐药突变)           │
│ • database-lookup 技能 → 查询 AlphaFold DB (EGFR结构)         │
│ • paper-lookup 技能 → 搜索 PubMed (耐药机制文献)              │
└──────────────────────────────────────────────────────────────┘
         │
         ▼
┌──────────────────────────────────────────────────────────────┐
│ 阶段 2：化学信息学分析                                        │
├──────────────────────────────────────────────────────────────┤
│ • rdkit 技能 → 计算分子描述符 (MW, LogP, TPSA)                │
│ • rdkit 技能 → 生成分子指纹和层次聚类                         │
│ • datamol 技能 → 基于骨架生成新化合物类似物                   │
│ • medchem 技能 → 应用 Lipinski 五规则过滤                     │
└──────────────────────────────────────────────────────────────┘
         │
         ▼
┌──────────────────────────────────────────────────────────────┐
│ 阶段 3：计算化学与机器学习                                    │
├──────────────────────────────────────────────────────────────┤
│ • deepchem 技能 → 训练图卷积网络预测 pIC50                    │
│ • diffdock 技能 → 分子对接 (野生型 & T790M突变型)              │
│ • molfeat 技能 → 分子特征化用于 ML 模型                       │
└──────────────────────────────────────────────────────────────┘
         │
         ▼
┌──────────────────────────────────────────────────────────────┐
│ 阶段 4：结果整合与报告                                        │
├──────────────────────────────────────────────────────────────┤
│ • scientific-visualization 技能 → 生成 SAR 图、对接结果可视化  │
│ • clinical-reports 技能 → 生成 PDF 综合报告                   │
│ • database-lookup 技能 → 查询 USPTO 专利情况                  │
└──────────────────────────────────────────────────────────────┘
```

### 4.3 任务执行的底层机制

从代码层面看，任务执行依赖以下机制：

| 机制 | 说明 |
|------|------|
| **Context Injection** | Agent 将相关 `SKILL.md` 内容注入对话上下文，作为 "系统提示" 的一部分 |
| **Tool Use** | 技能文档中标记 `allowed-tools: Read Write Edit Bash`，Agent 据此调用文件读写或 Shell |
| **Dependency Management** | 通过 `uv` 包管理器按需安装 Python 依赖（如 `uv pip install scanpy`） |
| **Incremental Execution** | Agent 分步骤执行，每步验证结果后再进行下一步 |
| **Error Recovery** | 执行失败时，Agent 参考 SKILL.md 的 Troubleshooting 部分尝试修复 |

---

## 五、具体使用方法

### 5.1 前置要求

- **Python**: 3.11+（推荐 3.12+）
- **包管理器**: [uv](https://docs.astral.sh/uv/)（必需，技能依赖安装用）
- **AI Agent 客户端**: Cursor / Claude Code / Codex / Gemini CLI 等

安装 uv：
```bash
# macOS / Linux
curl -LsSf https://astral.sh/uv/install.sh | sh

# Windows
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
```

### 5.2 安装技能库

#### 方式一：npx（推荐，跨平台）
```bash
npx skills add K-Dense-AI/scientific-agent-skills
```

#### 方式二：GitHub CLI
```bash
# 交互式浏览和安装
gh skill install K-Dense-AI/scientific-agent-skills

# 安装特定技能
gh skill install K-Dense-AI/scientific-agent-skills scanpy

# 指定目标 Agent 宿主
gh skill install K-Dense-AI/scientific-agent-skills --agent cursor
```

#### 方式三：手动安装（了解原理）
```bash
# 1. 克隆仓库
git clone https://github.com/K-Dense-AI/scientific-agent-skills.git

# 2. 将 scientific-skills/ 下的所需技能目录复制到你的 Agent skills 目录：
#    - Cursor: ~/.cursor/skills/ 或项目根目录 .cursor/skills/
#    - Claude Code: ~/.claude/skills/
#    - 其他 Agent: 参考对应文档的 skills 路径

# 3. 重启 Agent 客户端使其发现新技能
```

### 5.3 使用技能执行科学任务

安装完成后，直接用自然语言向 AI Agent 描述任务即可。**建议始终加上技能调用提示**：

```
Use available skills you have access to whenever possible.
```

#### 示例 1：单细胞分析
```
Use available skills you have access to whenever possible.
加载我的 10X 单细胞数据，进行质控过滤、标准化、PCA 降维、
UMAP 可视化、Leiden 聚类，然后鉴定每个 cluster 的 marker 基因。
```

#### 示例 2：药物分子性质计算
```
Use available skills you have access to whenever possible.
读取 SMILES 文件 compounds.smi，用 RDKit 计算每个分子的
分子量、LogP、TPSA、可旋转键数量，过滤出符合 Lipinski 五规则的分子，
生成结果表格和性质分布图。
```

#### 示例 3：多数据库联合查询
```
Use available skills you have access to whenever possible.
查询 TP53 基因的信息：从 NCBI Gene 获取基因摘要和通路信息，
从 UniProt 获取蛋白质序列和功能域，从 STRING 获取蛋白互作网络，
从 ClinVar 获取已知致病突变，最后汇总成一份结构化报告。
```

### 5.4 查看技能详情

每个技能的详细用法都在其 `SKILL.md` 中。查看方式：

```bash
# 直接阅读技能文档
cat scientific-skills/scanpy/SKILL.md

# 查看技能的辅助脚本
ls scientific-skills/scanpy/scripts/

# 查看技能的参考资料
ls scientific-skills/scanpy/references/
```

---

## 六、安全机制

### 6.1 自动化安全扫描

项目运行三层安全体系：

```
┌─────────────────────────────────────────────────────┐
│  1. 行为分析 (BehavioralAnalyzer)                    │
│     • 检测 prompt injection 模式                      │
│     • 识别数据外泄风险                                │
└─────────────────────────────────────────────────────┘
┌─────────────────────────────────────────────────────┐
│  2. 触发器分析 (TriggerAnalyzer)                     │
│     • 检查恶意代码触发条件                            │
│     • 识别未授权文件操作                              │
└─────────────────────────────────────────────────────┘
┌─────────────────────────────────────────────────────┐
│  3. LLM 智能分析 (LLMAnalyzer)                       │
│     • 用 Claude Sonnet 等大模型深度审查技能内容        │
│     • 检测语义层面的安全隐患                          │
└─────────────────────────────────────────────────────┘
```

### 6.2 CI/CD 安全流程

| 工作流 | 触发条件 | 功能 |
|--------|---------|------|
| `security-scan.yml` | 每周一 UTC 09:00 | 全量扫描所有138个技能，生成 `SECURITY.md` |
| `pr-skill-scan.yml` | PR 修改 `scientific-skills/**` 时 | 增量扫描变更的技能，结果以评论形式贴在 PR 上 |
| `release.yml` | `pyproject.toml` 版本号变更时 | 自动生成 Release 和 changelog |

### 6.3 用户安全建议

- **按需安装**：不要一次性安装全部技能，只安装实际需要的
- **阅读 SKILL.md**：安装前阅读技能文档，了解其功能、依赖和外部连接
- **运行安全扫描**：对第三方技能自行扫描：
  ```bash
  uv pip install cisco-ai-skill-scanner
  skill-scanner scan /path/to/skill --use-behavioral
  ```

---

## 七、特殊技能：autoskill（自动技能生成）

`autoskill` 是一个元技能（meta-skill），它能 **观察用户的工作模式并自动生成新技能**。

### 7.1 工作原理

```
用户屏幕/操作记录 → screenpipe 本地捕获
                              │
                              ▼
                    ┌─────────────────┐
                    │  本地 OCR 与聚类  │
                    │  提取工作流模式   │
                    └────────┬────────┘
                             │
                             ▼
                    ┌─────────────────┐
                    │ 与现有138个技能   │
                    │ 进行语义比对      │
                    └────────┬────────┘
                             │
                             ▼
                    ┌─────────────────┐
                    │  生成新技能草案   │
                    │  (SKILL.md + 脚本)│
                    └────────┬────────┘
                             │
                             ▼
                    用户审查 → 编辑 → 采纳/拒绝
```

### 7.2 隐私保护设计

- **本地处理**：所有屏幕数据只通过 localhost HTTP 传输
- **脱敏处理**：上传给 LLM 前自动移除邮箱、API Key、电话号码
- **默认本地 LLM**：推荐用 LM Studio + Gemma-4-31B 本地运行，数据不出机器
- **云端可选**：Claude / Foundry 后端需用户显式配置

---

## 八、实际案例详解

### 案例：多组学生物标志物发现

**目标**：整合 RNA-seq、蛋白质组学和代谢组学数据预测患者预后

**涉及技能**：PyDESeq2, pyOpenMS, HMDB, Metabolomics Workbench, UniProt, KEGG, STRING, statsmodels, scikit-learn, ClinicalTrials.gov

**执行逻辑**：

```python
# 步骤 1: RNA-seq 差异分析（PyDESeq2 技能）
"""SKILL.md 指导 Agent 完成：
   - 读取 count matrix
   - 估计 size factor 归一化
   - 离散度估计与收缩
   - Wald 检验筛选差异基因
"""

# 步骤 2: 蛋白质组学数据处理（pyOpenMS 技能）
"""SKILL.md 指导 Agent 完成：
   - LC-MS/MS 原始文件读取 (mzML)
   - 特征检测与匹配
   - 肽段鉴定和蛋白质定量
"""

# 步骤 3: 代谢物注释（database-lookup 技能 → HMDB/Metabolomics Workbench）
"""查询代谢物结构、通路关联、已知生物标志物信息"""

# 步骤 4: 通路富集分析（database-lookup 技能 → KEGG/Reactome）
"""将差异基因和蛋白质映射到生物学通路"""

# 步骤 5: 多组学整合（statsmodels + scikit-learn 技能）
"""- 用 statsmodels 进行组间相关性分析
   - 用 scikit-learn 构建多变量预测模型（如 RandomForest）
   - 特征重要性分析识别关键生物标志物
"""

# 步骤 6: 临床关联（database-lookup 技能 → ClinicalTrials.gov）
"""搜索与发现标志物相关的临床试验"""
```

---

## 九、开发自己的技能

### 9.1 最小技能模板

创建目录 `scientific-skills/my-skill/`，在其中放置 `SKILL.md`：

```yaml
---
name: my-skill
description: 一句话描述这个技能的能力和使用场景，让 Agent 能匹配到。
license: MIT
metadata:
    skill-author: Your Name
---

# My Skill 标题

## Overview
技能概述。

## When to Use This Skill
- 场景 1
- 场景 2

## Quick Start
```python
import some_package
# 代码示例
```

## Best Practices
注意事项和最佳实践。
```

### 9.2 提交贡献

1. Fork 本仓库
2. 创建功能分支：`git checkout -b feature/my-skill`
3. 遵循现有目录结构和文档规范
4. 运行安全扫描：`uv run python scan_pr_skills.py scientific-skills/my-skill`
5. 提交 PR，描述变更内容

---

## 十、FAQ

**Q: 技能是否需要联网？**
- 数据库查询类技能需要联网访问 API
- Python 包类技能在依赖安装完成后可离线运行

**Q: 为什么所有技能打包在一起而不是分开？**
- 科学研究本质上是跨学科的。打包在一起便于 Agent 在一个工作流中无缝桥接基因组学、化学、临床数据和机器学习，无需用户手动管理依赖关系。

**Q: 能否用于商业项目？**
- 本仓库采用 MIT 许可证，允许商业使用。但 **每个技能有自己的许可证**（在 `SKILL.md` 的 `license` 字段中），请分别确认。

**Q: Agent 能否使用没有专门技能的 Python 包？**
- **完全可以**。这 138 个技能是 "优化路径"（有详细文档和示例），Agent 本身具备使用任何 Python 包或 API 的能力。这些技能只是让常见工作流更快、更可靠。

---

## 十一、总结

Scientific Agent Skills 的本质是 **将分散的科学计算生态重新封装为 AI 可理解、可编排的模块化知识单元**。其架构设计遵循以下原则：

1. **开放性**：遵循 [Agent Skills](https://agentskills.io/) 开放标准，不与特定 AI 模型绑定
2. **模块化**：每个技能自包含，可独立使用也可组合编排
3. **实用性**：每个技能都包含可直接运行的代码示例，而非纯理论描述
4. **安全性**：自动化安全扫描 + LLM 深度审查 + 社区人工复核
5. **可扩展性**：通过 autoskill 和用户贡献持续自我进化

无论是单细胞的差异表达分析、蛋白质-配体对接、临床试验数据挖掘，还是跨学科的复杂多组学整合，这个项目都能将数天的工作量压缩到一次自然语言对话中完成。
