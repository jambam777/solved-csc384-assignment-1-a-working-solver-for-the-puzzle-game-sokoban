Download Link: https://assignmentchef.com/product/solved-csc384-assignment-1-a-working-solver-for-the-puzzle-game-sokoban
<br>
The goal of this assignment will be to implement a working solver for the puzzle game Sokoban shown in Figure 1. Sokoban is a puzzle game in which a warehouse robot must push boxes into storage spaces. The rules hold that only one box can be moved at a time, that boxes can only be pushed by a robot and not pulled, and that neither robots nor boxes can pass through obstacles (walls or other boxes). In addition, robots cannot push more than one box, i.e., if there are two boxes in a row, the robot cannot push them. The game is over when all the boxes are in their storage spots.

In our version of Sokoban the rules are slightly more complicated, as there may be more than one warehouse robot available to push boxes. These robots cannot pass through one another nor can they move simultaneously, however.

Sokoban can be played online at <a href="https://www.sokobanonline.com/play">https://www.sokobanonline.com/play</a><a href="https://www.sokobanonline.com/play">.</a> We recommend that you familiarize yourself with the rules and objective of the game before proceeding, although it is worth noting that the version that is presented online is only an example. We will give a formal description of the puzzle in the next section

<h1>2           Description of Sokoban</h1>

Sokoban has the following formal description. Read the description carefully. Note that our version differs from the standard one.

<ul>

 <li>The puzzle is played on a rectangle board that is a grid <em>board </em>with <em>N </em>squares in the <em>x</em>-dimension and <em>M </em>squares in the <em>y</em>-dimension.</li>

 <li>Each state contains the <em>x </em>and <em>y </em>coordinates for each robot, the boxes, the storage spots, and the obstacles.</li>

 <li>From each state, each robot can move North, South, East, or West. No two robots can move simultaneously, however. If a robot moves to the location of a box, the box will move one square in the same direction. Boxes and robots cannot pass through walls or obstacles, however. Robots cannot push more than one box at a time; if two boxes are in succession the robot will not be able to move them. Movements that cause a box to move more than one unit of the grid are also illegal. Whether or not a robot is pushing an object does not change the cost.</li>

 <li>Each movement is of equal cost.</li>

 <li>The goal is achieved when each box is located in a storage area on the grid.</li>

</ul>

Ideally, we will want our robots to organize everything before the supervisor arrives. This means that with each problem instance, you will be given a computation time constraint. You must attempt to provide some legal solution to the problem (i.e. a plan) within this constraint. Better plans will be plans that are shorter, i.e. that require fewer operators to complete.

Your goal is to implement anytime algorithms for this problem: ones that generates better solutions (i.e. shorter plans) the more computation time they are given.

<h1>3           Code You Have Been Provided</h1>

The file search.py, which is available from the website, provides a generic search engine framework and code to perform several different search routines. This code will serve as a base for your Sokoban solver. A brief description of the functionality of search.py follows. The code itself is documented and worth reading.

<ul>

 <li>An object of class StateSpace represents a node in the state space of a generic search problem. The base class defines a fixed interface that is used by the SearchEngine class to perform search in that state space.</li>

</ul>

For the Sokoban problem, we will define a concrete sub-class that inherits from StateSpace. This concrete sub-class will inherit some of the “utility” methods that are implemented in the base class. Each StateSpace object <em>s </em>has the following key attributes:

<ul>

 <li><em>s</em><em>.gval</em>: the <em>g </em>value of that node, i.e., the cost of getting to that state.</li>

 <li><em>s</em><em>.parent</em>: the parent StateSpace object of <em>s</em>, i.e., the StateSpace object that has <em>s </em>as a successor. This will be <em>None </em>if <em>s </em>is the initial state.</li>

 <li><em>s</em><em>.action</em>: a string that contains that name of the action that was applied to <em>s</em><em>.parent </em>to generate</li>

