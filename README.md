\# UCS vs A\* vs Hill Climbing

\### A Comparative Study of Classical AI Search Algorithms on the Dhaka Road Network



<p align="center">

&#x20; <a href="https://colab.research.google.com/github/MejbahZidan/AI-Group-11/blob/main/AI\_Lab\_Project.ipynb">

&#x20;   <img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open in Google Colab">

&#x20; </a>

&#x20; <img src="https://img.shields.io/badge/Python-3.x-3776AB?logo=python\&logoColor=white">

&#x20; <img src="https://img.shields.io/badge/OSMnx-OpenStreetMap-7EBC6F">

&#x20; <img src="https://img.shields.io/badge/NetworkX-Graph%20Analysis-orange">

&#x20; <img src="https://img.shields.io/badge/Seed-42-blue">

&#x20; <img src="https://img.shields.io/badge/Course-ICE--4108-green">

</p>



<p align="center">

&#x20; <strong>Bangladesh University of Professionals</strong><br>

&#x20; Department of ICT · BICE Program<br>

&#x20; Artificial Intelligence Laboratory · ICE-4108

</p>



\---



\## Overview



How efficiently can classical Artificial Intelligence search algorithms navigate a real-world road network?



This project presents a comparative experimental evaluation of three fundamental search strategies: \*\*Uniform Cost Search (UCS), A\* Search, and Hill Climbing\*\*.



Using real geographical road-network data from OpenStreetMap, we investigate their ability to find optimal routes between road intersections in Dhaka, Bangladesh.



Rather than evaluating these algorithms on small theoretical graphs, this experiment applies them to a geographically grounded transportation network, providing a practical comparison of search efficiency, solution quality, and reliability.



\*\*Research objective:\*\* To investigate how uninformed, informed, and local search strategies perform under identical routing conditions.



\## Experimental Highlights



<p align="center">

&#x20; <img src="https://img.shields.io/badge/Test%20Pairs-60-2563EB?style=for-the-badge">

&#x20; <img src="https://img.shields.io/badge/Algorithm%20Runs-180-7C3AED?style=for-the-badge">

&#x20; <img src="https://img.shields.io/badge/Random%20Seed-42-059669?style=for-the-badge">

</p>



\### The key findings



\- \*\*A\* matched UCS's optimal path cost in all 60 experiments.\*\*

\- A\* expanded approximately \*\*71.9% fewer nodes\*\* than UCS.

\- A\* achieved approximately \*\*2.44× faster execution\*\* on average.

\- Hill Climbing reached the destination in \*\*0 out of 60 experiments\*\*.



\### Performance Benchmark



\*\*Uniform Cost Search (UCS)\*\*



<details>

<summary>View UCS performance</summary>



\- Goal-reaching rate: \*\*100%\*\*

\- Optimal solution rate: \*\*100%\*\*

\- Mean path cost: 5,768.85 m

\- Mean nodes expanded: 4,461.88

\- Median nodes expanded: 4,475

\- Mean execution time: 8.68 ms



</details>



\*\*A\* Search (Haversine Heuristic)\*\*



<details>

<summary>View A\* performance</summary>



\- Goal-reaching rate: \*\*100%\*\*

\- Optimal solution rate: \*\*100%\*\*

\- Mean path cost: 5,768.85 m

\- Mean nodes expanded: 1,251.67

\- Median nodes expanded: 847.5

\- Mean execution time: 3.56 ms



</details>



\*\*Hill Climbing\*\*



<details>

<summary>View Hill Climbing performance</summary>



\- Goal-reaching rate: \*\*0%\*\*

\- Successful optimal solutions: \*\*0\*\*

\- Mean path cost: No valid route found

\- Mean nodes expanded: 11.2

\- Median nodes expanded: 6.5

\- Mean execution time: 0.46 ms



</details>



<blockquote>

&#x20; <strong>Principal finding:</strong> A\* provided the best balance of computational efficiency, reliability, and optimality. Hill Climbing consumed fewer computational resources but failed to produce a valid route in every experiment.

</blockquote>



\---



\## Visual Analysis



\### 01 — Search Exploration on the Dhaka Road Network



!\[UCS vs A\* vs Hill Climbing search visualization](assets/fig1\_search\_maps.png)



