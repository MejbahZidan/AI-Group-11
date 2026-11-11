\# 🗺️ UCS vs A\* vs Hill Climbing

\### A Comparative Study of Search Algorithms on the Real Dhaka Road Network



<p align="center">

&#x20; <strong>Can an intelligent search algorithm find the shortest route through a real city's road network more efficiently?</strong>

</p>



<p align="center">

&#x20; <a href="https://colab.research.google.com/github/MejbahZidan/AI-Group-11/blob/main/AI\_Lab\_Project.ipynb">

&#x20;   <img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open in Colab">

&#x20; </a>

&#x20; <img src="https://img.shields.io/badge/Python-3.x-blue?logo=python\&logoColor=white">

&#x20; <img src="https://img.shields.io/badge/OSMnx-OpenStreetMap-green">

&#x20; <img src="https://img.shields.io/badge/NetworkX-Graph%20Analysis-orange">

&#x20; <img src="https://img.shields.io/badge/Experiments-60%20Pairs-purple">

&#x20; <img src="https://img.shields.io/badge/Seed-42-informational">

</p>



<p align="center">

&#x20; <b>Bangladesh University of Professionals (BUP)</b><br>

&#x20; Department of ICT · BICE Program<br>

&#x20; Artificial Intelligence Laboratory · ICE-4108

</p>



\---



\## 📌 Overview



Finding the shortest route in a real-world road network is more complicated than searching through a small textbook graph.



Road networks contain thousands of intersections, different road lengths, directional connections, and complex branching structures.



This project investigates how three classical Artificial Intelligence search algorithms perform when solving the same route-finding problem on a real road network in Dhaka, Bangladesh.



We implemented and evaluated:



\- \*\*Uniform Cost Search (UCS)\*\* — an uninformed optimal search algorithm.

\- \*\*A\\\* Search\*\* — an informed search algorithm using the Haversine distance heuristic.

\- \*\*Hill Climbing\*\* — a local search algorithm that greedily selects the neighbour closest to the destination.



All three algorithms were evaluated on the same \*\*60 randomly selected start-goal pairs\*\*, using the same road network and random seed.



\### 🎯 The central finding



A\* achieved the same optimal route cost as UCS across all 60 experiments while expanding substantially fewer nodes.



Hill Climbing, despite its low computational effort, failed to reach the destination in every experiment.



\---



\## 📊 Key Results at a Glance



<p align="center">

&#x20; <img src="assets/fig2\_comparison\_dashboard.png" width="95%" alt="Algorithm performance comparison dashboard">

</p>



\### Performance comparison



| Evaluation Metric | UCS | A\* | Hill Climbing |

|:---|---:|---:|---:|

| Goal reached | 100% | \*\*100%\*\* | 0% |

| Optimal solution rate | 100% | \*\*100%\*\* | 0% |

| Mean path cost | 5,768.85 m | \*\*5,768.85 m\*\* | No route |

| Mean nodes expanded | 4,461.88 | \*\*1,251.67\*\* | 11.2 |

| Median nodes expanded | 4,475 | \*\*847.5\*\* | 6.5 |

| Mean execution time | 8.68 ms | \*\*3.56 ms\*\* | 0.46 ms |



<details>

<summary><b>View detailed experimental statistics</b></summary>



| Metric | UCS | A\* | Hill Climbing |

|:---|---:|---:|---:|

| Minimum nodes expanded | 1,123 | 214 | 1 |

| Maximum nodes expanded | 7,267 | 3,427 | 68 |

| Minimum execution time | 2.39 ms | 1.11 ms | 0.38 ms |

| Maximum execution time | 16.13 ms | 10.04 ms | 0.82 ms |

| Successful experiments | 60/60 | 60/60 | 0/60 |



</details>



\### 🏆 What do these numbers tell us?



\*\*A\* vs UCS\*\*



\- Both algorithms found optimal routes in all 60 experiments.

\- A\* expanded \*\*71.9% fewer nodes\*\* on average.

