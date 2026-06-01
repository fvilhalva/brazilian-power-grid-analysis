# brazilian-power-grid-analysis

> Graph-theoretic vulnerability analysis of Brazil's transmission grid to identify critical substations and model cascading failures from public ONS data.

[![Project Status](https://img.shields.io/badge/status-research-blue)](#)
[![Python](https://img.shields.io/badge/python-3.10%2B-3776AB?logo=python&logoColor=white)](#installation)
[![Workflow](https://github.com/fvilhalva/brazilian-power-grid-analysis/actions/workflows/copilot-swe-agent/copilot/badge.svg)](https://github.com/fvilhalva/brazilian-power-grid-analysis/actions/workflows/copilot-swe-agent/copilot)

## Abstract (Português)

Este projeto aplica teoria de grafos à rede brasileira de transmissão de energia elétrica (dados públicos do ONS), modelando subestações como nós e linhas de transmissão como arestas ponderadas por tensão (kV). O objetivo é identificar pontos críticos de falha por centralidade de intermediação, simular falhas em cascata por remoção sequencial de nós críticos e visualizar os impactos geograficamente no mapa do Brasil. A iniciativa busca preencher uma lacuna na literatura internacional, que ainda possui poucos estudos dedicados especificamente ao sistema elétrico brasileiro.

## Scientific Motivation

Power transmission networks are a canonical example of **critical infrastructure**, where localized failures can propagate into large-scale service disruption. Complex-network literature has shown that many infrastructures are robust to random failures but vulnerable to targeted attacks on high-centrality nodes (Albert, Jeong & Barabási, 2000; Crucitti, Latora & Marchiori, 2004). In power systems, this vulnerability is especially relevant because topological stress and operational coupling can trigger cascading effects (Buldyrev et al., 2010; Hines, Cotilla-Sanchez & Blumsack, 2010).

Despite this consolidated international research agenda, there are comparatively few studies focused specifically on the **Brazilian transmission grid**. This repository contributes by structuring a reproducible workflow tailored to Brazil's context and public data availability.

## Data Source (ONS)

Primary source: **Operador Nacional do Sistema Elétrico (ONS)** open data portal.

- Portal: https://dados.ons.org.br/
- Publisher page: https://www.ons.org.br/paginas/sobre-o-sin/o-que-e-o-sin

### Download Instructions

1. Access the ONS open data portal: https://dados.ons.org.br/
2. Search for transmission assets datasets (e.g., substations and transmission lines, including nominal voltage).
3. Download the selected datasets in CSV/XLSX/GeoJSON (depending on availability).
4. Store raw files in a local `data/raw/` directory (recommended structure below).
5. Preserve original column names and metadata for auditability/reproducibility.

> Note: Dataset names and schemas can change over time in the portal; always record dataset version/date in your analysis artifacts.

## Methodology (Short Overview)

The grid is modeled as a weighted graph where **substations are nodes** and **transmission lines are edges**, with nominal **voltage (kV)** used as an edge weight attribute. We compute **betweenness centrality** to rank structurally critical substations. Then, we run a cascading-failure experiment based on **sequential removal of the current highest-centrality node**, recalculating metrics after each step and tracking connectivity degradation (e.g., giant component size, fragmentation, path length changes). Finally, results are visualized both as network plots and geographically over the map of Brazil to support spatial interpretation of critical points.

## Repository Structure

Current repository snapshot:

```text
brazilian-power-grid-analysis/
└── README.md
```

Recommended structure for full reproducibility as the project evolves:

```text
brazilian-power-grid-analysis/
├── data/
│   ├── raw/              # Original ONS downloads (do not edit)
│   └── processed/        # Cleaned/derived analysis tables
├── notebooks/            # Exploratory and reproducible notebooks
├── src/                  # Analysis scripts and reusable functions
├── outputs/
│   ├── figures/          # Network and map figures
│   └── tables/           # Ranked critical nodes and metrics
└── README.md
```

## Installation

```bash
python -m venv .venv
source .venv/bin/activate  # Linux/macOS
pip install --upgrade pip
pip install networkx pandas geopandas matplotlib shapely
```

## How to Reproduce the Results

1. Download ONS datasets as described above.
2. Build a node table (substations with geographic coordinates) and edge table (transmission lines with voltage in kV).
3. Create the graph in Python (NetworkX), preserving voltage as an edge attribute.
4. Compute baseline centrality metrics (especially betweenness centrality).
5. Run cascading simulation by iteratively removing the highest-betweenness node and recomputing graph metrics at each iteration.
6. Export ranked critical nodes and degradation indicators.
7. Plot:
   - graph-space visualization (topological perspective)
   - geospatial map of critical nodes/affected regions over Brazil

## Figures (Placeholders)

### Figure 1 — Transmission Grid Map (Brazil)

![Placeholder — Brazil transmission vulnerability map](outputs/figures/placeholder-brazil-map.png)

### Figure 2 — Cascading Failure / Centrality Impact

![Placeholder — Cascade simulation graph](outputs/figures/placeholder-cascade-graph.png)

## Known Limitations

- A purely topological model does not represent full AC/DC power-flow physics or operational dispatch constraints.
- Data quality and schema changes in public ONS sources can affect reproducibility if versioning is not tracked.
- Voltage-weighted edges are an informative proxy, but additional electrical parameters (capacity, impedance, load/generation balance) are needed for engineering-grade risk assessment.
- Cascading simulations based only on centrality-driven removals may over/underestimate real-world propagation dynamics.

## How to Cite

### ABNT

VILHALVA, F. *brazilian-power-grid-analysis: graph-based vulnerability analysis of the Brazilian transmission grid using ONS open data*. GitHub repository, 2026. Available at: <https://github.com/fvilhalva/brazilian-power-grid-analysis>. Accessed on: 1 jun. 2026.

### BibTeX

```bibtex
@misc{vilhalva2026brazilianpowergrid,
  author       = {Vilhalva, F.},
  title        = {brazilian-power-grid-analysis: Graph-based vulnerability analysis of the Brazilian transmission grid using ONS open data},
  year         = {2026},
  howpublished = {\url{https://github.com/fvilhalva/brazilian-power-grid-analysis}},
  note         = {GitHub repository. Accessed: 2026-06-01}
}
```

## References (selected)

- ALBERT, Réka; JEONG, Hawoong; BARABÁSI, Albert-László. Error and attack tolerance of complex networks. *Nature*, 406, 378–382, 2000.
- BULDYREV, Sergey V. et al. Catastrophic cascade of failures in interdependent networks. *Nature*, 464, 1025–1028, 2010.
- CRUCITTI, Paolo; LATORA, Vito; MARCHIORI, Massimo. Model for cascading failures in complex networks. *Physical Review E*, 69(4), 045104, 2004.
- HINES, Paul; COTILLA-SANCHEZ, Eduardo; BLUMSACK, Seth. Do topological models provide good information about electricity infrastructure vulnerability? *Chaos*, 20(3), 033122, 2010.
