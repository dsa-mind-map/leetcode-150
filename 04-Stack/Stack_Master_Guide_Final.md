# Stack Pattern: Master Reference Guide

Every stack problem operates on a single overarching lifecycle:
*   **Defer (Push):** An element cannot be fully processed yet, so it is put on hold.
*   **Resolve (Pop):** A specific "trigger" condition arrives, finalizing the state of the deferred elements.

There are four distinct sub-patterns based on what the "trigger" is.

## The "Defer & Resolve" Master Comparison Table

To truly master the Stack pattern, you need to understand both the high-level **Sub-Pattern** (to know *when* to use a stack) and the specific **Problem mechanics** (to know exactly *what* triggers the Push/Pop). 

This combined table provides the ultimate cheat sheet for both macro-level pattern recognition and micro-level execution.

| Sub-Pattern | Problem | What triggers a `Pop` (Resolve)? | What triggers a `Push` (Defer)? |
| :--- | :--- | :--- | :--- |
| **1. Matching & Tracking** | Valid Parentheses | Matching closing bracket. | Every opening bracket. |
| | Min Stack | Explicit user `pop()` command. | Explicit user `push()` command. |
| **2. Expression Parsing** | Evaluate RPN | Mathematical operator (`+`, `-`, `*`, `/`). | Every number. |
| **3. Monotonic Stack** | Daily Temperatures | Strictly **greater** element. | Every element. |
| | Largest/Maximal Rectangle| Strictly **smaller** element. | Every element. |
| | Car Fleet | Car time $\le$ fleet ahead. | Every car's arrival time. |
| **4. Graph Traversal** | Iterative DFS | Popping to process the top node. | Every unvisited neighbor node. |

---

## Sub-Pattern 1: Matching & Tracking
**The Pop Trigger:** A closing bracket or an explicit command.
**Core Goal:** Verifying symmetry or retrieving historical states.

### 1. Valid Parentheses (LeetCode 20)
> **Question:** Given a string `s` containing just the characters `'('`, `')'`, `'{'`, `'}'`, `'['` and `']'`, determine if the input string is valid.
> An input string is valid if:
> 1. Open brackets must be closed by the same type of brackets.
> 2. Open brackets must be closed in the correct order.
> 3. Every close bracket has a corresponding open bracket of the same type.
> 
> **Example 1:** `s = "()"` -> **Output:** `true`
> **Example 2:** `s = "()[]{}"` -> **Output:** `true`
> **Example 3:** `s = "(]"` -> **Output:** `false`
> 
> **Constraints:** `1 <= s.length <= 10^4`, `s` consists of parentheses only.

```java
public boolean isValid(String s) {
    Stack<Character> stack = new Stack<>();
    for (char c : s.toCharArray()) {
        // 1. Resolve (Pop): Check if closing bracket matches the deferred opening bracket
        if (c == ')' || c == '}' || c == ']') {
            if (stack.isEmpty() || stack.pop() != c) return false;
        } 
        // 2. Defer (Push): We push the EXPECTED closing bracket to make matching easier
        else {
            if (c == '(') stack.push(')');
            else if (c == '{') stack.push('}');
            else if (c == '[') stack.push(']');
        }
    }
    return stack.isEmpty();
}
```
* **Time Complexity:** $O(N)$ - We iterate through the string exactly once.
* **Space Complexity:** $O(N)$ - In the worst-case scenario (all opening brackets), the stack will store all $N$ characters.

### 2. Min Stack (LeetCode 155)
> **Question:** Design a stack that supports push, pop, top, and retrieving the minimum element in constant time.
> Implement the `MinStack` class:
> * `MinStack()` initializes the stack object.
> * `void push(int val)` pushes the element `val` onto the stack.
> * `void pop()` removes the element on the top of the stack.
> * `int top()` gets the top element of the stack.
> * `int getMin()` retrieves the minimum element in the stack.
> You must implement a solution with `O(1)` time complexity for each function.

