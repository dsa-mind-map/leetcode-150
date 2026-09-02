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
> 1. open bracket -> push closed one.
> 
> 2. closed bracket
> 
> * stack empty-> means closed bracket has nothing to match with. -> return false.
> * stack not empty and closed bracket is not equal to top of the stack -> return false.


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

    Stack<Integer> values;
    Stack<Integer> mins;

    public MinStack() {
        // values = push input values as they are coming
        values = new Stack<>();

        // find min of "min of previous values" and "current value"
        mins = new Stack<>();
    }
    
    public void push(int val) {

        // first value
        if(values.isEmpty()){
            values.push(val);
            mins.push(val);
        }else{
            values.push(val);

            // "min of previous values"
            int previousMin = mins.peek();
            // find min of "min of previous values" and "current value"
            mins.push(Math.min(val, previousMin)); 
        }
        
    }
    
    public void pop() {
        // not empty check
        
        // pop from both
        if(!values.isEmpty()){
            values.pop();
        }
        if(!mins.isEmpty()){
            mins.pop();
        }
    }
    
    public int top() {
        // top only from "values" stack

        return values.peek();    // no need of empty check
    }
    
    public int getMin() {
        // min only from "mins" stack

        return mins.peek();      // no need of empty check
    }
}


```

```java
class MinStack {
    // array of size 2 = [value, min-value]
    Stack<int[]> stack;

    public MinStack() {
        stack = new Stack<>();
    }
    
    public void push(int val) {
        
        // first value
        if(stack.isEmpty()){

            stack.push(new int[]{val, val}); // current value is "min value"

        }else{

            int previousMin = stack.peek()[1]; 
            int minValue = Math.min(previousMin, val); // min of "current" and "previous"

            stack.push(new int[]{val, minValue});

        }
        
    }
    
    public void pop() {
        if(!stack.isEmpty()){
            stack.pop();
        }
        
    }
    
    public int top() {
        return stack.peek()[0]; // 0th index has value
    }
    
