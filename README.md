## List

데이터 만큼의 공간을 가져서 효율적으로 사용 가능

1. singly linked list (단방향 / 이전 노드 알 수 X)

   ```c++
   #include <iostream>
   
   class Node {
   public:
       int data;
       Node* next;
       Node() {
           next = NULL;
       }
   };
   
   Node *head;
   Node *tail;
   
   void insertNode(int data) {
       Node *nNode = new Node;
       tail -> next = nNode;
       nNode -> data = data;
       tail = nNode;
   }
   
   void deleteNode(Node *node) {
       Node *tmp = head;
   
       while(tmp -> next != NULL) {
           if(tmp -> next == node) {
               tmp -> next = node -> next;
           }
           tmp = tmp -> next;
       }
   }
   ```

   > ```c++
   > struct Node {
   > 	int data;
   > 	Node * next;
   > };
   > 
   > void addFirst(int key) {
   > 	Node * new_node = (Node*)malloc(sizeof(Node));
   > 	new_node->data = key;
   > 	new_node->next = NULL;
   > 
   > 	// Check first element
   > 	if (list == NULL) {
   > 		list = new_node;
   > 	} else {
   > 		// Add new node to head
   > 		new_node->next = list;
   > 		list = new_node;
   > 	}
   > }
   > 
   > void ascending_order_add(int key) {
   > 	Node * new_node = (Node*)malloc(sizeof(Node));
   > 	new_node->data = key;
   > 	new_node->next = NULL;
   > 
   > 	if (list == NULL) {
   > 		list = new_node;
   > 	} else {
   > 		Node * cur = list;
   > 		Node * prev = NULL;
   > 
   > 		if (cur->data > new_node->data) { // If first element is larger than key
   > 			new_node->next = cur;
   > 			list = new_node;
   > 		} else {
   > 			while (cur != NULL && cur->data < new_node->data) {
   > 				prev = cur;
   > 				cur = cur->next;
   > 			}
   > 			
   > 			if (cur != NULL) { // Add in middle
   > 				new_node->next = cur;
   > 				prev->next = new_node;
   > 			} else { // Add to end
   > 				prev->next = new_node;
   > 			}
   > 		}
   > 	}
   > }
   > 
   > bool remove(int key) {
   > 	if (list == NULL) return false;
   > 
   > 	if (list->data == key) {
   > 		Node * cur = list;
   > 		list = list->next;
   > 		free(cur);
   > 		return true;
   >   } else {
   > 		Node * cur = list->next;
   > 		Node * prev = list;
   >     
   > 		while (cur != NULL && cur->data != key) {
   > 			prev = cur;
   > 			cur = cur->next;
   > 		}
   > 
   > 		if (cur == NULL) return false;
   > 
   > 		prev->next = cur->next;
   > 		free(cur);
   >     
   > 		return true;
   > 	}
   > }
   > ```

2. doubly linked list (양방향)

   ```c++
   #include <iostream>
   
   class Node {
   public:
       int data;
       Node* prev;
       Node* next;
       Node() {
           prev = NULL;
           next = NULL;
       }
   };
   
   Node *head;
   Node *tail;
   
   void insertNode(int data) {
       Node *nNode = new Node;
       nNode -> prev = tail;
       nNode -> data = data;
       tail = nNode;
       tail -> next = nNode;
   }
   
   void deleteNode(Node *node) {
       Node *tmp = node -> prev;
       tmp -> next = node -> next;
   
       free(node);
   }
   ```

3. circular linked list

<br/>

## Queue

```c++
#include <iostream>
#define MAX 50000

int queue[MAX];
int head = 0, tail = 0;

void enQueue(int data) {
    if(tail == MAX) return;
    queue[tail++] = data;
}

int front() {
    if(head == tail) return -1;
    return queue[head];
}

void deQueue() {
    if(head == tail)  //queue가 비어있다
        return;
    head++;
}

// circular
void clear_queue(){
    head = tail;
}

void enQueue(int k){
    if ((tail + 1) % MAX == head) return;

    queue[rear] = k;
    rear = ++rear % MAX;
}

int deQueue(){
    if (haed == tail) return -1;

    int i = queue[front];
    front = ++front % MAX;
    return i;
}

void print_queue(){
    for (int i = front; i != tail; i = ++i % MAX)
        printf("%-6d", queue[i]);
}
```

```c++
#include <iostream>

class Node {
public:
    int data;
    Node* next;
    Node() {
        next = NULL;
    }
};

Node *head, *tail;

void enQueue(int data) {
    Node *nNode = new Node;
    nNode -> data = data;
    tail -> next = nNode;
    tail = nNode;
}

int front() {
    if(head == tail) return -1;
    return head -> data;
}

void deQueue() {
    if(head == tail) return;
    head = head -> next;
}

// struct 
typedef struct Node{
    int data;
    struct Node *prev;
    struct Node *next;
} Node;

Node *head, *tail;

void init(){
    head = (Node*)malloc(sizeof(Node));
    tail = (Node*)malloc(sizeof(Node));
    head->prev = head;
    head->next = tail;
    tail->prev = head;
    tail->next = tail;
}

void clear(){
    Node *t, *s;
    t = head->next;
    while(t != tail){
        s = t;
        t = t->next;
        free(s);
    }
    head->next = tail;
    tail->prev = head;
}

void enQueue(int k){
  	Node *t = (Node*)malloc(sizeof(Node));
    t->data = k;
    tail->prev->next = t;
    t->prev = tail->prev;
    tail->prev = t;
    t->next = tail;
}

int deQueue(void){
    Node *t = head->next;
    if (t == tail) return -1;
    int i = t->data;
    head->next = t->next;
    t->next->prev = head;
    free(t);
    return i;
}

void print_queue(void){
    Node *t = head->next;
    while (t != tail){
        printf("%-6d", t->data);
        t = t->next;
    }
}
```

