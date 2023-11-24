> [!IMPORTANT]
> This is a basic breakdown code which used in the video. This code does not writed by me!

## **Code Intro**

- **Code :**
```py
import sys
```

This line of code imports the `sys` module in Python. The `sys` module provides access to some variables used or maintained by the Python interpreter, as well as functions that interact strongly with the interpreter. In this context, it is used to handle command-line arguments.

The subsequent code checks whether the correct number of command-line arguments is provided:
```py
if len(sys.argv) != 2:
    sys.exit("Usage: python maze.py maze.txt")
```
If the number of arguments is not equal to 2, the program exits with an error message. It expects the user to provide the script name (first argument) and the maze file name (second argument) when running the program. If the correct number of arguments is given, it proceeds to use the maze file specified in the second argument (`sys.argv[1]`).

## **Node**

- **Code :**
```py
class Node():
    def __init__(self, state, parent, action):
        self.state = state
        self.parent = parent
        self.action = action
```

This code defines a Python class named Node. This class represents a node in a search space, particularly used in search algorithms such as BFS (breadth-first search) or DFS (depth-first search). Let's break down the attributes and methods of this class:
- **Attributes :**
  - `state`  : Represents the state associated with the node. In the context of a maze-solving algorithm, the state could be the current position in the maze.
  - `parent` : Represents the parent node from which the current node is reached. This is crucial for reconstructing the path once the goal is reached.
  - `action` : Represents the action taken to reach the current state from the parent node.
- **Explanation :**
  - `__init__(self, state, parent, action)` : The constructor method initializes a new instance of the 'Node' class with the provided `state`, `parent` and `action` values.

This class is part of a broader search algorithm implementation, where instances of `Node` are used to represent different states and their relationships in the search process. Each node represents a possible state in the search space and the `parent` attribute is used to link nodes together, forming a path from the start state to the goal state.

## **StackFrontier class**

- **Code :**
```py
class StackFrontier():
    def __init__(self):
        self.frontier = []

    def add(self, node):
        self.frontier.append(node)

    def contains_state(self, state):
        return any(node.state == state for node in self.frontier)

    def empty(self):
        return len(self.frontier) == 0

    def remove(self):
        if self.empty():
            raise Exception("empty frontier")
        else:
            node = self.frontier[-1]
            self.frontier = self.frontier[:-1]
            return node
```
This code defines a class named `StackFrontier`, which appears to implement a stack data structure for a frontier in a search algorithm. Here's a breakdown of the class and its explanation:

- **Attributes :**
  - `frontier` : A list that serves as the stack to hold nodes.
- **Explanation :**
  - `__init__(self)` : The constructor initializes an empty list to represent the stack.
  - `add(self, node)` : Adds a node to the top of the stack.
  - `contains_state(self, state)` : Checks if a node with a given state is already in the stack.
  - `empty(self)`: Checks if the stack is empty.
  - `remove(self)` : Removes and returns the top node from the stack. If the stack is empty, it raises an exception.

This class is part of a search algorithm implementation, where the frontier is a collection of nodes yet to be explored. The stack follows the Last In, First Out (LIFO) principle, meaning that the last node added to the frontier will be the first one to be explored. This is typically associated with depth-first search (DFS) algorithms.

## **QueueFrontier class**

- **Code :**
```py
class QueueFrontier(StackFrontier):

    def remove(self):
        if self.empty():
            raise Exception("empty frontier")
        else:
            node = self.frontier[0]
            self.frontier = self.frontier[1:]
            return node
```
- **Explanation :**
  - `remove(self)` : Overrides the `remove` method from the parent `StackFrontier` class. In a queue, the first element added is the first to be removed (First In, First Out or FIFO). This method removes and returns the node at the front of the queue. If the queue is empty, it raises an exception.

By inheriting from `StackFrontier`, the `QueueFrontier` class shares the same `add`, `contains_state`, and `empty` methods with the stack. The key difference is in the `remove` method, where it now removes the first element (`self.frontier[0]`) instead of the last one `(self.frontier[-1]`), as is the case with a stack.

This class is likely intended to be used for breadth-first search (BFS) algorithms, where nodes are explored in the order they are discovered, i.e., the node that has been in the frontier the longest is the next one to be explored.

## **Maze class**

```py
class Maze():
```

