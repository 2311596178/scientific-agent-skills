# 科学技能总览

> 本文档列出 Scientific Agent Skills 仓库中所有 138 个技能的完整描述。每个技能均包含其功能、适用场景和核心特性的详细说明。

---

## 科学数据库与数据访问

- **Database Lookup（数据库统一查询）** — 通过 REST API 搜索 78 个公共科学、生物医学、材料科学和经济数据库，返回结构化 JSON 结果。涵盖物理学/天文学（NASA、NIST、SDSS、SIMBAD、系外行星档案）、地球/环境科学（USGS、NOAA、EPA、OpenWeatherMap）、化学/药物（PubChem、ChEMBL、DrugBank、FDA、KEGG、DailyMed、ZINC、BindingDB）、材料科学（Materials Project、COD）、生物学/基因组学（Reactome、BRENDA、UniProt、STRING、Ensembl、NCBI Gene、GEO、GTEx、PDB、AlphaFold、InterPro、ChEBI、BioGRID、Gene Ontology、QuickGO、dbSNP、SRA、ENA、gnomAD、UCSC Genome、ENCODE、JASPAR、MouseMine、PRIDE、LINCS L1000、Human Protein Atlas、Human Cell Atlas、RummaGEO、Metabolomics Workbench、EMDB、Addgene）、疾病/临床（COSMIC、Open Targets、ClinPGx、ClinicalTrials.gov、OMIM、ClinVar、GDC/TCGA、cBioPortal、DisGeNET、GWAS Catalog、Monarch、HPO）、监管（FDA、USPTO、SEC EDGAR）、经济/金融（FRED、BEA、BLS、Federal Reserve、World Bank、ECB、US Treasury、Alpha Vantage、Data Commons）以及人口统计（US Census、Eurostat、WHO）。当用户需要查询化合物、药物、蛋白质、基因、通路、酶、基因表达、变异、临床试验、专利、SEC 文件、经济指标、晶体结构、天文对象、地震、天气或任何公共数据库 API 数据时使用此技能。

- **DepMap（癌症依赖图谱）** — 查询癌症依赖图谱（DepMap）获取癌细胞系基因依赖评分（CRISPR Chronos）、药物敏感性数据和基因效应谱。用于识别癌症特异性脆弱性、合成致死相互作用和验证肿瘤药物靶点。

- **Imaging Data Commons（NCI 影像数据共享）** — 使用 idc-index 查询和下载 NCI Imaging Data Commons 的公共癌症影像数据。用于访问大规模放射学（CT、MR、PET）和病理学数据集进行 AI 训练或研究。无需认证，支持按元数据查询、浏览器可视化和许可检查。

- **PrimeKG（精准医学知识图谱）** — 查询精准医学知识图谱（PrimeKG）获取多尺度生物医学数据，包括基因、药物、疾病、表型等。整合 20+ 生物医学资源到单一知识图谱中，用于药物重定位、疾病机制探索和靶点识别。

- **U.S. Treasury Fiscal Data（美国财政部财政数据）** — 美国财政部提供的免费开放 REST API，包含 54 个数据集和 182 个数据表。无需 API 密钥即可访问国债数据（1993 年至今的每日国债、1790 年至今的历史国债）、每日财政部报表（TGA 余额、存取款）、月度财政部报表（联邦预算收入和支出）、国债拍卖数据（1979 年以来的国库券、国库票据、国债、TIPS、FRN）、国债平均利率、财政部报告汇率（170+ 货币季度数据）、I Bond 和储蓄债券利率、TIPS/CPI 数据等。支持过滤、排序、分页和 CSV/XML/JSON 输出格式。

- **Hugging Science（Hugging Face 科学资源目录）** — Hugging Face 上科学数据集、模型、博客文章和交互式 Spaces 的精选 LLM 友好型目录，覆盖 17 个科学领域。通过标准 Hugging Face API 使用，包含 `fetch_catalog.py` 脚本支持按主题、类型或自由文本搜索过滤。

---

## 科学平台与集成

### 实验室信息管理系统（LIMS）与研发平台

