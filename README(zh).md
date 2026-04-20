# GraphSAT: Neural SAT Solving with GNN and GAT

本專案探索了使用連續優化（Continuous Optimization）與圖神經網路（Graph Neural Networks, GNN）來求解布林可滿足性問題（Boolean Satisfiability Problem, SAT）及最大可滿足性問題（MaxSAT）。

本研究實作並比較了傳統的 SAT Solver 演算法，以及基於 PyTorch 建構的幾種神經網路 SAT Solvers，並包含了用於自動化從 [MaxSAT Evaluation](https://maxsat-evaluations.github.io/) 下載測試資料集的工具。

詳盡的研究方法、實驗細節與結論請參閱專案內的 [`paper.pdf`](./paper.pdf)（或 [`paper(zh).pdf`](<./paper(zh).pdf>)）。

## 主要功能與特色 (Features)

1. **MaxSAT 資料集自動提取 (Dataset Preparation)**
   - 內建 `DatasetPreparer` 類別，可自動下載並解析 2017 至 2024 年 MaxSAT Evaluation Benchmarks 的 `.wcnf.xz` 檔案。
   - 將檔案轉換為可用於訓練或傳統求解器的軟硬子句（Soft & Hard Clauses）。
2. **困難 SAT 實例生成 (Hard Instances Generation)**
   - 利用 `pysat.examples.genhard` 生成諸如 Pigeonhole Principle (PHP)、Parity (PAR) 等經典的困難 SAT 問題，以作為壓力測試。
3. **傳統演算法基準 (Classical Baselines)**
   - 整合 `pysat` 中的 LSU (Linear SU) MaxSAT 求解器作為最佳化解的比較基準 (Baseline)。
4. **神經網路 SAT 求解器 (Neural SAT Solvers)**
   - **TorchmSAT**: 基於連續弛豫（Continuous Relaxations）與梯度下降的 SAT 求解器實作。
   - **GraphSAT (GNN)**: 將 SAT 公式轉換為二分圖（Bipartite Graph），並利用變數與子句之間的信息傳遞（Message Passing）來進行求解。
   - **GraphAttentionSAT (GAT)**: 在信息傳遞過程中引入注意力機制（Masked Attention），讓模型自動學習不同子句對變數指派的權重關係。

## 實驗與評估 (Experiments & Evaluation)

在 `graph_sat.ipynb` 中包含了完整的實驗流程：

1. **模型評估**：針對載入的 MaxSAT 資料或是生成的 Hard Instances，同時運行 `TorchmSAT`、`GraphSAT` 與 `GraphAttentionSAT`。
2. **效能比較**：透過 `MSELoss` 與自定義的 `compute_cost` 函數評估未滿足子句的數量（Unsatisfied Clauses）。
3. **視覺化輸出**：使用 `matplotlib` 將三種模型的 **Final Cost（未滿足子句數）**與 **Total Time（執行時間）**繪製成長條圖，便於直觀比較模型效能與收斂時間的 Trade-off。

## 環境需求與相容性 (Dependencies)

請確保您的環境中安裝了以下 Python 套件：

- `torch` (支援 GPU 加速)
- `python-sat` (PySAT)
- `numpy`
- `matplotlib`

若您使用 Jupyter Notebook 執行，部分套件缺失時程式碼中已內建 `!pip install` 指令進行自動安裝。

## 專案結構 (Project Structure)

```text
📂 graph-sat
├── 📄 graph_sat.ipynb    # 核心實驗代碼（包含資料處理、所有 SAT 模型實作及評估圖表產生）
├── 📄 lab.ipynb          # 實驗與測試草稿區
├── 📄 LICENSE            # 專案授權條款
├── 📄 paper.pdf          # 英文版研究論文
└── 📄 paper(zh).pdf      # 中文版研究論文
```