\- A\* was approximately \*\*2.44× faster\*\*.

\- A\* expanded fewer nodes in every tested pair.

\- The median node-expansion reduction was approximately 3.9×.



\*\*Hill Climbing\*\*



\- Expanded very few nodes.

\- Had the lowest execution time.

\- Failed in all 60 experiments.

\- Its low computational cost came from getting stuck before reaching the destination.



> \*\*Important:\*\* A fast algorithm is not necessarily a useful algorithm. Performance must be evaluated alongside solution quality and reliability.



\---



\## 🛣️ Visualizing the Search Process



The following visualization illustrates how the algorithms explore the same Dhaka road network.



<p align="center">

&#x20; <img src="assets/fig1\_search\_maps.png" width="100%" alt="UCS, A star and Hill Climbing search visualization">

</p>



\*\*Figure 1 — Search behaviour on the Dhaka road network.\*\*



For the illustrated example:



| Algorithm | Nodes Expanded | Execution Time |

|:---|---:|---:|

| UCS | 2,328 | 4.9 ms |

| A\* | 839 | 2.7 ms |

| Hill Climbing | 5 | Failed |



UCS and A\* successfully found the same 7,133-metre route.



A\* explored substantially fewer intersections, while Hill Climbing stopped after five nodes because it could not find a neighbour closer to the goal.



\---



\## 🧠 Problem Formulation



We model the Dhaka road network as a \*\*weighted directed graph\*\*:



\\\[

G=(V,E)

\\]



Where:



\- \\(V\\) represents road intersections.

\- \\(E\\) represents directed road connections.

\- Each edge has a weight representing its road-segment length in metres.



\### Search problem definition



| Component | Description |

|:---|:---|

| State | A road intersection |

| Initial state | Selected starting intersection |

| Actions | Travel along an outgoing road segment |

| Transition | Move to the connected intersection |

| Step cost | Length of the road segment |

| Goal test | Current intersection equals destination |

| Path cost | Sum of all road-segment lengths |



Our objective is to minimize the total distance travelled:



\\\[

\\pi^\*=\\arg\\min\_{\\pi}\\sum\_{e\\in\\pi}w(e)

\\]



In simple terms, the objective is to find the shortest available road route between two intersections.



\---



\## ⚙️ Algorithms Implemented



\### 1. Uniform Cost Search (UCS)



\*\*Category:\*\* Uninformed Search



UCS expands the node with the lowest accumulated path cost.



\\\[

f(n)=g(n)

\\]



Where \\(g(n)\\) represents the distance travelled from the starting point to the current node.



\*\*Characteristics:\*\*

\- Does not use a heuristic.

\- Maintains a priority queue.

\- Guarantees an optimal solution with non-negative edge costs.

\- Can explore many unnecessary intersections.



\### 2. A\* Search



\*\*Category:\*\* Informed Search



A\* combines the actual distance travelled with an estimated remaining distance.



\\\[

f(n)=g(n)+h(n)

\\]



Where:



\- \\(g(n)\\): Actual distance from the start.

\- \\(h(n)\\): Estimated distance from the current intersection to the destination.



\#### Haversine heuristic



We use the Haversine formula to estimate the straight-line geographical distance between two coordinates.



\\\[

a=\\sin^2\\left(\\frac{\\Delta\\varphi}{2}\\right)

+\\cos\\varphi\_1\\cos\\varphi\_2

\\sin^2\\left(\\frac{\\Delta\\lambda}{2}\\right)

\\]



\\\[

h(n)=2R\\arcsin(\\sqrt a)

\\]



Where:



\- \\(R=6,371,000\\) metres.

\- \\(\\varphi\\) represents latitude.

\- \\(\\lambda\\) represents longitude.



\*\*Why does A\* remain optimal?\*\*



The straight-line distance between two geographical points cannot exceed the length of a road route connecting them.



Therefore, the Haversine heuristic does not overestimate the remaining road distance. It is admissible for this distance-based problem.