- **Benchling Integration（Benchling 集成）** — 与 Benchling 研发平台集成，通过 Python SDK 和 REST API 程序化访问实验室数据管理，包括注册实体（DNA 序列、蛋白质）、库存系统（样品、容器、位置）、电子实验笔记本（条目、实验方案）、工作流（任务、自动化）和数据导出。

### 基因组学与生物医学数据云平台

- **DNAnexus Integration（DNAnexus 集成）** — DNAnexus 云平台综合工具包，用于基因组学和生物医学数据分析。涵盖构建和部署应用/小应用（Python/Bash）、管理数据对象（文件、记录、数据库）、运行分析和工作流、使用 dxpy Python SDK，以及配置应用元数据和依赖。

### 实验室自动化

- **Opentrons Integration（Opentrons 集成）** — 创建、编辑和调试 Opentrons Python Protocol API v2 协议，使用 Flex 和 OT-2 机器人实现自动化液体处理、移液工作流、硬件模块控制（热循环仪、温控、磁力、振荡加热、吸光度读板器）和实验器具管理。

- **Ginkgo Cloud Lab（Ginkgo 云实验室）** — 在 Ginkgo Bioworks 云实验室提交和管理协议，在可重构自动化推车（RAC）上自主执行实验。支持无细胞蛋白表达验证、优化和荧光像素艺术生成三种标准协议。

### 电子实验笔记本（ELN）

- **LabArchives Integration（LabArchives 集成）** — 与 LabArchives 电子实验笔记本 REST API 交互，程序化管理笔记本（备份、检索、管理）、条目（创建、评论、附件）和用户认证。支持多区域 API 端点（美国、英国、澳大利亚）和 OAuth 认证。

- **Open Notebook（开放式研究笔记本）** — 自托管、开源的 Google NotebookLM 替代方案，用于 AI 驱动的研究和文档分析。支持摄取多种内容源（PDF、视频、音频、网页、Office 文档），生成 AI 笔记和摘要，创建多人播客，实现文档聊天和跨材料搜索。

### 工作流平台与云执行

- **LatchBio Integration（LatchBio 集成）** — Latch 平台集成，用于构建、部署和执行生物信息学工作流。支持使用 Python 装饰器创建无服务器生物信息学流程、部署 Nextflow/Snakemake 流程、管理云数据和配置计算资源（CPU、GPU、内存、存储）。

### 显微镜与生物图像数据

- **OMERO Integration（OMERO 集成）** — 使用 Python 与 OMERO 显微镜数据管理系统交互，全面访问显微镜图像、数据集和筛选数据检索、像素数据分析、注释和元数据管理、ROI 创建和分析以及批量处理。

### 协议管理与共享

- **Protocols.io Integration（Protocols.io 集成）** — 通过 API 管理科学协议，支持协议发现（按关键词、DOI、类别搜索）、生命周期管理（创建、更新、DOI 发布）、分步骤程序文档、协作开发和文件管理。

---

## 科学软件包

### 生物信息学与基因组学

- **AnnData** — 用于处理注释数据矩阵的 Python 包，专为单细胞基因组学数据设计。基于 HDF5 的 h5ad 文件格式实现高效 I/O 和压缩，与 pandas DataFrames 集成处理元数据，支持稀疏矩阵以节省内存，与 Scanpy、scvi-tools 等工具无缝集成。

- **Arboreto** — 使用集成树方法从单细胞 RNA-seq 数据高效推断基因调控网络（GRN）。实现 GRNBoost2 和 GENIE3 算法，针对大规模单细胞数据集优化，支持并行处理和与 Scanpy/AnnData 工作流集成。

- **BioPython** — 计算生物学和生物信息学的综合 Python 库，提供序列操作、数据库访问和生物数据分析工具。包括序列对象、文件格式解析器（FASTA、FASTQ、GenBank、SAM/BAM、VCF、GFF）、NCBI 数据库访问、BLAST 集成、序列比对、系统发育学和蛋白质结构分析。

- **BioServices** — 统一编程访问 40+ 生物网络服务和数据库的 Python 库。支持 KEGG、UniProt、ChEBI、ChEMBL、Reactome、IntAct 等主要生物信息学资源，提供跨服务的一致 API、自动缓存和错误处理。

