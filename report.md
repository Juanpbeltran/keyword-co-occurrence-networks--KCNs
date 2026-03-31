# Report: Keyword Co-occurrence Networks in Scientific Publications

## 1. Introduction

Keyword Co-occurrence Networks (KCNs) are a bibliometric tool for mapping the intellectual landscape of a research field. In a KCN, each unique author-supplied keyword is a **node**, and two keywords are connected by a weighted **edge** whenever they appear together in the same publication. The weight of that edge reflects how frequently the pair co-occurs across the corpus.

This report interprets the results produced in `notebook.ipynb`, covering network structure, thematic communities, centrality rankings, and their implications for understanding the sampled research domain.

---

## 2. Dataset and Network Construction

The analysis uses a curated sample of **30 academic paper records** spanning four research themes:

| Theme | Papers |
|-------|--------|
| Machine Learning | 9 |
| Natural Language Processing | 6 |
| Network Science | 7 |
| Bioinformatics | 5 |
| Cross-cutting | 3 |

Each paper contributes a set of 4 author-supplied keywords. All pairwise keyword combinations within a paper are extracted to form co-occurrence edges; repeated combinations increment the edge weight.

### Resulting Network

| Metric | Value |
|--------|-------|
| Unique keywords (nodes) | 63 |
| Co-occurrence pairs (edges) | 153 |
| Network density | 0.0783 |
| Average degree | 4.86 |
| Maximum degree | 28 (*deep learning*) |
| Minimum degree | 3 |
| Average clustering coefficient | 0.2523 |
| Average shortest path length | 2.9186 |
| Connected | Yes |

The network is **fully connected** (one giant component), meaning every keyword can be reached from every other keyword through a chain of co-occurrences. The density of ~7.8% is typical for scientific KCNs derived from moderate-sized corpora — sparse enough to be interpretable but dense enough to reveal meaningful structure.

---

## 3. Community Structure

Applying greedy modularity maximisation identified **5 distinct thematic communities**:

| Community | Size | Core Theme | Representative Keywords |
|-----------|------|-----------|------------------------|
| 1 | 18 | Natural Language Processing | NLP, BERT, transformers, attention mechanisms, sentiment analysis, machine translation |
| 2 | 15 | Network Science | graph theory, network analysis, social networks, community detection, complex networks |
| 3 | 13 | Deep Learning Methods | deep learning, neural networks, GANs, reinforcement learning, federated learning, explainability |
| 4 | 10 | Bioinformatics / Computational Biology | bioinformatics, machine learning, RNA-seq, gene expression, CRISPR, AlphaFold |
| 5 | 7  | Graph-based Methods | graph neural networks, knowledge graphs, link prediction, embeddings |

### Interpretation

**Community 1 (NLP)** is the largest single community, reflecting the proliferation of transformer-based methods and the breadth of NLP subtasks (summarisation, QA, NER, translation). The co-occurrence hub *BERT* connects almost all subtask keywords, acting as a unifying anchor of modern NLP.

**Community 2 (Network Science)** coheres around *graph theory*, which appears in every paper within this cluster. The community encapsulates both methodological contributions (centrality measures, community detection algorithms) and application domains (social networks, epidemic models).

**Community 3 (Deep Learning)** captures the methodological backbone shared across application areas — *deep learning* and *neural networks* are foundational terms appearing alongside more specialised topics (GANs, RL, federated learning). The presence of *explainability* and *fairness* indicates growing attention to responsible AI within this community.

**Community 4 (Bioinformatics)** groups computational biology applications. *Machine learning* bridges general ML methods and their biological applications. Keywords like *AlphaFold*, *CRISPR*, and *RNA-seq* represent landmark tools that have drawn broad computational interest.

**Community 5 (Graph Methods)** is a smaller, tightly-knit cluster centred on graph neural networks. Its separation from Network Science (Community 2) reflects the GNN community's roots in deep learning rather than classical graph theory.

---

## 4. Centrality Analysis

### 4.1 Top Keywords by Degree Centrality

| Rank | Keyword | Degree | Degree Centrality |
|------|---------|--------|-------------------|
| 1 | deep learning | 28 | 0.4516 |
| 2 | NLP | 19 | 0.3065 |
| 3 | graph theory | 15 | 0.2419 |
| 3 | bioinformatics | 15 | 0.2419 |
| 5 | network analysis | 11 | 0.1774 |
| 6 | BERT | 10 | 0.1613 |
| 7 | neural networks | 9 | 0.1452 |
| 7 | graph neural networks | 9 | 0.1452 |
| 7 | social networks | 9 | 0.1452 |
| 10 | machine learning | 7 | 0.1129 |

