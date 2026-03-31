# Data Structures Comprehensive Guide

## Table of Contents
1. [Dynamic Memory Allocation](#dynamic-memory-allocation)
2. [Linked Lists](#linked-lists)
3. [Circular Singly Linked List](#circular-singly-linked-list)
4. [Doubly Linked List](#doubly-linked-list)
5. [Trees](#trees)

---

## Dynamic Memory Allocation

### 1. malloc() - Memory Allocation
Allocates a block of memory of specified size and returns a pointer to the beginning of the block.

**Syntax:** `ptr = (cast-type*) malloc(byte-size);`

```c
int *ptr;
ptr = (int*) malloc(5 * sizeof(int));
if(ptr == NULL) {
    printf("Memory allocation failed\n");
}
```

**Key Points:**
- Returns void pointer, needs typecasting
- Memory contains garbage values
- Returns NULL if allocation fails

### 2. calloc() - Contiguous Allocation
Allocates multiple blocks of memory and initializes all bytes to zero.

**Syntax:** `ptr = (cast-type*) calloc(n, element-size);`

```c
int *ptr;
ptr = (int*) calloc(5, sizeof(int));
// All 5 integers initialized to 0
```

**Key Points:**
- Takes two arguments (number and size)
- Initializes memory to zero
- Slower than malloc due to initialization

### 3. realloc() - Reallocation
Changes the size of previously allocated memory.

```c
int *ptr = (int*) malloc(5 * sizeof(int));
ptr = (int*) realloc(ptr, 10 * sizeof(int));
```

**Key Points:**
- Can increase or decrease memory size
- Preserves existing data
- May move memory to new location

### 4. free() - Deallocation
Frees previously allocated memory.

```c
free(ptr);
ptr = NULL; // Good practice
```

### Comparison Table

| Function | Initialization | Speed | Arguments | Use Case |
|----------|---------------|-------|-----------|----------|
| malloc() | Garbage values | Fast | 1 (size) | General allocation |
| calloc() | Zero | Slower | 2 (n, size) | When zero initialization needed |
| realloc() | Preserves data | Varies | 2 (ptr, size) | Resize existing memory |
| free() | - | - | 1 (ptr) | Deallocate memory |

---

## Linked Lists

### Definition and Terminology

**Linked List:** A linear data structure where elements are stored in nodes, and each node points to the next node.

**Key Terms:**
- **Node:** Basic unit containing data and pointer
- **Head:** Pointer to the first node
- **Data:** The value stored in the node
- **Next:** Pointer to the next node
- **NULL:** Marks the end of the list

### Node Structure
```c
struct Node {
    int data;
    struct Node *next;
};
```

### Singly Linked List (SLL)

#### getnode() - Create New Node
```c
struct Node* getnode(int value) {
    struct Node* newNode = (struct Node*) malloc(sizeof(struct Node));
    if(newNode == NULL) {
        printf("Memory allocation failed\n");
        return NULL;
    }
    newNode->data = value;
    newNode->next = NULL;
    return newNode;
}
```

#### freenode() - Free Single Node
```c
void freenode(struct Node* node) {
    if(node != NULL) {
        free(node);
        node = NULL;
    }
}
```

#### Insert at Beginning
```c
struct Node* insertAtBeginning(struct Node* head, int value) {
    struct Node* newNode = getnode(value);
    newNode->next = head;
    head = newNode;
    return head;
}
```

#### Insert at End
```c
struct Node* insertAtEnd(struct Node* head, int value) {
    struct Node* newNode = getnode(value);
    
    if(head == NULL) {
        return newNode;
    }
    
    struct Node* temp = head;
    while(temp->next != NULL) {
        temp = temp->next;
    }
    temp->next = newNode;
    return head;
}
```

#### Insert at Position
```c
struct Node* insertAtPosition(struct Node* head, int value, int position) {
    if(position == 1) {
        return insertAtBeginning(head, value);
    }
    
    struct Node* newNode = getnode(value);
    struct Node* temp = head;
    
    for(int i = 1; i < position - 1 && temp != NULL; i++) {
        temp = temp->next;
    }
    
    if(temp == NULL) {
        printf("Invalid position\n");
        return head;
    }
    
    newNode->next = temp->next;
    temp->next = newNode;
    return head;
}
```

#### Delete from Beginning
```c
struct Node* deleteFromBeginning(struct Node* head) {
    if(head == NULL) {
        printf("List is empty\n");
        return NULL;
    }
    
    struct Node* temp = head;
    head = head->next;
    free(temp);
    return head;
}
```

#### Delete from End
```c
struct Node* deleteFromEnd(struct Node* head) {
    if(head == NULL) {
        printf("List is empty\n");
        return NULL;
    }
    
    if(head->next == NULL) {
        free(head);
        return NULL;
    }
    
    struct Node* temp = head;
    while(temp->next->next != NULL) {
        temp = temp->next;
    }
    
    free(temp->next);
    temp->next = NULL;
    return head;
}
```

#### Delete by Value
```c
struct Node* deleteByValue(struct Node* head, int value) {
    if(head == NULL) return NULL;
    
    if(head->data == value) {
        struct Node* temp = head;
        head = head->next;
        free(temp);
        return head;
    }
    
    struct Node* temp = head;
    while(temp->next != NULL && temp->next->data != value) {
        temp = temp->next;
    }
    
    if(temp->next == NULL) {
        printf("Value not found\n");
        return head;
    }
    
    struct Node* toDelete = temp->next;
    temp->next = temp->next->next;
    free(toDelete);
    return head;
}
```

#### Display List
```c
void display(struct Node* head) {
    if(head == NULL) {
        printf("List is empty\n");
        return;
    }
    
    struct Node* temp = head;
    while(temp != NULL) {
        printf("%d -> ", temp->data);
        temp = temp->next;
    }
    printf("NULL\n");
}
```

#### Search in List
```c
int search(struct Node* head, int value) {
    struct Node* temp = head;
    int position = 1;
    
    while(temp != NULL) {
        if(temp->data == value) {
            return position;
        }
        temp = temp->next;
        position++;
    }
    return -1; // Not found
}
```

### Header Node Concept

A header node is a dummy node at the beginning of the list that doesn't contain actual data.

**Advantages:**
- Simplifies insertion/deletion operations
- Eliminates special cases for empty list
- Head pointer never changes

```c
struct Node* createHeaderList() {
    struct Node* header = getnode(0); // Dummy value
    return header;
}

void insertInHeaderList(struct Node* header, int value) {
    struct Node* newNode = getnode(value);
    newNode->next = header->next;
    header->next = newNode;
}
```

---

## Circular Singly Linked List (CSLL)

### Definition
Last node points back to the first node instead of NULL, forming a circle.

```c
struct Node {
    int data;
    struct Node* next;
};
```

### Insert at Beginning (CSLL)
```c
struct Node* insertBeginCSLL(struct Node* tail, int value) {
    struct Node* newNode = getnode(value);
    
    if(tail == NULL) {
        newNode->next = newNode; // Points to itself
        return newNode;
    }
    
    newNode->next = tail->next;
    tail->next = newNode;
    return tail;
}
```

### Insert at End (CSLL)
```c
struct Node* insertEndCSLL(struct Node* tail, int value) {
    struct Node* newNode = getnode(value);
    
    if(tail == NULL) {
        newNode->next = newNode;
        return newNode;
    }
    
    newNode->next = tail->next;
    tail->next = newNode;
    return newNode; // New node becomes tail
}
```

### Delete from Beginning (CSLL)
```c
struct Node* deleteBeginCSLL(struct Node* tail) {
    if(tail == NULL) return NULL;
    
    if(tail->next == tail) { // Only one node
        free(tail);
        return NULL;
    }
    
    struct Node* temp = tail->next;
    tail->next = temp->next;
    free(temp);
    return tail;
}
```

### Display CSLL
```c
void displayCSLL(struct Node* tail) {
    if(tail == NULL) {
        printf("List is empty\n");
        return;
    }
    
    struct Node* temp = tail->next;
    do {
        printf("%d -> ", temp->data);
        temp = temp->next;
    } while(temp != tail->next);
    printf("(back to start)\n");
}
```

### Application: Queue using CSLL

```c
typedef struct {
    struct Node* rear;
} Queue;

void initQueue(Queue* q) {
    q->rear = NULL;
}

void enqueue(Queue* q, int value) {
    q->rear = insertEndCSLL(q->rear, value);
}

int dequeue(Queue* q) {
    if(q->rear == NULL) {
        printf("Queue is empty\n");
        return -1;
    }
    
    struct Node* front = q->rear->next;
    int value = front->data;
    q->rear = deleteBeginCSLL(q->rear);
    return value;
}

int isEmpty(Queue* q) {
    return q->rear == NULL;
}

void displayQueue(Queue* q) {
    displayCSLL(q->rear);
}
```

**Queue Example Usage:**
```c
Queue q;
initQueue(&q);
enqueue(&q, 10);
enqueue(&q, 20);
enqueue(&q, 30);
displayQueue(&q); // 10 -> 20 -> 30
printf("Dequeued: %d\n", dequeue(&q)); // 10
displayQueue(&q); // 20 -> 30
```

---

## Doubly Linked List (DLL)

### Definition
Each node has two pointers: one to the next node and one to the previous node.

```c
struct DNode {
    int data;
    struct DNode* prev;
    struct DNode* next;
};
```

### Create DNode
```c
struct DNode* getDNode(int value) {
    struct DNode* newNode = (struct DNode*) malloc(sizeof(struct DNode));
    newNode->data = value;
    newNode->prev = NULL;
    newNode->next = NULL;
    return newNode;
}
```

### Insert at Beginning (DLL)
```c
struct DNode* insertBeginDLL(struct DNode* head, int value) {
    struct DNode* newNode = getDNode(value);
    
    if(head == NULL) {
        return newNode;
    }
    
    newNode->next = head;
    head->prev = newNode;
    return newNode;
}
```

### Insert at End (DLL)
```c
struct DNode* insertEndDLL(struct DNode* head, int value) {
    struct DNode* newNode = getDNode(value);
    
    if(head == NULL) {
        return newNode;
    }
    
    struct DNode* temp = head;
    while(temp->next != NULL) {
        temp = temp->next;
    }
    
    temp->next = newNode;
    newNode->prev = temp;
    return head;
}
```

### Delete from Beginning (DLL)
```c
struct DNode* deleteBeginDLL(struct DNode* head) {
    if(head == NULL) return NULL;
    
    struct DNode* temp = head;
    head = head->next;
    
    if(head != NULL) {
        head->prev = NULL;
    }
    
    free(temp);
    return head;
}
```

### Delete from End (DLL)
```c
struct DNode* deleteEndDLL(struct DNode* head) {
    if(head == NULL) return NULL;
    
    if(head->next == NULL) {
        free(head);
        return NULL;
    }
    
    struct DNode* temp = head;
    while(temp->next != NULL) {
        temp = temp->next;
    }
    
    temp->prev->next = NULL;
    free(temp);
    return head;
}
```

### Display DLL (Forward)
```c
void displayDLL(struct DNode* head) {
    struct DNode* temp = head;
    printf("NULL <-> ");
    while(temp != NULL) {
        printf("%d <-> ", temp->data);
        temp = temp->next;
    }
    printf("NULL\n");
}
```

### Display DLL (Backward)
```c
void displayReverseDLL(struct DNode* head) {
    if(head == NULL) return;
    
    struct DNode* temp = head;
    while(temp->next != NULL) {
        temp = temp->next;
    }
    
    printf("NULL <-> ");
    while(temp != NULL) {
        printf("%d <-> ", temp->data);
        temp = temp->prev;
    }
    printf("NULL\n");
}
```

---

## Circular Doubly Linked List (CDLL)

### Definition
Last node's next points to first node, and first node's prev points to last node.

### Insert at Beginning (CDLL)
```c
struct DNode* insertBeginCDLL(struct DNode* head, int value) {
    struct DNode* newNode = getDNode(value);
    
    if(head == NULL) {
        newNode->next = newNode;
        newNode->prev = newNode;
        return newNode;
    }
    
    struct DNode* tail = head->prev;
    
    newNode->next = head;
    newNode->prev = tail;
    head->prev = newNode;
    tail->next = newNode;
    
    return newNode;
}
```

### Display CDLL
```c
void displayCDLL(struct DNode* head) {
    if(head == NULL) return;
    
    struct DNode* temp = head;
    do {
        printf("%d <-> ", temp->data);
        temp = temp->next;
    } while(temp != head);
    printf("(circular)\n");
}
```

---

## Applications

### 1. Polynomial Multiplication

**Polynomial Representation:** Each node stores coefficient and exponent.

```c
struct PolyNode {
    int coeff;
    int exp;
    struct PolyNode* next;
};

struct PolyNode* createPolyNode(int coeff, int exp) {
    struct PolyNode* node = (struct PolyNode*) malloc(sizeof(struct PolyNode));
    node->coeff = coeff;
    node->exp = exp;
    node->next = NULL;
    return node;
}

struct PolyNode* insertPoly(struct PolyNode* head, int coeff, int exp) {
    struct PolyNode* newNode = createPolyNode(coeff, exp);
    
    if(head == NULL || head->exp < exp) {
        newNode->next = head;
        return newNode;
    }
    
    struct PolyNode* temp = head;
    while(temp->next != NULL && temp->next->exp > exp) {
        temp = temp->next;
    }
    
    newNode->next = temp->next;
    temp->next = newNode;
    return head;
}

struct PolyNode* multiplyPoly(struct PolyNode* p1, struct PolyNode* p2) {
    if(p1 == NULL || p2 == NULL) return NULL;
    
    struct PolyNode* result = NULL;
    
    for(struct PolyNode* i = p1; i != NULL; i = i->next) {
        for(struct PolyNode* j = p2; j != NULL; j = j->next) {
            int newCoeff = i->coeff * j->coeff;
            int newExp = i->exp + j->exp;
            result = insertPoly(result, newCoeff, newExp);
        }
    }
    
    return result;
}

void displayPoly(struct PolyNode* head) {
    if(head == NULL) {
        printf("0\n");
        return;
    }
    
    struct PolyNode* temp = head;
    while(temp != NULL) {
        printf("%dx^%d", temp->coeff, temp->exp);
        if(temp->next != NULL) printf(" + ");
        temp = temp->next;
    }
    printf("\n");
}
```

**Example:**
```c
// P1: 3x^2 + 5x + 2
// P2: 2x + 4
// Result: 6x^3 + 22x^2 + 24x + 8

struct PolyNode* p1 = NULL;
p1 = insertPoly(p1, 3, 2);
p1 = insertPoly(p1, 5, 1);
p1 = insertPoly(p1, 2, 0);

struct PolyNode* p2 = NULL;
p2 = insertPoly(p2, 2, 1);
p2 = insertPoly(p2, 4, 0);

struct PolyNode* result = multiplyPoly(p1, p2);
displayPoly(result);
```

### 2. Addition of Long Positive Integers

```c
struct DigitNode {
    int digit;
    struct DigitNode* next;
};

struct DigitNode* createDigitNode(int digit) {
    struct DigitNode* node = (struct DigitNode*) malloc(sizeof(struct DigitNode));
    node->digit = digit;
    node->next = NULL;
    return node;
}

struct DigitNode* addLongIntegers(struct DigitNode* num1, struct DigitNode* num2) {
    struct DigitNode* result = NULL;
    struct DigitNode* tail = NULL;
    int carry = 0;
    
    while(num1 != NULL || num2 != NULL || carry) {
        int sum = carry;
        
        if(num1 != NULL) {
            sum += num1->digit;
            num1 = num1->next;
        }
        
        if(num2 != NULL) {
            sum += num2->digit;
            num2 = num2->next;
        }
        
        carry = sum / 10;
        struct DigitNode* newNode = createDigitNode(sum % 10);
        
        if(result == NULL) {
            result = tail = newNode;
        } else {
            tail->next = newNode;
            tail = newNode;
        }
    }
    
    return result;
}

void displayNumber(struct DigitNode* head) {
    if(head == NULL) {
        printf("0\n");
        return;
    }
    
    // Store digits in array to print in reverse
    int digits[1000], count = 0;
    while(head != NULL) {
        digits[count++] = head->digit;
        head = head->next;
    }
    
    for(int i = count - 1; i >= 0; i--) {
        printf("%d", digits[i]);
    }
    printf("\n");
}
```

---

## Trees

### Recursive Definition
A tree is a collection of nodes where:
1. There is a special node called the root
2. The remaining nodes are partitioned into n ≥ 0 disjoint sets T1, T2, ..., Tn, where each set is a tree (subtrees)

### Terminology

- **Root:** Topmost node with no parent
- **Parent:** Node that has child nodes
- **Child:** Node descended from another node
- **Siblings:** Nodes with the same parent
- **Leaf/Terminal Node:** Node with no children
- **Internal Node:** Node with at least one child
- **Edge:** Connection between two nodes
- **Path:** Sequence of nodes connected by edges
- **Height of Tree:** Length of longest path from root to leaf
- **Depth of Node:** Length of path from root to that node
- **Level:** All nodes at the same depth
- **Degree of Node:** Number of children
- **Degree of Tree:** Maximum degree of any node

### Binary Tree (BT)

**Definition:** A tree where each node has at most two children (left and right).

```c
struct TreeNode {
    int data;
    struct TreeNode* left;
    struct TreeNode* right;
};

struct TreeNode* createNode(int data) {
    struct TreeNode* node = (struct TreeNode*) malloc(sizeof(struct TreeNode));
    node->data = data;
    node->left = NULL;
    node->right = NULL;
    return node;
}
```

**Types of Binary Trees:**
1. **Full Binary Tree:** Every node has 0 or 2 children
2. **Complete Binary Tree:** All levels filled except possibly the last, filled from left
3. **Perfect Binary Tree:** All internal nodes have 2 children, all leaves at same level
4. **Skewed Binary Tree:** All nodes have only left or only right children

### Tree Traversals

#### 1. Inorder (Left-Root-Right)
```c
void inorder(struct TreeNode* root) {
    if(root == NULL) return;
    
    inorder(root->left);
    printf("%d ", root->data);
    inorder(root->right);
}
```

#### 2. Preorder (Root-Left-Right)
```c
void preorder(struct TreeNode* root) {
    if(root == NULL) return;
    
    printf("%d ", root->data);
    preorder(root->left);
    preorder(root->right);
}
```

#### 3. Postorder (Left-Right-Root)
```c
void postorder(struct TreeNode* root) {
    if(root == NULL) return;
    
    postorder(root->left);
    postorder(root->right);
    printf("%d ", root->data);
}
```

### Binary Search Tree (BST)

**Properties:**
- Left subtree contains only nodes with values less than parent
- Right subtree contains only nodes with values greater than parent
- Both left and right subtrees are also BSTs

#### Insert in BST
```c
struct TreeNode* insertBST(struct TreeNode* root, int data) {
    if(root == NULL) {
        return createNode(data);
    }
    
    if(data < root->data) {
        root->left = insertBST(root->left, data);
    } else if(data > root->data) {
        root->right = insertBST(root->right, data);
    }
    
    return root;
}
```

#### Search in BST
```c
struct TreeNode* searchBST(struct TreeNode* root, int key) {
    if(root == NULL || root->data == key) {
        return root;
    }
    
    if(key < root->data) {
        return searchBST(root->left, key);
    }
    
    return searchBST(root->right, key);
}
```

#### Find Minimum
```c
struct TreeNode* findMin(struct TreeNode* root) {
    if(root == NULL) return NULL;
    
    while(root->left != NULL) {
        root = root->left;
    }
    return root;
}
```

#### Delete from BST
```c
struct TreeNode* deleteBST(struct TreeNode* root, int key) {
    if(root == NULL) return NULL;
    
    if(key < root->data) {
        root->left = deleteBST(root->left, key);
    } else if(key > root->data) {
        root->right = deleteBST(root->right, key);
    } else {
        // Node found
        
        // Case 1: No children
        if(root->left == NULL && root->right == NULL) {
            free(root);
            return NULL;
        }
        
        // Case 2: One child
        if(root->left == NULL) {
            struct TreeNode* temp = root->right;
            free(root);
            return temp;
        } else if(root->right == NULL) {
            struct TreeNode* temp = root->left;
            free(root);
            return temp;
        }
        
        // Case 3: Two children
        struct TreeNode* temp = findMin(root->right);
        root->data = temp->data;
        root->right = deleteBST(root->right, temp->data);
    }
    
    return root;
}
```

### Expression Trees (ET)

**Definition:** Binary tree where:
- Leaf nodes are operands
- Internal nodes are operators

```c
struct TreeNode* constructExpressionTree(char postfix[]) {
    struct TreeNode* stack[100];
    int top = -1;
    
    for(int i = 0; postfix[i] != '\0'; i++) {
        char ch = postfix[i];
        
        if(isalnum(ch)) {
            // Operand
            stack[++top] = createNode(ch);
        } else {
            // Operator
            struct TreeNode* node = createNode(ch);
            node->right = stack[top--];
            node->left = stack[top--];
            stack[++top] = node;
        }
    }
    
    return stack[top];
}

int evaluateExpressionTree(struct TreeNode* root) {
    if(root == NULL) return 0;
    
    if(root->left == NULL && root->right == NULL) {
        return root->data - '0'; // Convert char to int
    }
    
    int leftVal = evaluateExpressionTree(root->left);
    int rightVal = evaluateExpressionTree(root->right);
    
    switch(root->data) {
        case '+': return leftVal + rightVal;
        case '-': return leftVal - rightVal;
        case '*': return leftVal * rightVal;
        case '/': return leftVal / rightVal;
    }
    
    return 0;
}
```

**Example:**
```
Postfix: 23*5+
Expression Tree:
      +
     / \
    *   5
   / \
  2   3
Result: (2*3)+5 = 11
```

---

## Comparison Tables

### Linked List Types Comparison

| Feature | SLL | CSLL | DLL | CDLL |
|---------|-----|------|-----|------|
| Traversal | Forward only | Circular forward | Both directions | Circular both |
| Memory | 1 pointer/node | 1 pointer/node | 2 pointers/node | 2 pointers/node |
| Last node points to | NULL | First node | NULL | First node |
| Complexity (Insert) | O(1) at head | O(1) at head/tail | O(1) at head/tail | O(1) at head/tail |
| Use case | Simple lists | Queues, round-robin | Navigation both ways | Advanced applications |

### Tree Types Comparison

| Type | Properties | Search | Insert | Delete |
|------|-----------|--------|--------|--------|
| Binary Tree | ≤ 2 children | O(n) | O(n) | O(n) |
| BST | Ordered structure | O(log n) avg | O(log n) avg | O(log n) avg |
| Expression Tree | Operators as internal nodes | - | - | - |

---

## Important Points for Exam

### Memory Management
1. Always check if malloc/calloc returns NULL
2. Free memory in reverse order of allocation
3. Set pointers to NULL after freeing
4. Avoid memory leaks by freeing all allocated nodes

### Linked Lists
1. Always check for empty list before operations
2. Handle special cases: single node, two nodes
3. Update head pointer when needed
4. For circular lists, use do-while for traversal
5. In DLL, update both prev and next pointers

### Trees
1. BST inorder traversal gives sorted order
2. Height of empty tree is -1, single node is 0
3. For BST deletion, handle three cases: leaf, one child, two children
4. Expression tree postorder gives postfix expression
5. Preorder gives prefix, inorder gives infix (with parentheses)

### Common Mistakes to Avoid
1. Not checking NULL pointers before dereferencing
2. Forgetting to update tail in circular lists
3. Memory leaks from not freeing nodes
4. Incorrect pointer updates in insertion/deletion
5. Infinite loops in circular list traversal

---

## Practice Problems

1. Reverse a singly linked list
2. Detect cycle in a linked list
3. Find middle element of linked list
4. Merge two sorted linked lists
5. Check if BST is valid
6. Find kth smallest element in BST
7. Convert sorted array to BST
8. Level order traversal of tree
9. Find height of binary tree
10. Check if two trees are identical

---

## Complete Example Program

```c
#include <stdio.h>
#include <stdlib.h>

struct Node {
    int data;
    struct Node* next;
};

struct Node* getnode(int value) {
    struct Node* node = (struct Node*) malloc(sizeof(struct Node));
    node->data = value;
    node->next = NULL;
    return node;
}

struct Node* insertEnd(struct Node* head, int value) {
    struct Node* newNode = getnode(value);
    if(head == NULL) return newNode;
    
    struct Node* temp = head;
    while(temp->next != NULL) temp = temp->next;
    temp->next = newNode;
    return head;
}

void display(struct Node* head) {
    while(head != NULL) {
        printf("%d -> ", head->data);
        head = head->next;
    }
    printf("NULL\n");
}

void freeList(struct Node* head) {
    struct Node* temp;
    while(head != NULL) {
        temp = head;
        head = head->next;
        free(temp);
    }
}

int main() {
    struct Node* head = NULL;
    
    head = insertEnd(head, 10);
    head = insertEnd(head, 20);
    head = insertEnd(head, 30);
    
    printf("Linked List: ");
    display(head);
    
    freeList(head);
    return 0;
}
```

---

**End of Guide**

*Good luck with your exam! Remember to practice writing code by hand and understand the logic behind each operation.*