    public int getMin() {
        return stack.peek()[1]; // 1th index has min value
        
    }
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
>
---
> **TIPS**
> use "+-*/".contains(str)
>
> use if-else block instead of multiple if-blocks

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
```java
class Solution {
    public int evalRPN(String[] tokens) {

        Stack<Integer> stack = new Stack<>();
        
        for(String str : tokens){
            // operators
            if(str.equals("+") || str.equals("-") || str.equals("*") || str.equals("/") ){

                int second = stack.pop();
                int first = stack.pop();

                if(str.equals("+")){
                    stack.push(first + second);
                }
                if(str.equals("-")){
                    stack.push(first - second);
                }
                if(str.equals("*")){
                    stack.push(first * second);
                }
                if(str.equals("/")){
                    stack.push(first / second);
                }

            }else{
                stack.push(Integer.parseInt(str));
            }

        }

        return stack.peek();
    }
}


```
* **Time Complexity:** $O(N)$
* **Space Complexity:** $O(N)$;

### 3.5 Simplify Path (LeetCode 71)
> You are given an absolute path for a Unix-style file system, which always begins with a slash '/'. Your task is to transform this absolute path into its simplified canonical path.
> 
> The rules of a Unix-style file system are as follows:
> 
> * A single period '.' represents the current directory.
> * A double period '..' represents the previous/parent directory.
> * Multiple consecutive slashes such as '//' and '///' are treated as a single slash '/'.
> * Any sequence of periods that does not match the rules above should be treated as a valid directory or file name. For example, '...' and '....' are valid directory or file names.
>
> The simplified canonical path should follow these rules:
>  
> * The path must start with a single slash '/'.
> * Directories within the path must be separated by exactly one slash '/'.
> * The path must not end with a slash '/', unless it is the root directory.
> * The path must not have any single or double periods ('.' and '..') used to denote current or parent directories.
> * Return the simplified canonical path.
> 
>
> Example 2:
> 
> * Input: path = "/home//foo/"
> * 
> * Output: "/home/foo"
> * 
> * Explanation:
> * 
> * Multiple consecutive slashes are replaced by a single one.
> 
> Example 3:
> 
> * Input: path = "/home/user/Documents/../Pictures"
> * 
> * Output: "/home/user/Pictures"
> * 
> * Explanation:
> * 
> * A double period ".." refers to the directory up a level (the parent directory).
> 
> Example 4:
> 
> * Input: path = "/../"
> * 
> * Output: "/"
> * 
> * Explanation:
> * 
> * Going one level up from the root directory is not possible.
> 
> Example 5:
> 
> * Input: path = "/.../a/../b/c/../d/./"
> * 
> * Output: "/.../b/d"
> * 
> * Explanation:
> * 
> * "..." is a valid name for a directory in this problem.
> 
>  
> 
> Constraints:
> 
> * 1 <= path.length <= 3000
> * path consists of English letters, digits, period '.', slash '/' or '_'.
> * path is a valid absolute Unix path.

> **TIPS**
> split by "/"
>
> Use ArrayDeque so that we can pop from "First"
>
> StringBuilder to build convert to result string
>
> If result string is of size = 0 then return "/"
>
> **STACK is synchronized while DEQUE ( ArrayDeque) is not synchronized ( faster) .**

```java
class Solution {
    public String simplifyPath(String path) {

        String[] strs = path.split("/");

        Deque<String> deque = new ArrayDeque<>();

        for(String str: strs){
            
            // parent directory => move to previous directory
            if(str.equals("..")){
                if(!deque.isEmpty()){
                    deque.pollLast();
                }
            }else if(!str.isEmpty() && !str.equals(".")){
                // not empty
                // not "."

                // valid directory
                deque.addLast(str);
            }
        }

        // Reconstruct the path from the stack
        StringBuilder result = new StringBuilder();
        for (String dir : deque) {
            result.append("/").append(dir);
        }
        
        return result.length() == 0 ? "/" : result.toString();

    }
}
```

```java
class Solution {
public String simplifyPath(String path) {

    Stack<String> stack = new Stack<>();
    String[] components = path.split("/");
    
    for (String dir : components) {
        // parent directory => move to previous directory
        if(dir.equals("..")){
            if(!stack.isEmpty()){
                stack.pop();
            }
        }else if(!dir.isEmpty() && !dir.equals(".")){
            // not empty
            // not "."

            // valid directory
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
}
```
* **Time Complexity:** $O(N)$ - Where $N$ is the length of the string. We split by slashes and iterate through components once.
* **Space Complexity:** $O(N)$ - For storing the split components and the stack.


---

## Sub-Pattern 3: Monotonic Stack (next greater or next smaller) & Collision Resolution
> You are given an array of integers temperatures where temperatures[i] represents the daily temperatures on the ith day.
> 
> Return an array result where result[i] is the number of days after the ith day before a warmer temperature appears on a future day. If there is no day in the future where a warmer temperature will appear for the ith day, set result[i] to 0 instead.
> 
> **Example 1:**
> 
> * Input: temperatures = [30,38,30,36,35,40,28]
> 
> * Output: [1,4,1,2,1,0,0]
> **Example 2:**
> 
> * Input: temperatures = [22,21,20]
> 
> * Output: [0,0,0]
> **Constraints:**
> 
> 1 <= temperatures.length <= 100,000.
> 
> 1 <= temperatures[i] <= 100

**The Pop Trigger:** A strictly **greater/smaller** element or an opposing directional collision. Stack is popped because next greater/smaller element popped.
**Core Goal:** Finding spatial boundaries, timeline conditions, or resolving collisions in $O(N)$ time.

**TIPS**
> 1 - traverse array left to right. ( i = 0 to n-1 )
>
> 2 - If (temperature of "ith" index) > (temperature of "peek of the stack" index ) then pop the index. Calculate (i-poppedIndex). Repeat this until stack is empty or temp["peek of the stack"] < temp[i]
>
> Add ith index to stack.

**KEY LEARNINGS**
> "peek of the stack" index is "stack.peek()" because stack store the index.
> 
> 

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
> There are n cars traveling to the same destination on a one-lane highway.
> 
> You are given two arrays of integers position and speed, both of length n.
> 
> position[i] is the position of the ith car (in miles)
> speed[i] is the speed of the ith car (in miles per hour)
> The destination is at position target miles.
> 
> A car can not pass another car ahead of it. It can only catch up to another car and then drive at the same speed as the car ahead of it.
> 
> A car fleet is a non-empty set of cars driving at the same position and same speed. A single car is also considered a car fleet.
> 
> If a car catches up to a car fleet the moment the fleet reaches the destination, then the car is considered to be part of the fleet.
> 
> Return the number of different car fleets that will arrive at the destination.
> 
> **Example 1:**
> 
> * Input: target = 10, position = [1,4], speed = [3,2]
> * 
> * Output: 1
> * Explanation: The cars starting at 1 (speed 3) and 4 (speed 2) become a fleet, meeting each other at 10, the destination.
> 
> **Example 2:**
> 
> * Input: target = 10, position = [4,1,0,7], speed = [2,2,1,1]
> * 
> * Output: 3
> * Explanation: The cars starting at 4 and 7 become a fleet at position 10. The cars starting at 1 and 0 never catch up to the car ahead of them. Thus, there are 3 car fleets that will arrive at the destination.
> 
> **Constraints:**
> 
> * n == position.length == speed.length.
> * 1 <= n <= 100,000
> * 0 < target <= 1,000,000
> * 1 <= speed[i] <= 1,000,000
> * 0 <= position[i] < target
> * All the values of position are unique.
> * 
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
