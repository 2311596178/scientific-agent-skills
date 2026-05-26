# Scientific Agent Skills — ML Workbench 设计文档

> 将科学技能库改造为面向机器学习的独立桌面软件，脱离大模型依赖，通过 GUI 界面直接操作。

---

## 1. 项目背景与目标

### 1.1 现状

当前 `scientific-agent-skills` 是一个**面向 AI Agent 的文档型技能库**，包含 139 个科学领域技能（Skill）。每个 Skill 以 Markdown 文档（`SKILL.md`）为核心，辅以参考文档（`references/`）和示例脚本（`scripts/`），供大模型（Claude、Cursor、Codex 等）读取后生成代码。

**核心问题：**
- 强依赖大模型：没有 AI 就无法使用，用户无法直接利用其中的算法和工具
- 无界面：纯文本仓库，需要用户手动阅读文档、复制代码、配置环境
- 碎片化：139 个技能各自独立，没有统一的数据流和工作流串联

### 1.2 改造目标

将项目中的**机器学习相关技能**提取并封装为一个独立的桌面软件 **"ML Workbench"**，实现：

| 目标 | 说明 |
|:---|:---|
| **脱离大模型** | 用户无需 AI 即可直接选择算法、配置参数、运行模型 |
| **图形界面** | 提供可视化操作界面，降低使用门槛 |
| **端到端工作流** | 数据加载 → 预处理 → 模型训练 → 评估 → 可视化，一站式完成 |
| **技能可扩展** | 保留原有的 Skill 插件化结构，便于后续添加新算法 |

---

## 2. 现状分析

### 2.1 机器学习技能清单

从 139 个技能中筛选出 18 个 ML 核心技能：

| Skill | 领域 | 核心能力 |
|:---|:---|:---|
| `scikit-learn` | 经典 ML | 分类、回归、聚类、降维、预处理、Pipeline |
| `aeon` | 时间序列 | 时序分类、预测、异常检测、转换 |
| `pytorch-lightning` | 深度学习框架 | 神经网络训练、分布式训练、回调系统 |
| `timesfm-forecasting` | 时序预测 | 基于 Transformer 的时间序列预测 |
| `torch-geometric` | 图神经网络 | 图卷积、图注意力、图分类/回归 |
| `transformers` (HuggingFace) | NLP/CV | 预训练模型微调、文本/图像分类 |
| `umap-learn` | 降维可视化 | 非线性降维、高维数据可视化 |
| `pymc` | 概率编程 | 贝叶斯推断、MCMC 采样 |
| `networkx` | 图分析 | 网络分析、图算法 |
| `statsmodels` | 统计模型 | 回归分析、时间序列统计模型 |

### 2.2 Skill 结构解析

每个 Skill 的标准结构：

```
skill-name/
├── SKILL.md              # YAML frontmatter + Markdown 文档
├── references/
│   ├── quick_reference.md
│   ├── supervised_learning.md
│   └── ...               # 算法文档、API 参考
└── scripts/              # 可运行示例脚本
    ├── classification_pipeline.py
    └── clustering_analysis.py
```

**YAML frontmatter 示例（scikit-learn）：**
```yaml
---
name: scikit-learn
description: Machine learning in Python with scikit-learn. Use when working with supervised learning (classification, regression), unsupervised learning (clustering, dimensionality reduction)...
license: BSD-3-Clause license
metadata:
    skill-author: K-Dense Inc.
---
```

**关键洞察：**
- `SKILL.md` 中的代码块可以直接提取作为模板
- `references/` 中的文档可以解析为参数说明和算法选择指南
- `scripts/` 中的脚本可以直接集成或改造为模块化组件

---

## 3. 总体架构

### 3.1 架构概览

采用**插件化桌面应用**架构，核心是一个轻量级宿主程序，各 ML Skill 以插件形式加载。