<p align="center">

&#x20; <em>Figure 1. Geographical visualization of the explored search nodes and selected routes for an example start-goal pair.</em>

</p>



For the illustrated 7,133 m route:



\- UCS expanded 2,328 nodes.

\- A\* expanded 839 nodes.

\- Hill Climbing terminated after exploring 5 nodes without reaching the destination.



\### 02 — Comparative Performance Dashboard



!\[Algorithm performance comparison dashboard](assets/fig2\_comparison\_dashboard.png)



<p align="center">

&#x20; <em>Figure 2. Comparison of node expansion, execution time, reliability, and search effort across the evaluated algorithms.</em>

</p>



\---



\## Problem Formulation



The Dhaka road network is represented as a weighted directed graph:



\\\[

G=(V,E)

\\]



Where:



\- \\(V\\) denotes road intersections.

\- \\(E\\) denotes directed road segments.

\- \\(w(e)\\) represents the physical length of a road segment in metres.



The objective is to determine the minimum-distance path between a selected starting intersection and a destination.



\*\*Search environment\*\*



\- State: Current road intersection.

\- Initial state: Selected starting intersection.

\- Action: Traverse an outgoing road segment.

\- Transition: Move to an adjacent intersection.

\- Step cost: Road-segment length.

\- Goal test: Current intersection equals the destination.

\- Path cost: Total distance travelled.



The optimization objective is:



\\\[

\\pi^\*=\\arg\\min\_{\\pi}\\sum\_{e\\in\\pi}w(e)

\\]



\---



\## Search Algorithms



\### 01. Uniform Cost Search (UCS)



UCS is an uninformed search algorithm that expands the node with the lowest accumulated path cost.



\\\[

f(n)=g(n)

\\]



It guarantees an optimal solution when edge costs are non-negative.



In this experiment, UCS serves as the optimal baseline for evaluating A\*.



\### 02. A\* Search



A\* combines the accumulated path cost with a heuristic estimate of the remaining distance.



\\\[

f(n)=g(n)+h(n)

\\]



This project uses the \*\*Haversine distance\*\* as its geographical heuristic.



The Haversine formula estimates the great-circle distance between two geographical coordinates:



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



Here, \\(R\\) represents Earth's mean radius, while latitude and longitude are expressed in radians.



Since straight-line geographical distance cannot exceed the corresponding road-network distance under the experiment's distance-based cost model, the heuristic is admissible.



This allows A\* to preserve optimality while exploring fewer nodes than UCS.



\### 03. Hill Climbing



Hill Climbing is a local search algorithm that selects the neighbouring node with the smallest estimated geographical distance to the goal.



\\\[

n\_{\\text{next}}=

\\arg\\min\_{n\\in N(current)}h(n)

\\]



Unlike UCS and A\*, it does not maintain a global frontier or backtrack.



This makes it vulnerable to local minima, dead ends, and road-network detours.



The experiment demonstrates this limitation clearly: Hill Climbing failed to reach the destination in all 60 test cases.



\### Implementation Consistency



UCS and A\* share a common best-first search engine. Only the heuristic function changes:



\- UCS: \\(h(n)=0\\)

\- A\*: \\(h(n)=\\text{Haversine}(n,goal)\\)



This provides a consistent implementation framework for isolating the effect of heuristic guidance.



\---



\## Experimental Methodology



The experiment follows a controlled evaluation pipeline.



```mermaid

flowchart TD

&#x20;   A\["Acquire Dhaka Road Network"] --> B\["Graph Preprocessing"]

&#x20;   B --> C\["Largest Strongly Connected Component"]

&#x20;   C --> D\["Generate 60 Random Node Pairs"]

&#x20;   D --> E\["Execute UCS, A\* and Hill Climbing"]

&#x20;   E --> F\["Collect Performance Metrics"]

&#x20;   F --> G{"A\* Matches UCS Optimal Cost?"}

&#x20;   G -->|Yes| H\["Generate Results and Visualizations"]

&#x20;   G -->|No| I\["Investigate Heuristic or Implementation"]

&#x20;   H --> J\["Comparative Analysis"]

```



\### Road Network Acquisition



