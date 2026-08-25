# 42. Trapping Rain Water

**Difficulty:** Hard  
**Topic:** Arrays / Prefix & Suffix Ledgers (Two Pointers Pattern)

---

## 📝 Problem Description

Given `n` non-negative integers representing an elevation map where the width of each bar is `1`, compute how much water it can trap after raining.

**Example:**
* **Input:** `height = [0,1,0,2,1,0,1,3,2,1,2,1]`
* **Output:** `6`

---

## 🧠 The Mental Model: The Physics of Trapped Water

When approaching this problem, we look at it index by index. If you imagine yourself standing on top of a single building at index `i`, you must ask yourself: *"How much water can stay directly on top of my head without spilling?"*

Here is the step-by-step breakdown of the physics:

### 1. The "Prerequisite" (The Condition)
> *"If I am standing at any point, water will be only tapped on top of my building (where I am standing) only if I have something taller than my own building in the left & also in the right (this is the condition to stay water)."*

**Review:** Perfect. You correctly identified that water requires a "bowl" or "valley" shape. If either the left or the right side is missing a taller wall, the bowl is broken, and the water just spills off the edge.

### 2. The "Bottleneck" (The Quantity)
> *"How much will it store... the water on top of my building can go above to the min of those 2 tallest buildings. After that it can overflow..."*

**Review:** This is the most critical part of the logic, and your word choice of **"overflow"** is the best way to describe it. A bowl can only hold water up to its lowest edge. By saying it will "overflow" past the minimum of the two tall buildings, you have perfectly translated the real-world physics of water into the math formula: `Math.min(leftMax, rightMax)`.

### 3. The "Displacement" (Subtracting the Building)
> *"My building has some size I need to subtract that size. If my building has zero height then my building volume is zero and I don't need to subtract anything."*

**Review:** Exactly. The `Math.min` calculation gives you the **surface level** of the water from the ground up. But the building itself is made of concrete and takes up space! Subtracting the building's size is necessary to find out how much empty space was actually left for the water to fill. Your edge case logic is also flawless: if the building size is `0`, subtracting `0` means the entire height from the ground to the surface level is pure water.

### Why We Need Exclusive Boundaries
Because we are looking **strictly** at the buildings to the left and strictly to the right, we do not include our own building in the left/right ledgers. 

**The Catch:** If we are standing on the tallest building in the city, the `min` boundary of our neighbors will be *lower* than our own building. When we subtract our building's height, the math yields a **negative number**! 
**The Fix:** Water volume cannot be negative. If the calculation is negative, it simply means our building is a peak and holds 0 water. The `if (v > 0)` check flawlessly handles this physical reality.

---

## 💻 The Code (Exclusive Boundaries)

```java
class Solution {
    public int trap(int[] h) {

        int totalWater = 0;
        
        int n = h.length;

        int[] leftMax = new int[n];
        leftMax[0] = 0; // no element left to the first element

        for(int i = 1; i < n; i++){
            // previous LeftMax vs previous Element
            leftMax[i] = Math.max(leftMax[i-1], h[i-1]);
        }

        int[] rightMax = new int[n];
        rightMax[n-1] = 0; // no element right to the last element

        for(int i = n-2; i >= 0; i--){
            // previous rightMax vs previous Element
            rightMax[i] = Math.max(rightMax[i+1], h[i+1]);
        }

        for(int i = 0; i < n; i++){
            // this could be negative 
            int v = Math.min(leftMax[i], rightMax[i]) - h[i];

            if(v > 0) 
                totalWater = totalWater + v;
        }

        return totalWater;
    }
}

````

---
# 42. Trapping Rain Water (Optimal Two-Pointer Solution)

**Difficulty:** Hard  
**Topic:** Arrays / Two Pointers (Dynamic Boundary Tracking)

---

## 🧠 The Mental Model: The "Bottleneck" Insight


🧠 The Two-Pointer Mental Model (In Plain English)
1. The Bottleneck Check: if (h[left] <= h[right])

What it means: If I am standing at the left pointer, water will never leak to the right. The right side acts as a guaranteed giant wall because it is at least as tall as my current position. The left side is my absolute bottleneck.

2. The Valley (Trapping Water): if (h[left] < leftMax)

What it means: Since the right side is safely sealed, I only need to check my local left wall (leftMax). If my current building is shorter than leftMax, a valley is created!

The Math: I calculate the trapped water and add it to the total: volume += leftMax - h[left].

3. The New Peak (Updating Boundaries): if (h[left] >= leftMax)

What it means: I am not in a valley; I am standing on a new mountain peak. My building cannot hold water on its roof, but it becomes the new tallest left boundary for everyone else.

The Action: I simply update the ledger: leftMax = h[left].

(Note: After processing either step 2 or 3, I take one step inward: left++)

4. The Reverse Symmetry: if (h[left] > h[right])

What it means: The left side is now the guaranteed giant wall, making the right side the bottleneck. The right pointer goes into action using the exact same rules, but in reverse.

The Action: I compare h[right] against rightMax, calculate water or update the boundary, and take one step backward: right--.

---


---

## 💻 The Code ($\mathcal{O}(1)$ Space)

```java
class Solution {
    public int trap(int[] height) {
        if (height == null || height.length == 0) return 0;

        int left = 0;
        int right = height.length - 1;
        
        int leftMax = 0;
        int rightMax = 0;
        
        int totalWater = 0;

        // The two guards walk toward each other until they meet
        while (left < right) {
            
            // Step 1: Identify the bottleneck side
            if (height[left] < height[right]) {
                
                // We are working on the left side. 
                // Is the current building a new boundary wall, or a valley?
                if (height[left] >= leftMax) {
                    leftMax = height[left]; // New tall wall, update boundary (holds no water)
                } else {
                    totalWater += leftMax - height[left]; // Valley! Add water.
                }
                left++; // Move left guard inward
                
            } else {
                
                // We are working on the right side.
                // Is the current building a new boundary wall, or a valley?
                if (height[right] >= rightMax) {
                    rightMax = height[right]; // New tall wall, update boundary (holds no water)
                } else {
                    totalWater += rightMax - height[right]; // Valley! Add water.
                }
                right--; // Move right guard inward
                
            }
        }

        return totalWater;
    }
}
