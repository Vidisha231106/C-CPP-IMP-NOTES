# Data Structures and Algorithms - Complete Study Notes

## Table of Contents
1. [Binary Trees (BT)](#binary-trees)
2. [Binary Search Trees (BST)](#binary-search-trees)
3. [Expression Trees (ET)](#expression-trees)
4. [Tree Applications](#tree-applications)
5. [Heaps](#heaps)
6. [Threaded Binary Trees](#threaded-binary-trees)
7. [Balanced Trees](#balanced-trees)
8. [Graphs](#graphs)
9. [Hashing](#hashing)

---

## Binary Trees (BT)

### Definition
A Binary Tree is a hierarchical data structure where each node has at most two children, referred to as left child and right child.

### Basic Terminology
- **Root**: Topmost node
- **Parent**: Node with children
- **Child**: Descendant of a node
- **Leaf**: Node with no children
- **Height**: Longest path from root to leaf
- **Depth**: Distance from root to a node
- **Degree**: Number of children of a node

### Structure in C
```c
struct Node {
    int data;
    struct Node* left;
    struct Node* right;
};
```

### Creating a Node
```c
struct Node* createNode(int data) {
    struct Node* newNode = (struct Node*)malloc(sizeof(struct Node));
    newNode->data = data;
    newNode->left = NULL;
    newNode->right = NULL;
    return newNode;
}
```

### Insertion in Binary Tree
Binary trees can be inserted level by level (level order).

```c
struct Node* insertLevelOrder(struct Node* root, int data) {
    if (root == NULL) {
        return createNode(data);
    }
    
    // Use queue for level order insertion
    struct Node* queue[100];
    int front = 0, rear = 0;
    queue[rear++] = root;
    
    while (front < rear) {
        struct Node* temp = queue[front++];
        
        if (temp->left == NULL) {
            temp->left = createNode(data);
            return root;
        } else {
            queue[rear++] = temp->left;
        }
        
        if (temp->right == NULL) {
            temp->right = createNode(data);
            return root;
        } else {
            queue[rear++] = temp->right;
        }
    }
    return root;
}
```

### Deletion in Binary Tree
To delete a node, replace it with the deepest rightmost node.

```c
struct Node* deleteDeepest(struct Node* root, struct Node* dNode) {
    struct Node* queue[100];
    int front = 0, rear = 0;
    queue[rear++] = root;
    
    struct Node* temp;
    while (front < rear) {
        temp = queue[front++];
        
        if (temp == dNode) {
            temp = NULL;
            free(dNode);
            return root;
        }
        if (temp->right) {
            if (temp->right == dNode) {
                temp->right = NULL;
                free(dNode);
                return root;
            } else {
                queue[rear++] = temp->right;
            }
        }
        if (temp->left) {
            if (temp->left == dNode) {
                temp->left = NULL;
                free(dNode);
                return root;
            } else {
                queue[rear++] = temp->left;
            }
        }
    }
    return root;
}

struct Node* deletion(struct Node* root, int key) {
    if (root == NULL) return NULL;
    
    if (root->left == NULL && root->right == NULL) {
        if (root->data == key) {
            free(root);
            return NULL;
        }
        return root;
    }
    
    struct Node* queue[100];
    int front = 0, rear = 0;
    queue[rear++] = root;
    
    struct Node* temp;
    struct Node* keyNode = NULL;
    
    while (front < rear) {
        temp = queue[front++];
        
        if (temp->data == key)
            keyNode = temp;
        
        if (temp->left)
            queue[rear++] = temp->left;
        
        if (temp->right)
            queue[rear++] = temp->right;
    }
    
    if (keyNode != NULL) {
        int x = temp->data;
        deleteDeepest(root, temp);
        keyNode->data = x;
    }
    return root;
}
```

### Tree Traversals

#### 1. Inorder Traversal (Left-Root-Right)
```c
void inorder(struct Node* root) {
    if (root != NULL) {
        inorder(root->left);
        printf("%d ", root->data);
        inorder(root->right);
    }
}
```

#### 2. Preorder Traversal (Root-Left-Right)
```c
void preorder(struct Node* root) {
    if (root != NULL) {
        printf("%d ", root->data);
        preorder(root->left);
        preorder(root->right);
    }
}
```

#### 3. Postorder Traversal (Left-Right-Root)
```c
void postorder(struct Node* root) {
    if (root != NULL) {
        postorder(root->left);
        postorder(root->right);
        printf("%d ", root->data);
    }
}
```

#### 4. Level Order Traversal
```c
void levelOrder(struct Node* root) {
    if (root == NULL) return;
    
    struct Node* queue[100];
    int front = 0, rear = 0;
    queue[rear++] = root;
    
    while (front < rear) {
        struct Node* temp = queue[front++];
        printf("%d ", temp->data);
        
        if (temp->left != NULL)
            queue[rear++] = temp->left;
        
        if (temp->right != NULL)
            queue[rear++] = temp->right;
    }
}
```

### Display Methods
```c
// Display tree structure
void displayTree(struct Node* root, int space) {
    if (root == NULL) return;
    
    space += 5;
    displayTree(root->right, space);
    
    printf("\n");
    for (int i = 5; i < space; i++)
        printf(" ");
    printf("%d\n", root->data);
    
    displayTree(root->left, space);
}
```

---

## Binary Search Trees (BST)

### Definition
A Binary Search Tree is a binary tree where:
- Left subtree contains nodes with values less than root
- Right subtree contains nodes with values greater than root
- Both subtrees are also BSTs

### Properties
- Inorder traversal gives sorted sequence
- Search, Insert, Delete: O(h) where h is height
- Best case height: O(log n), Worst case: O(n)

### Insertion in BST
```c
struct Node* insertBST(struct Node* root, int data) {
    if (root == NULL) {
        return createNode(data);
    }
    
    if (data < root->data) {
        root->left = insertBST(root->left, data);
    } else if (data > root->data) {
        root->right = insertBST(root->right, data);
    }
    
    return root;
}
```

### Searching in BST
```c
struct Node* searchBST(struct Node* root, int key) {
    if (root == NULL || root->data == key) {
        return root;
    }
    
    if (key < root->data) {
        return searchBST(root->left, key);
    }
    
    return searchBST(root->right, key);
}
```

### Finding Minimum and Maximum
```c
struct Node* findMin(struct Node* root) {
    if (root == NULL) return NULL;
    
    while (root->left != NULL) {
        root = root->left;
    }
    return root;
}

struct Node* findMax(struct Node* root) {
    if (root == NULL) return NULL;
    
    while (root->right != NULL) {
        root = root->right;
    }
    return root;
}
```

### Deletion in BST
Three cases:
1. Node has no children (leaf)
2. Node has one child
3. Node has two children

```c
struct Node* deleteBST(struct Node* root, int key) {
    if (root == NULL) return root;
    
    if (key < root->data) {
        root->left = deleteBST(root->left, key);
    } else if (key > root->data) {
        root->right = deleteBST(root->right, key);
    } else {
        // Node with one child or no child
        if (root->left == NULL) {
            struct Node* temp = root->right;
            free(root);
            return temp;
        } else if (root->right == NULL) {
            struct Node* temp = root->left;
            free(root);
            return temp;
        }
        
        // Node with two children
        struct Node* temp = findMin(root->right);
        root->data = temp->data;
        root->right = deleteBST(root->right, temp->data);
    }
    return root;
}
```

---

## Expression Trees (ET)

### Definition
An Expression Tree is a binary tree where:
- Leaf nodes are operands
- Internal nodes are operators

### Construction from Postfix Expression
```c
int isOperator(char c) {
    return (c == '+' || c == '-' || c == '*' || c == '/' || c == '^');
}

struct Node* constructExpressionTree(char postfix[]) {
    struct Node* stack[100];
    int top = -1;
    
    for (int i = 0; postfix[i] != '\0'; i++) {
        if (!isOperator(postfix[i])) {
            // Operand
            struct Node* node = createNode(postfix[i]);
            stack[++top] = node;
        } else {
            // Operator
            struct Node* node = createNode(postfix[i]);
            node->right = stack[top--];
            node->left = stack[top--];
            stack[++top] = node;
        }
    }
    
    return stack[top];
}
```

### Evaluation of Expression Tree
```c
int evaluateExpressionTree(struct Node* root) {
    if (root == NULL) return 0;
    
    // Leaf node (operand)
    if (root->left == NULL && root->right == NULL) {
        return root->data - '0';
    }
    
    int leftVal = evaluateExpressionTree(root->left);
    int rightVal = evaluateExpressionTree(root->right);
    
    switch(root->data) {
        case '+': return leftVal + rightVal;
        case '-': return leftVal - rightVal;
        case '*': return leftVal * rightVal;
        case '/': return leftVal / rightVal;
        case '^': return pow(leftVal, rightVal);
    }
    return 0;
}
```

---

## Tree Applications

### 1. Tree Sort
Use BST to sort elements.

```c
void storeSorted(struct Node* root, int arr[], int* index) {
    if (root != NULL) {
        storeSorted(root->left, arr, index);
        arr[(*index)++] = root->data;
        storeSorted(root->right, arr, index);
    }
}

void treeSort(int arr[], int n) {
    struct Node* root = NULL;
    
    // Insert all elements into BST
    for (int i = 0; i < n; i++) {
        root = insertBST(root, arr[i]);
    }
    
    // Store inorder traversal back to array
    int index = 0;
    storeSorted(root, arr, &index);
}
```

**Time Complexity**: O(n log n) average, O(n²) worst case

### 2. Infix to Postfix Conversion
```c
int precedence(char op) {
    if (op == '^') return 3;
    if (op == '*' || op == '/') return 2;
    if (op == '+' || op == '-') return 1;
    return 0;
}

void infixToPostfix(char infix[], char postfix[]) {
    char stack[100];
    int top = -1;
    int j = 0;
    
    for (int i = 0; infix[i] != '\0'; i++) {
        char c = infix[i];
        
        if (isalnum(c)) {
            postfix[j++] = c;
        } else if (c == '(') {
            stack[++top] = c;
        } else if (c == ')') {
            while (top != -1 && stack[top] != '(') {
                postfix[j++] = stack[top--];
            }
            top--; // Remove '('
        } else {
            while (top != -1 && precedence(stack[top]) >= precedence(c)) {
                postfix[j++] = stack[top--];
            }
            stack[++top] = c;
        }
    }
    
    while (top != -1) {
        postfix[j++] = stack[top--];
    }
    postfix[j] = '\0';
}
```

### 3. Infix to Prefix Conversion
```c
void reverse(char str[]) {
    int len = strlen(str);
    for (int i = 0; i < len / 2; i++) {
        char temp = str[i];
        str[i] = str[len - i - 1];
        str[len - i - 1] = temp;
    }
}

void infixToPrefix(char infix[], char prefix[]) {
    char temp[100];
    strcpy(temp, infix);
    reverse(temp);
    
    // Replace '(' with ')' and vice versa
    for (int i = 0; temp[i] != '\0'; i++) {
        if (temp[i] == '(') temp[i] = ')';
        else if (temp[i] == ')') temp[i] = '(';
    }
    
    infixToPostfix(temp, prefix);
    reverse(prefix);
}
```

### 4. Postfix to Infix Conversion
```c
void postfixToInfix(char postfix[], char infix[]) {
    char stack[100][100];
    int top = -1;
    
    for (int i = 0; postfix[i] != '\0'; i++) {
        if (isalnum(postfix[i])) {
            char operand[2] = {postfix[i], '\0'};
            strcpy(stack[++top], operand);
        } else {
            char op1[100], op2[100], expr[100];
            strcpy(op2, stack[top--]);
            strcpy(op1, stack[top--]);
            
            sprintf(expr, "(%s%c%s)", op1, postfix[i], op2);
            strcpy(stack[++top], expr);
        }
    }
    strcpy(infix, stack[top]);
}
```

---

## Heaps

### Definition
A Heap is a complete binary tree that satisfies the heap property:
- **Max Heap**: Parent ≥ Children
- **Min Heap**: Parent ≤ Children

### Array Representation
For node at index i:
- Left child: 2i + 1
- Right child: 2i + 2
- Parent: (i - 1) / 2

### Heap Construction (Max Heap)

#### Insert Operation
```c
void insertMaxHeap(int heap[], int* n, int value) {
    (*n)++;
    int i = *n - 1;
    heap[i] = value;
    
    // Heapify up
    while (i != 0 && heap[(i - 1) / 2] < heap[i]) {
        int temp = heap[i];
        heap[i] = heap[(i - 1) / 2];
        heap[(i - 1) / 2] = temp;
        i = (i - 1) / 2;
    }
}
```

#### Heapify Operation
```c
void heapify(int heap[], int n, int i) {
    int largest = i;
    int left = 2 * i + 1;
    int right = 2 * i + 2;
    
    if (left < n && heap[left] > heap[largest])
        largest = left;
    
    if (right < n && heap[right] > heap[largest])
        largest = right;
    
    if (largest != i) {
        int temp = heap[i];
        heap[i] = heap[largest];
        heap[largest] = temp;
        heapify(heap, n, largest);
    }
}
```

#### Build Heap
```c
void buildMaxHeap(int heap[], int n) {
    for (int i = n / 2 - 1; i >= 0; i--) {
        heapify(heap, n, i);
    }
}
```

#### Delete Operation (Extract Max)
```c
int extractMax(int heap[], int* n) {
    if (*n <= 0) return -1;
    if (*n == 1) {
        (*n)--;
        return heap[0];
    }
    
    int root = heap[0];
    heap[0] = heap[*n - 1];
    (*n)--;
    heapify(heap, *n, 0);
    
    return root;
}
```

### Heap Sort
```c
void heapSort(int arr[], int n) {
    // Build max heap
    buildMaxHeap(arr, n);
    
    // Extract elements one by one
    for (int i = n - 1; i > 0; i--) {
        int temp = arr[0];
        arr[0] = arr[i];
        arr[i] = temp;
        
        heapify(arr, i, 0);
    }
}
```

**Time Complexity**: O(n log n)  
**Space Complexity**: O(1)

### Priority Queue using Heap
```c
typedef struct {
    int heap[100];
    int size;
} PriorityQueue;

void initPQ(PriorityQueue* pq) {
    pq->size = 0;
}

void enqueue(PriorityQueue* pq, int value) {
    insertMaxHeap(pq->heap, &pq->size, value);
}

int dequeue(PriorityQueue* pq) {
    return extractMax(pq->heap, &pq->size);
}

int peek(PriorityQueue* pq) {
    if (pq->size > 0)
        return pq->heap[0];
    return -1;
}

int isEmpty(PriorityQueue* pq) {
    return pq->size == 0;
}
```

---

## Threaded Binary Trees

### Definition
A Threaded Binary Tree is a binary tree where NULL pointers are replaced with threads (pointers) to facilitate traversal without recursion or stack.

### Types

#### 1. Single Threaded (Right Threaded)
Right NULL pointer points to inorder successor.

#### 2. Double Threaded
Both NULL pointers are threaded:
- Left NULL → Inorder predecessor
- Right NULL → Inorder successor

### Structure
```c
struct ThreadedNode {
    int data;
    struct ThreadedNode* left;
    struct ThreadedNode* right;
    int leftThread;  // 1 if left is thread, 0 if child
    int rightThread; // 1 if right is thread, 0 if child
};
```

### Insertion in Threaded BST
```c
struct ThreadedNode* createThreadedNode(int data) {
    struct ThreadedNode* node = (struct ThreadedNode*)malloc(sizeof(struct ThreadedNode));
    node->data = data;
    node->left = node->right = NULL;
    node->leftThread = node->rightThread = 1;
    return node;
}

struct ThreadedNode* insertThreaded(struct ThreadedNode* root, int data) {
    struct ThreadedNode* ptr = root;
    struct ThreadedNode* parent = NULL;
    
    // Find location to insert
    while (ptr != NULL) {
        parent = ptr;
        
        if (data < ptr->data) {
            if (ptr->leftThread == 0)
                ptr = ptr->left;
            else
                break;
        } else {
            if (ptr->rightThread == 0)
                ptr = ptr->right;
            else
                break;
        }
    }
    
    struct ThreadedNode* newNode = createThreadedNode(data);
    
    if (parent == NULL) {
        root = newNode;
    } else if (data < parent->data) {
        newNode->left = parent->left;
        newNode->right = parent;
        parent->leftThread = 0;
        parent->left = newNode;
    } else {
        newNode->right = parent->right;
        newNode->left = parent;
        parent->rightThread = 0;
        parent->right = newNode;
    }
    
    return root;
}
```

### Inorder Traversal (Without Recursion)
```c
struct ThreadedNode* leftmost(struct ThreadedNode* node) {
    if (node == NULL) return NULL;
    
    while (node->leftThread == 0)
        node = node->left;
    
    return node;
}

void inorderThreaded(struct ThreadedNode* root) {
    struct ThreadedNode* curr = leftmost(root);
    
    while (curr != NULL) {
        printf("%d ", curr->data);
        
        if (curr->rightThread == 1)
            curr = curr->right;
        else
            curr = leftmost(curr->right);
    }
}
```

### Applications
- Faster traversal without stack or recursion
- Efficient inorder traversal
- Memory efficient for large trees
- Used in database indexing

---

## Balanced Trees

### AVL Trees

#### Definition
An AVL tree is a self-balancing BST where the height difference between left and right subtrees (balance factor) is at most 1 for all nodes.

**Balance Factor** = Height(Left Subtree) - Height(Right Subtree)  
Values: -1, 0, or 1

#### Structure
```c
struct AVLNode {
    int data;
    struct AVLNode* left;
    struct AVLNode* right;
    int height;
};
```

#### Utility Functions
```c
int height(struct AVLNode* node) {
    if (node == NULL) return 0;
    return node->height;
}

int max(int a, int b) {
    return (a > b) ? a : b;
}

int getBalance(struct AVLNode* node) {
    if (node == NULL) return 0;
    return height(node->left) - height(node->right);
}

struct AVLNode* createAVLNode(int data) {
    struct AVLNode* node = (struct AVLNode*)malloc(sizeof(struct AVLNode));
    node->data = data;
    node->left = NULL;
    node->right = NULL;
    node->height = 1;
    return node;
}
```

#### Rotations

##### Right Rotation
```c
struct AVLNode* rightRotate(struct AVLNode* y) {
    struct AVLNode* x = y->left;
    struct AVLNode* T2 = x->right;
    
    x->right = y;
    y->left = T2;
    
    y->height = max(height(y->left), height(y->right)) + 1;
    x->height = max(height(x->left), height(x->right)) + 1;
    
    return x;
}
```

##### Left Rotation
```c
struct AVLNode* leftRotate(struct AVLNode* x) {
    struct AVLNode* y = x->right;
    struct AVLNode* T2 = y->left;
    
    y->left = x;
    x->right = T2;
    
    x->height = max(height(x->left), height(x->right)) + 1;
    y->height = max(height(y->left), height(y->right)) + 1;
    
    return y;
}
```

#### Insertion in AVL Tree
```c
struct AVLNode* insertAVL(struct AVLNode* node, int data) {
    // Standard BST insertion
    if (node == NULL)
        return createAVLNode(data);
    
    if (data < node->data)
        node->left = insertAVL(node->left, data);
    else if (data > node->data)
        node->right = insertAVL(node->right, data);
    else
        return node;
    
    // Update height
    node->height = 1 + max(height(node->left), height(node->right));
    
    // Get balance factor
    int balance = getBalance(node);
    
    // Left Left Case
    if (balance > 1 && data < node->left->data)
        return rightRotate(node);
    
    // Right Right Case
    if (balance < -1 && data > node->right->data)
        return leftRotate(node);
    
    // Left Right Case
    if (balance > 1 && data > node->left->data) {
        node->left = leftRotate(node->left);
        return rightRotate(node);
    }
    
    // Right Left Case
    if (balance < -1 && data < node->right->data) {
        node->right = rightRotate(node->right);
        return leftRotate(node);
    }
    
    return node;
}
```

#### Deletion in AVL Tree
```c
struct AVLNode* minValueNode(struct AVLNode* node) {
    struct AVLNode* current = node;
    while (current->left != NULL)
        current = current->left;
    return current;
}

struct AVLNode* deleteAVL(struct AVLNode* root, int data) {
    if (root == NULL) return root;
    
    if (data < root->data)
        root->left = deleteAVL(root->left, data);
    else if (data > root->data)
        root->right = deleteAVL(root->right, data);
    else {
        if ((root->left == NULL) || (root->right == NULL)) {
            struct AVLNode* temp = root->left ? root->left : root->right;
            
            if (temp == NULL) {
                temp = root;
                root = NULL;
            } else {
                *root = *temp;
            }
            free(temp);
        } else {
            struct AVLNode* temp = minValueNode(root->right);
            root->data = temp->data;
            root->right = deleteAVL(root->right, temp->data);
        }
    }
    
    if (root == NULL) return root;
    
    root->height = 1 + max(height(root->left), height(root->right));
    
    int balance = getBalance(root);
    
    // Left Left Case
    if (balance > 1 && getBalance(root->left) >= 0)
        return rightRotate(root);
    
    // Left Right Case
    if (balance > 1 && getBalance(root->left) < 0) {
        root->left = leftRotate(root->left);
        return rightRotate(root);
    }
    
    // Right Right Case
    if (balance < -1 && getBalance(root->right) <= 0)
        return leftRotate(root);
    
    // Right Left Case
    if (balance < -1 && getBalance(root->right) > 0) {
        root->right = rightRotate(root->right);
        return leftRotate(root);
    }
    
    return root;
}
```

### Splay Trees

#### Definition
A Splay Tree is a self-adjusting BST where recently accessed elements are moved to the root through splaying operations.

#### Splay Operations

##### Zig (Single Rotation)
```c
struct Node* rightRotateSplay(struct Node* x) {
    struct Node* y = x->left;
    x->left = y->right;
    y->right = x;
    return y;
}

struct Node* leftRotateSplay(struct Node* x) {
    struct Node* y = x->right;
    x->right = y->left;
    y->left = x;
    return y;
}
```

##### Splay Function
```c
struct Node* splay(struct Node* root, int key) {
    if (root == NULL || root->data == key)
        return root;
    
    // Key in left subtree
    if (root->data > key) {
        if (root->left == NULL) return root;
        
        // Zig-Zig (Left Left)
        if (root->left->data > key) {
            root->left->left = splay(root->left->left, key);
            root = rightRotateSplay(root);
        }
        // Zig-Zag (Left Right)
        else if (root->left->data < key) {
            root->left->right = splay(root->left->right, key);
            if (root->left->right != NULL)
                root->left = leftRotateSplay(root->left);
        }
        
        return (root->left == NULL) ? root : rightRotateSplay(root);
    }
    // Key in right subtree
    else {
        if (root->right == NULL) return root;
        
        // Zag-Zig (Right Left)
        if (root->right->data > key) {
            root->right->left = splay(root->right->left, key);
            if (root->right->left != NULL)
                root->right = rightRotateSplay(root->right);
        }
        // Zag-Zag (Right Right)
        else if (root->right->data < key) {
            root->right->right = splay(root->right->right, key);
            root = leftRotateSplay(root);
        }
        
        return (root->right == NULL) ? root : leftRotateSplay(root);
    }
}
```

##### Insert in Splay Tree
```c
struct Node* insertSplay(struct Node* root, int key) {
    if (root == NULL) return createNode(key);
    
    root = splay(root, key);
    
    if (root->data == key) return root;
    
    struct Node* newNode = createNode(key);
    
    if (root->data > key) {
        newNode->right = root;
        newNode->left = root->left;
        root->left = NULL;
    } else {
        newNode->left = root;
        newNode->right = root->right;
        root->right = NULL;
    }
    
    return newNode;
}
```

### Tries

#### Definition
A Trie (Prefix Tree) is a tree-like data structure used to store strings where each node represents a character.

#### Structure
```c
#define ALPHABET_SIZE 26

struct TrieNode {
    struct TrieNode* children[ALPHABET_SIZE];
    int isEndOfWord;
};
```

#### Create Node
```c
struct TrieNode* createTrieNode() {
    struct TrieNode* node = (struct TrieNode*)malloc(sizeof(struct TrieNode));
    node->isEndOfWord = 0;
    
    for (int i = 0; i < ALPHABET_SIZE; i++)
        node->children[i] = NULL;
    
    return node;
}
```

#### Insert in Trie
```c
void insertTrie(struct TrieNode* root, const char* key) {
    struct TrieNode* current = root;
    
    for (int i = 0; key[i] != '\0'; i++) {
        int index = key[i] - 'a';
        
        if (current->children[index] == NULL)
            current->children[index] = createTrieNode();
        
        current = current->children[index];
    }
    
    current->isEndOfWord = 1;
}
```

#### Search in Trie
```c
int searchTrie(struct TrieNode* root, const char* key) {
    struct TrieNode* current = root;
    
    for (int i = 0; key[i] != '\0'; i++) {
        int index = key[i] - 'a';
        
        if (current->children[index] == NULL)
            return 0;
        
        current = current->children[index];
    }
    
    return (current != NULL && current->isEndOfWord);
}
```

#### Delete from Trie
```c
int isEmpty(struct TrieNode* node) {
    for (int i = 0; i < ALPHABET_SIZE; i++)
        if (node->children[i] != NULL)
            return 0;
    return 1;
}

struct TrieNode* deleteTrie(struct TrieNode* root, const char* key, int depth) {
    if (root == NULL) return NULL;
    
    if (depth == strlen(key)) {
        if (root->isEndOfWord)
            root->isEndOfWord = 0;
        
        if (isEmpty(root)) {
            free(root);
            root = NULL;
        }
        return root;
    }
    
    int index = key[depth] - 'a';
    root->children[index] = deleteTrie(root->children[index], key, depth + 1);
    
    if (isEmpty(root) && root->isEndOfWord == 0) {
        free(root);
        root = NULL;
    }
    
    return root;
}
```

#### Applications of Tries
- Auto-complete/Type-ahead features
- Spell checkers
- IP routing (Longest prefix matching)
- Dictionary implementations
- String pattern matching

---

## Graphs

### Definition
A Graph G = (V, E) consists of:
- V: Set of vertices (nodes)
- E: Set of edges (connections)

### Types of Graphs
1. **Directed Graph (Digraph)**: Edges have direction
2. **Undirected Graph**: Edges have no direction
3. **Weighted Graph**: Edges have weights
4. **Unweighted Graph**: All edges have same weight

### Terminologies
- **Adjacent Vertices**: Connected by an edge
- **Degree**: Number of edges connected to a vertex
  - In-degree: Incoming edges
  - Out-degree: Outgoing edges
- **Path**: Sequence of vertices
- **Cycle**: Path that starts and ends at same vertex
- **Connected Graph**: Path exists between every pair of vertices
- **Complete Graph**: Every vertex connected to every other vertex

### Graph Representations

#### 1. Adjacency Matrix
```c
#define MAX_VERTICES 20

typedef struct {
    int vertices;
    int adjMatrix[MAX_VERTICES][MAX_VERTICES];
} GraphMatrix;

void initGraphMatrix(GraphMatrix* g, int vertices) {
    g->vertices = vertices;
    
    for (int i = 0; i < vertices; i++)
        for (int j = 0; j < vertices; j++)
            g->adjMatrix[i][j] = 0;
}

void addEdgeMatrix(GraphMatrix* g, int src, int dest) {
    g->adjMatrix[src][dest] = 1;
    g->adjMatrix[dest][src] = 1; // For undirected graph
}

void displayMatrix(GraphMatrix* g) {
    printf("Adjacency Matrix:\n");
    for (int i = 0; i < g->vertices; i++) {
        for (int j = 0; j < g->vertices; j++) {
            printf("%d ", g->adjMatrix[i][j]);
        }
        printf("\n");
    }
}
```

**Pros**: 
- O(1) edge lookup
- Simple implementation

**Cons**: 
- O(V²) space
- Inefficient for sparse graphs

#### 2. Adjacency List
```c
struct AdjListNode {
    int dest;
    struct AdjListNode* next;
};

struct AdjList {
    struct AdjListNode* head;
};

typedef struct {
    int vertices;
    struct AdjList* array;
} GraphList;

struct AdjListNode* createAdjListNode(int dest) {
    struct AdjListNode* newNode = (struct AdjListNode*)malloc(sizeof(struct AdjListNode));
    newNode->dest = dest;
    newNode->next = NULL;
    return newNode;
}

GraphList* createGraphList(int vertices) {
    GraphList* graph = (GraphList*)malloc(sizeof(GraphList));
    graph->vertices = vertices;
    graph->array = (struct AdjList*)malloc(vertices * sizeof(struct AdjList));
    
    for (int i = 0; i < vertices; i++)
        graph->array[i].head = NULL;
    
    return graph;
}

void addEdgeList(GraphList* graph, int src, int dest) {
    struct AdjListNode* newNode = createAdjListNode(dest);
    newNode->next = graph->array[src].head;
    graph->array[src].head = newNode;
    
    // For undirected graph
    newNode = createAdjListNode(src);
    newNode->next = graph->array[dest].head;
    graph->array[dest].head = newNode;
}

void displayList(GraphList* graph) {
    for (int i = 0; i < graph->vertices; i++) {
        struct AdjListNode* temp = graph->array[i].head;
        printf("Vertex %d: ", i);
        while (temp) {
            printf("%d -> ", temp->dest);
            temp = temp->next;
        }
        printf("NULL\n");
    }
}
```

**Pros**: 
- O(V + E) space
- Efficient for sparse graphs

**Cons**: 
- O(V) edge lookup

### Graph Traversals

#### 1. Breadth First Search (BFS)
```c
void BFS(GraphList* graph, int startVertex) {
    int visited[MAX_VERTICES] = {0};
    int queue[MAX_VERTICES];
    int front = 0, rear = 0;
    
    visited[startVertex] = 1;
    queue[rear++] = startVertex;
    
    printf("BFS Traversal: ");
    
    while (front < rear) {
        int currentVertex = queue[front++];
        printf("%d ", currentVertex);
        
        struct AdjListNode* temp = graph->array[currentVertex].head;
        
        while (temp) {
            int adjVertex = temp->dest;
            
            if (!visited[adjVertex]) {
                visited[adjVertex] = 1;
                queue[rear++] = adjVertex;
            }
            temp = temp->next;
        }
    }
    printf("\n");
}
```

**Time Complexity**: O(V + E)  
**Space Complexity**: O(V)

#### 2. Depth First Search (DFS)
```c
void DFSUtil(GraphList* graph, int vertex, int visited[]) {
    visited[vertex] = 1;
    printf("%d ", vertex);
    
    struct AdjListNode* temp = graph->array[vertex].head;
    
    while (temp) {
        int adjVertex = temp->dest;
        if (!visited[adjVertex])
            DFSUtil(graph, adjVertex, visited);
        temp = temp->next;
    }
}

void DFS(GraphList* graph, int startVertex) {
    int visited[MAX_VERTICES] = {0};
    
    printf("DFS Traversal: ");
    DFSUtil(graph, startVertex, visited);
    printf("\n");
}
```

**Time Complexity**: O(V + E)  
**Space Complexity**: O(V)

### Applications of Graphs
- Social networks
- Google Maps (shortest path)
- Web crawling
- Network routing
- Recommendation systems
- Circuit design

---

## Hashing

### Definition
Hashing is a technique to map data to a fixed-size array using a hash function. It provides O(1) average time complexity for search, insert, and delete operations.

### Hash Function
A hash function converts a key into an array index.

```c
int hashFunction(int key, int tableSize) {
    return key % tableSize;
}
```

### Properties of Good Hash Function
1. Uniform distribution
2. Fast computation
3. Minimizes collisions

### Collision
When two keys hash to the same index.

### Collision Resolution Strategies

#### 1. Open Hashing (Separate Chaining)

Each array position contains a linked list of elements that hash to that index.

```c
#define TABLE_SIZE 10

struct HashNode {
    int key;
    int value;
    struct HashNode* next;
};

struct HashTable {
    struct HashNode* table[TABLE_SIZE];
};

void initHashTable(struct HashTable* ht) {
    for (int i = 0; i < TABLE_SIZE; i++)
        ht->table[i] = NULL;
}

void insertChaining(struct HashTable* ht, int key, int value) {
    int index = hashFunction(key, TABLE_SIZE);
    
    struct HashNode* newNode = (struct HashNode*)malloc(sizeof(struct HashNode));
    newNode->key = key;
    newNode->value = value;
    newNode->next = ht->table[index];
    
    ht->table[index] = newNode;
}

int searchChaining(struct HashTable* ht, int key) {
    int index = hashFunction(key, TABLE_SIZE);
    struct HashNode* temp = ht->table[index];
    
    while (temp != NULL) {
        if (temp->key == key)
            return temp->value;
        temp = temp->next;
    }
    return -1; // Not found
}

void deleteChaining(struct HashTable* ht, int key) {
    int index = hashFunction(key, TABLE_SIZE);
    struct HashNode* temp = ht->table[index];
    struct HashNode* prev = NULL;
    
    while (temp != NULL && temp->key != key) {
        prev = temp;
        temp = temp->next;
    }
    
    if (temp == NULL) return; // Key not found
    
    if (prev == NULL)
        ht->table[index] = temp->next;
    else
        prev->next = temp->next;
    
    free(temp);
}

void displayChaining(struct HashTable* ht) {
    for (int i = 0; i < TABLE_SIZE; i++) {
        printf("Index %d: ", i);
        struct HashNode* temp = ht->table[i];
        while (temp != NULL) {
            printf("(%d, %d) -> ", temp->key, temp->value);
            temp = temp->next;
        }
        printf("NULL\n");
    }
}
```

**Advantages**:
- Simple implementation
- Never fills up
- Less sensitive to hash function

**Disadvantages**:
- Extra memory for pointers
- Cache performance

#### 2. Closed Hashing (Open Addressing)

All elements stored in the hash table itself. When collision occurs, find another slot.

##### a) Linear Probing

Search sequentially for next empty slot.

```c
#define HASH_SIZE 10

struct ClosedHashTable {
    int keys[HASH_SIZE];
    int values[HASH_SIZE];
    int occupied[HASH_SIZE];
};

void initClosedHashTable(struct ClosedHashTable* ht) {
    for (int i = 0; i < HASH_SIZE; i++) {
        ht->keys[i] = -1;
        ht->values[i] = -1;
        ht->occupied[i] = 0;
    }
}

int linearProbe(int key, int i) {
    return (hashFunction(key, HASH_SIZE) + i) % HASH_SIZE;
}

void insertLinearProbing(struct ClosedHashTable* ht, int key, int value) {
    int i = 0;
    int index;
    
    while (i < HASH_SIZE) {
        index = linearProbe(key, i);
        
        if (ht->occupied[index] == 0) {
            ht->keys[index] = key;
            ht->values[index] = value;
            ht->occupied[index] = 1;
            return;
        }
        i++;
    }
    printf("Hash table is full\n");
}

int searchLinearProbing(struct ClosedHashTable* ht, int key) {
    int i = 0;
    int index;
    
    while (i < HASH_SIZE) {
        index = linearProbe(key, i);
        
        if (ht->occupied[index] == 0)
            return -1;
        
        if (ht->keys[index] == key)
            return ht->values[index];
        
        i++;
    }
    return -1;
}

void displayLinearProbing(struct ClosedHashTable* ht) {
    printf("Hash Table (Linear Probing):\n");
    for (int i = 0; i < HASH_SIZE; i++) {
        if (ht->occupied[i])
            printf("Index %d: (%d, %d)\n", i, ht->keys[i], ht->values[i]);
        else
            printf("Index %d: EMPTY\n", i);
    }
}
```

**Problems**:
- Primary clustering

##### b) Quadratic Probing

Use quadratic function to find next slot.

```c
int quadraticProbe(int key, int i) {
    return (hashFunction(key, HASH_SIZE) + i * i) % HASH_SIZE;
}

void insertQuadraticProbing(struct ClosedHashTable* ht, int key, int value) {
    int i = 0;
    int index;
    
    while (i < HASH_SIZE) {
        index = quadraticProbe(key, i);
        
        if (ht->occupied[index] == 0) {
            ht->keys[index] = key;
            ht->values[index] = value;
            ht->occupied[index] = 1;
            return;
        }
        i++;
    }
    printf("Hash table is full\n");
}
```

**Problems**:
- Secondary clustering
- May not probe all slots

##### c) Double Hashing

Use second hash function for probing.

```c
int hash2(int key) {
    return 7 - (key % 7);
}

int doubleHash(int key, int i) {
    return (hashFunction(key, HASH_SIZE) + i * hash2(key)) % HASH_SIZE;
}

void insertDoubleHashing(struct ClosedHashTable* ht, int key, int value) {
    int i = 0;
    int index;
    
    while (i < HASH_SIZE) {
        index = doubleHash(key, i);
        
        if (ht->occupied[index] == 0) {
            ht->keys[index] = key;
            ht->values[index] = value;
            ht->occupied[index] = 1;
            return;
        }
        i++;
    }
    printf("Hash table is full\n");
}
```

**Advantages**:
- Uniform distribution
- Better than linear/quadratic probing

### Comparison of Collision Resolution

| Method | Average Search | Worst Search | Clustering |
|--------|---------------|--------------|------------|
| Separate Chaining | O(1 + α) | O(n) | No |
| Linear Probing | O(1/(1-α)) | O(n) | Primary |
| Quadratic Probing | O(1/(1-α)) | O(n) | Secondary |
| Double Hashing | O(1/(1-α)) | O(n) | Minimal |

Where α = Load Factor = n/m (n = number of elements, m = table size)

### Applications of Hashing
- Database indexing
- Caches (browser cache, DNS cache)
- Password verification
- Compiler symbol tables
- Cryptography
- Blockchain

---

## Summary of Time Complexities

### Tree Operations

| Operation | Binary Tree | BST (Avg) | BST (Worst) | AVL Tree | Heap |
|-----------|------------|-----------|-------------|----------|------|
| Search | O(n) | O(log n) | O(n) | O(log n) | O(n) |
| Insert | O(n) | O(log n) | O(n) | O(log n) | O(log n) |
| Delete | O(n) | O(log n) | O(n) | O(log n) | O(log n) |
| Traversal | O(n) | O(n) | O(n) | O(n) | O(n) |

### Sorting Algorithms

| Algorithm | Best | Average | Worst | Space |
|-----------|------|---------|-------|-------|
| Tree Sort | O(n log n) | O(n log n) | O(n²) | O(n) |
| Heap Sort | O(n log n) | O(n log n) | O(n log n) | O(1) |

### Graph Operations

| Operation | Adjacency Matrix | Adjacency List |
|-----------|-----------------|----------------|
| Add Edge | O(1) | O(1) |
| Remove Edge | O(1) | O(V) |
| Check Edge | O(1) | O(V) |
| BFS/DFS | O(V²) | O(V + E) |
| Space | O(V²) | O(V + E) |

### Hashing Operations

| Operation | Average | Worst |
|-----------|---------|-------|
| Search | O(1) | O(n) |
| Insert | O(1) | O(n) |
| Delete | O(1) | O(n) |

---

## Important Points to Remember

### Binary Trees
- Maximum nodes at level l = 2^l
- Maximum nodes in tree of height h = 2^(h+1) - 1
- Minimum height for n nodes = log₂(n+1) - 1

### BST
- Inorder traversal gives sorted sequence
- Best for searching when balanced
- Degenerates to linked list when unbalanced

### AVL Trees
- Balance factor: {-1, 0, 1}
- Four rotation cases: LL, RR, LR, RL
- Height ≤ 1.44 log₂(n)

### Heaps
- Complete binary tree
- Parent always greater/smaller than children
- Used in priority queues and heap sort

### Graphs
- Dense graph: Use adjacency matrix
- Sparse graph: Use adjacency list
- BFS for shortest path in unweighted graph
- DFS for connectivity and cycles

### Hashing
- Load factor α = n/m should be < 0.7
- Separate chaining: No limit on elements
- Open addressing: Limited by table size
- Good hash function essential for performance

---

## Practice Problems

1. Implement inorder traversal without recursion
2. Find height of binary tree
3. Check if tree is BST
4. Find kth smallest element in BST
5. Build heap from array
6. Implement BFS and DFS
7. Detect cycle in graph
8. Implement hash table with chaining
9. Find shortest path in graph
10. Balance an unbalanced BST

---

## Exam Tips

1. **Draw diagrams** for tree rotations and graph traversals
2. **Trace algorithms** step by step on paper
3. **Remember time complexities** for all operations
4. **Practice coding** all data structures from scratch
5. **Understand trade-offs** between different implementations
6. **Know applications** of each data structure
7. **Be clear on terminology** (heap vs binary tree, graph types)
8. **Master recursion** for tree operations
9. **Understand collision resolution** strategies thoroughly
10. **Practice converting** between infix, prefix, postfix

---

*End of Document*