This allows A\* to preserve optimality while guiding the search towards the destination.



\### 3. Hill Climbing



\*\*Category:\*\* Local Search



Hill Climbing selects the neighbouring intersection with the smallest estimated distance to the destination.



Unlike UCS and A\*:



\- It does not maintain a priority queue.

\- It does not maintain a global frontier.

\- It does not backtrack.

\- It can become trapped at local minima or dead ends.



If no neighbour offers a better heuristic value, the algorithm stops.



This explains why Hill Climbing performed poorly on the tested road network.



\### Algorithm comparison



| Property | UCS | A\* | Hill Climbing |

|:---|:---|:---|:---|

| Search type | Uninformed | Informed | Local |

| Heuristic | No | Yes | Yes |

| Evaluation | \\(g(n)\\) | \\(g(n)+h(n)\\) | \\(h(n)\\) |

| Priority queue | Yes | Yes | No |

| Backtracking | Yes | Yes | No |

| Complete | Yes\* | Yes\* | No |

| Optimal | Yes\* | Yes\* | No |

| Main advantage | Optimal baseline | Efficiency + optimality | Low search overhead |

| Main limitation | More exploration | Higher memory usage | Local minima |



\*Completeness and optimality assume the standard graph-search conditions, non-negative edge costs, and an admissible heuristic for A\*.\*



\---



\## 🔬 Experimental Methodology



To ensure a fair comparison, all three algorithms were evaluated under identical experimental conditions.



\### Experimental configuration



| Parameter | Configuration |

|:---|:---|

| Geographic area | Dhaka, Bangladesh |

| Network source | OpenStreetMap |

| Network type | Drive network |

| Download centre | 23.7461° N, 90.3742° E |

| Download radius | 3,500 metres |

| Number of pairs | 60 |

| Random seed | 42 |

| Minimum pair separation | 30% of map span |

| Total algorithm executions | 180 |

| Hill Climbing step limit | 20,000 |

| Optimality tolerance | 1 metre |



\### Experimental workflow



```mermaid

flowchart TD

&#x20;   A\["Download Dhaka road network"] --> B\["Construct directed weighted graph"]

&#x20;   B --> C\["Clean and select largest strongly connected component"]

&#x20;   C --> D\["Generate 60 random start-goal pairs"]

&#x20;   D --> E\["Run UCS, A\* and Hill Climbing"]

&#x20;   E --> F\["Record execution metrics"]

&#x20;   F --> G\["Compare route costs and performance"]

&#x20;   G --> H\["Generate tables and visualizations"]

```



\### Fairness and reproducibility



\- Every algorithm uses the same graph.

\- Every algorithm receives the same 60 start-goal pairs.

\- UCS and A\* use the same shared best-first search implementation.

\- The heuristic is the primary difference between UCS and A\*.

\- A fixed random seed ensures repeatable pair selection.

\- The largest strongly connected component is used to ensure directed reachability between selected pairs.



\### Evaluation metrics



We evaluate each algorithm using:



1\. \*\*Success Rate:\*\* Percentage of experiments where the destination is reached.

2\. \*\*Optimality Rate:\*\* Percentage of successful solutions matching the UCS reference cost within the specified tolerance.

3\. \*\*Path Cost:\*\* Total distance travelled in metres.

4\. \*\*Nodes Expanded:\*\* Number of nodes removed from the priority queue or visited during local search.

5\. \*\*Execution Time:\*\* Wall-clock execution time in milliseconds.



\---



\## 🚀 Getting Started



You can reproduce the experiment using Google Colab or run the notebook locally.



\### Option 1 — Google Colab (Recommended)



<p align="center">

&#x20; <a href="https://colab.research.google.com/github/MejbahZidan/AI-Group-11/blob/main/AI\_Lab\_Project.ipynb">

&#x20;   <img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open in Google Colab">

&#x20; </a>

</p>



1\. Open the notebook using the Colab button.

2\. Run the experiment cell.

3\. Allow the road network to load.

4\. Wait for all three algorithms to complete.

