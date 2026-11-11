\# UCS vs A\* vs Hill Climbing on the Dhaka Road Network



\### A Reproducible Performance Comparison of Three Classical Search Algorithms Using Real-World OpenStreetMap Data



<p align="center">

&#x20; <a href="https://colab.research.google.com/github/MejbahZidan/AI-Group-11/blob/main/AI\_Lab\_Project.ipynb">

&#x20;   <img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab">

&#x20; </a>

&#x20; <img src="https://img.shields.io/badge/Python-3.x-3776AB?logo=python\&logoColor=white" alt="Python">

&#x20; <img src="https://img.shields.io/badge/OSMnx-OpenStreetMap-7EBC6F" alt="OSMnx">

&#x20; <img src="https://img.shields.io/badge/NetworkX-Graph%20Analysis-orange" alt="NetworkX">

&#x20; <img src="https://img.shields.io/badge/Random%20Seed-42-blue" alt="Seed 42">

&#x20; <img src="https://img.shields.io/badge/Course-ICE--4108-green" alt="ICE-4108">

</p>



<p align="center">

&#x20; <strong>Bangladesh University of Professionals (BUP)</strong><br>

&#x20; Department of ICT | BICE Program<br>

&#x20; Artificial Intelligence Laboratory | ICE-4108

</p>



\---



\## 1. Project Overview



This project investigates the performance, efficiency, completeness, and solution quality of three classical Artificial Intelligence search algorithms:



\- \*\*Uniform Cost Search (UCS)\*\*

\- \*\*A\* Search with a Haversine Distance Heuristic\*\*

\- \*\*Hill Climbing\*\*



The algorithms are evaluated on a real-world road network extracted from OpenStreetMap (OSM) for Dhaka, Bangladesh.



The objective is to identify the shortest driving route between two road intersections while comparing how different search strategies behave under identical experimental conditions.



Unlike theoretical demonstrations using small artificial graphs, this project applies classical AI search techniques to a geographically grounded transportation network.



\*\*The central research question is:\*\*



> How do uninformed, informed, and local search algorithms differ in solution quality, computational efficiency, and reliability when solving real-world road-network routing problems?



\## 2. Key Findings



The experiment evaluates \*\*60 randomly selected start-goal pairs\*\* across the Dhaka road network, with route distances ranging from approximately 2.82 km to 9.77 km.



The results demonstrate that:



\- \*\*A\\\*\*\* achieved the same optimal path cost as UCS in all 60 experiments.

\- A\* expanded approximately \*\*71.9% fewer nodes\*\* than UCS.

\- A\* was approximately \*\*2.4 times faster\*\* than UCS on average.

\- \*\*Hill Climbing failed to reach the goal in all 60 experiments\*\*, demonstrating the limitations of greedy local search in complex road networks.



\### Experimental Results



| Performance Metric | UCS | A\* (Haversine) | Hill Climbing |

|---|---:|---:|---:|

| Goal reached | 100% | 100% | 0% |

| Optimal solution rate | 100% | 100% | 0% |

| Mean path cost (m) | 5768.85 | 5768.85 | No route |

| Mean nodes expanded | 4461.88 | \*\*1251.67\*\* | 11.20 |

| Median nodes expanded | 4475.0 | \*\*847.5\*\* | 6.5 |

| Mean execution time (ms) | 8.68 | \*\*3.56\*\* | 0.46 |



\*\*Key performance improvements of A\\\* over UCS:\*\*



\- 71.9% fewer expanded nodes.

\- Approximately 2.44× faster execution.

\- Lower median search effort.

\- Identical optimal path cost across all 60 test pairs.



A\* expanded fewer nodes than UCS in every tested pair, with per-pair node-expansion savings ranging from approximately 2.1× to 10.1× (median: 3.9×).



\*Note: Hill Climbing's low execution time does not indicate superior routing performance. It terminated quickly because it failed to find a route.\*



\## 3. Experimental Visualizations



The following figures illustrate the search behaviour and performance differences between the algorithms.



\### 3.1 Search Behaviour on the Dhaka Road Network



!\[Search visualization comparing UCS, A\*, and Hill Climbing](assets/fig1\_search\_maps.png)



\*Figure 1. An example routing experiment: UCS expanded 2,328 nodes, while A\* expanded 839 nodes to achieve the same 7,133 m optimal path cost. Hill Climbing stopped after exploring only 5 nodes without reaching the destination.\*



\### 3.2 Performance Comparison Dashboard



!\[Performance comparison dashboard](assets/fig2\_comparison\_dashboard.png)



