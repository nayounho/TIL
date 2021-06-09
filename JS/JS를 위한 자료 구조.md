# JS를 위한 자료 구조

## 1. stack
  - LIFO(Last In, First Out) 구조(선입후출)
  - 괄호 매칭이나 구간합을 구하는 데 응용 
  - 
```
const stack = [];

stack.push(1)
stack.push(2)

console.log(stack) // [1,2]

stack.pop(2)

console.log(stack) // [1]
```

## 2. Queue
  - FIFO(First In, First Out) 구조(선입선출)
  
```
const queue = [];

queue.push(1)
queue.push(2)

console.log(queue) //[1,2]

queue.shift()

console.log(queue) //[2]
```

## 3. Linked List
  - 포인터가 없는 Linked List를 자바스크립트에서 객체를 참조하는 방식으로 구현 

```
function Node(val) {
  this.val = val;
  this.next = null;
}

let head = new Node(0);
let node1 = new Node(1);
let node2 = new Node(2);

head.next = node1;
node1.next = node2;
```

## 4. Tree
  - 보통 이진트리(Binary Tree)를 많이 구현
  - 방법은 2가지
      - 배열
      - 연결 리스트