</ul>

<ol>

 <li><em>s</em>. Will be <em>“START” </em>if <em>s </em>is the initial state.</li>

</ol>

<ul>

 <li>An object of class SearchEngine <em>se </em>runs the search procedure. A SearchEngine object is initialized with a search strategy (‘<em>depth first</em>’, ‘<em>breadth first</em>’, ‘<em>best first</em>’, ‘<em>a star</em>’, or ‘<em>custom</em>’) and a cycle checking level (‘<em>none</em>’, ‘<em>path</em>’, or ‘<em>full</em>’).</li>

</ul>

Note that SearchEngine depends on two auxiliary classes:

<ul>

 <li>An object of class sNode <em>sn </em>which represents a node in the search space. Each object <em>sn </em>contains a StateSpace object and additional details: <em>hval</em>, i.e., the heuristic function value of that state and <em>gval</em>, i.e. the cost to arrive at that node from the initial state. An <em>fval </em><em>fn </em>and <em>weight </em>are tied to search nodes during the execution of a search, where applicable.</li>

 <li>An object of class Open is used to represent the search frontier. The search frontier will be organized in the way that is appropriate for a given search strategy.</li>

</ul>

When a SearchEngine’s search strategy is set to ‘<em>custom</em>’, you will have to specify the way that <em>f </em>values of nodes are calculated; these values will structure the order of the nodes that are expanded during your search.

Once a SearchEngine object has been instantiated, you can set up a specific search with:

<em>init search</em>(<em>initial state</em><em>,goal </em><em>fn</em><em>,heur </em><em>fn</em><em>, fval </em><em>fn</em>) and execute that search with <em>search</em>(<em>timebound</em><em>,costbound</em>) The arguments are as follows:

<ul>

 <li><em>initial state </em>will be an object of type StateSpace; it is your start state.</li>

 <li><em>goal </em><em>fn</em>(<em>s</em>) is a function which returns True if a given state <em>s </em>is a goal state and False otherwise.</li>

 <li><em>heuristic </em><em>fn</em>(<em>s</em>) is a function that returns a heuristic value for state <em>s</em>. This function will only be used if your search engine has been instantiated to be a heuristic search (e.g. <em>best first</em>).</li>

 <li><em>fval </em><em>fn</em>(<em>sNode</em><em>,weight</em>) defines <em>f </em>values for states. This function will only be used by your search engine if it has been instantiated to execute a ‘<em>custom</em>’ search. Note that this function takes in an <em>sNode </em>and that an <em>sNode </em>contains not only a state but additional measures of the state (e.g. a gval). The function also takes in a float <em>weight</em>. It will use the variables that are provided to arrive at an <em>f </em>value calculation for the state contained in the <em>sNode</em>.</li>

 <li><em>timebound </em>is a bound on the amount of time your code will be allowed to execute the search. Once the run time exceeds the time bound, the search must stop; if no solution has been found, the search will return <em>False</em>.</li>

 <li><em>costbound </em>is an optional parameter that is used to set boundaries on the cost of nodes that are explored. This <em>costbound </em>is defined as a list of three values. <em>costbound</em>[0] is used to prune states based on their <em>g</em>-values; any state with a <em>g</em>-value higher than <em>costbound</em>[0] will not be expanded. <em>costbound</em>[1] is used to prune states based on their <em>h</em>-values; any state with an <em>h</em>value higher than <em>costbound</em>[1] will not be expanded. Finally, <em>costbound</em>[2] is used to prune states based on their <em>f</em>-values; any state with an <em>f</em>-value higher than <em>costbound</em>[2] will not be expanded.</li>

</ul>

For this assignment we have also provided sokoban.py, which specializes StateSpace for the Sokoban problem. You will therefore not need to encode representations of Sokoban states or the successor function for Sokoban! These have been provided to you so that you can focus on implementing good search heuristics and anytime algorithms.