- **Cellxgene Census** — 查询和分析 CZ CELLxGENE Discover 普查中大规模单细胞 RNA-seq 数据的 Python 包。提供对 5000 万+ 细胞和 1000+ 数据集的访问，使用 TileDB-SOMA 格式实现可扩展查询。

- **deepTools** — 探索和可视化高通量测序（NGS）数据的综合 Python 工具套件，特别针对 ChIP-seq、RNA-seq 和 ATAC-seq 实验。提供命令行工具和 Python API 处理 BAM 和 bigWig 文件。

- **FlowIO** — 读取和操作流式细胞术标准（FCS）文件的 Python 库。支持 FCS 文件（版本 2.0、3.0、3.1）的高效解析、事件数据访问、元数据提取和转换为 pandas DataFrames。

- **gget** — 基因组数据库快速查询的命令行工具和 Python 包。提供对 Ensembl、UniProt、NCBI、PDB、COSMIC 等数据库的快速访问，支持单命令查询、批处理和 pandas DataFrame 集成。

- **geniml** — 基因组区间机器学习工具包，提供在 BED 文件上构建 ML 模型的无监督方法。关键能力包括 Region2Vec、BEDspace、scEmbed、consensus peak 构建和综合工具。

- **gtars** — 高性能 Rust 基因组区间分析工具包，提供 Python 绑定。包括 IGD 索引重叠检测、覆盖度轨迹生成、基因组标记化、参考序列管理和单细胞基因组学片段处理。

- **Polars-Bio** — 在 Polars DataFrames 上进行高性能基因组区间操作和生物信息学文件 I/O。提供 BED/VCF/BAM/GFF 区间的重叠、最近邻、合并、覆盖度、补集和减法操作。

- **pysam** — 读取、写入和操作基因组数据文件（SAM/BAM/CRAM 比对、VCF/BCF 变异、FASTA/FASTQ 序列），支持 pileup 分析、覆盖度计算和生物信息学工作流。

- **PyDESeq2** — DESeq2 差异基因表达分析方法的 Python 实现，用于 bulk RNA-seq 数据。使用负二项广义线性模型确定实验条件间的差异表达，处理复杂实验设计、批次效应和重复。

- **Scanpy** — 基于 AnnData 的单细胞 RNA-seq 数据分析综合 Python 工具包。提供端到端工作流：预处理、降维（PCA、UMAP、t-SNE）、聚类（Leiden、Louvain）、marker 基因鉴定、轨迹推断和可视化。

- **scVelo** — RNA 速度分析，从未剪接/剪接 mRNA 动态估计细胞状态转变。推断轨迹方向、计算潜在时间并识别驱动基因。

- **scvi-tools** — 单细胞组学分析的概率深度学习模型。基于 PyTorch 的框架提供 25+ 模型：scVI/scANVI（RNA-seq 整合）、totalVI（CITE-seq）、MultiVI（多组学）、PeakVI（ATAC-seq）、空间转录组学反卷积等。

- **scikit-bio** — 生物信息学 Python 库，提供生物序列分析的数据结构、算法和解析器。包括序列对象、序列比对算法、系统发育树操作、多样性指标和文件格式解析器。

- **TileDB-VCF** — 使用 TileDB 多维稀疏数组技术高效存储和检索基因组变异调用数据的高性能 C++ 库。支持增量样本添加、压缩存储和跨基因组区域及样本的并行查询。

- **Zarr** — 实现 Zarr 分块压缩 N 维数组存储格式的 Python 库。支持 NumPy 风格数组、多种压缩编解码器、部分数组读取和云存储（S3、GCS、Azure）。

### 数据管理与基础设施

- **LaminDB** — 使生物数据可查询、可追溯、可复现和 FAIR 的开源数据框架。结合数据湖架构、谱系追踪、特征存储、生物本体（20+ 本体）、LIMS 和 ELN 功能。

- **Modal** — 无服务器云平台，以最小配置运行 Python 代码，专为 AI/ML 工作负载和科学计算设计。支持强大 GPU（T4 到 B200+）、自动扩缩容和按量付费。

- **Optimize for GPU（GPU 加速优化）** — 使用 NVIDIA RAPIDS 生态系统将 Python 代码 GPU 加速，实现 10x–1000x 性能提升。涵盖 12 个 GPU 库：CuPy、Numba CUDA、Warp、cuDF、cuML、cuGraph、KvikIO、cuxfilter、cuCIM、cuVS、cuSpatial 和 RAFT。

