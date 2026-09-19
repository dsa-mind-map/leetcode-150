# 📚 Linked List Master Problems: Problem Directory

Welcome to the problem directory! This guide serves as a central hub to navigate through the 12 core LeetCode linked list problems, categorized by their structural pillars, common pitfalls, and optimized Java implementations.

---

## 🔗 Quick Navigation Links

### 🚀 Phase 1: Fundamentals & Pointers
1. **[Linked List Cycle (LeetCode 141)](problems/lc_141_linked_list_cycle.md)**
   * **Pillar:** Fast & Slow Pointers
   * **Key Focus:** Detecting reference equality without value collision false positives.
2. **[Remove Nth Node From End of List (LeetCode 19)](problems/lc_019_remove_nth_node_from_end.md)**
   * **Pillar:** Sentinel Pattern & Fast Gap
   * **Key Focus:** Sliding window distance calculation for single-pass deletion.
3. **[Find the Duplicate Number (LeetCode 287)](problems/lc_287_find_the_duplicate_number.md)**
   * **Pillar:** Array as Cycle Pointer
   * **Key Focus:** Pigeonhole principle mapped to Floyd's Cycle Detection algorithm.
4. **[Reverse Linked List (LeetCode 206)](problems/lc_206_reverse_linked_list.md)**
   * **Pillar:** Pointer Re-routing (3-Step Dance)
   * **Key Focus:** Saving forward references before mutating pointers.

### 🔀 Phase 2: Sub-List Manipulation & Re-routing
5. **[Reverse Linked List II (LeetCode 92)](problems/lc_092_reverse_linked_list_ii.md)**
   * **Pillar:** Surgical Sub-List Extraction
   * **Key Focus:** Patching boundaries before and after range reversal.
6. **[Reverse Nodes in k-Group (LeetCode 25)](problems/lc_025_reverse_nodes_in_k_group.md)**
   * **Pillar:** State Management & Count Checks
   * **Key Focus:** Leaving remaining nodes untouched when length $< k$.
7. **[Merge Two Sorted Lists (LeetCode 21)](problems/lc_021_merge_two_sorted_lists.md)**
   * **Pillar:** Sentinel Combiner
   * **Key Focus:** Appending remainder lists efficiently.

### ➕ Phase 3: Composite Workflows & Advanced LLD
8. **[Add Two Numbers (LeetCode 2)](problems/lc_002_add_two_numbers.md)**
   * **Pillar:** Combiner with Math Logic
   * **Key Focus:** Propagating carry bounds past individual list terminations.
9. **[Reorder List (LeetCode 143)](problems/lc_143_reorder_list.md)**
   * **Pillar:** Find Mid $\rightarrow$ Reverse $\rightarrow$ Merge
   * **Key Focus:** Severing the first half list reference to prevent cycles.
10. **[Copy List with Random Pointer (LeetCode 138)](problems/lc_138_copy_list_with_random_pointer.md)**
    * **Pillar:** Multi-Pass Interweaving
    * **Key Focus:** Achieving $O(1)$ space deep copies via neighbor mapping.
11. **[Merge k Sorted Lists (LeetCode 23)](problems/lc_023_merge_k_sorted_lists.md)**
    * **Pillar:** Min-Heap Combiner
    * **Key Focus:** Handling empty head arrays and preventing time-limit exceptions.
12. **[LRU Cache (LeetCode 146)](problems/lc_146_lru_cache.md)**
    * **Pillar:** Doubly Linked Lists & Hashing
    * **Key Focus:** Low-level design invariants for $O(1)$ lookups and updates.

---
*Tip: Select any of the links above to view the detailed problem breakdown, code implementation, and common pitfalls.*