The file sokoban.py contains:

<ul>

 <li>An object of class sokobanState, which is a StateSpace with these additional key attributes:

  <ul>

   <li><em>s</em><em>.width</em>: the width of the Sokoban board</li>

   <li><em>s</em><em>.height</em>: the height of the Sokoban board</li>

   <li><em>s</em><em>.robots</em>: positions for each robot that is on the board. Each robot position is a tuple (<em>x</em><em>,y</em>), that denotes the robot’s <em>x </em>and <em>y </em></li>

   <li><em>s</em><em>.boxes</em>: positions for each box that is on the board. Each box position is also an (<em>x</em><em>,y</em>)</li>

   <li><em>s</em><em>.storage</em>: positions for each storage bin that is on the board (also (<em>x</em><em>,y</em>) tuples).</li>

   <li><em>s</em><em>.obstacles</em>: positions for obstacles that are on the board. Obstacles, like robots and boxes, are also tuples of (<em>x</em><em>,y</em>)</li>

  </ul></li>

 <li>sokobanState also contains the following key functions:

  <ul>

   <li><em>successors</em>(): This function generates a list of SokobanStates that are successors to a given SokobanState. Each state will be annotated by the action that was used to arrive at the SokobanState. These actions are (<em>r</em><em>,d</em>) tuples wherein <em>r </em>denotes the index of the robot that moved <em>d </em>denotes the direction of movement of the robot.</li>

   <li><em>hashable state</em>(): This is a function that calculates a unique index to represents a particular sokobanState. It is used to facilitate path and cycle checking.</li>

   <li><em>print state</em>(): This function prints a sokobanState to stdout.</li>

  </ul></li>

</ul>

Note that sokobanState depends on one auxiliary class:

<ul>

 <li>An object of class Direction, which is used to define the directions that each robot can move and the effect of this movement.</li>

</ul>

Also note that sokoban.py contains a set of 20 initial states for Sokoban problems, which are stored in the tuple <em>PROBLEMS</em>. You can use these states to test your implementations.

The file solution.py contains the methods that need to be implemented.

The file autograder.py runs some tests on your code to give you an indication of how well your methods perform.

<h1>4           Assignment Specifics</h1>

To complete this assignment you must modify solution.py to:

<ul>

 <li>Implement a Manhattan distance heuristic (<em>heur manhattan distance</em>(<em>state</em>)). This heuristic will be used to estimate how many moves a current state is from a goal state. Your implementation should calculate the sum of Manhattan distances between each box that has yet to be stored and the storage point nearest to it. Ignore the positions of obstacles in your calculations and assume that many boxes can be stored at one location.</li>

 <li>Implement Anytime Greedy Best-First Search (<em>anytime gbfs</em>(<em>initial state</em>;<em>heur </em><em>fn</em>;<em>timebound</em>)). Details regarding this algorithm are provided in Section 5.</li>

 <li>Implement Anytime Weighted A* (<em>weighted astar</em>(<em>initail state</em><em>,timebound</em>)). Details about this algorithm are provided in Section 6. Note that your implementation will require you to instantiate a SearchEngine object with a custom search strategy. To do this you must therefore define an f-value function (<em>fval </em><em>function</em>(<em>sNode</em><em>,weight</em>)) and remember to provide this when you execute <em>init search</em>.</li>

 <li>Implement a non-trivial heuristic for Sokoban that improves on the Manhattan distance heuristic (<em>heur alternate</em>(<em>state</em>)).</li>

 <li>You should give three tips (2 sentences each) as if you were advising someone who was attempting this problem for this first time on what to do. Write these tips in the file tips.txt.</li>

</ul>

Note that when we are testing your code, we will limit each run of your algorithm on teach.cs to a fixed time bound. Instances that are not solved within this limit will provide an interesting evaluation metric: failure rate.

<h1>5           Anytime Greedy Best-First Search</h1>