### 化学信息学与药物发现

- **Datamol** — 基于 RDKit 构建的分子操作和特征化 Python 库。提供分子 I/O、标准化、变换、特征化、并行处理和与 ML 流程的集成。

- **DeepChem** — 分子机器学习和药物发现的深度学习框架。提供图神经网络（GCN、GAT、MPNN）、分子特征化、预训练模型和 MoleculeNet 基准套件（50+ 数据集）。

- **DiffDock** — 基于扩散模型的最先进分子对接方法。使用扩散模型生成多样化、高质量的蛋白质-配体结合构象，无需穷举搜索。

- **MedChem** — 药物化学分析和类药性评估 Python 库。提供分子描述符计算、ADMET 性质预测、类药性过滤器（Lipinski 五规则等）和合成可及性评分。

- **Molfeat** — 提供 100+ 分子特征化器的综合 Python 库。包括分子指纹（ECFP、MACCS）、分子描述符、图表示和预训练模型（MolBERT、ChemBERTa）。

- **PyTDC** — 提供治疗数据公共库（TDC）访问的 Python 库，包含药物发现和开发的精选数据集和基准。包括 ADMET 预测、药物-靶点相互作用、药物-药物相互作用等数据集。

- **RDKit** — 开源化学信息学工具包。提供分子 I/O、200+ 描述符、分子指纹、SMARTS 子结构搜索、3D 坐标生成、药效团感知和反应处理。

- **Rowan** — 具有 Python API 的云端量子化学平台。提供 45+ 化学计算：pKa 预测、氧化还原电位、溶解度、构象搜索、几何优化、蛋白质-配体对接和 AI 驱动的蛋白质共折叠。

- **TorchDrug** — 基于 PyTorch 的药物发现机器学习平台，提供 40+ 数据集和 20+ GNN 模型。

### 蛋白质组学与质谱

- **matchms** — 支持 40+ 过滤器的质谱数据处理和相似性匹配。提供谱库匹配（Cosine、Modified Cosine）、元数据协调和分子指纹比较。

- **pyOpenMS** — 用于蛋白质组学和代谢组学的全面质谱数据分析（LC-MS/MS 处理、肽段鉴定、特征检测、定量化和化学计算）。

### 医学影像与数字病理

- **histolab** — 用于全切片图像（WSI）处理和分析的数字病理工具包。提供自动组织检测、深度学习流程瓦片提取和十亿像素病理图像预处理。

- **PathML** — 用于全切片图像分析、组织分割和病理数据机器学习的综合计算病理学工具包。

- **pydicom** — 处理 DICOM 医学影像文件的纯 Python 包。支持 CT、MRI、X 光、超声、PET 等多种模态，提供像素数据提取、元数据访问、匿名化和格式转换。

### 医疗 AI 与临床机器学习

- **NeuroKit2** — 分析 ECG、EEG、EDA、RSP、PPG、EMG 和 EOG 等生理数据的综合生物信号处理工具包。包括自动化信号处理、心率变异性分析、EEG 分析和自主神经系统评估。

- **PyHealth** — 用于开发、测试和部署临床数据机器学习模型的综合医疗 AI 工具包。提供 10+ 医疗数据集、20+ 临床预测任务、33+ 模型和全面的数据处理工具。

### 临床文档与决策支持

- **Clinical Decision Support（临床决策支持）** — 为制药和临床研究设置生成专业临床决策支持（CDS）文档。包括患者队列分析和治疗建议报告，支持 GRADE 证据分级和统计分析。

- **Clinical Reports（临床报告）** — 按照既定指南和标准撰写综合临床报告。涵盖病例报告、诊断报告、临床试验报告和患者文档。

- **Treatment Plans（治疗方案）** — 为所有临床专科生成简洁（3-4 页）的聚焦医疗治疗方案。支持一般医疗治疗、康复治疗、心理健康护理、慢性病管理和疼痛管理。

### 神经科学与电生理

- **BIDS** — 脑成像数据结构（BIDS）标准，用于组织和描述神经科学和生物医学研究数据集。现已覆盖 11 种模态：MRI、PET、EEG、MEG、iEEG、EMG 等。