```
┌─────────────────────────────────────────┐
│            ML Workbench (GUI)           │
│  ┌─────────┐ ┌─────────┐ ┌──────────┐ │
│  │ 数据面板 │ │ 模型配置 │ │ 结果展示  │ │
│  └─────────┘ └─────────┘ └──────────┘ │
├─────────────────────────────────────────┤
│         Workflow Engine (工作流引擎)     │
│    数据加载 → 预处理 → 训练 → 评估       │
├─────────────────────────────────────────┤
│         Skill Plugin System (插件系统)   │
│  ┌─────────┐ ┌─────────┐ ┌──────────┐ │
│  │scikit-  │ │  aeon   │ │pytorch-  │ │
│  │ learn   │ │         │ │lightning │ │
│  └─────────┘ └─────────┘ └──────────┘ │
├─────────────────────────────────────────┤
│         Data & Model Manager            │
│    Dataset Registry │ Model Registry    │
├─────────────────────────────────────────┤
│              Python Runtime             │
│    (内置 conda/uv 环境，隔离依赖)        │
└─────────────────────────────────────────┘
```

### 3.2 核心设计原则

1. **Skill 即插件**：每个 ML Skill 保持原有目录结构，通过 `skill.json` 清单文件声明能力
2. **声明式工作流**：用户通过 GUI 拖拽/配置生成工作流 JSON，而非写代码
3. **代码可导出**：任何工作流都可以导出为纯 Python 脚本，脱离软件运行
4. **零配置启动**：内置 Python 环境，首次启动自动安装所需依赖

---

## 4. 核心模块设计

### 4.1 Skill Plugin System（技能插件系统）

每个 Skill 需要增加一个 `skill.json` 描述文件，说明其能力：

```json
{
  "id": "scikit-learn",
  "name": "Scikit-learn",
  "version": "1.5.0",
  "category": "machine-learning",
  "icon": "icons/sklearn.png",
  "entrypoint": "plugin.py",
  "capabilities": {
    "tasks": ["classification", "regression", "clustering", "dimensionality-reduction"],
    "input_types": ["tabular"],
    "output_types": ["model", "predictions", "metrics"]
  },
  "dependencies": ["scikit-learn>=1.5.0", "matplotlib", "seaborn"],
  "parameters": {
    "algorithm": {
      "type": "enum",
      "options": ["RandomForestClassifier", "SVC", "LogisticRegression", "GradientBoostingClassifier"],
      "default": "RandomForestClassifier"
    },
    "n_estimators": {
      "type": "int",
      "min": 1,
      "max": 1000,
      "default": 100,
      "condition": "algorithm in ['RandomForestClassifier', 'GradientBoostingClassifier']"
    },
    "test_size": {
      "type": "float",
      "min": 0.01,
      "max": 0.99,
      "default": 0.2
    }
  }
}
```

**自动提取机制：**
- 从现有 `SKILL.md` 的 YAML frontmatter 提取基础信息
- 从 Markdown 中的代码块和参数表格自动解析 `parameters` 定义
- 从 `scripts/` 目录提取可运行脚本作为工作流模板

### 4.2 Workflow Engine（工作流引擎）

工作流以**有向无环图（DAG）**表示，节点是操作，边是数据流。

```python
# 工作流定义示例（内部 JSON 表示）
{
  "workflow_id": "classification-wf-001",
  "nodes": [
    {
      "id": "load-data",
      "type": "data_loader",
      "config": {"format": "csv", "path": "data/iris.csv"}
    },
    {
      "id": "preprocess",
      "type": "sklearn_preprocessor",
      "config": {"scaler": "StandardScaler", "encoder": "OneHotEncoder"},
      "inputs": [{"from": "load-data", "port": "dataset"}]
    },
    {
      "id": "train",
      "type": "sklearn_trainer",
      "config": {"algorithm": "RandomForestClassifier", "n_estimators": 100},
      "inputs": [{"from": "preprocess", "port": "dataset"}]
    },
    {
      "id": "evaluate",
      "type": "sklearn_evaluator",
      "config": {"metrics": ["accuracy", "f1", "roc_auc"]},
      "inputs": [{"from": "train", "port": "model"}]
    }
  ]
}
```

**执行模式：**
- **即时模式**：单节点修改后自动重跑下游（类似 Jupyter 的 reactive 执行）
- **批处理模式**：完整运行整个 DAG，适合生产环境

### 4.3 Data Manager（数据管理器）

统一管理输入数据和中间结果：

| 功能 | 说明 |
|:---|:---|
| **数据集注册** | 加载 CSV、Excel、Parquet、图像文件夹等，自动推断数据类型 |
| **数据预览** | 表格视图 + 统计摘要 + 分布可视化 |
| **特征类型推断** | 自动识别数值型、类别型、文本型、时序型特征 |
| **版本控制** | 记录每次预处理后的数据快照，支持回滚 |

