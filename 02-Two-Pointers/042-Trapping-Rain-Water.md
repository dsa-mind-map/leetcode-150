# 42. Trapping Rain Water

**Difficulty:** Hard  
**Topic:** Arrays / Prefix & Suffix Ledgers (Two Pointers Pattern)

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