- **Neuropixels-Analysis** — 使用 SpikeInterface、Allen Institute 和 IBL 最佳实践分析 Neuropixels 高密度神经记录的全面工具包。支持从原始数据到发表级筛选单元的完整工作流。

### 蛋白质工程与设计

- **Adaptyv** — 自动化蛋白质测试和验证的云实验室平台。支持结合检测、表达测试、热稳定性测量和酶活性检测，以及计算优化工具进行预筛选。

- **ESM（进化尺度建模）** — EvolutionaryScale 的最先进蛋白质语言模型，用于蛋白质设计、结构预测和表示学习。包括 ESM3（多模态生成模型）和 ESM C（高效嵌入模型）。

- **Glycoengineering（糖工程）** — 分析和工程蛋白质糖基化。扫描 N-糖基化序列，预测 O-糖基化热点，访问精选糖工程工具。

- **Molecular Dynamics（分子动力学）** — 使用 OpenMM 和 MDAnalysis 运行和分析分子动力学模拟。设置蛋白质/小分子系统、定义力场、运行能量最小化和生产 MD、分析轨迹。

### 机器学习与深度学习

- **aeon** — 提供跨 7 个领域最先进算法的全面 scikit-learn 兼容时间序列机器学习工具包：分类、回归、聚类、预测、异常检测、分割和相似性搜索。

- **Cirq** — Google 量子计算框架，用于设计、模拟和运行量子电路。最适合针对 Google Quantum AI 硬件、设计噪声感知电路和运行量子表征实验。

- **PufferLib** — 通过优化向量化和原生多智能体支持实现 1M-4M 步/秒的高性能强化学习库。

- **PyMC** — 贝叶斯统计建模和概率编程综合 Python 库。提供 MCMC 采样（NUTS、Metropolis-Hastings）、变分推断（ADVI、SVGD）、高斯过程和时间序列模型。

- **PyMOO** — 使用进化算法进行多目标优化的 Python 框架。实现 NSGA-II、NSGA-III、MOEA/D、SPEA2 等算法。

- **PyTorch Lightning** — 组织 PyTorch 代码以消除样板代码的深度学习框架。自动化训练工作流，支持多 GPU/TPU 训练和实验追踪。

- **PennyLane** — 跨平台量子计算、量子机器学习和量子化学 Python 库。支持量子电路自动微分、与 PyTorch/JAX/NumPy 集成和跨模拟器/量子硬件的设备无关执行。

- **Qiskit** — 世界上最流行的开源量子计算框架。提供 100+ 量子门、83 倍更快的电路优化、可视化工具和 IBM Quantum 硬件执行。

- **QuTiP** — Python 量子工具箱，用于模拟和分析量子力学系统。提供量子态、算符、时间演化求解器、分析工具和可视化。

- **scikit-learn** — 经典机器学习的行业标准 Python 库。提供全面的监督学习（分类、回归）、无监督学习（聚类、降维、异常检测）、数据预处理和模型评估。

- **scikit-survival** — 基于 scikit-learn 的审查数据生存分析和事件时间建模。提供 Cox 比例风险模型、随机生存森林和专门的评估指标。

- **SHAP** — 使用博弈论 Shapley 值进行模型可解释性和解释的统一方法。支持 TreeExplainer、DeepExplainer、KernelExplainer 和综合可视化。

- **Stable Baselines3** — 基于 PyTorch 的强化学习库，提供 PPO、SAC、DQN 等可靠实现。

- **statsmodels** — 统计建模和计量经济学（OLS、GLM、logit/probit、ARIMA、时间序列预测、假设检验、诊断）。

- **TimesFM Forecasting** — 使用 Google TimesFM 基础模型进行零样本时间序列预测。支持任何单变量时间序列（销售、传感器、能源、生命体征、天气）。

- **Torch Geometric** — 分子和几何数据的图神经网络。

- **Transformers** — NLP、计算机视觉、音频和多模态任务的最先进机器学习模型。提供 100 万+ 预训练模型和全面的 Trainer API。

- **UMAP-learn** — 降维和流形学习的均匀流形近似和投影（UMAP）Python 实现。提供快速、可扩展的非线性降维。