### 4.4 Model Manager（模型管理器）

| 功能 | 说明 |
|:---|:---|
| **模型注册** | 保存训练好的模型及超参数、训练日志 |
| **模型对比** | 并排比较多个模型的指标、学习曲线、特征重要性 |
| **导出部署** | 导出为 ONNX、Pickle、或生成部署脚本 |
| **实验追踪** | 自动记录每次实验的参数和结果（轻量级 MLflow） |

---

## 5. 技术选型

### 5.1 GUI 框架

| 候选方案 | 优点 | 缺点 | 结论 |
|:---|:---|:---|:---|
| **PySide6 (Qt)** | 成熟、性能高、组件丰富、原生感强 | 包体积大 (~100MB) | ✅ **首选** |
| Dear PyGui | 极轻量、GPU 渲染、代码简洁 | 组件少、生态弱、不够成熟 | ❌ 放弃 |
| Streamlit/Gradio | 开发快、适合数据应用 | 非桌面应用、灵活性差 | ❌ 放弃 |
| Tauri + Web 前端 | 包小、界面美观 | 需要会前端、Rust 依赖 | ⚠️ 备选 |

**最终选择：PySide6**
- 桌面应用体验最佳
- 与 Python 数据科学生态无缝集成（matplotlib、pyqtgraph 等）
- 可以内嵌 Web 视图（展示 Plotly、Bokeh 等交互式图表）

### 5.2 依赖管理

| 层级 | 工具 | 说明 |
|:---|:---|:---|
| 运行环境 | `uv` | 极快的 Python 包管理器 |
| 环境隔离 | 内置 venv | 每个 Skill 可拥有独立虚拟环境 |
| 大依赖 | 按需下载 | PyTorch、Transformers 等大模型库不打包，首次使用时提示安装 |

### 5.3 关键依赖库

```toml
[project]
dependencies = [
    "pyside6>=6.7",           # GUI 框架
    "pyqtgraph>=0.13",        # 高性能可视化
    "pandas>=2.0",            # 数据处理
    "polars>=0.20",           # 高性能 DataFrame（备选）
    "pyarrow>=15",            # 列式存储
    "pydantic>=2.0",          # 数据验证
    "networkx>=3.0",          # 工作流 DAG
    "sqlalchemy>=2.0",        # 本地元数据存储
    "cloudpickle>=3.0",       # 模型序列化
]
```

---

## 6. 用户界面设计

### 6.1 主界面布局

```
┌──────────────────────────────────────────────────────────────┐
│  [File] [Edit] [View] [Workflow] [Skills] [Help]             │  ← 菜单栏
├──────────┬──────────────────────────────┬────────────────────┤
│          │                              │                    │
│  Skill   │                              │   Properties /     │
│  Browser │     Canvas (Workflow Editor) │   Hyperparameters  │
│          │                              │                    │
│ ┌──────┐ │                              │ ┌────────────────┐ │
│ │sklearn│ │    ┌─────────┐   ┌───────┐  │ │ Algorithm      │ │
│ │ aeon │ │    │Load Data│──▶│Preproc│  │ │ [RandomForest ▼│ │
│ │torch │ │    └─────────┘   └───┬───┘  │ │                │ │
│ │ ...  │ │                      │      │ │ n_estimators   │ │
│ └──────┘ │                   ┌──▼──┐   │ │ [100    ]      │ │
│          │                   │Train│   │ │                │ │
│          │                   └──┬──┘   │ │ max_depth      │ │
│          │                   ┌──▼───┐  │ │ [None      ]   │ │
│          │                   │Eval  │  │ └────────────────┘ │
│          │                   └──────┘  │                    │
│          │                              │   [Run] [Export]   │
├──────────┴──────────────────────────────┴────────────────────┤
│  📊 Results: Metrics | Plots | Model | Logs | Export         │  ← 底部结果面板
└──────────────────────────────────────────────────────────────┘
```

### 6.2 主要界面组件

**Skill Browser（左侧）**
- 按领域分类的树形列表（分类/回归/聚类/时序/深度学习/图网络）
- 搜索框快速定位 Skill
- 拖拽 Skill 到画布创建节点

