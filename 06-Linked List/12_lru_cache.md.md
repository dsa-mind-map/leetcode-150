# 12. LRU Cache (LeetCode 146)

**Alignment:** Pillar 6 (Doubly Linked Lists & Hashing)  
**Additional Learning:** Low-Level Design (LLD). A HashMap provides $O(1)$ key lookups, while a custom Doubly Linked List provides $O(1)$ positional updates.

---

## 📋 Problem Description
Implement the Least Recently Used (LRU) cache class `LRUCache` with `get(key)` and `put(key, value)` methods running in $O(1)$ average time complexity.

**Constraints:**
* $1 \le \text{capacity} \le 3000$

---

## ⚠️ Common Pitfalls
Managing bi-directional wiring poorly. Breaking `remove()` and `insertAtHead()` operations into clean helper methods prevents spaghetti code and keeps pointers synced.

---

## 💻 Java Implementation

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

        head = new Node(0, 0);
        tail = new Node(0, 0);

        head.next = tail;
        tail.prev = head;  
    }
    
    public int get(int key) {
        if(!map.containsKey(key)) return -1;

        Node node = map.get(key);
        remove(node);
        insertAtHead(node);
        return node.val;
    }
    
    public void put(int key, int value) {
        if(map.containsKey(key)){
            Node node = map.get(key);
            node.val = value;
            remove(node);
            insertAtHead(node);
        } else {
            if(map.size() == capacity){
                Node lru = tail.prev;
                remove(lru); 
                map.remove(lru.key); 
            }
            Node node = new Node(key, value);
            insertAtHead(node); 
            map.put(key, node); 
        }
    }

    void remove(Node node){
        node.prev.next = node.next;
        node.next.prev = node.prev;
    }

    void insertAtHead(Node node){
        node.next = head.next; 
        node.prev = head;      

        head.next.prev = node; 
        head.next = node;      
    }
}