### 材料科学与化学

- **Astropy** — 天文学和天体物理学的综合 Python 库。包括坐标系统转换、FITS 文件操作、宇宙学计算、精确时间处理和 WCS 变换。

- **COBRApy** — 约束重建和分析（COBRA）代谢网络的 Python 包。提供通量平衡分析（FBA）、通量变异性分析（FVA）、基因敲除模拟和通路分析。

- **Pymatgen** — 材料科学计算和分析的 Python Materials Genomics 库。提供晶体结构操作、相图构建、电子结构分析和材料性质计算。

### 工程与仿真

- **MATLAB/Octave** — 矩阵运算、数据分析、可视化和科学计算的数值计算环境。MATLAB 为商业软件，GNU Octave 为免费开源替代方案。

- **FluidSim** — 使用伪谱法和 FFT 的高性能计算流体力学（CFD）面向对象 Python 框架。提供 2D/3D 不可压缩 Navier-Stokes 方程求解器。

- **SimPy** — 基于进程的离散事件仿真框架，用于建模具有进程、队列和资源争用的系统。

- **SymPy** — Python 符号数学库，使用数学符号进行精确计算。提供符号代数、微积分、方程求解、矩阵和线性代数、物理学和代码生成。

### 数据分析与可视化

- **Dask** — 用于大于内存数据集的并行计算，支持分布式 DataFrames、Arrays、Bags 和 Futures。

- **GeoMaster** — 综合地理空间科学技能，涵盖遥感、GIS、空间分析、地球观测机器学习和 30+ 科学领域。支持 8 种编程语言和 500+ 代码示例。

- **GeoPandas** — 扩展 pandas 用于处理地理空间矢量数据的 Python 库。提供 GeoDataFrame 和 GeoSeries 数据结构，支持空间文件格式读写、几何操作和空间分析。

- **Matplotlib** — 创建发表级静态、动画和交互式可视化的综合 Python 绘图库。

- **NetworkX** — 创建、分析和可视化复杂网络和图的综合工具包。支持 100+ 算法、50+ 图生成器和 8+ 布局算法。

- **Plotly** — Python 交互式科学和统计数据可视化库，提供 40+ 图表类型。

- **Polars** — 用 Rust 编写的高性能 DataFrame 库，具有 Python 绑定。通常比 pandas 快 5-30 倍。

- **Seaborn** — 具有数据集导向接口、自动置信区间和发表级主题的统计数据可视化。

- **Vaex** — 用于惰性核外 DataFrames 的高性能 Python 库，每秒处理超过 10 亿行。

### 系统发育与进化生物学

- **ETE Toolkit** — 系统发育树操作、可视化和分析的 Python 库。支持树构建、操作、比较和发表级可视化。

- **Phylogenetics** — 使用 MAFFT（多重比对）、IQ-TREE 2（最大似然）和 FastTree（快速 NJ/ML）构建和分析系统发育树。

### 多组学与 AI 智能体框架

- **HypoGeniC** — 使用大型语言模型加速科学发现的自动化假设生成和测试。提供数据驱动假设生成、文献洞察与经验模式结合的协同方法和机制组合方法。

### 科学传播与出版

- **BGPT Paper Search** — 通过 BGPT MCP 服务器搜索科学论文并检索从全文中提取的结构化实验数据。返回每篇论文 25+ 字段。

- **pyzotero** — Zotero Web API v3 的 Python 客户端。程序化管理 Zotero 参考库，支持 BibTeX、CSL-JSON 和格式化书目 HTML 导出。

- **Citation Management** — 综合引文管理。搜索 Google Scholar 和 PubMed，提取准确元数据，验证引文并生成正确格式化的 BibTeX 条目。

- **Generate Image** — 使用 OpenRouter 图像生成模型进行 AI 驱动的科学插图、示意图和可视化生成。

- **Infographics** — 使用 Nano Banana Pro AI 创建专业信息图，Gemini 3 Pro 质量审核。支持 10 种信息图类型和 8 种行业风格。

- **LaTeX Posters** — 使用 beamerposter、tikzposter 或 baposter 创建专业研究海报。

