# 42. Trapping Rain Water

**Difficulty:** Hard  
**Topic:** Arrays / Prefix & Suffix Ledgers (Two Pointers Pattern)

---

## 🧠 The Mental Model: The Physics of Trapped Water

When approaching this problem, we look at it index by index. If you imagine yourself standing on top of a single building at index `i`, you must ask yourself: *"How much water can stay directly on top of my head without spilling?"*

Your analysis of this physical reality is perfectly accurate and forms the exact logic of the algorithm:

1. **The Condition to Trap Water:** Water will only be trapped on top of your building *if* there is a building taller than yours somewhere to your left, **AND** a building taller than yours somewhere to your right. If either side is missing a taller wall, the water just slides off.
2. **The Quantity (The Overflow Limit):** How much water will stay? The water level rises until it hits the shorter of the two tallest boundaries. It can only go up to the `Math.min(tallestLeft, tallestRight)`. Any drop of water above that minimum boundary will overflow and spill out.
3. **Subtracting the Building (Displacement):** The water level tells us the total height of the "column" (Water + Building). To find *just* the water, we take that total water level and subtract the physical size of the building itself. If your building has a height of zero, you subtract zero (the whole column is water).

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

        for(int i=1; i<n; i++){
            // previous LeftMax vs previous Element
            leftMax[i] = Math.max(leftMax[i-1], h[i-1]);
        }

        int[] rightMax = new int[n];
        rightMax[n-1] = 0; // no element right to the last element

        for(int i=n-2; i>=0; i--){
            // previous rightMax vs previous Element
            rightMax[i] = Math.max(rightMax[i+1], h[i+1]);
        }

        for(int i=0; i<n; i++){

            // this could be negative 
            int v = Math.min(leftMax[i], rightMax[i]) - h[i];

            if(v > 0) 
                totalWater = totalWater + v;

        }

        return totalWater;

    }
}