The road network is obtained from OpenStreetMap using OSMnx.



\*\*Geographical configuration\*\*



\- Study area: Dhaka, Bangladesh

\- Centre: 23.7461° N, 90.3742° E

\- Download radius: 3,500 metres

\- Network type: Driving



\### Graph Preprocessing



The downloaded network is converted into a directed graph and restricted to its largest strongly connected component.



This ensures that every selected pair has a directed path available in the underlying graph.



\### Test Pair Generation



Sixty random start-goal pairs are generated using a fixed random seed of 42.



To prevent trivial routing scenarios, the geographical separation between the selected nodes must be at least 30% of the map span.



Every algorithm is evaluated using exactly the same test pairs.



\### Performance Measurement



Each algorithm is independently evaluated using:



\- Goal-reaching success rate

\- Optimal solution rate

\- Total path cost

\- Number of expanded nodes

\- Median node expansions

\- Execution time in milliseconds



The total experimental workload is:



\\\[

60\\text{ pairs}\\times3\\text{ algorithms}=180\\text{ runs}

\\]



\### Reproducibility Configuration



| Configuration | Value |

|---|---|

| Random seed | 42 |

| Test pairs | 60 |

| Total runs | 180 |

| Map radius | 3,500 m |

| Minimum pair separation | 30% of map span |

| Hill Climbing step limit | 20,000 |

| Optimization metric | Road distance |



\---



\## Results and Discussion



\### A\* vs UCS: The Advantage of Heuristic Guidance



UCS successfully identified optimal paths for every test pair. However, without geographical guidance, it explored substantially more nodes.



A\* used the Haversine heuristic to prioritize geographically promising intersections. Consequently, it reduced the search effort while preserving the optimal path cost.



The results show that heuristic information can significantly improve the practical efficiency of graph search without sacrificing solution quality.



\### Why Hill Climbing Failed



Dhaka's road network does not always provide a geographically direct path toward the destination.



A locally attractive move can lead to a dead end or an intersection from which reaching the destination requires temporarily moving farther away.



Because Hill Climbing does not maintain alternative candidate paths or backtrack, it cannot recover from such decisions.



Its low execution time therefore reflects early termination rather than successful optimization.



\### Overall Assessment



The experiment demonstrates that computational efficiency alone is insufficient to evaluate a search algorithm.



A useful routing algorithm must balance:



1\. Solution quality.

2\. Computational efficiency.

3\. Completeness and reliability.



Among the three evaluated approaches, A\* provides the strongest overall performance for this experimental routing problem.



\---



\## Limitations and Future Work



\### Current Limitations



\- The optimization considers road distance rather than actual travel time.

\- The geographical coverage is limited to one area of Dhaka.

\- Real-time traffic and congestion are not considered.

\- Vehicle-specific restrictions are not incorporated.

\- Peak memory consumption is not directly measured.

\- Solution depth, represented by the number of road segments, is not recorded.

\- Hill Climbing is evaluated without random restarts or recovery mechanisms.



\### Future Research Directions



Potential extensions include:



\- Integrating traffic-aware and travel-time-based edge weights.

\- Expanding the evaluation to additional geographical regions.

\- Investigating landmark-based ALT heuristics.

\- Comparing A\* with Contraction Hierarchies.

\- Evaluating random-restart Hill Climbing.

\- Measuring memory consumption and solution depth.

\- Investigating routing performance under different road-network densities.



\---



\## Repository Structure



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



The primary experiment is implemented in a single Jupyter Notebook cell for convenient execution and reproducibility.



\---



\## Installation and Execution



\### Google Colab (Recommended)



The project can be executed directly through Google Colab without configuring a local Python environment.



1\. Open the notebook using the Colab badge at the top of this repository.

2\. Execute the notebook cell.

3\. Allow the required dependencies to install.

4\. Wait for the road network to load or download.

5\. Review the experimental results and generated visualizations.



\### Local Environment



Clone the repository:



```bash

git clone https://github.com/MejbahZidan/AI-Group-11.git

cd AI-Group-11

```



Install the required dependencies:



```bash

pip install osmnx networkx numpy pandas matplotlib jupyter

```



Launch the notebook:



```bash

jupyter notebook AI\_Lab\_Project.ipynb

```



\### Network Availability



The notebook first checks for an existing `dhaka\_street\_network.graphml` file in the project directory.



If the file is unavailable, it attempts to retrieve the Dhaka road network through OSMnx.



If OpenStreetMap is inaccessible, a synthetic jittered-grid network is used as a fallback.



\*\*Note:\*\* Synthetic fallback results are not equivalent to the reported experiments conducted on real Dhaka road-network data.



\---



\## Generated Outputs



A successful experiment produces the following artifacts:



\*\*summary.csv\*\* — Aggregated performance statistics for the three algorithms.



\*\*all\_runs.csv\*\* — Individual experimental records for all 180 runs.



\*\*fig1\_search\_maps.png\*\* — Geographical search exploration and route visualization.



\*\*fig2\_comparison\_dashboard.png\*\* — Multi-panel performance comparison.



The notebook also provides the generated results in a downloadable ZIP archive.



\---



\## Technology Stack



<p align="center">

&#x20; <img src="https://img.shields.io/badge/Python-3776AB?style=flat-square\&logo=python\&logoColor=white">

&#x20; <img src="https://img.shields.io/badge/Google%20Colab-F9AB00?style=flat-square\&logo=googlecolab\&logoColor=white">

&#x20; <img src="https://img.shields.io/badge/OSMnx-7EBC6F?style=flat-square">

&#x20; <img src="https://img.shields.io/badge/NetworkX-FF8C00?style=flat-square">

&#x20; <img src="https://img.shields.io/badge/NumPy-013243?style=flat-square\&logo=numpy\&logoColor=white">

&#x20; <img src="https://img.shields.io/badge/Pandas-150458?style=flat-square\&logo=pandas\&logoColor=white">

&#x20; <img src="https://img.shields.io/badge/Matplotlib-11557C?style=flat-square">

</p>



\---



\## Project Team



\*\*Bangladesh University of Professionals (BUP)\*\*  

Department of ICT · BICE 2023



| Contributor | Student ID |

|---|---|

| Md. Mejbaul Islam Zidan | 23549009069 |

| Md. Hemel Parvej | 23549009005 |

| Most. Nair Nazmim Roja | 2254901013 |



\*\*Academic Supervision\*\*



\- Jasiya Fairiz Raisa — Lecturer, Department of ICT.

\- Nazneen Akter — Assistant Professor, Department of ICT.



\*\*Course:\*\* Artificial Intelligence Laboratory (ICE-4108)



\---



\## References



1\. S. Russell and P. Norvig, \*Artificial Intelligence: A Modern Approach\*, 4th ed. Pearson, 2020.



2\. P. E. Hart, N. J. Nilsson, and B. Raphael, "A Formal Basis for the Heuristic Determination of Minimum Cost Paths," \*IEEE Transactions on Systems Science and Cybernetics\*, vol. 4, no. 2, pp. 100–107, 1968.



3\. E. W. Dijkstra, "A Note on Two Problems in Connexion with Graphs," \*Numerische Mathematik\*, vol. 1, pp. 269–271, 1959.



4\. G. Boeing, "OSMnx: New Methods for Acquiring, Constructing, Analyzing, and Visualizing Complex Street Networks," \*Computers, Environment and Urban Systems\*, vol. 65, pp. 126–139, 2017.



5\. OpenStreetMap Contributors. \[OpenStreetMap](https://www.openstreetmap.org/).



6\. A. Hagberg, D. Schult, and P. Swart, "Exploring Network Structure, Dynamics, and Function Using NetworkX," \*Proceedings of the 7th Python in Science Conference\*, pp. 11–15, 2008.



\---



\## License and Attribution



Geographical data used in this project is provided by OpenStreetMap contributors and is available under the Open Database License (ODbL).



\[OpenStreetMap Copyright and Attribution](https://www.openstreetmap.org/copyright)



The project source code does not currently declare a separate software license. All rights to the original code remain with their respective authors unless a license is added.



\---



<p align="center">

&#x20; <strong>Artificial Intelligence Laboratory · ICE-4108</strong><br>

&#x20; Bangladesh University of Professionals<br>

&#x20; Dhaka, Bangladesh

</p>