```java
class MinStack {
    // We defer an array of [current_value, minimum_at_this_point]
    Stack<int[]> stack = new Stack<>();

    public void push(int val) {
        // Defer (Push)
        if (stack.isEmpty()) {
            stack.push(new int[]{val, val});
        } else {
            stack.push(new int[]{val, Math.min(val, stack.peek()[1])});
        }
    }

    public void pop() {
        // Resolve (Pop)
        stack.pop();
    }

    public int top() {
        return stack.peek()[0];
    }

    public int getMin() {
        return stack.peek()[1];
    }
}
```
* **Time Complexity:** $O(1)$ for all operations - Array indexing and stack push/pop are constant time operations.
* **Space Complexity:** $O(N)$ - We store $N$ elements in the stack along with their historical minimums.

---

## Sub-Pattern 2: Expression Parsing
**The Pop Trigger:** A mathematical operator (`+`, `-`, `*`, `/`).
**Core Goal:** Collapsing multiple operands into one result.

### 3. Evaluate Reverse Polish Notation (LeetCode 150)
> **Question:** You are given an array of strings `tokens` that represents an arithmetic expression in a Reverse Polish Notation.
> Evaluate the expression. Return an integer that represents the value of the expression.
> * The valid operators are `+`, `-`, `*`, and `/`.
> * Each operand may be an integer or another expression.
> * The division between two integers always truncates toward zero.
> 
> **Example:** `tokens = ["2","1","+","3","*"]` -> **Output:** `9`
> Explanation: `((2 + 1) * 3) = 9`

```java
public int evalRPN(String[] tokens) {
    Stack<Integer> stack = new Stack<>();
    for (String t : tokens) {
        // 1. Resolve (Pop): Evaluate the last two deferred numbers when an operator arrives
        if ("+-*/".contains(t)) {
            int b = stack.pop(); // Note: 2nd operand is popped first
            int a = stack.pop();
            if (t.equals("+")) stack.push(a + b);
            else if (t.equals("-")) stack.push(a - b);
            else if (t.equals("*")) stack.push(a * b);
            else if (t.equals("/")) stack.push(a / b);
        } 
        // 2. Defer (Push): Wait for an operator
        else {
            stack.push(Integer.parseInt(t));
        }
    }
    return stack.pop();
}
```
* **Time Complexity:** $O(N)$ - We iterate through the tokens exactly once. Integer parsing and basic arithmetic are constant time.
* **Space Complexity:** $O(N)$ - In the worst-case scenario (many numbers followed by operators at the end), the stack will store up to $O(N/2)$ elements.

---

## Sub-Pattern 3: Monotonic Stack
**The Pop Trigger:** A strictly greater or strictly smaller element.
**Core Goal:** Finding spatial boundaries or next greater/smaller elements in $O(N)$ time.

### 4. Daily Temperatures (LeetCode 739)
> **Question:** Given an array of integers `temperatures` represents the daily temperatures, return an array `answer` such that `answer[i]` is the number of days you have to wait after the `i`th day to get a warmer temperature. If there is no future day for which this is possible, keep `answer[i] == 0` instead.
> 
> **Example:** `temperatures = [73,74,75,71,69,72,76,73]` 
> **Output:** `[1,1,4,2,1,1,0,0]`

```java
public int[] dailyTemperatures(int[] temperatures) {
    int[] ans = new int[temperatures.length];
    Stack<Integer> stack = new Stack<>(); 
    
    for (int i = 0; i < temperatures.length; i++) {
        // 1. Resolve (Pop): The current day is warmer than waiting days
        while (!stack.isEmpty() && temperatures[i] > temperatures[stack.peek()]) {
            int prevIndex = stack.pop();
            ans[prevIndex] = i - prevIndex; 
        }
        // 2. Defer (Push): Wait for a warmer day
        stack.push(i);
    }
    return ans;
}
```
* **Time Complexity:** $O(N)$ - Although there is a `while` loop inside the `for` loop, each element is pushed and popped strictly at most once. Therefore, the inner loop runs a maximum of $N$ times across the entire program.
* **Space Complexity:** $O(N)$ - To store the deferred indices in the stack.