*Deep learning* is by far the most-connected keyword (degree 28), appearing in 10 of the 30 papers — almost always alongside other terms from different communities. This confirms its role as the dominant methodological paradigm spanning multiple sub-fields. *NLP* is the second hub, connecting the language-processing cluster to deep-learning and bioinformatics papers.

### 4.2 Betweenness Centrality — Bridge Keywords

| Rank | Keyword | Betweenness |
|------|---------|-------------|
| 1 | deep learning | 0.4747 |
| 2 | graph theory | 0.3697 |
| 3 | NLP | 0.3113 |
| 4 | bioinformatics | 0.3010 |
| 5 | graph neural networks | 0.1896 |

Betweenness centrality identifies keywords that act as **bridges** between otherwise distant parts of the network. While *deep learning* tops this list too (owing to its sheer connectivity), the more informative finding is that **graph theory** and **bioinformatics** have betweenness scores disproportionately high relative to their degree. This indicates that:

- *Graph theory* bridges the Network Science community and the Graph Methods community (and also connects to some ML papers).
- *Bioinformatics* connects the Bioinformatics community to the NLP community (via biomedical text mining) and to the Graph Methods community (via knowledge graph applications in biology).

Removing either of these bridge keywords would significantly fragment the network into isolated clusters — a useful insight for researchers seeking cross-disciplinary collaboration opportunities.

### 4.3 PageRank — Influential Keywords

PageRank rewards keywords whose neighbours are themselves highly connected. The top-ranked terms (*deep learning*, *NLP*, *graph theory*, *bioinformatics*) mirror the degree and betweenness rankings, reinforcing that these are genuinely central concepts rather than artefacts of a single metric. *BERT* achieves a notably high PageRank (0.0377, rank 6) despite a lower betweenness score, because it is tightly coupled to several high-degree NLP keywords.

---

## 5. Edge-Weight Distribution

| Weight | Number of edges |
|--------|----------------|
| 1 | 134 |
| 2 | 14 |
| 3 | 2 |
| 4 | 3 |

The distribution is **heavily right-skewed**: 87.6% of edges have weight 1, meaning most keyword pairs co-occur in only a single paper. Strong co-occurrences (weight ≥ 2) identify 19 keyword pairs that are robustly associated across multiple publications — these form the **backbone** of the network.

The backbone visualisation (see `notebook.ipynb`, Cell 14) reveals that the strongest associations cluster within communities rather than across them, consistent with the community-detection results. The most heavily weighted edges likely connect foundational method–application pairs (e.g., *deep learning* ↔ *neural networks*, *graph theory* ↔ *network analysis*).

---

## 6. Limitations

1. **Sample size**: 30 papers is a demonstration sample. Real-world KCN studies typically use hundreds to thousands of publications from databases such as Web of Science or Scopus. Results at this scale should be treated as illustrative.
2. **Keyword normalisation**: Author-supplied keywords are used as-is. In larger corpora, stemming, lemmatisation, or ontology-based normalisation (e.g., MeSH terms in biomedical literature) would be required to avoid duplicating semantically identical terms.
3. **Temporal dynamics**: The current analysis is static. A longitudinal KCN, splitting papers by publication year, would reveal emerging topics, declining fields, and the evolution of cross-disciplinary bridges.
4. **Directed vs undirected**: Co-occurrence is inherently symmetric, but citation-based networks would benefit from directed models.

---

## 7. Conclusions

This project demonstrates that **Keyword Co-occurrence Networks** are an effective tool for:

- **Mapping research landscapes** — five coherent thematic communities emerged naturally from the data, corresponding to real disciplinary boundaries (NLP, Network Science, Deep Learning, Bioinformatics, Graph Methods).
- **Identifying influential topics** — *deep learning* dominates as both degree and betweenness hub, confirming its cross-disciplinary importance.
- **Detecting bridges** — *graph theory* and *bioinformatics* serve as connectors between otherwise separate communities, highlighting natural interdisciplinary collaboration opportunities.
- **Revealing core associations** — the backbone network (weight ≥ 2) distils the most robust keyword relationships, filtering noise from incidental co-occurrences.

Future work could scale this analysis to thousands of papers, incorporate temporal slicing to track topic evolution, and apply more sophisticated community-detection algorithms (e.g., Leiden) to improve resolution.
