# Vehicle Routing Problem with Parcel Lockers (VRPL)

Optimization of a last-mile delivery system that combines **home delivery** and **parcel locker pickup**, developed for the *Operations Research 2* course at the University of Naples Federico II.

## Problem
A logistics operator must serve a set of customers from a central depot with a capacitated fleet. Each customer can either receive the parcel at home or pick it up at a nearby locker (within a maximum admissibility radius). The goal is to balance service level and operating cost by deciding:
- which lockers to activate,
- which customers to assign to each locker,
- the vehicle routes.

## Mathematical model
MILP formulation based on the **One-Commodity Flow (OCF)** approach, which handles in a single model:
- subtour elimination,
- vehicle capacity (Q) and locker capacity (v_b),
- customer–locker assignment within radius R_b.

Objective function: `min Z = α1 · Σ d_ij x_ij + α2 · Σ c_ib y_ib`
(transport cost + customer access cost, with α1 = 1 and α2 = 0.1)

## Solution approaches
| Approach | Description |
|---|---|
| **Exact model** | Full MILP solved with Gurobi |
| **Matheuristic (Reduced Formulation)** | Heuristic selection of the 8 most promising lockers (coverage + spatial diversity), greedy customer assignment, exact VRP solved with Gurobi |
| **Pure greedy heuristic** | Same assignment logic + Nearest Neighbor routing constrained by residual vehicle capacity |

## Repository structure
| File | Content |
|---|---|
| `istanzapiccola.ipynb` | Exact MILP model on the small instance (5 customers, 2 lockers) |
| `istanzagrande.ipynb` | Exact MILP model on the large instance (100 customers, 12 candidate lockers, 10 vehicles) |
| `euristica1.ipynb` | Matheuristic (Reduced Formulation) |
| `euristica2.ipynb` | Pure greedy heuristic (Nearest Neighbor) |

## Results (large instance)
| Approach | Objective | Time | Gap |
|---|---|---|---|
| Exact model | 2162.93 | 600 s (time limit) | 41.2% |
| Matheuristic | 1821.50 | ~25 s | 0% (reduced problem) |
| Greedy | 2127.77 | < 1 s | n/a |

- The matheuristic improves on the best exact solution by about **16%** while cutting computation time by **96%**.
- With 150 customers, the exact model finds no feasible solution within 3600 s, which confirms the need for heuristic methods.
- In the final solution, 79 customers out of 100 are served via 8 active lockers.

## How to run
1. Install Python with Jupyter (e.g. via Anaconda) and the Gurobi solver: `pip install gurobipy`
2. A Gurobi license is required for large instances (free academic licenses are available at gurobi.com).
3. Open the notebooks in Jupyter and run all cells.

## Tech stack
Python · Gurobi (gurobipy) · Matplotlib · Jupyter Notebook

## Author
Fabiana D'Alisa, MSc student in Management Engineering, University of Naples Federico II
