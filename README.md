<div align="center">



\# UCS vs A\* vs Hill Climbing



\### Route finding on the real Dhaka road network: a reproducible comparison of three classical search algorithms



<a href="https://colab.research.google.com/github/MejbahZidan/AI-Group-11/blob/main/AI\_Lab\_Project.ipynb">

&#x20; <img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab">

</a>

<img src="https://img.shields.io/badge/Python-3.x-3776AB?logo=python\&logoColor=white" alt="Python">

<img src="https://img.shields.io/badge/OSMnx-OpenStreetMap-7EBC6F" alt="OSMnx">

<img src="https://img.shields.io/badge/NetworkX-graph-F7931E" alt="NetworkX">

<img src="https://img.shields.io/badge/random%20seed-42-informational" alt="Seed 42">

<img src="https://img.shields.io/badge/course-ICE--4108-1F2A44" alt="ICE-4108">



\*\*Bangladesh University of Professionals (BUP)\*\* | Department of ICT | BICE Program

Artificial Intelligence Laboratory (AIL), ICE-4108



\[Results](#results) | \[How it works](#how-it-works) | \[Quick start](#quick-start) | \[Reproducibility](#reproducibility) | \[FAQ](#faq) | \[Cite](#cite-this-work)



</div>



\---



\## TL;DR



We ran \*\*Uniform Cost Search (UCS)\*\*, \*\*A\\\*\*\* and \*\*Hill Climbing\*\* on the same \*\*60 random start/goal pairs\*\* across Dhaka (routes of 2.82 km to 9.77 km, mean 5.77 km), using real OpenStreetMap data.



<div align="center">



| | UCS | \*\*A\\\*\*\* | Hill Climbing |

|---|:---:|:---:|:---:|

| Reached the goal | 100% | \*\*100%\*\* | \*\*0%\*\* |

| Found the optimal route | 100% | \*\*100%\*\* | 0% |

| Mean nodes expanded | 4461.88 | \*\*1251.67\*\* | 11.2 |

| Mean time (ms) | 8.68 | \*\*3.56\*\* | 0.46 |



</div>



\- \*\*A\\\* is as good as UCS and much cheaper.\*\* Same optimal cost on all 60 pairs, \*\*71.9% fewer nodes\*\*, about \*\*2.4x faster\*\*.

\- \*\*Hill Climbing is unreliable.\*\* It is cheap only because it gets stuck early, and it reached the goal in \*\*0 of 60\*\* runs.



\---



\## Why this project



Textbook search algorithms are usually shown on tiny graphs. This project asks a practical question: \*\*how do uninformed, informed and local search compare on a real city road network?\*\*



| Family | Algorithm | Role in the study |

|---|---|---|

| Uninformed | \*\*UCS\*\* | Optimal baseline |

| Informed | \*\*A\\\*\*\* (Haversine heuristic) | Shows the value of a heuristic |

| Local | \*\*Hill Climbing\*\* | Shows the limits of greedy search |



Together they illustrate the trade-off between \*\*guarantee\*\*, \*\*speed\*\* and \*\*memory\*\*.



\---



\## Results



\### Full comparison (60 pairs)



| Metric | UCS | A\* (Haversine) | Hill Climbing |

|---|---:|---:|---:|

| Goal reached | 100% | 100% | 0% |

| Optimal solution rate | 100% | 100% | 0% |

| Mean path cost (m) | 5768.85 | 5768.85 | no route found |

| Mean nodes expanded | 4461.88 | \*\*1251.67\*\* | 11.2 |

| Median nodes expanded | 4475.0 | \*\*847.5\*\* | 6.5 |

| Nodes expanded (min to max) | 1123 to 7267 | 214 to 3427 | 1 to 68 |

| Mean time (ms) | 8.68 | \*\*3.56\*\* | 0.46 |

| Time (min to max, ms) | 2.39 to 16.13 | 1.11 to 10.04 | 0.38 to 0.82 |



\- A\\\* expanded fewer nodes than UCS on \*\*all 60 pairs\*\*, with a per-pair saving of \*\*2.1x to 10.1x\*\* (median \*\*3.9x\*\*).

\- A\\\* cost equalled UCS cost on \*\*every\*\* pair, so the optimality check passed 60 out of 60.



> \[!NOTE]

> Hill Climbing's low time and node count are not a strength. It stops quickly because it fails to find a route.



\### Search behaviour on the map



<div align="center">

&#x20; <img src="assets/fig1\_search\_maps.png" alt="Explored nodes of UCS, A\* and Hill Climbing on the Dhaka map for one start/goal pair" width="100%">

</div>



\*Figure 1. One example pair. UCS expanded 2,328 nodes (4.9 ms) and A\\\* expanded 839 nodes (2.7 ms) for the same 7133 m route. Hill Climbing stopped after 5 nodes and failed. Colour shows expansion order, yellow (early) to dark (late). The black line is the route.\*



\### Performance dashboard



<div align="center">

&#x20; <img src="assets/fig2\_comparison\_dashboard.png" alt="Dashboard of mean nodes expanded, mean runtime, reliability and effort versus route length" width="85%">

</div>



\*Figure 2. Mean nodes expanded, mean runtime, reliability, and effort versus route length. UCS and A\\\* effort rises with distance, but A\\\* stays well below UCS at every route length.\*



\---



\## How it works



```mermaid

flowchart TD

&#x20;   A\["Load Dhaka road graph"] --> B\["Clean: keep largest strongly connected component"]

&#x20;   B --> C\["Choose 60 random start/goal pairs"]

&#x20;   C --> D\["Run UCS, A\* and Hill Climbing on every pair"]

&#x20;   D --> E\["Record success, cost, nodes expanded, time"]

&#x20;   E --> F{"A\* cost equals UCS cost on every pair?"}

&#x20;   F -- Yes --> G\["Build summary table and figures"]

&#x20;   F -- No --> H\["Stop: heuristic not admissible"]

```



\### Problem formulation



The road network is a weighted directed graph $G = (V, E)$. Nodes are intersections and edges are road segments weighted by their length in metres.



| Component | Definition |

|---|---|

| State | A road intersection (graph node) |

| Initial state | The selected start intersection |

| Actions | Drive along an outgoing road segment |

| Transition | Move to the intersection at the other end of the segment |

| Step cost | Length of the road segment in metres |

| Goal test | The current node is the goal intersection |

| Path cost | Sum of the segment lengths along the route |



The objective is the minimum-length path:



$$\\pi^\* = \\arg\\min\_{\\pi} \\sum\_{e \\in \\pi} w(e)$$



\### The three algorithms



| | UCS | A\* | Hill Climbing |

|---|---|---|---|

| Type | Uninformed | Informed | Local search |

| Priority | $g(n)$ | $g(n) + h(n)$ | $h(n)$ of neighbours only |

| Uses heuristic | No | Yes | Yes |

| Keeps a frontier | Yes | Yes | No |

| Complete | Yes | Yes | No |

| Optimal | Yes | Yes, if $h$ is admissible | No |

| Time | $O((V+E)\\log V)$ with a binary heap | Same worst case, usually far less | $O(m \\cdot b)$ |

| Space | Frontier + explored set | Frontier + explored set | $O(1)$ extra |



Here $g(n)$ is the road distance travelled so far, $h(n)$ is the straight-line estimate to the goal, $m$ is the number of steps taken and $b$ is the branching factor.



\*\*One engine for UCS and A\\\*.\*\* Both run on the same best-first search with priority $f(n) = g(n) + h(n)$:



| Algorithm | Heuristic |

|---|---|

| UCS | $h(n) = 0$ |

| A\* | $h(n) = $ Haversine distance to the goal |



Because the engine is shared, any difference in the results comes from the heuristic alone.



\*\*Haversine heuristic.\*\*



$$a = \\sin^2\\left(\\frac{\\Delta\\varphi}{2}\\right) + \\cos\\varphi\_1 \\cos\\varphi\_2 \\sin^2\\left(\\frac{\\Delta\\lambda}{2}\\right)$$



$$h(n) = 2R \\arcsin\\left(\\sqrt{a}\\right), \\quad R = 6{,}371{,}000 \\text{ m}$$



A road is never shorter than the straight line between its ends, so $h(n)$ never overestimates the remaining distance. It is \*\*admissible\*\*, which is why A\\\* stays optimal.



\*\*Hill Climbing\*\* moves to the neighbour closest to the goal and stops if no neighbour is closer. It has no frontier and cannot backtrack.



<details>

<summary><b>Show the core implementation</b></summary>



```python

def best\_first(s, t, h):

&#x20;   # Shared engine: priority = g + h. h = 0 gives UCS; h = Haversine gives A\*

&#x20;   g = {s: 0.0}; parent = {s: -1}; pq = \[(h\[s], 0.0, s)]; order = \[]

&#x20;   while pq:

&#x20;       \_, gc, u = heapq.heappop(pq)

&#x20;       if gc > g\[u] + 1e-9: continue

&#x20;       order.append(u)

&#x20;       if u == t: break

&#x20;       for v, w in adj\[u]:

&#x20;           ng = gc + w

&#x20;           if ng < g.get(v, math.inf):

&#x20;               g\[v] = ng; parent\[v] = u

&#x20;               heapq.heappush(pq, (ng + h\[v], ng, v))

&#x20;   # the route is rebuilt from the parent links



def ucs(s, t):    return best\_first(s, t, \[0.0] \* N)

def a\_star(s, t): return best\_first(s, t, hav\_to(t).tolist())



def hill\_climb(s, t, max\_steps=20000):

&#x20;   h = hav\_to(t); cur, path, cost = s, \[s], 0.0

&#x20;   while cur != t and len(path) < max\_steps:

&#x20;       best, bh, bw = None, h\[cur], 0.0

&#x20;       for v, w in adj\[cur]:

&#x20;           if h\[v] < bh: best, bh, bw = v, h\[v], w

&#x20;       if best is None: break          # dead end or local minimum

&#x20;       cur = best; cost += bw; path.append(cur)

```



</details>



\---



\## Reproducibility



Everything is controlled and repeatable.



| Item | Setting |

|---|---|

| Data | OpenStreetMap drive network, 3,500 m radius around 23.7461 N, 90.3742 E |

| Cleaning | Parallel roads reduced to the shortest, self-loops removed, \*\*largest strongly connected component only\*\* (so every pair is reachable) |

| Pairs | 60 random start/goal pairs, each at least \*\*30% of the map span\*\* apart |

| Fairness | Same graph, same pairs and same shared engine for every algorithm |

| Seed | \*\*42\*\* |

| Total runs | 60 pairs x 3 algorithms = \*\*180\*\* |

| Hill Climbing step limit | 20,000 |

| Optimality tolerance | 1 m |



\*\*Metrics:\*\* success rate, optimal rate (cost equals UCS cost), path cost, nodes expanded (proxy for time and space), and wall-clock time per run in ms (includes the heuristic calculation).



\---



\## Quick start



\### Option A: Google Colab (recommended, no setup)



1\. Click the \*\*Open In Colab\*\* badge at the top of this page.

2\. Run the single cell. Packages install automatically.

3\. Wait for the Dhaka network to download or load.

4\. Review the tables and figures, then download the results ZIP.



\### Option B: Run locally



```bash

git clone https://github.com/MejbahZidan/AI-Group-11.git

cd AI-Group-11

pip install osmnx networkx numpy pandas matplotlib jupyter

jupyter notebook AI\_Lab\_Project.ipynb

```



\### Where the map data comes from



1\. If `dhaka\_street\_network.graphml` is next to the notebook, it is used.

2\. Otherwise the Dhaka drive network is downloaded with OSMnx.

3\. If OpenStreetMap is unreachable, a synthetic jittered-grid network is used so the notebook still runs.



> \[!IMPORTANT]

> Results from the synthetic fallback will \*\*not\*\* match the Dhaka results reported here.



\### Configuration



Edit these constants at the top of the notebook cell:



| Constant | Default | Meaning |

|---|---|---|

| `CENTER` | `(23.7461, 90.3742)` | Download centre (lat, lon) |

| `RADIUS\_M` | `3500` | Download radius in metres |

| `N\_PAIRS` | `60` | Number of start/goal pairs |

| `MIN\_SPAN` | `0.30` | Minimum pair distance as a fraction of map span |

| `SEED` | `42` | Random seed |



\### Outputs



| File | Description |

|---|---|

| `summary.csv` | Aggregated statistics for the three algorithms |

| `all\_runs.csv` | Result of every algorithm on every pair (180 rows) |

| `fig1\_search\_maps.png` | Explored nodes and route for one example pair |

| `fig2\_comparison\_dashboard.png` | Four-panel comparison dashboard |



The notebook also packages these files as a downloadable ZIP.



\---



\## FAQ



<details>

<summary><b>Why is Hill Climbing the fastest if it never finds a route?</b></summary>



Because it stops at the first node where no neighbour is closer to the goal. In a city, dead ends and roads that must first turn away from the goal are common, so it terminates early with no route. Speed means nothing without a valid answer.

</details>



<details>

<summary><b>Why does A\* stay optimal?</b></summary>



Its heuristic is admissible. The Haversine distance is a straight line, and a road is never shorter than the straight line between the same two points, so the estimate never overestimates. The notebook checks this directly: A\* cost equals UCS cost on every pair.

</details>



<details>

<summary><b>Why keep only the largest strongly connected component?</b></summary>



It guarantees a directed route exists between every pair. Any failure is then caused by the algorithm, not by an unreachable destination.

</details>



<details>

<summary><b>Is the time comparison fair?</b></summary>



Yes. UCS and A\* run on the same engine, and the A\* time includes computing the Haversine heuristic. Only the heuristic differs.

</details>



<details>

<summary><b>Was memory measured?</b></summary>



Not directly. Nodes expanded is used as the measure of space, since UCS and A\* store a frontier and an explored set while Hill Climbing stores only its current path.

</details>



\---



\## Beyond A\*: what would come next



For this study A\\\* is the best of the three chosen algorithms because it is both optimal and efficient. For production navigation, other methods can do better:



| Algorithm | What it improves | Trade-off |

|---|---|---|

| Greedy Best-First | Faster | Not optimal |

| Weighted A\* | Faster | Bounded suboptimality |

| Bidirectional A\* | About half the search space | More complex |

| ALT landmarks A\* | Tighter heuristic | Needs preprocessing |

| Contraction Hierarchies | Fastest, used by real routing engines | Heavy preprocessing, static graph |

| IDA\* | Low memory | Repeated work on road graphs |

| Hill Climbing with restarts | Can escape local minima | Still no guarantee |



\---



\## Limitations and roadmap



\*\*Current limitations\*\*



\- Cost is road length only (no travel time, traffic or vehicle restrictions)

\- One area of Dhaka is covered

\- Memory and solution depth (number of road segments) were not recorded

\- Hill Climbing was tested without random restarts



\*\*Roadmap\*\*



\- \[ ] Travel-time and traffic-aware edge weights

\- \[ ] More cities and different road densities

\- \[ ] Landmark (ALT) heuristics versus Haversine

\- \[ ] Contraction Hierarchies comparison

\- \[ ] Random-restart Hill Climbing

\- \[ ] Peak memory and solution depth measurements



\---



\## Repository structure



```text

AI-Group-11/

|-- AI\_Lab\_Project.ipynb          # Full experiment in a single Colab cell

|-- assets/                       # Figures shown in this README

|   |-- fig1\_search\_maps.png

|   `-- fig2\_comparison\_dashboard.png

|-- results/                      # Generated by the notebook

|   |-- summary.csv

|   |-- all\_runs.csv

|   |-- fig1\_search\_maps.png

|   `-- fig2\_comparison\_dashboard.png

`-- README.md

```



\*\*Tech stack:\*\* Python 3, Google Colab, OSMnx, NetworkX, NumPy, Pandas, Matplotlib, heapq.



\---



\## Cite this work



```bibtex

@misc{aigroup11\_2026,

&#x20; title        = {Comparative Analysis of UCS, A\* and Hill Climbing for Route Finding on the Dhaka Road Network},

&#x20; author       = {Zidan, Md. Mejbaul Islam and Parvej, Md. Hemel and Roja, Most. Nair Nazmim},

&#x20; year         = {2026},

&#x20; howpublished = {\\url{https://github.com/MejbahZidan/AI-Group-11}},

&#x20; note         = {Artificial Intelligence Laboratory (ICE-4108), Bangladesh University of Professionals}

}

```



\---



\## Team



| Contributor | Student ID |

|---|---|

| Md. Mejbaul Islam Zidan | 23549009069 |

| Md. Hemel Parvej | 23549009005 |

| Most. Nair Nazmim Roja | 2254901013 |



\*\*Program:\*\* BICE 2023 | \*\*Department:\*\* ICT | \*\*Institution:\*\* Bangladesh University of Professionals (BUP) | \*\*Course:\*\* Artificial Intelligence Laboratory (ICE-4108)



\*\*Academic supervision\*\*



\- Jasiya Fairiz Raisa, Lecturer, Department of ICT

\- Nazneen Akter, Assistant Professor, Department of ICT



\---



\## References



1\. S. Russell and P. Norvig, \*Artificial Intelligence: A Modern Approach\*, 4th ed. Pearson, 2020.

2\. P. E. Hart, N. J. Nilsson and B. Raphael, "A Formal Basis for the Heuristic Determination of Minimum Cost Paths," \*IEEE Transactions on Systems Science and Cybernetics\*, vol. 4, no. 2, pp. 100-107, 1968.

3\. E. W. Dijkstra, "A Note on Two Problems in Connexion with Graphs," \*Numerische Mathematik\*, vol. 1, pp. 269-271, 1959.

4\. G. Boeing, "OSMnx: New Methods for Acquiring, Constructing, Analyzing, and Visualizing Complex Street Networks," \*Computers, Environment and Urban Systems\*, vol. 65, pp. 126-139, 2017.

5\. OpenStreetMap contributors, \[OpenStreetMap](https://www.openstreetmap.org/).

6\. A. Hagberg, D. Schult and P. Swart, "Exploring Network Structure, Dynamics, and Function Using NetworkX," \*Proceedings of the 7th Python in Science Conference\*, pp. 11-15, 2008.



\## License and attribution



Map data (c) OpenStreetMap contributors, available under the \[Open Database License (ODbL)](https://www.openstreetmap.org/copyright).



The project source code does not yet declare a software license. Until a `LICENSE` file is added, all rights to the original code remain with the authors.



\---



<div align="center">



\*\*Artificial Intelligence Laboratory | ICE-4108 | Bangladesh University of Professionals | Dhaka, Bangladesh\*\*



</div>