### 4.5 Car Fleet (LeetCode 853)
> **Question:** There are `n` cars going to the same destination along a one-lane road. The destination is `target` miles away.
> You are given two integer arrays `position` and `speed`, both of length `n`, where `position[i]` is the position of the `i`th car and `speed[i]` is the speed of the `i`th car (in miles per hour).
> A car can never pass another car ahead of it, but it can catch up to it and drive bumper to bumper at the same speed. The faster car will slow down to match the slower car's speed. The distance between these two cars is ignored (i.e., they are assumed to have the same position).
> A car fleet is some non-empty set of cars driving at the same position and same speed. Note that a single car is also a car fleet.
> If a car catches up to a car fleet right at the destination point, it will still be considered as one car fleet.
> Return the number of car fleets that will arrive at the destination.
> 
> **Example 1:** `target = 12`, `position = [10,8,0,5,3]`, `speed = [2,4,1,1,3]`
> **Output:** `3`
> Explanation: 
> * The cars starting at 10 (speed 2) and 8 (speed 4) become a fleet, meeting each other at 12.
> * The car starting at 0 does not catch up to any other car, so it is a fleet by itself.
> * The cars starting at 5 (speed 1) and 3 (speed 3) become a fleet, meeting each other at 6. The fleet moves at speed 1 until it reaches target.

```java
public int carFleet(int target, int[] position, int[] speed) {
    int n = position.length;
    double[][] cars = new double[n][2];
    for (int i = 0; i < n; i++) {
        cars[i][0] = position[i];
        cars[i][1] = (double) (target - position[i]) / speed[i]; // Time to reach target
    }
    
    // Sort cars by position in descending order (closest to target first)
    Arrays.sort(cars, (a, b) -> Double.compare(b[0], a[0]));
    
    Stack<Double> stack = new Stack<>();
    for (double[] car : cars) {
        // 2. Defer (Push): Assume this car forms its own fleet
        stack.push(car[1]);
        
        // 1. Resolve (Pop): If this car takes less or equal time than the fleet ahead,
        // it catches up and merges into that fleet. We pop it out.
        if (stack.size() >= 2) {
            double currentCarTime = stack.pop();
            double fleetAheadTime = stack.peek();
            
            if (currentCarTime > fleetAheadTime) {
                // Cannot catch up, forms a new independent fleet. Push it back.
                stack.push(currentCarTime);
            }
        }
    }
    return stack.size();
}
```
* **Time Complexity:** $O(N \log N)$ - Sorting the cars by position takes $O(N \log N)$ time. The stack operations take $O(N)$ time.
* **Space Complexity:** $O(N)$ - For storing the `cars` array and the `stack`.

### 5. Largest Rectangle in Histogram (LeetCode 84)
> **Question:** Given an array of integers `heights` representing the histogram's bar height where the width of each bar is `1`, return the area of the largest rectangle in the histogram.
> 
> **Example:** `heights = [2,1,5,6,2,3]` -> **Output:** `10`
> Explanation: The largest rectangle is shown in the histogram, which has an area = 10 units.