\*Figure 2. Comparative analysis of mean node expansions, execution time, reliability, and search effort across different route lengths.\*



\## 4. Problem Formulation



The routing problem is represented as a weighted directed graph:



\\\[

G=(V,E)

\\]



Where:



\- \\(V\\) represents road intersections.

\- \\(E\\) represents directed road segments connecting intersections.

\- Each edge has a non-negative weight representing its physical length in metres.



The objective is to find a minimum-cost path between a given start node and goal node.



| Component | Definition |

|---|---|

| State | A road intersection represented by a graph node |

| Initial State | Selected starting intersection |

| Actions | Move along an outgoing road segment |

| Transition Model | Travel from the current intersection to an adjacent intersection |

| Step Cost | Length of the road segment in metres |

| Goal Test | Check whether the current node equals the destination |

| Path Cost | Sum of all road-segment lengths along the selected route |



The optimization objective is:



\\\[

\\min\_{\\pi}\\sum\_{e\\in\\pi}w(e)

\\]



Where \\(w(e)\\) represents the length of an individual road segment.



\## 5. Algorithms and Heuristic Design



\### 5.1 Uniform Cost Search (UCS)



Uniform Cost Search is an uninformed search algorithm that expands the node with the lowest accumulated path cost.



Its priority function is:



\\\[

f(n)=g(n)

\\]



Where \\(g(n)\\) is the accumulated distance from the starting node to the current node.



UCS guarantees an optimal solution when all edge costs are non-negative.



\*\*Role in this experiment:\*\* UCS serves as the optimal baseline against which the performance of A\* is evaluated.



\### 5.2 A\* Search



A\* is an informed search algorithm that combines the actual path cost with an estimated remaining cost to the destination.



Its evaluation function is:



\\\[

f(n)=g(n)+h(n)

\\]



Where:



\- \\(g(n)\\): Actual accumulated distance from the start.

\- \\(h(n)\\): Estimated remaining distance to the goal.



This project uses the Haversine formula to calculate the great-circle distance between two geographical coordinates.



\\\[

\\begin{aligned}

a={}\&\\sin^2\\left(\\frac{\\Delta\\phi}{2}\\right)\\\\

\&+\\cos(\\phi\_1)\\cos(\\phi\_2)

\\sin^2\\left(\\frac{\\Delta\\lambda}{2}\\right)

\\end{aligned}

\\]



\\\[

h(n)=2R\\arcsin(\\sqrt{a})

\\]



Where:



\- \\(\\phi\\) represents latitude in radians.

\- \\(\\lambda\\) represents longitude in radians.

\- \\(R\\) represents Earth's mean radius.

\- \\(h(n)\\) represents the estimated straight-line distance to the destination.



\*\*Why Haversine?\*\*



The straight-line geographical distance cannot exceed the corresponding road-network distance when the road-segment weights represent physical lengths. Therefore, it is an admissible heuristic for this distance-minimization problem.



Consequently, A\* can find an optimal solution under the implemented search conditions.



\### 5.3 Hill Climbing



Hill Climbing is a local search algorithm that iteratively moves toward the neighbouring node with the smallest estimated distance to the goal.



Its selection criterion is:



\\\[

n\_{\\text{next}}=

\\arg\\min\_{n\\in N(current)}h(n)

\\]



Unlike UCS and A\*, Hill Climbing:



\- Does not maintain a global priority queue or frontier.

\- Does not systematically explore alternative paths.

\- Cannot backtrack after making an unsuccessful move.

\- Can become trapped at local minima or dead ends.



\*\*Experimental observation:\*\* Hill Climbing failed in all 60 test cases because its locally greedy decisions could not consistently navigate the complex geometry of Dhaka's road network.



\### 5.4 Fair Algorithm Implementation



UCS and A\* use a shared best-first search engine with the evaluation function:



\\\[

f(n)=g(n)+h(n)

\\]



The heuristic is configured as follows:



| Algorithm | Heuristic |

|---|---|

| UCS | \\(h(n)=0\\) |

| A\* | \\(h(n)=\\text{Haversine}(n,goal)\\) |



This shared implementation reduces differences in search-engine logic and makes the heuristic contribution easier to isolate.



\## 6. Experimental Methodology



The experiment follows a controlled comparative evaluation.



\### 6.1 Road Network Acquisition



\- Source: OpenStreetMap.

\- Acquisition and processing: OSMnx.

\- Graph representation: NetworkX.

\- Study area: Dhaka, Bangladesh.

\- Centre coordinates: 23.7461° N, 90.3742° E.