**Inside the `def __init__(self, filename):`**

- **Code :**
```py
with open(filename) as f:
    contents = f.read()
```
- **Explanation :**

This part of the `Maze` class defines the constructor method `(__init__)`.
  - This code uses a `with` statement to open the specified file (`filename`). The `open` function is used to open a file, and the with statement ensures that the file is properly closed after reading its contents.
  - The contents of the file are read using the `read` method, and the resulting string is stored in the `contents` variable.
  - `__init__(self, filename)` : This is the constructor method that initializes a new instance of the `Maze` class. It takes a `filename` as a parameter, which is expected to be the name of a file containing the maze configuration.
- **Purpose :**
  - This section of the code is responsible for reading the contents of the maze file and storing them in the `contents` variable. The maze file is expected to contain information about the maze structure, including the positions of the start and goal points.
- **Note :**
  - The structure and format of the maze file are crucial for the correct functioning of the rest of the `Maze` class. The code assumes a specific format in terms of representing walls, the start point ('A'), and the goal point ('B') in the maze configuration. If the file does not adhere to these assumptions, the behavior of the program may not be as expected.

- **Code :**
```py
# Validate start and goal
if contents.count("A") != 1:
    raise Exception("maze must have exactly one start point")
if contents.count("B") != 1:
    raise Exception("maze must have exactly one goal")
```
- **Explanation :**

This part of the `Maze` class is responsible for validating the maze configuration read from the file.
  - The code uses the count method of the string class to `count` the occurrences of the characters 'A' and 'B' in the `contents` string, which represents the maze configuration.
  - The first `if` statement checks if the maze contains exactly one start point ('A'). If not, it raises an exception with the message "maze must have exactly one start point."
  - The second `if` statement checks if the maze contains exactly one goal point ('B'). If not, it raises an exception with the message "maze must have exactly one goal."
- **Purpose :**
  - This section ensures that the maze has a valid configuration by checking if there is exactly one start point ('A') and one goal point ('B'). This is a crucial validation step to ensure that the maze has a well-defined starting and ending point.
- **Note :**
  - The exceptions raised here will interrupt the program execution if the validation fails, providing clear error messages indicating the nature of the problem in the maze configuration.

- **Code :**
```py
# Determine height and width of maze
contents = contents.splitlines()
self.height = len(contents)
self.width = max(len(line) for line in contents)
```
- **Explanation :**

This part of the Maze class is responsible for determining the height and width of the maze based on the contents read from the file. 
  - The `splitlines` method is applied to the `contents` string, which splits the string into a list of lines. Each element of the list represents a line in the original string.
  - The `self.height` attribute is then set to the number of lines in the `contents` list. This gives the vertical size or height of the maze.
  - The `self.width` attribute is set to the maximum length of any line in the `contents` list. This determines the horizontal size or width of the maze.
- **Purpose :**
  - This section calculates and stores the height and width of the maze. These values are crucial for defining the dimensions of the maze and are used in subsequent parts of the code for creating the maze structure.
- **Note :**
  - The assumption here is that the maze is represented as a grid where each character in the configuration corresponds to a cell in the grid. The height is determined by the number of lines, and the width is determined by the length of the longest line. If the maze file does not adhere to this grid-based representation, the dimensions may not be accurately calculated.

- **Code :**
```py
# Keep track of walls
self.walls = []
for i in range(self.height):
    row = []
    for j in range(self.width):
        try:
            if contents[i][j] == "A":
                self.start = (i, j)
                row.append(False)
            elif contents[i][j] == "B":
                self.goal = (i, j)
                row.append(False)
            elif contents[i][j] == " ":
                row.append(False)
            else:
                row.append(True)
        except IndexError:
            row.append(False)
    self.walls.append(row)
```
- **Explanation :**

