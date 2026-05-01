# Heat_budget

本项目自 2022 年开始，围绕“黑潮延伸区 / 东北太平洋”海洋混合层热收支分析展开。项目目标是构建混合层热收支预算模型，分析海表温度变化来源、热量通量、水平平流与垂直混合贡献，并评估预算残差。  
- 本项目由Hanxue Yu与[Sebastian Yang](https://www.researchgate.net/profile/Haocheng-Yang-6)在[OUC-COAS Jian Shi](https://www.researchgate.net/profile/Jian-Shi-46)指导下开发，开发周期为2022-2024，最后成为了Hanxue Yu本科论文的重要来源  
- 该项目Code由Hanxue Yu维护  
- 感谢JS!! 
- Hanxue Yu成为了USTC ESS 空间物理研究生  
2026_5_2 本项目永久停工封存

## 1. 项目概要

- 目标：计算海洋混合层热收支，理解混合层温度变化与海洋热异常（如 MHW）之间的关系。
- 区域：黑潮延伸区、东北太平洋、部分北太平洋海域。
- 主要方法：
  - MLTT（Mixed Layer Temperature Tendency）
  - Q（表面净热通量）
  - HADV（水平平流）
  - OVMIX（垂直混合 / 竖直输运）
  - RES（预算残差）
- 主要数据来源：ERA5、GLORYS、GODAS、OISST、BRAN 等再分析与观测数据。

## 2. 目录结构

- `README.md`：本说明文件。
- `code/`：主要分析流程与结果生成 notebook。
  - `1_14datacomb.ipynb`：数据整合与预处理。
  - `1_15_HeatBudgetV2.ipynb`：热收支预算主体分析。
  - `1_19heatBudgetV2.1.ipynb`：预算验证与模型迭代。
  - `1_20u&cloud.ipynb`、`1_26u&cloud copy.ipynb`：风场 / 云场相关分析。
  - `12_2_combineQ.ipynb`：热通量合成与处理。
  - `12_6_loadGlo24.ipynb`：加载 GLORYS 1/4° 数据。
  - `12_9heatbudget.ipynb`：预算计算与结果汇总。
  - `12_21_Lower_res.ipynb`：低分辨率测试与对比。
- `code/drawcode/`：绘图与气候态展示 notebook。
- `hpc/`：HPC 集群专用分析、批处理、数据下载与大规模计算结果。
- `NorthPacific/`：北太平洋 SSTA / SST 分析。
- `data/`：本地小数据样本与结果文件（项目中未包含全部原始大数据）。
- `2022/`、`2023/`：项目思路、公式推导、误差优化方案与集群操作手册。
- `godas.sh`：GODAS 数据下载脚本。
- `ncep.sh`：NCEP 数据下载脚本。
- `ssh脚本.222 -L 20260`：远程端口转发 / JupyterLab 访问脚本。

## 3. 核心计算公式

- 热收支预算公式：
  - `Mixed Layer Temperature Tendency = Q + HADV + OVMIX`
- 其中：
  - `Q` = 表面净热通量（短波 + 长波 + 潜热 + 感热）
  - `HADV` = 水平平流项
  - `OVMIX` = 竖直混合与竖直输运项
- 预算残差：
  - `RES = MLTT - (Q + HADV + OVMIX)`

## 4. 主要数据说明

- ERA5 变量（主要用于大气通量与风场）：
  - `u10`、`v10`、`hcc`、`lcc`、`mcc`、`msl`、`slhf`、`sshf`、`ssr`、`str`
- GLORYS 变量（主要用于海洋内部变量）：
  - `thetao`（海水位温）、`uo`、`vo`、`depth`、`times`
- 其他数据源：
  - GODAS：`ucur`、`vcur`、`pottmp`、`dbss_obml`
  - OISST：海表温度数据下载与处理
  - BRAN：混合层深度（MLD）、海温、速度等高分辨率数据

## 5. 使用建议

1. 先统一数据路径：
   - 当前 notebook 中存在多处硬编码路径，例如 `H:\OceanData\...`、`E:\OceanData\...`、`/lustre/...`。
   - 运行前应检查并替换为当前环境可用的目录。
2. 推荐执行顺序：
   - `code/1_14datacomb.ipynb`
   - `code/1_15_HeatBudgetV2.ipynb`
   - `code/1_19heatBudgetV2.1.ipynb`
   - `code/12_9heatbudget.ipynb`
3. 结果分析与绘图：
   - `code/1_20u&cloud.ipynb`
   - `code/drawcode/12_10Q&Q_clim.ipynb`
   - `hpc/` 中的 `11_26_12to4.ipynb`、`11_27_tendency.ipynb` 等用于大规模计算与对比分析。
4. 集群运行与环境说明：
   - `2023/08/YHC集群操作手册V1.0.md` 包含 JupyterLab、PBS 脚本和远程访问说明。

## 6. 维护建议

- 将路径信息集中到一个配置单元或配置文件中，避免各 notebook 中重复硬编码。
- 每个 notebook 顶部补充“目的 / 输入 / 输出 / 运行顺序”四段说明。
- 将重复的数据读取、处理、绘图逻辑逐步提取为独立 Python 模块或函数库。
- 保存每次实验参数固定值（如 MLD 标准、差分格式、时间间隔）到文档中，便于后续对比。

## 7. 关键参考文档

- `2022/11/25思路整理.md`：混合层热收支公式推导、数据选择与算法思路。
- `2022/12/11_23_RES减小指南.md`：预算残差优化策略与差分计算说明。
- `2023/08/YHC集群操作手册V1.0.md`：集群环境与远程运行说明。

---

> 说明：本仓库当前更偏向以 notebook 形式实现分析流程，后续如果希望长期维护，建议把核心计算流程逐步封装为脚本/模块，并把数据路径配置集中管理。