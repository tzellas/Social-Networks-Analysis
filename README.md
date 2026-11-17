<div align="center">

# 🕸️ Social Network Analysis

Network structure, community detection and link prediction on real and synthetic networks, in three Jupyter notebooks.

![Python](https://img.shields.io/badge/Python_3.11-3776AB?logo=python&logoColor=white)
![NetworkX](https://img.shields.io/badge/NetworkX-2E7D9A)
![scikit-learn](https://img.shields.io/badge/scikit--learn-F7931E?logo=scikitlearn&logoColor=white)
![Node2Vec](https://img.shields.io/badge/Node2Vec-6A4C93)
![igraph](https://img.shields.io/badge/igraph-1F77B4)

<sub>Author · [Apostolos Tzellas](https://github.com/tzellas)</sub>

</div>

---

Can we predict whether two concepts are related from the connections around them? In a knowledge graph, concepts are dots and relationships are lines between them. I use the network's structure to score which pairs of concepts are likely to be connected.

For link prediction, I turn shared neighbors and other local graph patterns into features for a Random Forest, then compare it with a model trained on Node2Vec representations of nodes. I also use community detection to group densely connected nodes and compare the email network's groups with departments. Finally, I measure paths, centrality, and connectivity across generated networks to examine how structure affects reachability and robustness. Each notebook includes the code, figures, and interpretation.

## What this project does

- 🔗 **Predict relationships:** Use graph features and Node2Vec embeddings to score pairs of concepts, then test predictions against known relationships set aside for evaluation.
- 👥 **Find groups:** Compare ways to identify tightly connected communities, including whether email groups match formal departments.
- 🕸️ **Explore structure:** Measure how network shape changes paths, influential nodes, and resilience when nodes are removed.

## Main findings

- **Simple graph features outperform the tested embedding pipelines.** The DBpedia holdout gives the similarity-feature Random Forest average precision **0.606** and F1 **0.610**, versus **0.545** and **0.557** for Node2Vec with sum/average features. Embedding classifiers are shallower and untuned, so this compares pipelines rather than isolating representation quality.
- **Accuracy hides link recovery.** Only **0.25%** of candidate pairs are positive. The similarity baseline recovers **25 of 57** test links with zero observed false alarms; balancing recovers **38**, but produces **472** false alarms. Class weighting changes the trade-off rather than automatically improving it.
- **Communication groups differ from departments.** The email network's modularity-selected spectral partition contains **11 groups**, with modularity **0.376** and department agreement **ARI = 0.207**. The institution has 42 departments: dense communication groups only partly match its organization chart.
- **Shortcuts change reachability.** In a 308-node ring, rewiring probability 0.1 reduces mean shortest-path length from **38.88 to 6.48 hops**. Clustering remains **0.361**, compared with **0.500** before rewiring. These figures describe the stated seeded realizations.

## Explore the notebooks

| Notebook | Question | Methods |
|---|---|---|
| [03 · Link Prediction](03_link_prediction.ipynb) | Can network structure predict relationships between concepts? | Jaccard; Random Forest tuning; Node2Vec and edge operators; precision–recall curves; confusion matrices |
| [02 · Community Detection](02_community_detection.ipynb) | How do algorithms and objectives change detected groups? | Eight networks; Girvan–Newman, spectral clustering, greedy modularity; department-label agreement |
| [01 · Network Structure](01_network_structure.ipynb) | How do connection rules affect reachability and robustness? | Centrality; PageRank; 100-trial connectivity sweeps; minimum node cuts; small-world rewiring |

Each notebook can be read independently. For a progression through the concepts, follow **01 → 02 → 03**.

## Read or run

Saved outputs let you inspect the work without executing it. To rerun, open a notebook in Jupyter, VS Code, or Colab and run from top to bottom. Python **3.11** was used for verification. Installation cells and all analysis functions are included in each notebook.

Internet access is needed for dependency installation and the first dataset download. Downloads are cached in the system temporary directory and verified against snapshot hashes; Game of Thrones files are pinned to a commit. Run installation cells sequentially when notebooks share an environment, restarting kernels if dependencies change. Full analyses can take several minutes.

## Data and interpretation

Sources are [KONECT DBpedia](https://konect.cc/networks/dbpedia-similar/), SNAP's [email](https://snap.stanford.edu/data/email-Eu-core.html) and [Stanford web](https://snap.stanford.edu/data/web-Stanford.html) graphs, [Newman's football network](https://websites.umich.edu/~mejn/netdata/), and [Beveridge's Game of Thrones networks](https://github.com/mathbeveridge/gameofthrones). Attribution and preprocessing are documented in the notebooks. Game of Thrones data use **CC BY-NC-SA 4.0**; consult source terms before reusing datasets.

Link prediction evaluates one **transductive reconstruction split**: pairs share nodes and an observed graph. Positives are edges outside a spanning forest; negatives include easy cross-component pairs. Results do not establish future-link forecasting or performance on unseen nodes. Community searches stop at 50 groups, and reference topologies differ in density. These limits are discussed alongside the results.