5\. Review the generated comparison tables and visualizations.

6\. Download the results ZIP.



\### Option 2 — Run Locally



Clone the repository:



```bash

git clone https://github.com/MejbahZidan/AI-Group-11.git

```



Navigate to the project:



```bash

cd AI-Group-11

```



Install dependencies:



```bash

pip install osmnx networkx numpy pandas matplotlib jupyter

```



Launch the notebook:



```bash

jupyter notebook AI\_Lab\_Project.ipynb

```



\### Data loading behaviour



The notebook follows this order:



1\. Uses `dhaka\_street\_network.graphml` if available.

2\. Otherwise, attempts to download the Dhaka road network using OSMnx.

3\. If OpenStreetMap cannot be reached, uses a synthetic jittered-grid network as a fallback.



> \*\*Note:\*\* Synthetic fallback results are not the same as the real Dhaka road-network results reported in this study.



\---



\## 📁 Project Structure



```text

AI-Group-11/

│

├── AI\_Lab\_Project.ipynb

│

├── assets/

│   ├── fig1\_search\_maps.png

│   └── fig2\_comparison\_dashboard.png

│

├── results/

│   ├── summary.csv

│   ├── all\_runs.csv

│   ├── fig1\_search\_maps.png

│   └── fig2\_comparison\_dashboard.png

│

└── README.md

```



\### Generated outputs



| Output | Description |

|:---|:---|

| `summary.csv` | Aggregated performance statistics |

| `all\_runs.csv` | Individual results from all 180 executions |

| `fig1\_search\_maps.png` | Search exploration visualization |

| `fig2\_comparison\_dashboard.png` | Performance comparison dashboard |



\---



\## ⚠️ Limitations



Although the experiment demonstrates the practical differences between classical search algorithms, several limitations remain:



\- Edge weights represent road distance, not actual travel time.

\- Traffic conditions and congestion are not considered.

\- The experiment covers only one geographical area of Dhaka.

\- Memory consumption was not directly measured.

\- Search depth was not recorded.

\- Hill Climbing was evaluated without random restarts.



\### Future Improvements



\- \[ ] Introduce traffic-aware and travel-time-based edge weights.

\- \[ ] Expand experiments to other cities in Bangladesh.

\- \[ ] Compare Haversine with landmark-based heuristics (ALT).

\- \[ ] Implement Bidirectional A\*.

\- \[ ] Explore Contraction Hierarchies.

\- \[ ] Evaluate Random-Restart Hill Climbing.

\- \[ ] Measure peak memory consumption.

\- \[ ] Record solution depth and path length in number of segments.



\---



\## ❓ Frequently Asked Questions



<details>

<summary><b>Why is A\* better than UCS in this experiment?</b></summary>



A\* uses the Haversine heuristic to estimate the remaining distance to the destination. This guides the search towards promising intersections instead of exploring every direction equally.



As a result, it achieved the same optimal path cost while expanding 71.9% fewer nodes on average.

</details>



<details>

<summary><b>Why did Hill Climbing fail in all 60 experiments?</b></summary>



Hill Climbing only considers the immediate neighbours of its current node. It cannot backtrack when a locally attractive choice leads to a dead end or when reaching the destination requires temporarily moving away from it.



It therefore stopped before reaching the goal in all tested experiments.

</details>



<details>

<summary><b>Why use the largest strongly connected component?</b></summary>



The road network is directed. Selecting the largest strongly connected component ensures that a directed path exists between every pair of nodes within that component.



This helps isolate algorithm performance from connectivity-related failures.

</details>



<details>

<summary><b>Does lower execution time always mean a better algorithm?</b></summary>



No. Hill Climbing had the lowest execution time, but it failed to find a route in every experiment.



An algorithm must be evaluated based on efficiency, solution quality and reliability together.

</details>



<details>

<summary><b>Was memory consumption measured?</b></summary>



