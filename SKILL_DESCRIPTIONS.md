# ML Workbench — 技能描述文档

> 基于 `scientific-skills/` 目录中 16 个机器学习相关技能的详细梳理，作为软件开发的参考依据。

---

## 目录

1. [scikit-learn](#1-scikit-learn) — 经典机器学习
2. [aeon](#2-aeon) — 时间序列分析
3. [pytorch-lightning](#3-pytorch-lightning) — 深度学习框架
4. [timesfm-forecasting](#4-timesfm-forecasting) — 零样本时序预测
5. [torch-geometric](#5-torch-geometric) — 图神经网络
6. [transformers](#6-transformers) — 预训练模型
7. [umap-learn](#7-umap-learn) — 非线性降维
8. [pymc](#8-pymc) — 概率编程
9. [networkx](#9-networkx) — 图分析
10. [statsmodels](#10-statsmodels) — 统计建模
11. [shap](#11-shap) — 模型可解释性
12. [exploratory-data-analysis](#12-exploratory-data-analysis) — 数据探索
13. [statistical-analysis](#13-statistical-analysis) — 统计分析
14. [matplotlib](#14-matplotlib) — 基础可视化
15. [seaborn](#15-seaborn) — 统计可视化
16. [dask](#16-dask) — 分布式计算

---

## 1. scikit-learn

| 属性 | 内容 |
|:---|:---|
| **名称** | scikit-learn |
| **领域** | 经典机器学习 |
| **定位** | 工业标准的 Python 经典 ML 库 |
| **核心能力** | 分类、回归、聚类、降维、预处理、Pipeline、模型评估、超参数调优 |
| **关键算法** | 随机森林、梯度提升、SVM、KNN、K-Means、DBSCAN、PCA、Logistic Regression、Ridge/Lasso |
| **关键组件** | `Pipeline`, `ColumnTransformer`, `GridSearchCV`, `StandardScaler`, `OneHotEncoder`, `train_test_split` |
| **安装** | `pip install scikit-learn` |
| **适用场景** | 结构化数据的分类/回归/聚类；生产级 ML Pipeline；算法对比与选型 |
| **成熟度** | ⭐⭐⭐⭐⭐ 极其成熟，生态最完善 |
| **GUI 化难度** | ⭐⭐☆☆☆ 低。API 统一（fit/predict），参数文档完善，最优先 GUI 化 |

**为什么优先：**
- API 高度统一：所有模型都是 `fit()` → `predict()` → `score()`
- Pipeline 机制成熟：预处理 + 模型可以无缝串联
- 文档和社区最完善，用户学习成本低
- 不依赖 GPU，普通电脑就能跑

---

## 2. aeon

| 属性 | 内容 |
|:---|:---|
| **名称** | aeon |
| **领域** | 时间序列分析 |
| **定位** | 兼容 sklearn 接口的时间序列 ML 工具包 |
| **核心能力** | 时序分类、时序回归、时序聚类、预测、异常检测、分割、相似性搜索 |
| **关键算法** | Rocket/MiniRocket、HIVECOTEV2、InceptionTime、ShapeletTransform、TimeSeriesKMeans(DTW)、ARIMA |
| **关键组件** | `RocketClassifier`, `TimeSeriesKMeans`, `ARIMA` |
| **安装** | `pip install aeon` |
| **适用场景** | 传感器数据分析、金融时序预测、设备异常检测、语音/动作识别 |
| **成熟度** | ⭐⭐⭐⭐☆ 较成熟，sklearn 兼容 |
| **GUI 化难度** | ⭐⭐⭐☆☆ 中。数据格式（时间序列）需要特殊处理，但接口与 sklearn 类似 |

**注意：**
- 时序数据的输入格式与表格数据不同（需要保留时间索引或序列结构）
- 部分算法（如 Rocket）是专门针对时序特征设计的，需要向用户解释适用场景

---

## 3. pytorch-lightning

| 属性 | 内容 |
|:---|:---|
| **名称** | pytorch-lightning |
| **领域** | 深度学习框架 |
| **定位** | PyTorch 的高级封装，消除样板代码，自动化训练流程 |
| **核心能力** | 神经网络训练自动化、多 GPU/TPU 编排、回调系统、实验日志、分布式训练 |
| **关键组件** | `LightningModule`, `Trainer`, `LightningDataModule`, `ModelCheckpoint`, `EarlyStopping` |
| **安装** | `pip install pytorch-lightning` |
| **适用场景** | 自定义神经网络、图像/文本/音频深度学习、需要多卡训练的大规模模型 |
| **成熟度** | ⭐⭐⭐⭐⭐ 非常成熟，被广泛使用 |
| **GUI 化难度** | ⭐⭐⭐⭐☆ 较高。需要用户定义网络结构（`LightningModule`），不完全适合纯 GUI 操作 |

**注意：**
- 用户需要自定义网络架构，这很难完全 GUI 化
- GUI 层面更适合做"预训练模型微调"（如用 Lightning 封装好的图像分类模板）
- 对于完全自定义网络，建议保留"代码模式"让用户手写 `LightningModule`

---

## 4. timesfm-forecasting

| 属性 | 内容 |
|:---|:---|
| **名称** | timesfm-forecasting |
| **领域** | 零样本时序预测 |
| **定位** | Google 的 TimesFM 基础模型，无需训练即可预测任意单变量时序 |
| **核心能力** | 零样本预测、概率预测（分位数区间）、批量预测、支持 1-16384 个上下文点 |
| **关键组件** | TimesFM 2.5（200M 参数，decoder-only 架构） |
| **安装** | `pip install timesfm[torch]` |
| **适用场景** | 销售预测、传感器预测、能源负荷预测、任何单变量时序的快速预测 |
| **成熟度** | ⭐⭐⭐☆☆ 较新，基于预训练大模型 |
| **GUI 化难度** | ⭐⭐☆☆☆ 低。零样本特性意味着用户只需要提供数据，无需调参 |

**注意：**
- 模型文件较大（~800MB），首次使用需下载
- 纯零样本，不需要训练界面，只需要数据输入 + 预测输出
- 可与 aeon 的时序功能形成互补：aeon 做有监督训练，timesfm 做零样本快速预测

---

## 5. torch-geometric

| 属性 | 内容 |
|:---|:---|
| **名称** | torch-geometric (PyG) |
| **领域** | 图神经网络 |
| **定位** | PyTorch 上的图神经网络标准库 |
| **核心能力** | 图数据结构、60+ GNN 层、可扩展小批量训练、异构图支持、节点/边/图级任务 |
| **关键算法** | GCN、GAT、GraphSAGE、GIN、消息传递网络 |
| **关键组件** | `Data`, `HeteroData`, `GCNConv`, `NeighborLoader` |
| **安装** | `pip install torch-geometric` |
| **适用场景** | 社交网络分析、分子图预测、推荐系统、知识图谱 |
| **成熟度** | ⭐⭐⭐⭐☆ 成熟，学术和工业都在用 |
| **GUI 化难度** | ⭐⭐⭐⭐☆ 较高。图数据的输入格式特殊（边列表 + 节点特征），需要专门的图数据导入界面 |

**注意：**
- 图数据通常以 edge list 或邻接矩阵形式存在，与表格数据完全不同
- 用户群体相对专业，建议 Phase 3 再做 GUI 化

---

## 6. transformers

| 属性 | 内容 |
|:---|:---|
| **名称** | transformers (Hugging Face) |
| **领域** | NLP / CV / 多模态 |
| **定位** | 预训练模型生态，提供数千个即用模型 |
| **核心能力** | 文本生成、分类、问答、翻译、摘要、图像分类、语音识别、模型微调 |
| **关键组件** | `pipeline`, `AutoModel`, `AutoTokenizer`, `Trainer` |
| **安装** | `pip install transformers` |
| **适用场景** | 文本分类、情感分析、命名实体识别、图像分类、语音识别、大模型微调 |
| **成熟度** | ⭐⭐⭐⭐⭐ 极其成熟，生态最大 |
| **GUI 化难度** | ⭐⭐⭐⭐☆ 较高。模型文件巨大（GB 级），GPU 依赖强，微调需要理解较多概念 |

**注意：**
- 模型下载和显存需求是主要门槛
- GUI 层面更适合封装为"选择任务 → 选择预训练模型 → 微调/推理"的简单流程
- 完整微调界面复杂，建议 Phase 3 再做

---

## 7. umap-learn

| 属性 | 内容 |
|:---|:---|
| **名称** | umap-learn |
| **领域** | 非线性降维 |
| **定位** | t-SNE 的更快替代，保留局部和全局结构 |
| **核心能力** | 非线性降维、2D/3D 可视化、聚类预处理、监督/参数化 UMAP |
| **关键参数** | `n_neighbors`（局部/全局平衡）、`min_dist`（点间距）、`metric`（距离度量） |
| **安装** | `pip install umap-learn` |
| **适用场景** | 高维数据可视化、聚类前降维、单细胞数据可视化、嵌入学习 |
| **成熟度** | ⭐⭐⭐⭐☆ 成熟，被广泛使用 |
| **GUI 化难度** | ⭐⭐☆☆☆ 低。API 与 sklearn 兼容（`fit_transform`），参数少且直观 |

**注意：**
- 必须配合标准化使用（`StandardScaler`），GUI 中应自动提示
- 主要输出是 2D/3D 坐标，需要专门的散点图可视化界面
- 可与 HDBSCAN 聚类联动（UMAP 降维 → HDBSCAN 聚类是经典组合）

---

## 8. pymc

| 属性 | 内容 |
|:---|:---|
| **名称** | pymc |
| **领域** | 概率编程 / 贝叶斯统计 |
| **定位** | Python 贝叶斯建模和概率编程库 |
| **核心能力** | 贝叶斯模型构建、MCMC 采样（NUTS）、变分推断、先验/后验预测检验、模型比较 |
| **关键组件** | `pm.Model`, `pm.sample` (NUTS), `pm.sample_prior_predictive`, ArviZ 诊断 |
| **安装** | `pip install pymc` |
| **适用场景** | 不确定性量化、层次模型、小样本推断、缺失数据处理、测量误差建模 |
| **成熟度** | ⭐⭐⭐⭐☆ 成熟，学术界常用 |
| **GUI 化难度** | ⭐⭐⭐⭐⭐ 极高。贝叶斯建模需要用户理解先验、似然、后验等概念，难以 GUI 化 |

**注意：**
- 贝叶斯统计门槛高，用户群体小众
- 建议 Phase 3 甚至不做 GUI 化，仅作为知识库参考
- 如果要做，只能封装为模板（如"贝叶斯线性回归"一键运行）

---

## 9. networkx

| 属性 | 内容 |
|:---|:---|
| **名称** | networkx |
| **领域** | 图分析 |
| **定位** | Python 图论和网络分析库 |
| **核心能力** | 图创建与操作、最短路径、中心性分析、社区检测、网络生成、图可视化 |
| **关键算法** | PageRank、Dijkstra、社区检测（贪心模块度）、介数中心性、最小生成树 |
| **关键组件** | `Graph`, `DiGraph`, `shortest_path`, `pagerank`, `greedy_modularity_communities` |
| **安装** | `pip install networkx` |
| **适用场景** | 社交网络分析、交通网络优化、引用网络、知识图谱分析 |
| **成熟度** | ⭐⭐⭐⭐⭐ 极其成熟 |
| **GUI 化难度** | ⭐⭐⭐☆☆ 中等。图的可视化和交互需要专门设计，但算法调用简单 |

**注意：**
- 需要专门的图数据导入界面（边列表、邻接矩阵）
- 图可视化用 matplotlib 效果有限，建议结合 `pyvis` 或 `cytoscape` 做交互式网络图
- 用户群体偏专业，建议 Phase 2/3 再做

---

## 10. statsmodels

| 属性 | 内容 |
|:---|:---|
| **名称** | statsmodels |
| **领域** | 统计建模 |
| **定位** | Python 统计建模首选库，擅长推断和诊断 |
| **核心能力** | 回归分析（OLS/GLS/WLS）、广义线性模型、时间序列（ARIMA/SARIMAX/VAR）、统计检验、模型诊断 |
| **关键算法** | OLS、Logit/Probit、ARIMA、SARIMAX、VAR、分位数回归 |
| **关键组件** | `OLS`, `GLM`, `ARIMA`, `summary()`（出版级表格） |
| **安装** | `pip install statsmodels` |
| **适用场景** | 计量经济学、时间序列预测、统计推断、假设检验、出版级回归表格 |
| **成熟度** | ⭐⭐⭐⭐⭐ 极其成熟 |
| **GUI 化难度** | ⭐⭐⭐☆☆ 中等。回归模型参数多但结构化，时间序列模型（ARIMA 阶数选择）需要专门引导 |

**注意：**
- `summary()` 输出的表格非常详细，是出版级的，GUI 中应保留这种详细输出
- ARIMA 的 `(p,d,q)` 参数选择对用户不友好，GUI 中可以提供自动定阶（AIC/BIC）
- 与 scikit-learn 的回归形成互补：statsmodels 重推断（p值、置信区间），sklearn 重预测

---

## 11. shap

| 属性 | 内容 |
|:---|:---|
| **名称** | shap |
| **领域** | 模型可解释性 |
| **定位** | 基于博弈论的统一模型解释框架 |
| **核心能力** | SHAP 值计算、全局/局部特征重要性、模型调试、公平性分析、多种可视化 |
| **关键算法** | TreeExplainer、DeepExplainer、KernelExplainer、LinearExplainer |
| **关键可视化** | beeswarm、waterfall、bar、force、heatmap、scatter |
| **安装** | `pip install shap` |
| **适用场景** | 解释模型预测原因、特征重要性分析、模型调试、合规审计（金融/医疗） |
| **成熟度** | ⭐⭐⭐⭐⭐ 极其成熟，行业标准 |
| **GUI 化难度** | ⭐⭐⭐☆☆ 中等。计算 SHAP 值简单，但可视化种类多，需要设计专门的图表面板 |

**注意：**
- 必须依附于已训练的模型，不能独立使用
- 不同模型类型需要选择不同的 Explainer（TreeExplainer 用于树模型，KernelExplainer 用于黑盒模型）
- GUI 中应自动根据模型类型选择最佳 Explainer
- 计算可能较慢（尤其是 KernelExplainer），需要进度指示

---

## 12. exploratory-data-analysis

| 属性 | 内容 |
|:---|:---|
| **名称** | exploratory-data-analysis |
| **领域** | 数据探索 |
| **定位** | 科学数据的自动探索和报告生成 |
| **核心能力** | 自动文件类型检测、格式特定元数据提取、数据质量评估、统计摘要、可视化推荐、Markdown 报告生成 |
| **关键组件** | 文件类型检测器、领域特定分析器、报告生成器 |
| **安装** | 依赖文件类型（`pandas`, `numpy`, `matplotlib`, 可选 `biopython`, `h5py` 等） |
| **适用场景** | 拿到未知数据时的快速探索、数据质量检查、生成数据文档 |
| **成熟度** | ⭐⭐⭐☆☆ 中等。脚本质量一般，主要是示例性质 |
| **GUI 化难度** | ⭐⭐☆☆☆ 低。核心逻辑（读取文件 → 计算统计量 → 生成图表）容易 GUI 化 |

**注意：**
- 现有脚本包含大量生物/化学/医学专用格式检测，这些可以去掉
- 保留通用数据格式的分析能力：CSV、Excel、Parquet、JSON、HDF5、NumPy
- 核心价值是"自动报告"，GUI 中应做成"一键生成 EDA 报告"按钮

---

## 13. statistical-analysis

| 属性 | 内容 |
|:---|:---|
| **名称** | statistical-analysis |
| **领域** | 统计分析 |
| **定位** | 统计检验选择、假设检验、APA 格式报告 |
| **核心能力** | 检验选择指导、假设检验、效应量计算、统计功效分析、APA 格式报告 |
| **关键算法** | t 检验、ANOVA、卡方检验、Mann-Whitney U、Wilcoxon、Kruskal-Wallis、Pearson/Spearman 相关、线性/逻辑回归 |
| **安装** | `pip install scipy statsmodels` |
| **适用场景** | 学术研究统计检验、A/B 测试分析、问卷数据分析、出版级统计报告 |
| **成熟度** | ⭐⭐⭐⭐☆ 成熟（依赖 scipy/statsmodels） |
| **GUI 化难度** | ⭐⭐⭐☆☆ 中等。检验选择需要引导用户（根据数据类型和问题类型推荐检验） |

**注意：**
- "检验选择"是核心难点：用户不知道用 t 检验还是 Mann-Whitney U
- GUI 中应设计引导流程："你的数据是正态分布吗？→ 是 → 用 t 检验"
- 与 exploratory-data-analysis 可以合并为"数据分析"模块

---

## 14. matplotlib

| 属性 | 内容 |
|:---|:---|
| **名称** | matplotlib |
| **领域** | 基础可视化 |
| **定位** | Python 可视化的基石 |
| **核心能力** | 线图、散点图、柱状图、直方图、热力图、等高线图、3D 图、动画、完全自定义样式 |
| **关键组件** | `Figure`, `Axes`, `pyplot`, `savefig`, `GridSpec` |
| **安装** | `pip install matplotlib` |
| **适用场景** | 任何需要自定义图表的场景、出版级图表、多子图排版 |
| **成熟度** | ⭐⭐⭐⭐⭐ 极其成熟 |
| **GUI 化难度** | ⭐⭐☆☆☆ 低。作为底层库，GUI 中通过封装提供常用图表模板 |

**注意：**
- matplotlib 是底层库，用户通常不直接使用，而是通过 GUI 间接调用
- GUI 中的图表生成应使用 matplotlib 作为后端，但提供更高层的交互界面
- 图表导出（PNG/PDF/SVG）是必备功能

---

## 15. seaborn

| 属性 | 内容 |
|:---|:---|
| **名称** | seaborn |
| **领域** | 统计可视化 |
| **定位** | 基于 matplotlib 的高级统计图表库 |
| **核心能力** | 面向 DataFrame 的统计图表、关系图、分类图、分布图、回归图、多面板分面、内置统计估计 |
| **关键组件** | `scatterplot`, `lineplot`, `boxplot`, `violinplot`, `pairplot`, `heatmap`, `lmplot` |
| **安装** | `pip install seaborn` |
| **适用场景** | 快速探索性可视化、统计图表（带置信区间）、多变量分析、热力图、箱线图 |
| **成熟度** | ⭐⭐⭐⭐⭐ 极其成熟 |
| **GUI 化难度** | ⭐⭐☆☆☆ 低。与 pandas 集成好，GUI 中通过选择列名即可生成图表 |

**注意：**
- 与 matplotlib 配合使用：seaborn 负责高级统计图表，matplotlib 负责精细调整
- GUI 中应提供"选择 X 轴、Y 轴、颜色分组 → 生成图表"的简单流程
- `pairplot` 和 `heatmap` 是 EDA 中最常用的图表

---

## 16. dask

| 属性 | 内容 |
|:---|:---|
| **名称** | dask |
| **领域** | 分布式计算 |
| **定位** | 超出内存/多核并行计算的扩展方案 |
| **核心能力** | 超出内存的 DataFrame/Array 计算、多核并行、多机分布式、任务调度 |
| **关键组件** | Dask DataFrame、Dask Array、Dask Delayed、`compute()` |
| **安装** | `pip install dask` |
| **适用场景** | 大数据集（> 内存）、批量文件处理、并行特征工程、分布式 ML |
| **成熟度** | ⭐⭐⭐⭐☆ 成熟 |
| **GUI 化难度** | ⭐⭐⭐⭐☆ 较高。对用户透明难度大，主要是作为"大数据模式"的后端切换 |

**注意：**
- 不适合直接 GUI 化，更适合作为**后端优化**
- 设计思路：当用户数据 > 可用内存时，自动提示"是否启用 Dask 模式？"
- GUI 层面的操作不变，底层默默切换到 Dask DataFrame
- 这是一个"锦上添花"的功能，不是核心功能

---

## 附录：GUI 化优先级总表

| 优先级 | 技能 | 理由 |
|:---|:---|:---|
| **P0 (Phase 1)** | scikit-learn | API 统一、最常用、最成熟、最易 GUI 化 |
| **P0 (Phase 1)** | matplotlib + seaborn | 作为图表后端，用户无感知 |
| **P1 (Phase 2)** | umap-learn | API 简单，降维可视化是高频需求 |
| **P1 (Phase 2)** | shap | 模型解释是刚需，依附于 sklearn 模型 |
| **P1 (Phase 2)** | aeon | 时序分析需求大，接口与 sklearn 兼容 |
| **P1 (Phase 2)** | statsmodels | 回归推断是 sklearn 的补充，时间序列是独立需求 |
| **P2 (Phase 3)** | exploratory-data-analysis + statistical-analysis | 可合并为"数据分析"模块，作为辅助功能 |
| **P2 (Phase 3)** | timesfm-forecasting | 零样本特性适合 GUI，但模型体积大 |
| **P3 (可选)** | pytorch-lightning + transformers | 深度学习门槛高，需要专门设计简化流程 |
| **P3 (可选)** | torch-geometric + networkx | 图数据输入特殊，用户群体专业 |
| **P3 (可选)** | pymc | 贝叶斯门槛太高，太小众 |
| **P3 (可选)** | dask | 作为底层优化，用户不直接感知 |