Greedy best-first search expands nodes with lowest <em>h</em>(<em>node</em>) first. The solution found by this algorithm may not be optimal. Anytime greedy best-first search (which is called <em>anytime gbfs </em>in the code) continues searching after a solution is found in order to improve solution quality. Since we have found a path to the goal after the first iteration, we can introduce a cost bound for pruning: if node has <em>g</em>(<em>node</em>) greater than the best path to the goal found so far, we can prune it. The algorithm returns either when we have expanded all non-pruned nodes, in which case the best solution found by the algorithm is the optimal solution, or when it runs out of time. We prune based on the <em>g value </em>of the node only because greedy best-first search is not necessarily run with an admissible heuristic.

Record the time when <em>anytime gbfs </em>is called with <em>os</em><em>.times</em>()[0]. Each time you call search, you should update the time bound with the remaining allowed time. The automarking script will confirm that your algorithm obeys the specified time bound.

<h1>6           Anytime Weighted A*</h1>

Instead of A*’s regular node-valuation formula (<em>f </em>= <em>g</em>(<em>node</em>)+<em>h</em>(<em>node</em>)), Weighted A* introduces a weighted formula:

<em>f </em>= <em>g</em>(<em>node</em>)+<em>w</em>∗<em>h</em>(<em>node</em>)

where <em>g</em>(<em>node</em>) is the cost of the path to <em>node</em>, <em>h</em>(<em>node</em>) is the estimated cost of getting from <em>node </em>to the goal, and <em>w </em><em>&gt; </em>1 is a bias towards states that are closer to the goal. Theoretically, the smaller <em>w </em>is, the better the solution will be (i.e. the closer to the optimal solution it will be … <em>why and under what assumptions??</em>). However, different values of <em>w </em>will require different computation times.

Weighted A* typically starts with a value for <em>w </em>that is more than 1. Since the first solution that is found by Weighted A* may not be optimal if <em>w </em><em>&gt; </em>1, we can keep searching after we have found a solution. Anytime Weighted A* continues to search, decreasing <em>w </em>at each iteration, until either there are no nodes left to expand (and our best solution is the optimal one) or it runs out of time. Since a solution we find in one iteration may not be optimal, we can introduce a cost bound for pruning during subsequent iterations: if node has a <em>g</em>(<em>node</em>)+<em>h</em>(<em>node</em>) value that is greater than the cost of the best path to the goal found so far, we can prune it.

When you are passing in an <em>f val </em>function to <em>init search </em>for this problem, you will need to have specified the weight for the <em>fval </em><em>function</em>. You can do this by wrapping the <em>fval </em><em>function</em>(<em>sN</em><em>,weight</em>) you have written in an anonymous function, i.e.,

<em>wrapped fval function </em>= (<em>lambda sN </em>: <em>fval </em><em>function</em>(<em>sN</em><em>,weight</em>))

Figure 2: A state of the Water Jugs puzzle.

<h1>7           StateSpace Example: WaterJugs.py</h1>

WaterJugs.py contains an example implementation of the search engine for the Water Jugs problem shown in Figure 2.

<ul>

 <li>You have two containers that can be used to store water. One has a three-gallon capacity, and the other has a four-gallon capacity. Each has an initial, known, amount of water in it.</li>

 <li>You have the following actions available:

  <ul>

   <li>You can fill either container from the tap until it is full.</li>

   <li>You can dump the water from either container.</li>

   <li>You can pour the water from one container into the other, until either the source container is empty or the destination container is full.</li>

  </ul></li>

 <li>You are given a goal amount of water to have in each container. You are trying to achieve that goal in the minimum number of actions, assuming the actions have uniform cost.</li>

</ul>

WaterJugs.py has an implementation of the Water Jugs puzzle that is suitable for using with search.py. Note that in addition to implementing the three key methods of StateSpace, the author has created a set of tests that show how to operate the search engine. You should study these to see how the search engine works.