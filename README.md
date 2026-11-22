# Sheep-Wolves-Semantic-Nets-Agent-

# Agent Overview
A Python agent that solves the Sheep and Wolves river-crossing game for an arbitrary number of sheep and wolves. This agent was built for a mini-project for CS 7637: Knowledge-Based Artificial Intelligence at Georgia Tech.

> **Note:** The source code is not included in this repository due to university academic-integrity restrictions. This README documents the problem, my approach, and what I learned.

## The Problem
A shepherd must ferry sheep and wolves from the left bank of a river to the right bank.
 
- The boat carries one or two animals at a time and cannot cross empty.
- On either bank, wolves must never outnumber sheep, unless there are no sheep on that bank.
- The agent receives the starting counts of sheep and wolves (sheep >= wolves) and returns a list of moves, or an empty list if the puzzle is unsolvable.
Each move is a tuple `(sheep, wolves)` describing how many of each animal cross in the boat. Moves alternate direction: left to right, then right to left, and so on.
 
**Example (3 sheep, 3 wolves)** has a classic 11-move solution:
 
```
[(1, 1), (1, 0), (0, 2), (0, 1), (2, 0), (1, 1), (2, 0), (0, 1), (0, 2), (0, 1), (0, 2)]
```
## Solving Approach
This game is treated as a state-space search problem, using the semantic-network representation taught in the course.

### State representation
 
A state is a triple:
 
```
(sheep on left bank, wolves on left bank, boat side)
```
 
The right bank is derived from the totals, so it never needs to be stored. The start state is `(S, W, left)` and the goal state is `(0, 0, right)`.
 
### State generation
 
From any state, the agent looks at the animalsavailable on the boat's current bank and then generates every legal boat load of 1 or 2 animals (with at most 5 combinations: (1,0), (0,1), (1,1), (2,0), (0,2)). Each load produces a successor state with the boat on the opposite bank.
 
### State testing
 
A successor is kept only if it passes a validity check:
 
1. No animal count is negative.
2. On each bank, either there are no sheep, or sheep >= wolves.
3. 
### Search strategy used: Breadth-First Search
 
I used the BFS algorithm with:
 
- a queue of states still to explore,
- a visited set to avoid revisiting states and looping,
- parent and move records so the path can be reconstructed by walking back from the goal.
Because BFS expands states level by level, the first time it reaches the goal it has found a solution with the minimum number of moves. That gives optimal solutions for every solvable input. If the queue empties without reaching the goal, the puzzle is unsolvable and the agent returns `[]`.
 
## Agent Efficiency
 
- The number of distinct states is at most about 2 x (S+1) x (W+1), and each state has at most 5 successors.
- Since we have the visited set, the search is roughly polynomial in the number of animals, not exponential, since no state is expanded twice.
- This stays fast even for the largest cases in the autograder (up to 25 sheep and 25 wolves).
  
## Performance and Edge Cases
 
- All the solvable cases are solved correctly and optimally, by the BFS guarantee.
- The unsolvable cases are correctly detected by exhausting the reachable state space. A boat capacity of two is a real constraint: for example, the classic puzzle becomes impossible once the number of sheep and wolves grow equal and large enough.
- Invalid starts (more wolves than sheep) return an empty list.
- 

## Course Details
 
| Item | Detail |
|---|---|
| Course | CS 7637, Knowledge-Based AI (Georgia Tech) |
| Assignment | Mini-Project 1: Sheep & Wolves |
| Concepts applied | Semantic nets, state spaces, generate-and-test, BFS |
| Language | Python 3 (standard library only, using collections.deque) |
 

 
*Author: [Shruti Jayavardhanan]*