- **Market Research Reports** — 生成顶级咨询公司风格的综合市场研究报告（50+ 页），包括 Porter 五力、PESTLE、SWOT 等战略分析。

- **PPTX Posters** — 使用 PowerPoint/HTML 格式创建专业研究海报。

- **Scientific Schematics** — 使用 Nano Banana Pro AI 创建发表级科学示意图，Gemini 3 Pro 质量审核。

- **Scientific Slides** — 使用 PowerPoint 和 LaTeX Beamer 构建研究演讲幻灯片。

- **Venue Templates** — 访问主要科学出版场所的 LaTeX 模板、格式要求和投稿指南。

### 文档处理与转换

- **DOCX** — 创建、读取、编辑和操作 Word 文档（.docx 文件）。支持专业文档格式化。

- **MarkItDown** — 将 20+ 文件格式转换为针对 LLM 处理优化的 Markdown 的 Python 工具。转换 Office 文档、图像（OCR）、音频（转录）、网页内容和结构化数据。

- **Markdown & Mermaid Writing** — 创建科学文档、报告、分析和可视化的综合 Markdown 和 Mermaid 图表写作技能。

- **PDF** — 读取、创建、合并、拆分、旋转、水印、加密/解密和操作 PDF 文件。

- **PPTX** — 创建、读取、编辑和操作 PowerPoint 演示文稿（.pptx 文件）。

- **XLSX** — 电子表格创建、编辑和分析，支持公式、格式化、数据分析和可视化。

### 实验室自动化与设备控制

- **PyLabRobot** — 硬件无关的纯 Python SDK，用于自动化和自主实验室。统一接口控制液体处理机器人、读板器、振荡加热器、培养箱、离心机、泵和天平。

### 计算资源与工具发现

- **Get Available Resources** — 检测可用计算资源并为科学计算任务生成策略建议。自动识别 CPU 能力、GPU 可用性、内存约束和磁盘空间。

### 研究方法与提案写作

- **Paperzilla** — 与智能体讨论 Paperzilla 项目、推荐和经典论文。

- **Paper Lookup** — 搜索 10 个学术论文数据库查找研究论文、预印本和学术文章。

- **Research Grants** — 撰写 NSF、NIH、DOE 和 DARPA 的竞争性研究提案。

- **Research Lookup** — 使用 Perplexity Sonar Pro Search 或 Sonar Reasoning Pro 模型查找当前研究信息。

- **Scholar Evaluation** — 应用 ScholarEval 框架系统评估学术和研究工作。

### 法规与标准合规

- **ISO 13485 Certification** — 为医疗器械质量管理体系准备 ISO 13485:2016 认证文档的综合工具包。涵盖 31 项必需程序。

### 科学思维与分析

#### 分析与方法论

- **Exploratory Data Analysis** — 具有自动统计、可视化和洞察的综合 EDA 工具包。

- **Hypothesis Generation** — 生成和评估科学假设的结构化框架。

- **Literature Review** — 支持多个科学数据库的系统文献检索和综述工具包，遵循 PRISMA 指南。

- **Peer Review** — 对所有科学学科进行高质量科学同行评审的综合工具包。

- **Scientific Brainstorming** — 通过结构化构思工作流生成新颖研究想法的对话式头脑风暴伙伴。

- **Scientific Critical Thinking** — 严格科学推理和评估的工具和方法。

- **Scientific Visualization** — 使用 matplotlib 和 seaborn 创建发表级科学图形的最佳实践和模板。

- **Scientific Writing** — 使用 IMRAD 格式撰写、结构和格式化科学研究论文的综合工具包。

- **Statistical Analysis** — 综合统计检验、功效分析和实验设计。

#### 决策与情景分析

- **Consciousness Council** — 运行多视角心智委员会审议，用于复杂问题的多角度思考。

- **DHDNA Profiler** — 从任何文本中提取认知模式和思维指纹。

- **What-If Oracle** — 运行结构化 What-If 情景分析，进行多分支可能性探索。

#### 网络搜索与信息检索

- **Exa Search** — 通过 Exa API 进行高质量网络搜索，特别针对科学和技术内容优化。

- **Parallel Web** — 搜索网页、提取 URL 内容并使用 Parallel Chat API 和 Extract API 运行深度研究。
