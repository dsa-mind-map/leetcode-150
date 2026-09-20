
# 12. LRU Cache (LeetCode 146)

**Alignment:** Pillar 6 (Doubly Linked Lists & Hashing)  
**Additional Learning:** Low-Level Design (LLD). A `HashMap` provides $O(1)$ key lookups, while a custom Doubly Linked List (DDL) provides $O(1)$ structural updates.

---

## 🏛️ DDL Invariants
1. `prev` is null **only** for the sentinel `head`.
2. `next` is null **only** for the sentinel `tail`.
3. All intermediate nodes have non-null `next` and `prev` pointers.
4. $O(1)$ insertion at head due to the sentinel head node.
5. $O(1)$ removal at tail due to the sentinel tail node.

---

## 📋 Problem Description
Implement the Least Recently Used (LRU) cache class `LRUCache`. It should support the following operations with $O(1)$ average time complexity:
* `LRUCache(int capacity)` — Initializes the LRU cache with positive size capacity.
* `int get(int key)` — Returns the value of the key if it exists, otherwise returns `-1`.
* `void put(int key, int value)` — Updates the value of the key if it exists, or adds it. If capacity is exceeded, evicts the least recently used key.

### Constraints
* $1 \le \text{capacity} \le 3000$
* $0 \le \text{key} \le 10^4$
* $0 \le \text{value} \le 10^5$
* At most $2 \times 10^5$ calls will be made to `get` and `put`.

---

## ⚠️ Common Pitfalls
Poor pointer management breaks bi-directional links. Isolate pointer manipulations into dedicated `remove()` and `insertAtHead()` helper methods to eliminate spaghetti code and maintain absolute pointer synchronization.

---

## 💻 Optimized Java Implementation

```java
class LRUCache {

    class Node {
        int key;
        int val;
        Node next;
        Node prev;

        Node(int key, int val){
            this.key = key;
            this.val = val; 
        }
    }

    Map<Integer, Node> map;
    Node head;
    Node tail;
    int capacity;

    public LRUCache(int capacity) {
        this.capacity = capacity;
        this.map = new HashMap<>(capacity);

        // Sentinel dummy nodes eliminate edge cases during insertion/deletion
        head = new Node(0, 0);
        tail = new Node(0, 0);

        head.next = tail;
        tail.prev = head;  
    }
    
    public int get(int key) {
        if (!map.containsKey(key)) return -1;

        Node node = map.get(key);
        remove(node);
        insertAtHead(node); // CRITICAL: Accessing makes it the Most Recently Used node
        return node.val;
    }
    
    public void put(int key, int value) {
        if (map.containsKey(key)) {
            Node node = map.get(key);
            node.val = value; // Update value
            
            remove(node);
            insertAtHead(node); // CRITICAL: Updating makes it the Most Recently Used node
        } else {
            if (map.size() == capacity) {
                Node lru = tail.prev; // CRITICAL: LRU node always sits right before sentinel tail
                remove(lru); 
                map.remove(lru.key); // Evict from hashmap
            }
            Node node = new Node(key, value);
            insertAtHead(node); 
            map.put(key, node); 
        }
    }

    void remove(Node node){
        node.prev.next = node.next; // CRITICAL: Bypass target node forward
        node.next.prev = node.prev; // CRITICAL: Bypass target node backward
    }

    void insertAtHead(Node node){
        node.next = head.next;      // CRITICAL: Point new node to old first node
        node.prev = head;           // CRITICAL: Point new node to sentinel head

        head.next.prev = node;      // CRITICAL: Point old first node back to new node
        head.next = node;           // CRITICAL: Point sentinel head forward to new node
    }
}

```

```

```