**Canvas（中间）**
- 节点式工作流编辑器（类似 Node-RED、KNIME）
- 节点间连线表示数据流
- 支持框选、复制、粘贴、撤销/重做
- 右键菜单：运行到此处、查看源码、导出代码

**Properties Panel（右侧）**
- 选中节点后显示可配置参数
- 根据 `skill.json` 自动生成表单（滑块、下拉框、复选框）
- 参数变更实时验证（如条件参数：选 RandomForest 才显示 n_estimators）

**Results Panel（底部）**
- **Metrics**：表格展示精度、召回率、F1 等指标
- **Plots**：混淆矩阵、ROC 曲线、学习曲线、特征重要性（matplotlib/pyqtgraph）
- **Model**：模型摘要、参数列表、序列化导出
- **Logs**：训练过程日志、错误信息
- **Code**：自动生成并展示对应的 Python 代码

### 6.3 典型操作流程

**场景：用随机森林对鸢尾花数据集进行分类**

1. 用户拖拽 **"Load Dataset"** 节点到画布，上传 `iris.csv`
2. 拖拽 **"Scikit-learn → Classification"** 节点，连接到数据集
3. 在右侧属性面板选择算法 `RandomForestClassifier`，设置 `n_estimators=100`
4. 点击 **"Run"**
5. 底部 Results Panel 自动展示：
   - 分类报告表格
   - 混淆矩阵热力图
   - 特征重要性柱状图
   - 自动生成的 Python 脚本
6. 用户点击 **"Export → Python Script"**，获得可独立运行的 `.py` 文件

---

## 7. 无大模型模式设计

### 7.1 核心策略

当前技能库的代码生成依赖大模型理解 `SKILL.md` 后写出代码。改造后：

| 原流程 | 新流程 |
|:---|:---|
| 用户描述需求 → 大模型读 SKILL.md → 生成代码 → 用户运行 | 用户拖拽节点 → 软件解析 skill.json → 执行预置模板 → 展示结果 |

**关键技术：模板代码系统**

每个 Skill 插件内置一套**参数化代码模板**，根据用户在 GUI 上的配置填充参数后执行：

```python
# sklearn_classification_template.py（插件内置模板）
from sklearn.{{algorithm_module}} import {{algorithm_class}}
from sklearn.metrics import {{metrics}}

# 自动注入用户配置的参数
model = {{algorithm_class}}(
{% for param, value in hyperparameters.items() %}
    {{param}}={{value}},
{% endfor %}
)

model.fit(X_train, y_train)
y_pred = model.predict(X_test)

# 自动计算并返回指标
results = {
{% for metric in metrics %}
    '{{metric}}': {{metric}}(y_test, y_pred),
{% endfor %}
}
```

模板使用 **Jinja2** 渲染，渲染后的代码通过 `subprocess` 或 `exec` 在隔离环境中执行。

### 7.2 Skill 文档复用

原有 `SKILL.md` 和 `references/` 被转化为软件的**帮助系统**：
- 用户在属性面板点击 **"?"** 图标，弹出该参数的说明（从 references 文档提取）
- 每个节点右键 → **"View Documentation"**，展示原 SKILL.md 的渲染内容

---

## 8. 数据流与工作流

### 8.1 数据类型系统

定义统一的数据类型，确保节点间兼容：

```python
class DataType(Enum):
    TABULAR = "tabular"           # pandas DataFrame
    ARRAY = "array"               # numpy ndarray
    MODEL = "model"               # sklearn estimator / torch.nn.Module
    PREDICTIONS = "predictions"   # Series / ndarray
    METRICS = "metrics"           # dict[str, float]
    PLOT = "plot"                 # matplotlib Figure / base64 PNG
    TIME_SERIES = "time_series"   # aeon 时序格式
    GRAPH = "graph"               # networkx Graph / PyG Data
    TEXT = "text"                 # str / list[str]
    IMAGE = "image"               # PIL Image / tensor
```

### 8.2 节点间数据传递

节点输出通过**内存共享 + 序列化备份**实现：
- 同进程内：直接传递 Python 对象引用
- 跨进程/恢复时：从 `~/.mlworkbench/cache/{workflow_id}/{node_id}.pkl` 加载

### 8.3 工作流执行器

