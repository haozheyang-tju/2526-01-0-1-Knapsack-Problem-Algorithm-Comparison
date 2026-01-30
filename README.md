# 0/1 背包问题多算法对比（Algorithm Comparison on 0/1 Knapsack）

本仓库在**统一的实验协议**与**可复现的随机种子控制**下，对 0/1 背包问题（0/1 Knapsack Problem）上的多种代表性算法进行系统化对比：  
- **经典精确算法**：分支限界（基础版 / 增强版）  
- **近似 / 随机化算法**：贪心（GreedyDensity / Greedy+Max）、集束搜索（Classic / Bounded）、模拟退火（SA / 记忆 SA）  
- **元启发式算法**：遗传算法（GA / 改进 GA）、二进制粒子群（BPSO）、二进制布谷鸟搜索（BCS）

> 课程报告对比了 **12 种算法**，覆盖 **8 类实例集**（每类 20 个样本、每样本重复 5 次），并以运行时间、解质量 Gap 及其方差等指标评测。  
> 详细实验设置与结论见 `report.docx`。  

---

## 目录结构

> 仓库由 `data/`、`src/` 两个目录与 `report.docx`、`defense.pptx` 两个文件组成。

```text
.
├── data/                    # 数据与实验输出目录（见下文“输出说明”）
├── src/                     # 代码（Jupyter Notebooks）
│   ├── 毛子鋆（maozijun）.ipynb        # 近似：贪心 GreedyDensity / Greedy+Max
│   ├── 刘伦（liulun）.ipynb            # 经典：集束搜索 Classic / Bounded
│   ├── 邓康（dengkang）.ipynb          # 经典：分支限界 bb_basic / bb_enhanced
│   ├── 张涛（zhangtao）.ipynb          # 近似：SA / 记忆 SA（Memetic SA）
│   ├── 梁天宇（liangtianyu）.ipynb      # 元启发：GA / 改进 GA
│   ├── 杨昊哲（yanghaozhe）.ipynb       # 元启发：BPSO / BCS
│   └── zong.ipynb                      # 全算法汇总对比 + 8-panel 综合可视化
├── report.docx               # 课程报告（完整理论 + 实验 + 结论）
└── defense.pptx              # 答辩PPT
```

---

## 环境依赖

推荐：Python **3.9+**（建议 3.10/3.11）。

主要依赖（不同 notebook 会用到不同子集）：
- numpy, pandas
- matplotlib（可视化）
- scipy（统计检验/置信区间等）
- tqdm（进度条）
- scikit-learn（回归拟合、R² 等）
- seaborn（部分 notebook 的可视化）
- jupyter / notebook（运行 `.ipynb`）

安装示例：

```bash
python -m venv .venv
# Windows: .venv\Scripts\activate
# macOS/Linux: source .venv/bin/activate
pip install -U pip
pip install numpy pandas matplotlib scipy tqdm scikit-learn seaborn jupyter
```

---

## 快速开始（推荐：一键生成全算法对比结果）

`src/zong.ipynb` 是**汇总入口**：包含算法注册表、统一数据集生成、基准测试与综合 8-panel 图。

### 方式 A：在 Jupyter 中运行（最直观）
```bash
# 在仓库根目录启动（确保相对路径输出到根目录/指定目录）
jupyter lab
```
打开 `src/zong.ipynb` → “Run All”。

### 方式 B：命令行无界面执行（可复现 & 便于批量跑）
```bash
jupyter nbconvert --to notebook --execute src/zong.ipynb   --ExecutePreprocessor.timeout=-1   --output zong.executed.ipynb
```

> 说明：多个 notebook 在代码末尾包含 `if __name__ == "__main__":` 入口，因此也可先 `nbconvert --to python` 再 `python xxx.py` 执行（适合服务器/CI）。

---

## 单算法（或双算法）对比复现

每个成员 notebook 聚焦一组算法，并通常包含：
1) 问题定义、实例意义与算法实现  
2) 理论分析：时间/空间复杂度、性能保证、瓶颈  
3) 实验设计与数据生成（≥5 类实例、统计重复）  
4) 评估指标统计与输出  
5) 可视化与一致性/显著性/预测模型等分析（按 notebook 具体实现）

按需打开并运行对应 notebook 即可。

---

## 实验数据与实例集（统一协议）

实验主要基于**合成数据集**，包含多种结构/分布的 0/1 背包实例族（例如 Uncorrelated、WeaklyCorr、StronglyCorr、InverseCorr、HeavyTailed、GreedyTrap 等），并通过固定随机种子与稳定哈希生成可复现实例。  

---

## 输出说明（运行后自动生成）

> 多数 notebook 会在**运行目录**下自动创建输出文件夹；如果你希望输出统一落在 `data/` 下，可将代码中的 `OUTDIR` / `*_DIR` 前缀改为 `data/...`。

常见输出（以汇总与通用评估为主）：

- `outputs_exp_section3/`
  - `raw_results_section3.csv`：第 3 节实验原始记录（逐实例/逐重复）
  - `summary_preview_section3.csv`：快速汇总预览

- `outputs_metrics_section4/`
  - `metrics_agg.csv`：指标聚合表（均值、方差、置信区间等）
  - `scalability_fit_full.csv`、`scalability_fit_subset.csv`：可扩展性拟合结果
  - 可能还包含内存峰值、能耗代理等统计表（以各 notebook 实现为准）

- `outputs_analysis_section5/`
  - `comprehensive_analysis_8panel.png`：综合 8-panel 对比图（汇总 notebook 生成）

部分 notebook（如分支限界、模拟退火专章）可能使用独立输出目录/文件名（以 notebook 内变量为准）。

---

## 报告与答辩材料

- `report.docx`：完整报告（理论推导、实验方案、结果分析、适用边界总结等）  
- `defense.pptx`：答辩用展示材料  

---

## 作者与分工

- 毛子鋆：贪心近似（GreedyDensity / Greedy+Max）
- 刘伦：集束搜索（Classic / Bounded）
- 邓康：分支限界（基础 / 增强）
- 张涛：模拟退火（SA / 记忆 SA）
- 梁天宇：遗传算法（GA / 改进 GA）
- 杨昊哲：BPSO / BCS

---

## 许可与声明

本仓库用于课程作业与学术交流。若需二次发布或用于商业用途，请先与作者沟通并补充合适的开源许可证。