> 우선순위 큐
>
> ```c++
> typedef struct {
> 	int heap[SIZE];
> 	int count;
> } PriorityQueue;
> 
> void swap(int* a, int* b) {
> 	int tmp = *a;
> 	*a = *b;
> 	*b = tmp;
> }
> 
> void push(PriorityQueue* pq, int data) {
> 	if (pq->count >= SIZE) return;
> 	int now = pq->count;
> 	pq->heap[now] = data;
> 	int parent = (now - 1) / 2;
> 
> 	while (now > 0 && pq->heap[now] > pq->heap[parent]) {
> 		swap(&pq->heap[now], &pq->heap[parent]);
> 		now = parent;
> 		parent = (now - 1) / 2;
> 	}
> 
> 	pq->count++;
> }
> 
> int pop(PriorityQueue* pq) {
> 	if (pq->count <= 0) return -1;
> 	int res = pq->heap[0];
> 	pq->count--;
> 	pq->heap[0] = pq->heap[pq->count];
> 	int now = 0, left = 1, right = 2;
> 
> 	while (now <= pq->count) {
> 		int change = now;
> 		if (left <= pq->count && pq->heap[now] < pq->heap[left]) change = left;
> 		if (right <= pq->count && pq->heap[now] < pq->heap[right]) change = right;
> 		if (change == now) break;
> 
> 		swap(&pq->heap[now], &pq->heap[change]);
> 		now = change;
> 		left = now * 2 + 1;
> 		right = now * 2 + 2;
> 	}
> 
> 	return res;
> }
> ```

<br/>

## Stack

- 배열

  - 장점 : 구현 간단 , 단점 : 사이즈 고정

  ```c++
  int stack[50000];
  int top = 0;
  
  void push(int data) {
      if(top >= 50000) return;
      stack[top++] = data;
  }
  
  int Top() {
      if(top < 0) return -1;
      return stack[top - 1];
  }
  
  void pop() {
      if(top < 0) return;
      top--;
      return;
  }
  
  void print_stack(){
      for (int i = top; i >= 0; i--){
          printf("%-6d", stack[i]);
      }
  }
  ```

- 리스트

  - 장점 : 공간효율성, 단점 : 구현 복잠

  ```c++
  #include <iostream>
  
  class Node {
  public:
      int data;
      Node* next;
      Node() {
          next = NULL;
      }
  }
  
  Node *head;
  Node *tail;
  Node stack;
  
  // stack -> next가 가장 마지막에 저장된 원소가 되도록 설정
  // tail이 가장 처음 들어온 원소
  void push(int data) {
      Node *nNode = new Node;
      nNode -> data = data;
      nNode -> next = stack -> next;
      stack -> next = nNode;
  }
  
  int Top() {
      if(stack.next != NULL) {
          return stack.next -> data;
      }
  }
  
  void pop() {
      if(stack.next != NULL) {
          stack.next = stack.next -> next;
      }
  }
  // stack 포인터가 아닌 것은 변하지 않기 때문
  // 절대적인 고유한 일이 있기 때문에
  ```

<br/>

# Sort

- 삽입 정렬

  - 배열 구현했을 때 맨 앞으로 이동하려면 shift 연산 많아 비효율 (O(n^3))
  - 리스트 O(n^2)

- 병합 정렬

  ```c++
  #define SIZE 1000
  
  void mergeSort(int data[], int l, int r) {
    int mid;
    if(l < r) {
      mid = (l + r) / 2;
      mergeSort(data, l, mid);
      mergeSort(data, mid + 1, r);
      merge(data, l, mid, r);
    }
  }
  
  void merge(int data[], int l, int mid, int r) {
    int L = l, R = mid + 1, k = l;
    int tmp[SIZE];
    
    while(L <= mid && R <= r) {
      if(data[L] <= data[R]) tmp[k++] = data[L++];
      else tmp[k++] = data[R++];
    }
    
    while(L <= mid) tmp[k++] = data[L++];
    while(R <= r) tmp[k++] = data[R++];
    
    for(int i = l; i <= r; i++) data[i] = tmp[i];
  }
  ```

- 퀵 정렬

  pivot을 인덱스가 아닌 값으로 저장하는 것 주의!

  ```c++
  void qSort(int data[], int l, int r) {
    int L = l, R = r, pos = data[(l + r) / 2];
    
    while(L <= R) {
      while(data[L] < pos) L++;
      while(data[R] > pos) R--;
      if(L <= R) {
        swap(data[L], data[R]);
        L++, R--;
      }
    }
    
    if(l < R) qSort(data, l, R);
    if(r > L) qSort(data, L, r);
  }
  ```

  <br/>

> 퀴즈
>
> ```c++
> #include <iostream>
> 
> class dot {
> public:
>   int y;
>   int x;
> }
> 
> dot D[10000];
> int top = 10000;
> 
> int map[100][100];
> 
> int main() {
>   for(int i = 0; i < 100; i++) {
>     for(int j = 0; j < 100; j++) {
>       int idx = i * 100 + j;
>       D[idx].y = i;
>       D[idx].x = j;
>     }
>   }
>   
>   int c = 0;
>   while(top != 0) {
>     int i = rand() % top;
>     map[D[i].y][D[i].x] = c++;
>     if(top > 0) D[i] = D[top - 1];
>     top--;
>   }
>   
>   return 0;
> }
> ```

