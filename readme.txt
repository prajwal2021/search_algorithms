Basic Questions:
1.	What is your implementation strategy for DFS? Explain.
My implementation strategy for DFS uses a util.Stack as its fringe, which uses LIFO principle and explores as deeply as possible before backtracking. The algorithm keeps track of visited states using a set datatype in python this is used to prevent any cycles or redundancy. Each item is pushed onto the stack is a tuple(state,path) where state is the current node and path is the list of actions taken to reach that state from the start. When a state is popped from the stack it is checked if it is a goal state else it checks if it hasn't been visited it will mark it as visited and its successors are pushed onto the stack along with their paths. This continues until a goal state is found or stack becomes empty.
2.	What is your implementation strategy for BFS? Explain.
The implementation strategy I used for BFS is similar to DFS with the code perspective but in BFS I'm using util.Queue as its fringe which follows FIFO concept, here in BFS the algorithm explores all nodes at the current depth level before moving to the nodes at the next depth which delivers the shortest path in terms of actions taken. As I mentioned earlier similar to DFS set is used to keep track of visited states to avoid redundancy and each item is pushed onto the queue is a tuple(state,path), when any state is popped it is checked against goal state. If it is not the goal it will be marked as visited if it hasn't been visited and successors and their paths are added to the queue and this process continues until goal is reached or queue is empty.
3.	What is your implementation strategy for UFS? Explain.
For the UCS strategy I used a util.PriorityQueue as its fringe priority queue data structure prioritizes states based on their accumulated cost g(n) which is always the least cost path to the goal. Each item stored in the priority queue is a tuple(state,path,cost), where cost is the total cost to reach this state. To handle the revisiting of nodes with a cheaper path a dictionary best_costs stores the minimum cost found so far. If the state is popped and its cost is not less than the previously recorded cost, this means cheaper path to this state is already been processed so it skips else it is updated and all successors are pushed onto the priority queue with their updated costs and paths using total_cost as the priority.

4.	What is your implementation strategy for A*? Explain.
My implementation strategy of A* has the same foundation of UCS but with the added heuristic function. It also uses priority queue as the fringe, but the priority for each state is determined by f(n) = g(n) + h(n), g(n) is the actual cost from the start node to the current node and h(n) is the estimated cost from the current node to the goal provided by the heuristic function. This allows the A* to find the optimal path by prioritizing nodes that are both cheap and close to the goal. The items in the priority queue are (state, path, g_cost), dictionary best_costs is used to store the minimum cost. Heuristic function is passed as an argument to aStarSearch and is used to calculate h(n) for each next_state before putting it into the fringe.
5.	Explain your implementation strategy for the CornersProblem. What is your state representation?
My strategy focuses on finding the shortest path to visit all four corners of the maze. The main aspect of this problem is to capture both pacman's current location and visitation status of the corners. To achieve this my state representation is a tuple: (current_pos, visited_corners).
current_pos: this is a tuple(x,y) which represents the pacman's current coordinates.
visited_corners: this is a tuple of Booleans eg(False, True, False, False) where each Boolean corresponds to corner and indicates whether that corner has been visited or not.
The getStartState method initializes the pacman starting position with all corners marked as false. The isGoalState method checks if all Booleans in visited_corners are true.
The getSuccessors method for each legal move calculates the new_pos and updates the visited_corners if new_pos coincides with an unvisited corner, this approach ensures that the search algorithm tracks progress of visiting all corners.

6.	What is your heuristics for the CornersProblem? How did you implement it?
For the CornersProblem, I implemented a heuristic that calculates the maximum maze distance from Pacman's current position to any unvisited corner. Here's how I approached it:

• **Main idea**: Since we need to visit all corners, the minimum cost will be at least as much as the distance to the farthest unvisited corner
• **Implementation steps**:
  - First, I check which corners are still unvisited by looking at the visited tuple
  - For each unvisited corner, I calculate the maze distance from current position using mazeDistance function
  - I take the maximum of all these distances as my heuristic value
• **Why this works**: This gives a lower bound on the actual cost needed, so it's admissible
• **Edge case**: If all corners are already visited, I return 0 since no more steps are needed
• **Benefits**: This heuristic helps A* focus on paths that get closer to the remaining corners, making the search more efficient
Advanced Questions:
1.	Based on your implementation of DFS, what happens if you fail to push all successors to the fringe (see GetSuccessors)?
If I fail to push all successors to the fringe in my implementation,the algorithm would be compromised in the below shown ways:
•	Incompleteness – DFS might fail to find a solution even if a solution exists. If the path to the goal lies through successor that was not pushed to the fringe the algorithm will never explore that path and might miss the goal.
•	Suboptimal solutions ,if cost matters – even if a solution is found it might not be a good solution because if some successors are omitted the algorithm will be forced to explore longer and costlier path to reach the goal which is not the desired solution.
Incorrect behaviour – fundamental characterstic of DFS is to exhaustively explore a branch before backtracking, not pushing all successors means that certain branches are prematurely pruned or not explored at all, leading to incomplete search of the state space.
2.	Does BFS give fewer actions to reach the goal compared to DFS? Explain.
Yes BFS is guaranteed to find a path with the fewest actions to the goal , but DFS does not, This difference is from the strategies shown below:
BFS: explores the search space level by level. It checks all nodes at depth ‘d’ before moving to any node at depth ‘d+1’.therefore the first time BFS encounters the goal state it must have done via a path that is short compared to any other path to the goal.This property makes BFS optimal for finding the shortest path in unweighted graphs or if step costs are uniform
DFS: DFS on the other hand explores as deeply as possible along a single path before backtracking . It might find a very long path to the goal is the shorter path exists and is on a different branch that is explored later. DFS prioritizes depth over path length and that is the reason why it doesn’t guarantee finding the shortest path.

3.	Manypath: You have expanded some paths in all search algorithms, which of these algorithms has the fewer expansion, and why?
A* algorithm has fewer expansions compared to BFS, DFS, and UCS provided with a well designed admissible and consistent heuristic.By using informed heuristic A* targets cheap and close goals g(n) and h(n), this approach allows A* to prune unpromising branches of the search tree efficiently than other uninformed search algorithms and significantly expand fewer nodes.
4.	Explain how the implementation of A* differs from UCS. 
The primary difference between A* and UCS lies in how they prioritize nodes in their priorityQueue fringes. Below are the key differences:
Priority function: UCS- expands nodes based only on actual path cost g(n) whereas A* expands nodes using f(n) = g(n) + h(n).
UCS doesn’t use heuristic and its uninformed search whereas A* uses heuristic function for goal estimation.
UCS priority is current path cost (g(n)) only, A* priority is path cost plus heuristic (g(n) + h(n))
Both may revisit nodes if a cheaper path is found but only A* uses heuristic to direct the search.
A* is UCS + heuristic which is smarter and faster search.

5.	Node expansion: How many nodes were expanded in Question 7?
4137 nodes were expanded in the in the question 7 , which is much lesser than the last case given the programming pdf assignment 