```java
public int largestRectangleArea(int[] heights) {
    int maxArea = 0;
    Stack<Integer> stack = new Stack<>();
    
    for (int i = 0; i <= heights.length; i++) {
        int currentHeight = (i == heights.length) ? 0 : heights[i];
        
        // 1. Resolve (Pop): The current bar is shorter, trapping previous taller bars
        while (!stack.isEmpty() && currentHeight < heights[stack.peek()]) {
            int height = heights[stack.pop()];
            int width = stack.isEmpty() ? i : i - stack.peek() - 1;
            maxArea = Math.max(maxArea, height * width);
        }
        // 2. Defer (Push): Wait for a shorter boundary
        stack.push(i);
    }
    return maxArea;
}
```
* **Time Complexity:** $O(N)$ - Every bar is pushed and popped from the monotonic stack at most once.
* **Space Complexity:** $O(N)$ - In the worst-case scenario (a strictly increasing histogram), the stack will store all $N$ indices.

### 6. Maximal Rectangle (LeetCode 85)
> **Question:** Given a `rows x cols` binary `matrix` filled with `0`'s and `1`'s, find the largest rectangle containing only `1`'s and return its area.
> 
> **Example:** `matrix = [["1","0","1","0","0"],["1","0","1","1","1"],["1","1","1","1","1"],["1","0","0","1","0"]]`
> **Output:** `6`

```java
class Solution {
    public int maximalRectangle(char[][] matrix) {
        if (matrix == null || matrix.length == 0 || matrix[0].length == 0) return 0;
        
        int cols = matrix[0].length;
        int[] heights = new int[cols];
        int maxArea = 0;
        
        // Process the matrix row by row
        for (char[] row : matrix) {
            for (int j = 0; j < cols; j++) {
                if (row[j] == '1') {
                    heights[j]++;
                } else {
                    heights[j] = 0;
                }
            }
            // Reuse the Largest Rectangle in Histogram logic
            maxArea = Math.max(maxArea, largestRectangleArea(heights));
        }
        
        return maxArea;
    }
    
    private int largestRectangleArea(int[] heights) {
        int maxArea = 0;
        Stack<Integer> stack = new Stack<>();
        
        for (int i = 0; i <= heights.length; i++) {
            int currentHeight = (i == heights.length) ? 0 : heights[i];
            while (!stack.isEmpty() && currentHeight < heights[stack.peek()]) {
                int height = heights[stack.pop()];
                int width = stack.isEmpty() ? i : i - stack.peek() - 1;
                maxArea = Math.max(maxArea, height * width);
            }
            stack.push(i);
        }
        return maxArea;
    }
}
```
* **Time Complexity:** $O(R \times C)$ - Where $R$ is the number of rows and $C$ is the number of columns. Calculating the histogram for a row takes $O(C)$ time, and the monotonic stack evaluation takes $O(C)$ time. We do this for all $R$ rows.
* **Space Complexity:** $O(C)$ - The `heights` array and the `stack` take space proportional to the number of columns.

---

## Sub-Pattern 4: Graph Traversal (Iterative DFS)
**The Pop Trigger:** Processing the top node.
**Core Goal:** Simulating the system call stack explicitly to traverse a graph.

### 7. Iterative Depth-First Search (Template)
> **Goal:** Traverse a graph deeply before exploring neighbors, without using recursive function calls (which risk `StackOverflowError` on massive graphs).

```java
public void iterativeDFS(Node start) {
    Stack<Node> stack = new Stack<>();
    Set<Node> visited = new HashSet<>();
    
    // Defer the starting node
    stack.push(start);
    
    while (!stack.isEmpty()) {
        // Resolve (Pop): Process the most recently deferred node
        Node curr = stack.pop();
        
        if (!visited.add(curr)) continue; 
        
        // --> Process current node logic goes here <--
        
        // Defer (Push): Add all unvisited neighbors to the stack
        for (Node neighbor : curr.neighbors) {
            if (!visited.contains(neighbor)) {
                stack.push(neighbor);
            }
        }
    }
}
```
* **Time Complexity:** $O(V + E)$ - Where $V$ is vertices (nodes) and $E$ is edges. We visit every node and check every edge connecting them.
* **Space Complexity:** $O(V)$ - The `visited` set and the `stack` can store up to $V$ nodes in the worst case (e.g., a straight-line graph).
