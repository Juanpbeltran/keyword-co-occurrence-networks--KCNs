# Keyword Co-occurrence Networks (KCNs) in Scientific Publications

This project constructs and analyses **Keyword Co-occurrence Networks (KCNs)** from scientific publication metadata. By linking keywords that appear together within the same paper, KCNs reveal thematic clusters, bridging concepts, and the evolving structure of a research field.

## Motivation

Traditional keyword frequency counts treat each keyword in isolation. A co-occurrence network captures *relationships* between keywords, exposing how topics are grouped, which concepts bridge different fields, and which terms are central to the scientific conversation.

## Repository Contents

| File | Description |
|------|-------------|
| `notebook.ipynb` | Jupyter Notebook containing all code, visualisations, and inline commentary. Covers data preparation, graph construction, centrality analysis, community detection, and network visualisation. |
| `README.md` | Brief overview of the project and repository contents (this file). |
| `report.md` | Written report interpreting the results: network structure, community findings, centrality rankings, and their implications for research mapping. |

## Methodology at a Glance

1. **Dataset** – 30 representative academic paper records across four research themes (Machine Learning, NLP, Network Science, Bioinformatics).
2. **Graph construction** – Keywords become nodes; co-occurrence within a paper adds or increments an edge weight.
3. **Analysis** – Degree centrality, betweenness centrality, PageRank, and greedy modularity community detection.
4. **Visualisation** – Full KCN and backbone KCN (strong co-occurrences only), plus centrality bar charts and edge-weight distribution.

## Key Results (Summary)

- **63 unique keywords**, **153 co-occurrence edges**, network density **0.0783**.
- **5 thematic communities** detected (NLP, Network Science, Deep Learning, Bioinformatics, Graph Methods).
- Top hub keywords: *deep learning* (degree 28), *NLP* (degree 19), *graph theory* (degree 15).
- *Graph theory* and *bioinformatics* score highest on betweenness centrality relative to their degree, confirming their role as cross-community bridges.

## Requirements

```
networkx
matplotlib
pandas
numpy
scipy
jupyter
```

Install with:

```bash
pip install networkx matplotlib pandas numpy scipy jupyter
```

Then open the notebook:

```bash
jupyter notebook notebook.ipynb
```

