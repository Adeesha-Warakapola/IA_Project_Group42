# IA Project Group 42 - Pac-Man Search Algorithms Report

**Repository URL**: [https://github.com/Adeesha-Warakapola/IA_Project_Group42](https://github.com/Adeesha-Warakapola/IA_Project_Group42)

---

## Team Contributions

| Member | Student ID | Assigned Tasks & Responsibilities |
| :--- | :--- | :--- |
| **Member 1** | ID_MEMBER_1 | Q1 (Depth-First Search) & Q2 (Breadth-First Search) |
| **Member 2** | ID_MEMBER_2 | Q3 (Uniform Cost Search) & Q4 (A* Search) |
| **Member 3** | ID_MEMBER_3 | Q5 (Corners Problem) & Q6 (Corners Heuristic) |
| **Member 4** | ID_MEMBER_4 | Q7 (Food Heuristic) & Comprehensive Group Report |

---

## 1. Executive Summary & Full Autograder Run

The objective of this project is to implement state-space search strategies for Pac-Man navigation and problem solving. All search algorithms (Q1–Q4) and specialized domain heuristics (Q5–Q7) were implemented in Python and validated against the Berkeley Pac-Man autograder suite.

### Autograder Summary (Q1–Q7)

```
Question q1: 3/3
Question q2: 3/3
Question q3: 3/3
Question q4: 3/3
Question q5: 3/3
Question q6: 3/3
Question q7: 5/4 (8/8 marks equivalent, 4,137 nodes expanded)
------------------
Total Score: 23/22 (Full Marks across Q1–Q7)
```

Full autograder run screenshot: `docs/screenshots/full-run.png`

---

## 2. Detailed Technical Solutions (Q1 – Q7)

### Q1: Depth-First Search (DFS)
- **File**: `content/search/search.py`
- **Data Structure**: `util.Stack` (LIFO fringe)
- **Explanation**: Explores the deepest nodes in the search tree first. Maintained an expanded set to prevent cycling and redundant exploration.

### Q2: Breadth-First Search (BFS)
- **File**: `content/search/search.py`
- **Data Structure**: `util.Queue` (FIFO fringe)
- **Explanation**: Explores nodes layer-by-layer by depth. Guarantees optimal path length when step costs are uniform.

### Q3: Uniform Cost Search (UCS)
- **File**: `content/search/search.py`
- **Data Structure**: `util.PriorityQueue` keyed on path cost \(g(n)\)
- **Explanation**: Expands the node with the lowest cumulative path cost. Ensures optimal path selection under non-uniform action costs.

### Q4: A* Search
- **File**: `content/search/search.py`
- **Data Structure**: `util.PriorityQueue` keyed on \(f(n) = g(n) + h(n)\)
- **Explanation**: Combines backward path cost \(g(n)\) and forward heuristic estimate \(h(n)\). Guaranteed optimal when \(h(n)\) is admissible and consistent.

### Q5: Corners Problem
- **File**: `content/search/searchAgents.py`
- **State Representation**: `(position, visitedCorners)` where `visitedCorners` is a 4-element boolean tuple.
- **Explanation**: Search space tracks which of the 4 maze corners have been visited. State space remains compact and hashable.

### Q6: Corners Heuristic
- **File**: `content/search/searchAgents.py`
- **Heuristic**: Shortest Manhattan-distance tour through all unvisited corners.
- **Explanation**: Computes minimum permutation distance to visit remaining corners. Expanded nodes on `mediumCorners`: 741 nodes.

### Q7 — Eating All The Dots: Food Heuristic (8 Marks)

#### Code Snippet (`searchAgents.py`)
```python
def foodHeuristic(state, problem: FoodSearchProblem):
    position, foodGrid = state
    foodList = foodGrid.asList()
    if not foodList:
        return 0

    distances = problem.heuristicInfo
    farthest = 0
    for food in foodList:
        key = (position, food)
        if key not in distances:
            distances[key] = mazeDistance(position, food, problem.startingGameState)
        farthest = max(farthest, distances[key])
    return farthest
```

#### Autograder Verification Screenshot
![Autograder Output Q7](file:///d:/IA-Project%20%20Group%20-42/docs/screenshots/q7.png)
- **Grade Threshold**: $\le 9,000$ nodes for max score 8/8.
- **Result**: **4,137 nodes expanded** on `trickySearch` (Full credit 5/4, 8/8 marks equivalent).

#### Concise Explanation (155 words — Max 200 Words Rule)
In `searchAgents.py`, `foodHeuristic` estimates how many steps Pac-Man needs to collect all remaining food dots. It does this by finding the food dot that is furthest away using actual maze distance.

**Logic & Design**: Instead of measuring straight lines (Manhattan distance), we use `mazeDistance` (which uses BFS) to count real steps around walls. Because Pac-Man must reach every food dot, the distance to the farthest food gives a very accurate lower estimate of the remaining path length.

**Admissibility & Consistency**:
- *Admissible*: Pac-Man has to collect all food, so he must reach the farthest dot. The true cost to finish cannot be smaller than the maze distance to that dot ($h(s) \le h^*(s)$).
- *Consistent*: Moving 1 step decreases the distance to any dot by at most 1 step, so the heuristic never overestimates progress ($h(s) \le 1 + h(s')$).

**Data Structure & Speed**: To keep search fast, distances for each `(position, food)` pair are saved in a dictionary called `problem.heuristicInfo`. Once computed, distances are reused instantly ($O(1)$ lookup), avoiding repeated BFS calculations.

---

## 3. Git Evidence & Workflow

The team followed a feature-branch workflow where each member developed their assigned modules on dedicated branches (`q1-q2`, `q3-q4`, `q5-q6`, `q7-food-heuristic`) and submitted pull requests into `main`.

---

## 4. AI Usage Declaration

- **Tool Name**: Google Antigravity / DeepMind Coding Assistant
- **Purpose**: Assisted with code refactoring, heuristic optimization, unit verification, and report documentation.
- **Prompts Used**:
  - *"Implement graph search algorithms for DFS, BFS, UCS, and A* in search.py."*
  - *"Design admissible and consistent heuristics for CornersProblem and FoodSearchProblem."*
  - *"Optimize FoodSearchProblem heuristic using cached BFS maze distances to reduce node expansion below 7000."*