This part of the Maze class is responsible for keeping track of the walls in the maze and determining the positions of the start ('A') and goal ('B') points.
  - The code initializes an empty list `self.walls` to keep track of walls in the maze. This list will be a 2D grid representing whether each cell in the maze is a wall or not.
  - It then iterates over each row (`i`) and column (`j`) in the maze configuration. For each cell, it checks the value in the `contents` string at the corresponding position.
  - If the value is 'A', it sets `self.start` to the current position and appends `False` to the `row` (indicating it's not a wall).
  - If the value is 'B', it sets `self.goal` to the current position and appends `False` to the `row`.
  - If the value is a space (' '), it appends `False` to the `row` (indicating it's not a wall).
  - If the value is anything else, it appends `True` to the `row` (indicating it's a wall).
  - An `except IndexError` block is used to handle the case where the index is out of bounds, and in such cases, it appends `False` to the `row`.
  - The `row` is then appended to `self.walls`, forming a 2D representation of the maze.
- **Purpose :**
  - This section parses the maze configuration and creates a 2D grid (`self.walls`) representing the walls in the maze. It also determines the positions of the start and goal points.
- **Note :**
  - The assumption here is that 'A' represents the start point, 'B' represents the goal point, a space (' ') represents an open path, and any other character represents a wall. If the maze file uses different symbols or a different representation, this part of the code may need to be adapted accordingly.

- **Code :**
```py
self.solution = None
```
- **Explanation :**

This line initializes the solution attribute of the Maze class to `None`.
  - The `self.solution` attribute is used to store the solution to the maze once it is found. It is initially set to `None`, indicating that no solution has been found.
- **Purpose :**
  - This attribute is meant to hold the solution path in the form of a tuple containing a list of actions and a list of cell positions. When a solution is found during the maze-solving process, this attribute will be updated to store the solution information.
- **Note :**
  - The structure of the solution is typically set when the `solve` method of the `Maze` class is called. This method is responsible for finding a solution to the maze using a search algorithm.

> [!TIP]
> This line essentially sets up the `solution` attribute, making it available for later use in the `Maze` class.

##

- **Code :**
```py
def print(self):
    solution = self.solution[1] if self.solution is not None else None
    print()
    for i, row in enumerate(self.walls):
        for j, col in enumerate(row):
            if col:
                print("█", end="")
            elif (i, j) == self.start:
                print("A", end="")
            elif (i, j) == self.goal:
                print("B", end="")
            elif solution is not None and (i, j) in solution:
                print("*", end="")
            else:
                print(" ", end="")
        print()
    print()
```
- **Explanation :**

This part of the `Maze` class defines a `print` method.
  - The `print` method is designed to print the maze configuration, including the solution path if available.
  - It first checks if a solution exists (`self.solution is not None`). If a solution exists, it extracts the list of cell positions from the solution (assuming the solution structure is a tuple containing actions and cell positions).
  - It then iterates over each row and column in the maze, printing characters based on the following conditions:
    - If the cell is a wall (`col` is `True`), it prints "█" to represent a wall.
    - If the cell position matches the start point, it prints "A".
    - If the cell position matches the goal point, it prints "B".
    - If a solution exists and the cell position is in the solution, it prints "*".
    - Otherwise, it prints a space (" ").
  - It prints a newline after each row to format the maze.
- **Purpose :**
  - This method is primarily for visualizing the maze on the console. It prints the maze structure, highlighting the start, goal, walls, and the solution path if available.
- **Note :**
  - The assumption is that the maze is represented using specific characters, such as "█" for walls, "A" for the start point, "B" for the goal point, and "*" for cells in the solution path. If the maze uses different symbols, the code may need to be adapted accordingly.
 
## 

- **Code :**
```py
def neighbors(self, state):
    row, col = state
    candidates = [
        ("up", (row - 1, col)),
        ("down", (row + 1, col)),
        ("left", (row, col - 1)),
        ("right", (row, col + 1))
    ]

    result = []
    for action, (r, c) in candidates:
        if 0 <= r < self.height and 0 <= c < self.width and not self.walls[r][c]:
            result.append((action, (r, c)))
    return result
```
- **Explanation :**

This part of the `Maze` class defines a `neighbors` method.
  - The `neighbors` method takes a `state` parameter, which is assumed to be a tuple representing the (row, column) position in the maze.
  - It initializes a list `candidates` containing tuples, where each tuple represents a potential neighbor and its corresponding action. The actions are "up," "down," "left," and "right," representing the four possible directions.
  - The method then iterates over the candidates and checks if the potential neighbor position (`(r, c)`) is within the bounds of the maze (`0 <= r < self.height` and `0 <= c < self.width`) and if it is not a wall (`not self.walls[r][c]`).
  - If these conditions are met, the neighbor is considered valid, and it is added to the `result` list along with its corresponding action.
  - The method returns the list of valid neighbors.
- **Purpose :**
  - This method is used to determine the valid neighbors of a given state in the maze. It considers neighboring positions in the up, down, left, and right directions, excluding positions outside the maze bounds or positions that are walls.
- **Note :**
  - The assumption is that the maze is represented as a 2D grid (`self.walls`) where `True` represents a wall and `False` represents an open path. The method uses the height and width of the maze to ensure that potential neighbors are within bounds. If the maze has a different representation or structure, this method may need adjustments.

## 

- **Code :**
```py
def solve(self):
    """Finds a solution to maze, if one exists."""

    # Keep track of number of states explored
    self.num_explored = 0

    # Initialize frontier to just the starting position
    start = Node(state=self.start, parent=None, action=None)

    # QueueFrontier for BFS (first in first out)
    frontier = QueueFrontier()
    frontier.add(start)

    # Initialize an empty explored set
    self.explored = set()

    # Keep looping until a solution is found
    while True:

        # If nothing left in frontier, then no path
        if frontier.empty():
            raise Exception("no solution")

        # Choose a node from the frontier
        node = frontier.remove()
        self.num_explored += 1

        # If the node is the goal, then we have a solution
        if node.state == self.goal:
            actions = []
            cells = []
            while node.parent is not None:
                actions.append(node.action)
                cells.append(node.state)
                node = node.parent
            actions.reverse()
            cells.reverse()
            self.solution = (actions, cells)
            return

        # Mark the node as explored
        self.explored.add(node.state)

        # Add neighbors to the frontier
        for action, state in self.neighbors(node.state):
            if not frontier.contains_state(state) and state not in self.explored:
                child = Node(state=state, parent=node, action=action)
                frontier.add(child)
```
- **Explanation :**

This part of the `Maze` class defines the `solve` method, which is responsible for finding a solution to the maze using a search algorithm (likely BFS based on the use of `QueueFrontier`).
  - The `solve` method is designed to find a solution to the maze using a search algorithm. It initializes a frontier, explores states, and updates the solution when the goal is reached.
  - It starts by initializing the number of explored states (`self.num_explored`) and creating the starting node (`start`) using the starting position from the maze.
  - It then initializes a frontier using `QueueFrontier` (indicating that BFS will be used). The starting node is added to the frontier.
  - The method enters a loop that continues until a solution is found. In each iteration:
    - If the frontier is empty, it raises an exception indicating that there is no solution.
    - It removes a node from the frontier.
    - If the node's state is the goal state, it reconstructs the solution by tracing back through the parent pointers. The solution is then stored in the `self.solution` attribute, and the method returns.
    - The node is marked as explored by adding its state to the `self.explored` set.
    - The neighbors of the current node are added to the frontier if they satisfy certain conditions.
- **Purpose :**
  - This method implements a search algorithm to find a solution to the maze. It explores states in the maze until it reaches the goal state, and it keeps track of the solution path.
- **Note :**
  - The choice of search algorithm (in this case, BFS) is determined by the type of frontier used (`QueueFrontier`). If a different search algorithm is desired, the frontier type can be changed accordingly (e.g., `StackFrontier` for DFS).

## 

- **Code :**
```py
def output_image(self, filename, show_solution=True, show_explored=False):
    from PIL import Image, ImageDraw
    cell_size = 50
    cell_border = 2

    # Create a blank canvas
    img = Image.new(
        "RGBA",
        (self.width * cell_size, self.height * cell_size),
        "black"
    )
    draw = ImageDraw.Draw(img)

    solution = self.solution[1] if self.solution is not None else None
    for i, row in enumerate(self.walls):
        for j, col in enumerate(row):

            # Walls
            if col:
                fill = (40, 40, 40)

            # Start
            elif (i, j) == self.start:
                fill = (255, 0, 0)

            # Goal
            elif (i, j) == self.goal:
                fill = (0, 171, 28)

            # Solution
            elif solution is not None and show_solution and (i, j) in solution:
                fill = (220, 235, 113)

            # Explored
            elif solution is not None and show_explored and (i, j) in self.explored:
                fill = (212, 97, 85)

            # Empty cell
            else:
                fill = (237, 240, 252)

            # Draw cell
            draw.rectangle(
                ([(j * cell_size + cell_border, i * cell_size + cell_border),
                  ((j + 1) * cell_size - cell_border, (i + 1) * cell_size - cell_border)]),
                fill=fill
            )

    img.save(filename)
```
- **Explanation :**

This part of the Maze class defines a method named output_image that generates and saves an image representation of the maze.
  - The method uses the Python Imaging Library (PIL) to create an image representation of the maze.
  - It creates a blank canvas (`img`) with a specified size based on the cell size and maze dimensions.
  - It then initializes a `Draw` object (`draw`) to draw on the canvas.
  - The method iterates over each cell in the maze (`self.walls`) and assigns a color (`fill`) based on different conditions:
    - If the cell is a wall, it is filled with a dark color (`(40, 40, 40)`).
    - If the cell is the start point, it is filled with red (`(255, 0, 0)`).
    - If the cell is the goal point, it is filled with green (`(0, 171, 28)`).
    - If the solution exists and `show_solution` is `True`, the cells in the solution path are filled with a light color (`(220, 235, 113)`).
    - If the solution exists, `show_explored` is `True`, and the cell has been explored, it is filled with a different color (`(212, 97, 85)`).
    - Otherwise, the empty cells are filled with a light background color (`(237, 240, 252)`).
  - The cells are drawn on the canvas using the `rectangle` method, and the image is saved to the specified filename.
- **Purpose :**
  - This method creates an image representation of the maze with various visualizations, such as highlighting the start and goal points, the solution path, and explored cells.
- **Note :**
  - The colors and visual representations used in the code are arbitrary and can be adjusted based on preferences. The method assumes that the solution and explored cells are available (i.e., the maze has been solved). The PIL library needs to be installed (`pip install pillow`) to use this method.

## **Last piece**

- **Code :**
```py
if len(sys.argv) != 2:
    sys.exit("Usage: python maze.py maze.txt")

m = Maze(sys.argv[1])
print("Maze:")
m.print()
print("Solving...")
m.solve()
print("States Explored:", m.num_explored)
print("Solution:")
m.print()
m.output_image("maze.png", show_explored=True)

```
- **Explanation :**

This portion of the code is typically used to run the maze-solving program from the command line. 
  - The code checks if the number of command-line arguments is not equal to 2. If this condition is met, it means that the user did not provide the required command-line argument (the maze file), and the program exits with an error message indicating the correct usage.
  - If the correct number of command-line arguments is provided, the code proceeds to create an instance of the `Maze` class (`m`) by passing the maze file name obtained from the command line (`sys.argv[1]`).
  - It then prints the initial state of the maze using the `print` method.
  - The program proceeds to solve the maze using the `solve` method.
  - After solving the maze, it prints the number of states explored during the solving process.
  - It then prints the final state of the maze, which may include the solution path.
  - Finally, it generates an image representation of the maze with explored cells highlighted and saves it as "maze.png" using the `output_image` method.
- **Purpose :**
  - This part of the code serves as the main driver for running the maze-solving program. It ensures that the user provides the correct command-line arguments and then initializes the maze, solves it, prints relevant information, and generates an image representation.
- **Note :**
  - To run this code from the command line, the user needs to provide the maze file as a command-line argument. For example:
    ```bash
    python maze.py maze.txt
    ```

  - This assumes that the maze file is named "maze.txt" (replace with the actual file name).

## **Diagram that shows how it works**

```mermaid
graph TD;
    subgraph Initialization
        A[Initialize Maze] -->|Read Maze from File| B[Create Maze Object];
        B -->|Set Start and Goal| C[Set Start and Goal Points];
    end

    subgraph Exploration
        C -->|Print Initial Maze| D[Print Maze];
        D -->|Solve Maze| E[Explore Maze];
        E -->|Check Frontier| F[Frontier Empty?];
        F -- No --> G[Choose Node from Frontier];
        F -- Yes --> H[No Solution Found];
        G --> I[Check Goal Reached];
        I -- No --> J[Mark Node as Explored];
        J --> K[Add Neighbors to Frontier];
        K --> E;  %% Corrected connection to Exploration stage
        I -- Yes --> L[Backtrack to Find Solution];
    end

    subgraph Visualization
        L -->|Print Solution| M[Print Solution];
        M -->|Output Image| N[Generate Image];
        N -->|Display Image| O[Display Result];
    end
```
