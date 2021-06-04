---
title: LRU缓存函数
date: 2021-06-04 19:23:34
tags: 前端
categories:
- 前端
---
+ 写一个LRU 缓存函数。
```java
class LRUCache {
    class DLinkedNode {
        int key;
        int value;
        DLinkedNode pre;
        DLinkedNode next;
        public DLinkedNode(){}
        public DLinkedNode(int key, int value){
            this.key = key;
            this.value = value;
        }
    }   
    private Map<Integer, DLinkedNode> cache = new Map();
    private int size;
    private int capacity;
    private DLinkedNode head, tail;
    public LRUCache(int capacity){
        this.size = 0;
        this.capacity =capacity;
        
        // 使用伪头部和伪尾部节点
        head = new DLinkedNode();
        tail = new DLinkedNode();
        head.next = tail;
        tail.pre = head;
    }
   
    public int get(int key){ 
        DLinkedNode node = cache.get(key);
        if(node == null){
            return -1;
        }
        // 如果 key 存在，先通过哈希表定位，再移到头部
        moveToHead(node);
        return node.value;
    }
   
    public void put(int key, int value){
        DLinkedNode node = cache.get(key);
        if(node == null){
            DLinkNode newNode = new DLinkNode(key, value);
            cache.put(key, newNode);
            ++ size;
            if(size > capacity) {
                DLinkedNode tail = removeTail();
                cache.remove(tail.key);
                moveToHead(node);
            }
        }
    }
    
    private void addToHead(DLinkedNode node){
        node.pre = head;
        node.next = head.next;
        head.next.pre = node;
        head.next = node;
    }
    
    private void removeNode(DLinkedNode node){
        node.pre.next = node.next;
        node.next.pre = node.pre;
    }
    
    private void moveToHead(DLinkedNode node){
        removeNode(node);
        addToHead(node);
    }
    
    private DLinkedNode removeTail(){
        DLinkedNode res = tail.pre;
        removeNode(res);
        return res;
    }
}
```
