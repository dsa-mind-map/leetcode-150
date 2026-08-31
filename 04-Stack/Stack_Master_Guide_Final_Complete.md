# Stack Pattern: Master Reference Guide (Tier 1 & 2 Optimized)

Every stack problem operates on a single overarching lifecycle:
*   **Defer (Push):** An element cannot be fully processed yet, so it is put on hold.
*   **Resolve (Pop):** A specific "trigger" condition arrives, finalizing the state of the deferred elements.

There are four distinct sub-patterns based on what the "trigger" is.

## The "Defer & Resolve" Master Comparison Table

| Sub-Pattern | Problem | What triggers a `Pop` (Resolve)? | What triggers a `Push` (Defer)? |
| :--- | :--- | :--- | :--- |
| **1. Matching & Tracking** | Valid Parentheses | Matching closing bracket. | Every opening bracket. |
| | Min Stack | Explicit user `pop()` command. | Explicit user `push()` command. |
| **2. Expression Parsing** | Evaluate RPN | Mathematical operator (`+`, `-`, `*`, `/`). | Every number. |
| | Simplify Path | Reaching a slash `/` (process directory component). | Every directory name or token. |
| **3. Monotonic Stack** | Daily Temperatures | Strictly **greater** element. | Every element. |
| | Largest/Maximal Rectangle| Strictly **smaller** element. | Every element. |
| | Car Fleet | Car time $\le$ fleet ahead. | Every car's arrival time. |
| | Asteroid Collision | Opposite signs with smaller/equal size magnitude. | Every asteroid. |
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

### SOLUTION APPROACH :
> Push the expected closing symbol.
> 
> if current character is closing bracket then compare with popped character.
> 
> stack.isEmpty() → there is no opening bracket to match → invalid.
> 
> stack.pop() != ch → the most recent expected closing bracket is different from the current one → invalid.
> 
> Reject on mismatch.
> 
> Continue with next character, if closing character then pop & compare. If open character then push expected closing.

```java
 s = "()[]{}"
 char = (   stack = [)]
 char = )   stack = []
 char = [   stack = []]
 char = ]   stack = []
 char = {   stack = [}]
 char = }   stack = []
```
---