No. Nodes expanded were recorded as a search-effort metric, not as a direct measurement of memory usage. UCS and A\* maintain frontiers and explored sets, while Hill Climbing uses a much more limited search structure.

</details>



\---



\## 🔭 Beyond This Experiment



A\* performed best among the three selected algorithms in our experiments.



However, real-world navigation systems can use more advanced techniques.



| Algorithm | Potential advantage |

|:---|:---|

| Greedy Best-First Search | More aggressive heuristic guidance |

| Weighted A\* | Faster search with bounded suboptimality under appropriate conditions |

| Bidirectional A\* | Search from both endpoints |

| ALT | Improved heuristic estimates using landmarks |

| Contraction Hierarchies | Extremely fast road-network queries after preprocessing |

| IDA\* | Lower memory requirements |

| Random-Restart Hill Climbing | Attempts to escape local minima |



\---



\## 👨‍💻 Team Members



This project was developed as part of the Artificial Intelligence Laboratory course at Bangladesh University of Professionals.



| Contributor | Student ID |

|:---|:---|

| Md. Mejbaul Islam Zidan | 23549009069 |

| Md. Hemel Parvej | 23549009005 |

| Most. Nair Nazmim Roja | 2254901013 |



\*\*Academic Supervision\*\*



\- Jasiya Fairiz Raisa — Lecturer, Department of ICT

\- Nazneen Akter — Assistant Professor, Department of ICT



\*\*Institution:\*\* Bangladesh University of Professionals (BUP)  

\*\*Program:\*\* BICE 2023  

\*\*Department:\*\* ICT  

\*\*Course:\*\* Artificial Intelligence Laboratory (ICE-4108)



\---



\## 📚 References



1\. Russell, S. and Norvig, P. (2020). \*Artificial Intelligence: A Modern Approach\*, 4th Edition. Pearson.

2\. Hart, P. E., Nilsson, N. J. and Raphael, B. (1968). A Formal Basis for the Heuristic Determination of Minimum Cost Paths. \*IEEE Transactions on Systems Science and Cybernetics\*, 4(2), 100–107.

3\. Dijkstra, E. W. (1959). A Note on Two Problems in Connexion with Graphs. \*Numerische Mathematik\*, 1, 269–271.

4\. Boeing, G. (2017). OSMnx: New Methods for Acquiring, Constructing, Analyzing, and Visualizing Complex Street Networks. \*Computers, Environment and Urban Systems\*, 65, 126–139.

5\. \[OpenStreetMap Contributors](https://www.openstreetmap.org/).

6\. Hagberg, A., Schult, D. and Swart, P. (2008). Exploring Network Structure, Dynamics, and Function Using NetworkX. \*Proceedings of the 7th Python in Science Conference\*, 11–15.



\---



\## 📖 Citation



If you reference this project in academic work:



```bibtex

@misc{aigroup11\_2026,

&#x20; title = {Comparative Analysis of UCS, A\* and Hill Climbing

&#x20;          for Route Finding on the Dhaka Road Network},

&#x20; author = {Zidan, Md. Mejbaul Islam and

&#x20;           Parvej, Md. Hemel and

&#x20;           Roja, Most. Nair Nazmim},

&#x20; year = {2026},

&#x20; howpublished = {\\url{https://github.com/MejbahZidan/AI-Group-11}},

&#x20; note = {Artificial Intelligence Laboratory, ICE-4108,

&#x20;         Bangladesh University of Professionals}

}

```



\---



\## 📜 License \& Attribution



Road-network data is provided by \[OpenStreetMap contributors](https://www.openstreetmap.org/copyright) under the Open Database License (ODbL).



The original project source code does not currently declare a software license. Until a `LICENSE` file is added, all rights to the original code remain with the authors.



\---



<p align="center">

&#x20; <strong>Built with curiosity, algorithms, and real-world data.</strong>

&#x20; <br><br>

&#x20; Artificial Intelligence Laboratory · ICE-4108

&#x20; <br>

&#x20; Bangladesh University of Professionals · Dhaka, Bangladesh

</p>