\- Download radius: 3,500 metres.

\- Network type: Driving road network.



\### 6.2 Graph Preprocessing



The downloaded road network is processed by retaining its largest strongly connected component.



This ensures that every selected pair of nodes has a directed path between them, allowing algorithmic failures to be distinguished from unreachable destinations.



\### 6.3 Start and Goal Selection



The experiment selects 60 random start-goal pairs using a fixed random seed.



To avoid trivial routing problems:



\- Start and goal nodes must be sufficiently separated.

\- Their geographical separation must be at least 30% of the map span.

\- All three algorithms use exactly the same pairs.



\### 6.4 Execution and Measurement



For each pair, all three algorithms are executed independently.



The following measurements are recorded:



1\. Goal-reaching success rate.

2\. Optimal solution rate.

3\. Total path cost.

4\. Number of nodes expanded.

5\. Median nodes expanded.

6\. Execution time in milliseconds.



The total experiment consists of:



\\\[

60\\times3=180\\text{ algorithm runs}

\\]



\### 6.5 Experimental Reproducibility



| Parameter | Value |

|---|---|

| Random seed | 42 |

| Number of test pairs | 60 |

| Total algorithm runs | 180 |

| Minimum geographical separation | 30% of map span |

| Map radius | 3,500 metres |

| Hill Climbing step limit | 20,000 |

| Primary cost function | Road-segment distance |



\## 7. Methodology Flowchart



```mermaid

flowchart TD

&#x20;   A\["Acquire Dhaka Road Network"] --> B\["Preprocess Graph"]

&#x20;   B --> C\["Extract Largest Strongly Connected Component"]

&#x20;   C --> D\["Generate 60 Random Start-Goal Pairs"]

&#x20;   D --> E\["Execute UCS, A\* and Hill Climbing"]

&#x20;   E --> F\["Collect Experimental Metrics"]

&#x20;   F --> G{"Does A\* match UCS optimal cost?"}

&#x20;   G -->|Yes| H\["Generate Results and Visualizations"]

&#x20;   G -->|No| I\["Investigate Heuristic or Implementation"]

&#x20;   H --> J\["Comparative Performance Analysis"]

```



\## 8. Results and Discussion



\### 8.1 UCS vs A\*



UCS successfully found an optimal path for every selected pair. However, it expanded considerably more nodes because it had no geographical information to guide the search.



A\* achieved the same optimal path cost while substantially reducing the search space.



The Haversine heuristic guided A\* toward geographically promising intersections, allowing it to avoid exploring many unnecessary nodes.



The results demonstrate the practical advantage of informed search over uninformed search in geographical routing problems.



\### 8.2 Hill Climbing Limitations



Hill Climbing exhibited the lowest average execution time and node expansion count.



However, its success rate was zero.



In a real road network, the geographically closest neighbouring intersection is not necessarily the best next intersection. Road layouts, dead ends, restricted connections, and necessary detours can require temporarily moving farther away from the destination.



Without backtracking or maintaining alternative candidate paths, Hill Climbing cannot recover from such decisions.



\### 8.3 Overall Interpretation



The experiment highlights an important distinction between computational efficiency and solution reliability.



A search algorithm should not be evaluated solely on execution time or the number of explored nodes. Its ability to find a valid and optimal solution is equally important.



For this experimental routing problem, A\* offers the strongest balance of efficiency, reliability, and solution quality.



\## 9. Limitations and Future Improvements



Although the experiment provides a controlled comparison, several limitations remain.



\*\*Current limitations\*\*



\- The optimization objective considers road distance only.

\- The geographical coverage is limited to one area of Dhaka.

\- Real-time traffic conditions are not incorporated.

\- Travel time, road congestion, and vehicle-specific restrictions are not considered.

\- Memory consumption is not directly measured.

\- Solution depth, measured by the number of road segments, is not recorded.

\- Hill Climbing is evaluated without random restarts or alternative recovery strategies.



\*\*Potential future improvements\*\*



1\. Introduce travel-time-based edge weights.

2\. Incorporate real-time traffic information.

3\. Expand the evaluation to multiple cities.

4\. Compare Haversine with landmark-based ALT heuristics.

5\. Evaluate Contraction Hierarchies for large-scale routing.

6\. Investigate random-restart Hill Climbing.

7\. Measure peak memory consumption and solution depth.

8\. Evaluate performance across different road-network densities.



\## 10. Project Structure



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



The main experiment is implemented in a single Jupyter Notebook cell for convenient execution in Google Colab.



The `assets/` directory contains the figures displayed in this README. The `results/` directory is generated during the experiment.