```java
class Solution {
    public boolean isValid(String s) {
        
        Stack<Character> stack = new Stack<>();

        for(char ch : s.toCharArray()){

            // open bracket -> push closed one.
            if(ch == '(') stack.push(')'); 
            if(ch == '{') stack.push('}'); 
            if(ch == '[') stack.push(']'); 

            // closed bracket
            if(ch == ')' || ch == '}' || ch == ']' ){

                if(stack.isEmpty()){ // closed bracket has nothing to match with.
                    return false;
                }
                if(stack.pop() != ch){ // closed bracket isnot equal to top of the stack.
                    return false;
                }

            }
        }

        return stack.isEmpty();
    }
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

> **Example 1:**
> 
> Input
> ["MinStack","push","push","push","getMin","pop","top","getMin"]
> [[],[-2],[0],[-3],[],[],[],[]]
> 
> Output
> [null,null,null,null,-3,null,0,-2]
> 
> Explanation
> * MinStack minStack = new MinStack();
> * minStack.push(-2);
> * minStack.push(0);
> * minStack.push(-3);
> * minStack.getMin(); // return -3
> * minStack.pop();
> * minStack.top();    // return 0
> * minStack.getMin(); // return -2

```java
class MinStack {
    Stack<int[]> stack = new Stack<>();
    public void push(int val) {
        if (stack.isEmpty()) stack.push(new int[]{val, val});
        else stack.push(new int[]{val, Math.min(val, stack.peek()[1])});
    }
    public void pop() { stack.pop(); }
    public int top() { return stack.peek()[0]; }
    public int getMin() { return stack.peek()[1]; }
}
```
* **Time Complexity:** $O(1)$ for all operations.
* **Space Complexity:** $O(N)$ - We store $N$ elements along with their historical minimums.

---

## Sub-Pattern 2: Expression & String Parsing
**The Pop Trigger:** A mathematical operator (`+`, `-`, `*`, `/`) or directory navigation token (`..`).
**Core Goal:** Collapsing multiple operands or resolving path hierarchies into one result.

### 3. Evaluate Reverse Polish Notation (LeetCode 150)
> **Question:** You are given an array of strings `tokens` that represents an arithmetic expression in Reverse Polish Notation. Evaluate the expression.

```java
public int evalRPN(String[] tokens) {
    Stack<Integer> stack = new Stack<>();
    for (String t : tokens) {
        if ("+-*/".contains(t)) {
            int b = stack.pop();
            int a = stack.pop();
            if (t.equals("+")) stack.push(a + b);
            else if (t.equals("-")) stack.push(a - b);
            else if (t.equals("*")) stack.push(a * b);
            else if (t.equals("/")) stack.push(a / b);
        } else {
            stack.push(Integer.parseInt(t));
        }
    }
    return stack.pop();
}
```
* **Time Complexity:** $O(N)$
* **Space Complexity:** $O(N)$;

### 3.5 Simplify Path (LeetCode 71)
> **Question:** Given a string `path`, which is an absolute path (starting with a slash `'/'`) to a file or directory in a Unix-style file system, convert it to the simplified canonical path.
> In a Unix-style file system, a period `'.'` refers to the current directory, a double period `'..'` refers to the directory up a level, and any multiple consecutive slashes (e.g. `'//'`) are treated as a single slash `'/'`. For this problem, any other format of periods such as `'...'` are treated as file/directory names.
> The canonical path should have the following format:
> * The path starts with a single slash `'/'`.
> * Any two directories are separated by a single slash `'/'`.
> * The path does not end with a trailing `'/'`.
> * The path only contains the directories on the path from the root directory to the target file or directory (i.e., no period `'.'` or double period `'..'`).
> 
> Return the simplified canonical path.
> 
> **Example 1:** `path = "/home/"` -> **Output:** `"/home"`
> **Example 2:** `path = "/../"` -> **Output:** `"/"`

```java
public String simplifyPath(String path) {
    Stack<String> stack = new Stack<>();
    String[] components = path.split("/");
    
    for (String dir : components) {
        if (dir.equals("") || dir.equals(".")) {
            continue; // Ignore empty spaces and current directory symbols
        } else if (dir.equals("..")) {
            // Resolve (Pop): Go up a directory level if stack is not empty
            if (!stack.isEmpty()) {
                stack.pop();
            }
        } else {
            // Defer (Push): Valid directory name pushed to stack
            stack.push(dir);
        }
    }
    
    // Reconstruct the path from the stack
    StringBuilder result = new StringBuilder();
    for (String dir : stack) {
        result.append("/").append(dir);
    }
    
    return result.length() == 0 ? "/" : result.toString();
}
```
* **Time Complexity:** $O(N)$ - Where $N$ is the length of the string. We split by slashes and iterate through components once.
* **Space Complexity:** $O(N)$ - For storing the split components and the stack.


---

## Sub-Pattern 3: Monotonic Stack & Collision Resolution
**The Pop Trigger:** A strictly greater/smaller element or an opposing directional collision.
**Core Goal:** Finding spatial boundaries, timeline conditions, or resolving collisions in $O(N)$ time.

### 4. Daily Temperatures (LeetCode 739)
```java
public int[] dailyTemperatures(int[] temperatures) {
    int[] ans = new int[temperatures.length];
    Stack<Integer> stack = new Stack<>();
    for (int i = 0; i < temperatures.length; i++) {
        while (!stack.isEmpty() && temperatures[i] > temperatures[stack.peek()]) {
            int prevIndex = stack.pop();
            ans[prevIndex] = i - prevIndex;
        }
        stack.push(i);
    }
    return ans;
}
```
* **Time Complexity:** $O(N)$
* **Space Complexity:** $O(N)$

### 4.5 Car Fleet (LeetCode 853)
```java
public int carFleet(int target, int[] position, int[] speed) {
    int n = position.length;
    double[][] cars = new double[n][2];
    for (int i = 0; i < n; i++) {
        cars[i][0] = position[i];
        cars[i][1] = (double) (target - position[i]) / speed[i];
    }
    Arrays.sort(cars, (a, b) -> Double.compare(b[0], a[0]));
    Stack<Double> stack = new Stack<>();
    for (double[] car : cars) {
        stack.push(car[1]);
        if (stack.size() >= 2) {
            double currentCarTime = stack.pop();
            double fleetAheadTime = stack.peek();
            if (currentCarTime > fleetAheadTime) {
                stack.push(currentCarTime);
            }
        }
    }
    return stack.size();
}
```
* **Time Complexity:** $O(N \log N)$
* **Space Complexity:** $O(N)$

### 4.6 Asteroid Collision (LeetCode 735)
> **Question:** We are given an array `asteroids` of integers representing asteroids in a row. The indices of the asteriod in the array represent their relative position in space.
> For each asteroid, its absolute value represents its size, and the sign represents its direction (positive meaning right, negative meaning left). Each asteroid moves at the same speed.
> Find out the state of the asteroids after all collisions. If two asteroids meet, the smaller one will explode. If both are the same size, both will explode. Two asteroids moving in the same direction will never meet.
> 
> **Example 1:** `asteroids = [5,10,-5]` -> **Output:** `[5,10]`
> Explanation: The 10 and -5 collide resulting in 10. The 5 and 10 never collide.
> **Example 2:** `asteroids = [8,-8]` -> **Output:** `[]`
> Explanation: The 8 and -8 collide exploding each other.

```java
public int[] asteroidCollision(int[] asteroids) {
    Stack<Integer> stack = new Stack<>();
    
    for (int ast : asteroids) {
        boolean destroyed = false;
        
        // Resolve (Pop): Collision happens only when a moving-left asteroid (-) meets a moving-right asteroid (+)
        while (!stack.isEmpty() && ast < 0 && stack.peek() > 0) {
            if (stack.peek() < -ast) {
                stack.pop(); // The top asteroid is smaller, it explodes. Loop continues.
                continue;
            } else if (stack.peek() == -ast) {
                stack.pop(); // Both asteroids explode
            }
            destroyed = true; // Current asteroid is destroyed
            break;
        }
        
        // Defer (Push): If current asteroid survives all collisions, push it to stack
        if (!destroyed) {
            stack.push(ast);
        }
    }
    
    int[] result = new int[stack.size()];
    for (int i = result.length - 1; i >= 0; i--) {
        result[i] = stack.pop();
    }
    return result;
}
```
* **Time Complexity:** $O(N)$ - Each asteroid is pushed and popped at most once.
* **Space Complexity:** $O(N)$ - For the stack holding surviving asteroids.


### 5. Largest Rectangle in Histogram (LeetCode 84)
```java
public int largestRectangleArea(int[] heights) {
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
```
* **Time Complexity:** $O(N)$
* **Space Complexity:** $O(N)$

### 6. Maximal Rectangle (LeetCode 85)
```java
class Solution {
    public int maximalRectangle(char[][] matrix) {
        if (matrix == null || matrix.length == 0 || matrix[0].length == 0) return 0;
        int cols = matrix[0].length;
        int[] heights = new int[cols];
        int maxArea = 0;
        for (char[] row : matrix) {
            for (int j = 0; j < cols; j++) {
                if (row[j] == '1') heights[j]++;
                else heights[j] = 0;
            }
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
* **Time Complexity:** $O(R \times C)$
* **Space Complexity:** $O(C)$

---

## Sub-Pattern 4: Graph Traversal (Iterative DFS)
```java
public void iterativeDFS(Node start) {
    Stack<Node> stack = new Stack<>();
    Set<Node> visited = new HashSet<>();
    stack.push(start);
    while (!stack.isEmpty()) {
        Node curr = stack.pop();
        if (!visited.add(curr)) continue;
        for (Node neighbor : curr.neighbors) {
            if (!visited.contains(neighbor)) {
                stack.push(neighbor);
            }
        }
    }
}
```
* **Time Complexity:** $O(V + E)$
* **Space Complexity:** $O(V)$