```python
async def execute_node(node: Node, inputs: dict) -> dict:
    """执行单个节点"""
    skill = load_skill(node.skill_id)
    
    # 1. 渲染代码模板
    code = skill.render_template(node.template_id, node.config, inputs)
    
    # 2. 在隔离环境中执行
    result = await runtime.execute(code, timeout=300)
    
    # 3. 类型校验与转换
    outputs = skill.validate_outputs(result)
    
    # 4. 缓存结果
    cache.save(node.id, outputs)
    
    return outputs
```

---

## 9. 实施路线图

### Phase 1：基础框架（2-3 周）

- [ ] 搭建 PySide6 主窗口框架（菜单、布局、Dock Widget）
- [ ] 实现 Skill Plugin System（加载、解析 `skill.json`）
- [ ] 实现 Canvas 工作流编辑器（节点、连线、拖拽）
- [ ] 实现 `scikit-learn` 插件（分类/回归/聚类）作为 MVP

### Phase 2：核心能力（3-4 周）

- [ ] 实现 Data Manager（CSV/Excel 加载、预览、特征推断）
- [ ] 实现 Workflow Engine（DAG 执行、缓存、重跑）
- [ ] 实现 Results Panel（指标表格、基础图表）
- [ ] 添加 `aeon`、`statsmodels` 时序分析插件
- [ ] 代码导出功能（生成可独立运行的 Python 脚本）

### Phase 3：扩展与 polish（2-3 周）

- [ ] 添加 `pytorch-lightning`、`transformers` 深度学习插件
- [ ] 添加 AutoML 插件（如 `auto-sklearn`、`TPOT`）
- [ ] 实现 Model Manager（模型保存、对比、导出 ONNX）
- [ ] 实验追踪系统（本地 SQLite 存储）
- [ ] 打包发布（PyInstaller → Windows/macOS/Linux 安装包）

### Phase 4：生态建设（持续）

- [ ] 自动化工具：从现有 `SKILL.md` 批量生成 `skill.json`
- [ ] 社区插件市场（允许用户分享自定义 Skill）
- [ ] 与原始仓库的同步机制（upstream 更新后自动提取新 Skill）

---

## 10. 风险与应对

| 风险 | 影响 | 应对策略 |
|:---|:---|:---|
| PyTorch/Transformers 包体积过大 | 安装包 > 1GB | 核心应用不打包大依赖，首次使用时在线安装 |
| 139 个技能全部 GUI 化工作量巨大 | 无法按期完成 | 优先 ML 技能（18 个），其余按需逐步添加 |
| 复杂深度学习工作流难以用节点表达 | 用户体验差 | 保留"代码模式"，允许在节点中直接写 Python |
| 跨平台兼容性（macOS/Windows/Linux） | 维护成本高 | 使用 PySide6（跨平台），CI 构建三个平台安装包 |

---

## 附录：文件结构预览

```
ml-workbench/
├── src/
│   ├── app/                    # 主应用框架
│   │   ├── main_window.py
│   │   ├── menu_bar.py
│   │   └── status_bar.py
│   ├── canvas/                 # 工作流画布
│   │   ├── canvas_widget.py
│   │   ├── node_item.py
│   │   ├── connection_item.py
│   │   └── canvas_controller.py
│   ├── panels/                 # 侧边/底部面板
│   │   ├── skill_browser.py
│   │   ├── properties_panel.py
│   │   ├── data_preview.py
│   │   └── results_panel.py
│   ├── engine/                 # 工作流引擎
│   │   ├── workflow.py
│   │   ├── executor.py
│   │   ├── data_types.py
│   │   └── cache_manager.py
│   ├── skills/                 # 内置 Skill 插件
│   │   ├── sklearn/
│   │   │   ├── skill.json
│   │   │   ├── plugin.py
│   │   │   └── templates/
│   │   ├── aeon/
│   │   └── ...
│   ├── runtime/                # Python 代码执行环境
│   │   ├── isolated_env.py
│   │   └── code_runner.py
│   └── utils/
│       └── skill_parser.py     # 从 SKILL.md 自动生成 skill.json
├── assets/
│   ├── icons/
│   └── styles/
├── docs/
├── tests/
└── pyproject.toml
```

---

> 本文档作为软件化改造的总体设计依据。具体实现时，可根据开发进度和资源情况对 Phase 内容进行调整。
