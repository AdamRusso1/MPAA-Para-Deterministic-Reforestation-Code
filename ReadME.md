# MPAA Group Project: Optimising Reforestation in Pará, Brazil 🌳

![Python](https://img.shields.io/badge/Python-3.8%2B-blue.svg)
![Gurobi](https://img.shields.io/badge/Optimizer-Gurobi-red.svg)
![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-orange.svg)
![Optimization](https://img.shields.io/badge/Model-MILP-success.svg)

## 📖 Project Overview

This repository contains the deterministic mathematical programming models developed for Part II of the **"Mathematical Programming in Advanced Analytics" (BUST10134)** group project.

Our project aims to optimize the spatial allocation of a **US$ 489,850,000 reforestation budget** across 144 municipalities in the state of Pará, Brazil. By utilizing Mixed-Integer Linear Programming (MILP), the model seeks to maximize ecological returns (aligned with UN SDG 15) while systematically mitigating the risk of reversal (e.g., future deforestation).

This project uses the Gurobi optimisation solver, however different solvers are able to be used by changed the `pulp.[ENTER SOLVER HERE]` in the code when the models run.

## 📄 Final Report

The full written report is available at [`Report/MPAA_Group_6_Final_Report.pdf`](Report/MPAA_Group_6_Final_Report.pdf).

### Key Objectives

- **Maximize Biodiversity & Threat Mitigation**
- **Maximize Carbon Sequestration Potential**
- **Maximize Deforestation Urgency Intervention**
- **Minimize Reversal Risk**
- **Minimize Spatial Dispersion** (using Fortet–Glover linearisation)

---

## 📂 Repository Structure

To ensure the scripts run correctly, your repository should match the following structure once the data is downloaded:

```text
MPAA-Para-Deterministic-Reforestation-Code/
│
├── Data/                   <-- MUST populate the shapefiles manually (See 'Data Acquisition' below)
│   ├── MPAA_model_data.csv
│   ├── distance_matrix_named.csv
│   ├── BR_Municipios_2024.cpg
│   ├── BR_Municipios_2024.dbf
│   ├── BR_Municipios_2024.prj
│   ├── BR_Municipios_2024.shx
│
│
├── Figures/                <-- Generated visualizations (radar plots, maps, etc.)
├── Outputs/                <-- Generated CSV tables (payoff tables, allocations)
│
├── 01_individual_objective_analysis.ipynb
├── 02_nadir_utopia_tolerance_sensitivity.ipynb
├── 03_main_lexicographix_model.ipynb
├── 04_heuristic_model.ipynb
├── 05_adverse_investor_sensitivity.ipynb
├── 06_budget_sensitivity_analysis.ipynb
├── 07_optimal_budget_ndc.ipynb
└── ReadME.md
```

## Data Acquisition

Due to GitHub's file size constraints, and spatial shapefiles required to run the maps and models are hosted externally on Google Drive. The datasets that are **already in the /Data folder are `MPAA_model_data.csv` and `distance_matrix_named.csv`**. You must **download and place the shapefile data into the correct directory (/Data Folder) before running the notebooks**.

1. **To access the data**: Click [this Google Drive Link](https://drive.google.com/drive/folders/1eYqQlgD9T13tnWwqNcJmfywB5vX8ZDMk?usp=drive_link) to open the project data folder

2. **Download the .shp file**: the shapefile `BR_Municipios_2024.shp`. The others (`.shx`, `.dbf`, `.prj`, `.cpg` are already in the /Data folder).

3. **Place in Root Directory**: Place the `BR_Municipios_2024.shp` downloaded file directly inside the /Data folder.

## ⚙️ Installation & Setup

1. Clone the repository:

```bash
git clone https://github.com/alasdairwood04/MPAA-Para-Deterministic-Reforestation-Code.git
cd mpaa-para-deterministic-reforestation-code
```

2. Install dependencies:

Install the required Python packages using:

```bash
pip install gurobipy pandas matplotlib seaborn numpy geopandas pulp
```

3. Gurobi License:
   The optimization models rely on the Gurobi solver. You must have an active [Gurobi License](https://www.gurobi.com/solutions/licensing/) (an Academic License is sufficient) configured on your machine.

(_note: other solvers can be used for this code, would just need to amend the pulp code for the solver of your choice. You can run the following code to see available solvers on your machine_)

```python
solver_list = pulp.listSolvers(onlyAvailable=True)
print(solver_list)
```

### 🚀 Execution Guide & Methodology

The implementation is divided into sequential Jupyter Notebooks to ensure logical flow and modularity. **Please run the notebooks in the numbered order**:

1. `01_individual_objective_analysis.ipynb`

- Purpose: Solves the MILP independently for each objective to establish theoretical upper and lower bounds (Utopia and Nadir points).

2. `02_nadir_utopia_tolerance_sensitivity.ipynb`

- Purpose: Generates the payoff table and analyzes the trade-offs between competing objectives using the Nadir/Utopia values.

3. `03_main_lexicographix_model.ipynb`

- Purpose: Implements the core multi-objective optimization using a Hierarchical/Lexicographic approach. It prioritizes objectives sequentially while applying epsilon-constraint logic to preserve secondary goals.

4. `04_heuristic_model.ipynb`

- Purpose: A benchmark heuristic approach (greedy algorithm based on cost-effectiveness) to compare against our exact MILP optimal solutions.

5. `05_adverse_investor_sensitivity.ipynb`

- Purpose: Explores an alternative scenario prioritising minimising reversal risk. Adapts model weights to reflect a highly risk-averse funding entity.

6. `06_budget_sensitivity_analysis.ipynb`

- Purpose: Analysis that varies the primary constraint (the US$ 489.85M budget) to evaluate how marginal changes in funding impact the optimal spatial allocation and total hectares restored. The low budget scenario is US\$ 221.37M and the high budget scenario is US\$ 790M, with the medium budget being the same as the original US$ 489.85M budget for comparison.

7. `07_optimal_budget_ndc.ipynb`

- Purpose: To quantify the absolute minimum financial investment required to meet the 5 year policy target of restoring 5.65 million hectares in Pará. A Budget minimisation model in formulated without factoring geographical dispersion penalties - but still implementing physical bound constraints

### 📊 Results & Outputs

Running the notebooks will automatically populate the Outputs/ and Figures/ folders.

- Outputs: Includes CSVs of optimal allocations per municipality, lexicographic tolerances, and comparison metrics.
- Figures: Includes spatial budget maps of Pará, radar plots comparing policies, and budget sensitivity charts.
