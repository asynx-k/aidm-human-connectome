# Human Brain Connectome: Network Analysis

Laboratory work 1 for the AIDM course: an analysis of the human brain connectome from the [Human Connectome Project](https://www.humanconnectome.org/) as a complex network. The notebook covers graph statistics, community detection, assortativity and a simulation of transcranial magnetic stimulation (TMS).

All the work is in [AIDM_lab_1.ipynb](AIDM_lab_1.ipynb).

## Data

`data/` holds the same connectome at three resolutions, as GraphML:

| File | Nodes (largest component) | Edges |
|---|---|---|
| `brain_scale60.graphml` | 110 | 1,378 |
| `brain_scale250.graphml` | 370 | 5,863 |
| `brain_scale500.graphml` | 748 | 11,261 |

The analysis uses the highest resolution (`scale500`). Nodes are brain regions from the Desikan–Killiany atlas, subdivided at higher resolutions, with 3D positions and hemisphere labels. Edges are white-matter connections found by tractography, with mean fibre length, fractional anisotropy and fibre count.

## What the notebook covers

**Problem 1: Graph analysis.**
Largest connected component (748 of 1,015 nodes; the remaining nodes are isolated), clustering, diameter, radius and shortest paths. Then the degree distribution: a power-law fit with the Clauset–Shalizi–Newman bootstrap test, compared with log-normal and exponential fits. Then the most central regions by degree, betweenness and closeness, and whether the brain is a complex network.

**Problem 2: Community detection.**
Label propagation, Louvain and Girvan–Newman, compared by modularity and by the anatomical areas and hemispheres that make up each community. Girvan–Newman uses edge betweenness estimated from a sample of nodes so that it runs on the full graph, and the level with the highest modularity is kept.

**Problem 3: Assortativity.**
Degree assortativity for the whole connectome and for each Louvain community, average neighbour degree as a function of degree, and the normalised rich-club coefficient, for the whole connectome and for each Louvain community.

**Problem 4: TMS simulation.**
Edges are weighted by `exp(-λ · fibre length)`, with λ = 1 / median fibre length. Stimulation is simulated with message passing and with a random walk, which can optionally be divided by its stationary distribution. Targets come from the rTMS clinical guidelines:

| Target | Area in the data | Clinical use |
|---|---|---|
| Hand M1 | `Precentral` (left) | motor recovery after stroke, pain |
| Temporoparietal cortex | `Superiortemporal` (left) | auditory hallucinations |
| DLPFC | `Rostralmiddlefrontal` (left) | depression |

The two methods are compared by the Jaccard overlap of the areas they reach, with side-by-side 3D plots.

## Main results

| | |
|---|---|
| Radius / diameter / average shortest path | 5 / 9 / 3.10 |
| Average clustering / transitivity | 0.639 / 0.443 |
| Degree distribution | heavy-tailed; a pure power law is rejected, and log-normal fits better |
| Most central regions | thalamus, caudate and putamen in both hemispheres |
| Modularity: Louvain / label propagation / Girvan–Newman | 0.62 / 0.61 / 0.49 (the Girvan–Newman split is essentially left vs right) |
| Degree assortativity | −0.024, close to neutral |
| TMS, message passing vs random walk | the same top areas (Jaccard 0.82–1.00 on the top 10), but the random walk reaches 1.5–2.3 times more areas |

Louvain and label propagation are randomised, so community results vary slightly between runs unless a seed is set.

## Setup

Requires Python 3.15 or newer and [uv](https://docs.astral.sh/uv/). Install the dependencies with:

```bash
uv sync
```

Then open `AIDM_lab_1.ipynb` in Jupyter or VS Code, select the project's environment as the kernel, and run all cells from the top.
```