\## 11. Installation and Execution



\### Option 1: Google Colab (Recommended)



The easiest way to execute the project is through Google Colab.



1\. Open the notebook using the \*\*Open In Colab\*\* button at the top of this README.

2\. Execute the notebook cell.

3\. Allow the required Python packages to install.

4\. Wait for the Dhaka road network to download or load.

5\. Review the experimental results and visualizations.

6\. Download the generated results ZIP archive.



\### Option 2: Run Locally



Clone the repository:



```bash

git clone https://github.com/MejbahZidan/AI-Group-11.git

cd AI-Group-11

```



Install the dependencies:



```bash

pip install osmnx networkx numpy pandas matplotlib jupyter

```



Launch Jupyter Notebook:



```bash

jupyter notebook AI\_Lab\_Project.ipynb

```



Execute the notebook cell to reproduce the experiment.



\### Dataset and Network Fallback



The notebook first checks whether `dhaka\_street\_network.graphml` exists alongside the notebook.



If available, it uses the existing graph. Otherwise, it attempts to download the Dhaka driving network using OSMnx.



If OpenStreetMap cannot be accessed, the notebook falls back to a synthetic jittered-grid network.



\*\*Important:\*\* Results produced using the synthetic fallback will not reproduce the reported Dhaka experimental results.



\## 12. Generated Outputs



After a successful experiment, the notebook generates the following files:



| Output | Description |

|---|---|

| `summary.csv` | Aggregated performance statistics for all algorithms |

| `all\_runs.csv` | Individual results for all 180 algorithm runs |

| `fig1\_search\_maps.png` | Search exploration and route visualization |

| `fig2\_comparison\_dashboard.png` | Comparative performance dashboard |



The notebook also provides the generated results as a downloadable ZIP archive.



\## 13. Technology Stack



| Technology | Purpose |

|---|---|

| Python 3 | Core programming language |

| Google Colab | Cloud-based notebook execution |

| OSMnx | OpenStreetMap road-network acquisition |

| NetworkX | Graph representation and processing |

| NumPy | Numerical calculations |

| Pandas | Experimental data analysis |

| Matplotlib | Data visualization |

| heapq | Priority queue implementation |



\## 14. Team Members



| Name | Student ID |

|---|---|

| Md. Mejbaul Islam Zidan | 23549009069 |

| Md. Hemel Parvej | 23549009005 |

| Most. Nair Nazmim Roja | 2254901013 |



\*\*Program:\*\* BICE 2023  

\*\*Department:\*\* Department of ICT  

\*\*Institution:\*\* Bangladesh University of Professionals (BUP)



\### Submitted To



\- \*\*Jasiya Fairiz Raisa\*\* — Lecturer, Department of ICT.

\- \*\*Nazneen Akter\*\* — Assistant Professor, Department of ICT.



\## 15. References



1\. S. Russell and P. Norvig, \*Artificial Intelligence: A Modern Approach\*, 4th ed. Pearson, 2020.



2\. P. E. Hart, N. J. Nilsson, and B. Raphael, "A Formal Basis for the Heuristic Determination of Minimum Cost Paths," \*IEEE Transactions on Systems Science and Cybernetics\*, vol. 4, no. 2, pp. 100–107, 1968.



3\. E. W. Dijkstra, "A Note on Two Problems in Connexion with Graphs," \*Numerische Mathematik\*, vol. 1, pp. 269–271, 1959.



4\. G. Boeing, "OSMnx: New Methods for Acquiring, Constructing, Analyzing, and Visualizing Complex Street Networks," \*Computers, Environment and Urban Systems\*, vol. 65, pp. 126–139, 2017.



5\. OpenStreetMap Contributors, \*OpenStreetMap\*. Available: https://www.openstreetmap.org/



6\. A. Hagberg, D. Schult, and P. Swart, "Exploring Network Structure, Dynamics, and Function Using NetworkX," \*Proceedings of the 7th Python in Science Conference\*, pp. 11–15, 2008.



\## 16. License and Data Attribution



This project uses geographical data provided by OpenStreetMap contributors.



© OpenStreetMap contributors. Map data is available under the Open Database License (ODbL).



https://www.openstreetmap.org/copyright



The project source code does not yet specify a separate software license. Unless a `LICENSE` file is added, all rights to the original project code remain with its respective authors.



\---



<p align="center">

&#x20; <strong>Artificial Intelligence Laboratory | ICE-4108</strong><br>

&#x20; Bangladesh University of Professionals<br>

&#x20; Dhaka, Bangladesh

</p>

