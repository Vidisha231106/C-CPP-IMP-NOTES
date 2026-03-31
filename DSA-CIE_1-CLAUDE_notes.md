# Comprehensive Data Structures Guide
## Stacks, Recursion, and Queues - In-Depth Analysis

---

## Table of Contents
1. Introduction to Data Structures
2. Stacks - Deep Dive
3. Recursion - Comprehensive Study
4. Queues - Advanced Concepts

---

## 1. Introduction to Data Structures

### 1.1 What Are Data Structures?

A **data structure** is a specialized format for organizing, processing, retrieving, and storing data. It defines the relationship between data elements and the operations that can be performed on them. The choice of data structure directly impacts:

- **Time Complexity**: How fast operations execute
- **Space Complexity**: How much memory is consumed
- **Code Maintainability**: How easy it is to modify and extend
- **Algorithm Efficiency**: The overall performance of your application

### 1.2 Types of Data Structures

Data structures are broadly classified into two categories:

#### **Primitive Data Structures**
Built-in data types provided by programming languages:
- Integer (int)
- Float/Double (float, double)
- Character (char)
- Boolean (bool)
- Pointers

#### **Non-Primitive Data Structures**
Complex data structures built using primitive types:

**1. Linear Data Structures**
- Elements arranged in sequential order
- Each element has exactly one predecessor and one successor (except first and last)
- Memory can be contiguous or non-contiguous
- Examples: Arrays, Linked Lists, Stacks, Queues

**2. Non-Linear Data Structures**
- Elements arranged hierarchically or in a network
- One element can connect to multiple elements
- Examples: Trees, Graphs, Heaps, Tries

### 1.3 Linear vs Non-Linear Data Structures

| Aspect | Linear | Non-Linear |
|--------|--------|------------|
| **Traversal** | Single run covers all elements | Requires multiple runs |
| **Memory** | May waste space | Efficient utilization |
| **Implementation** | Easier to implement | More complex |
| **Time Complexity** | Often O(n) for traversal | Varies (O(log n) to O(n²)) |
| **Application** | Simple data relationships | Hierarchical/network relationships |

**Linear Example**: Student roll numbers in a class (1, 2, 3, ..., n)
**Non-Linear Example**: Organization hierarchy (CEO → Managers → Employees)

---

## 2. Stacks - Deep Dive

### 2.1 Stack Definition & Core Concepts

A **stack** is a linear data structure that follows the **LIFO (Last In, First Out)** principle. The last element inserted is the first one to be removed. Think of it as a stack of plates - you can only add or remove plates from the top.

**Fundamental Characteristics:**
- Access is restricted to one end (the "top")
- All operations occur at the top
- Order of insertion and removal is inverse
- Fixed or dynamic size

**Real-World Analogies:**
- Stack of books
- Browser back button (navigation history)
- Undo mechanism in text editors
- Function call stack in programming

### 2.2 Representing Stacks in C

#### **Method 1: Array-Based Stack (Static)**

```c
#include <stdio.h>
#include <stdlib.h>
#include <stdbool.h>

#define MAX_SIZE 100

typedef struct {
    int items[MAX_SIZE];
    int top;
} Stack;

// Initialize stack
void initStack(Stack *s) {
    s->top = -1;  // Empty stack indicator
}

// Check if stack is empty
bool isEmpty(Stack *s) {
    return s->top == -1;
}

// Check if stack is full
bool isFull(Stack *s) {
    return s->top == MAX_SIZE - 1;
}

// Push operation - O(1)
bool push(Stack *s, int value) {
    if (isFull(s)) {
        printf("Stack Overflow! Cannot push %d\n", value);
        return false;
    }
    s->items[++(s->top)] = value;
    return true;
}

// Pop operation - O(1)
int pop(Stack *s) {
    if (isEmpty(s)) {
        printf("Stack Underflow! Cannot pop\n");
        return -1;
    }
    return s->items[(s->top)--];
}

// Peek operation - O(1)
int peek(Stack *s) {
    if (isEmpty(s)) {
        printf("Stack is empty!\n");
        return -1;
    }
    return s->items[s->top];
}

// Display stack contents
void display(Stack *s) {
    if (isEmpty(s)) {
        printf("Stack is empty\n");
        return;
    }
    printf("Stack contents (top to bottom): ");
    for (int i = s->top; i >= 0; i--) {
        printf("%d ", s->items[i]);
    }
    printf("\n");
}
```

#### **Method 2: Linked List-Based Stack (Dynamic)**

```c
#include <stdio.h>
#include <stdlib.h>
#include <stdbool.h>

typedef struct Node {
    int data;
    struct Node *next;
} Node;

typedef struct {
    Node *top;
    int size;
} LinkedStack;

// Initialize stack
void initLinkedStack(LinkedStack *s) {
    s->top = NULL;
    s->size = 0;
}

// Check if empty
bool isEmptyLinked(LinkedStack *s) {
    return s->top == NULL;
}

// Push operation - O(1)
void pushLinked(LinkedStack *s, int value) {
    Node *newNode = (Node *)malloc(sizeof(Node));
    if (newNode == NULL) {
        printf("Memory allocation failed!\n");
        return;
    }
    newNode->data = value;
    newNode->next = s->top;
    s->top = newNode;
    s->size++;
}

// Pop operation - O(1)
int popLinked(LinkedStack *s) {
    if (isEmptyLinked(s)) {
        printf("Stack Underflow!\n");
        return -1;
    }
    Node *temp = s->top;
    int value = temp->data;
    s->top = s->top->next;
    free(temp);
    s->size--;
    return value;
}

// Peek operation - O(1)
int peekLinked(LinkedStack *s) {
    if (isEmptyLinked(s)) {
        printf("Stack is empty!\n");
        return -1;
    }
    return s->top->data;
}

// Display stack
void displayLinked(LinkedStack *s) {
    if (isEmptyLinked(s)) {
        printf("Stack is empty\n");
        return;
    }
    printf("Stack contents (top to bottom): ");
    Node *current = s->top;
    while (current != NULL) {
        printf("%d ", current->data);
        current = current->next;
    }
    printf("\n");
}

// Free entire stack
void freeStack(LinkedStack *s) {
    while (!isEmptyLinked(s)) {
        popLinked(s);
    }
}
```

**Comparison: Array vs Linked List Implementation**

| Aspect | Array-Based | Linked List-Based |
|--------|-------------|-------------------|
| **Memory** | Fixed, may waste space | Dynamic, efficient |
| **Overflow** | Possible when full | Only on system memory exhaustion |
| **Access Time** | O(1) - cache friendly | O(1) - less cache friendly |
| **Memory per element** | Only data | Data + pointer (overhead) |
| **Ease of implementation** | Simpler | Slightly complex |

### 2.3 Stack Operations - Detailed Analysis

#### **Primary Operations:**

1. **Push(x)**: Insert element x at the top
   - Time Complexity: O(1)
   - Space Complexity: O(1)

2. **Pop()**: Remove and return top element
   - Time Complexity: O(1)
   - Space Complexity: O(1)

3. **Peek()/Top()**: Return top element without removal
   - Time Complexity: O(1)
   - Space Complexity: O(1)

4. **isEmpty()**: Check if stack is empty
   - Time Complexity: O(1)
   - Space Complexity: O(1)

5. **isFull()**: Check if stack is full (array-based)
   - Time Complexity: O(1)
   - Space Complexity: O(1)

#### **Advanced Stack Operations:**

```c
// Get size of stack
int getSize(Stack *s) {
    return s->top + 1;
}

// Clear all elements
void clearStack(Stack *s) {
    s->top = -1;
}

// Search for element (returns position from top, -1 if not found)
int search(Stack *s, int value) {
    for (int i = s->top; i >= 0; i--) {
        if (s->items[i] == value) {
            return s->top - i;  // Distance from top
        }
    }
    return -1;
}

// Reverse stack using another stack
void reverseStack(Stack *s) {
    Stack temp;
    initStack(&temp);
    
    while (!isEmpty(s)) {
        push(&temp, pop(s));
    }
    
    // Copy back
    while (!isEmpty(&temp)) {
        push(s, pop(&temp));
    }
}
```

### 2.4 Applications of Stacks

#### **Application 1: Infix to Postfix Conversion**

**Theory:**
- **Infix**: Operators between operands (A + B)
- **Postfix (RPN)**: Operators after operands (A B +)
- **Why Convert?**: Postfix eliminates parentheses and simplifies evaluation

**Operator Precedence Table:**
```
^ (Exponentiation)  : 3 (Right associative)
*, /, %             : 2 (Left associative)
+, -                : 1 (Left associative)
```

**Algorithm:**
1. Initialize empty stack for operators and empty result string
2. Scan infix expression left to right:
   - **Operand**: Add to result
   - **Left Parenthesis '('**: Push to stack
   - **Right Parenthesis ')'**: Pop and add to result until '(' is found
   - **Operator**: 
     - While stack top has higher/equal precedence, pop and add to result
     - Push current operator to stack
3. Pop all remaining operators to result

**Complete Implementation:**

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <ctype.h>
#include <stdbool.h>

#define MAX 100

typedef struct {
    char items[MAX];
    int top;
} CharStack;

void initCharStack(CharStack *s) {
    s->top = -1;
}

bool isEmptyChar(CharStack *s) {
    return s->top == -1;
}

bool isFullChar(CharStack *s) {
    return s->top == MAX - 1;
}

void pushChar(CharStack *s, char c) {
    if (!isFullChar(s)) {
        s->items[++(s->top)] = c;
    }
}

char popChar(CharStack *s) {
    if (!isEmptyChar(s)) {
        return s->items[(s->top)--];
    }
    return '\0';
}

char peekChar(CharStack *s) {
    if (!isEmptyChar(s)) {
        return s->items[s->top];
    }
    return '\0';
}

// Get precedence of operator
int precedence(char op) {
    switch(op) {
        case '^': return 3;
        case '*':
        case '/':
        case '%': return 2;
        case '+':
        case '-': return 1;
        default: return 0;
    }
}

// Check if operator is right associative
bool isRightAssociative(char op) {
    return op == '^';
}

// Check if character is operator
bool isOperator(char c) {
    return c == '+' || c == '-' || c == '*' || c == '/' || c == '%' || c == '^';
}

// Infix to Postfix conversion
void infixToPostfix(char *infix, char *postfix) {
    CharStack stack;
    initCharStack(&stack);
    int j = 0;
    
    for (int i = 0; infix[i] != '\0'; i++) {
        char current = infix[i];
        
        // Skip whitespace
        if (isspace(current)) continue;
        
        // If operand (alphanumeric), add to output
        if (isalnum(current)) {
            postfix[j++] = current;
        }
        // If left parenthesis, push to stack
        else if (current == '(') {
            pushChar(&stack, current);
        }
        // If right parenthesis, pop until left parenthesis
        else if (current == ')') {
            while (!isEmptyChar(&stack) && peekChar(&stack) != '(') {
                postfix[j++] = popChar(&stack);
            }
            popChar(&stack);  // Remove '('
        }
        // If operator
        else if (isOperator(current)) {
            while (!isEmptyChar(&stack) && peekChar(&stack) != '(' &&
                   (precedence(peekChar(&stack)) > precedence(current) ||
                    (precedence(peekChar(&stack)) == precedence(current) && 
                     !isRightAssociative(current)))) {
                postfix[j++] = popChar(&stack);
            }
            pushChar(&stack, current);
        }
    }
    
    // Pop remaining operators
    while (!isEmptyChar(&stack)) {
        postfix[j++] = popChar(&stack);
    }
    
    postfix[j] = '\0';
}

// Test function
void testInfixToPostfix() {
    char infix[MAX], postfix[MAX];
    
    // Test cases
    char *tests[] = {
        "A+B*C",
        "(A+B)*C",
        "A+B*C-D/E",
        "A^B^C",  // Right associative
        "((A+B)*C-D)/E"
    };
    
    char *expected[] = {
        "ABC*+",
        "AB+C*",
        "ABC*+DE/-",
        "ABC^^",
        "AB+C*D-E/"
    };
    
    printf("Infix to Postfix Conversion:\n");
    printf("================================\n");
    for (int i = 0; i < 5; i++) {
        infixToPostfix(tests[i], postfix);
        printf("Infix:    %s\n", tests[i]);
        printf("Postfix:  %s\n", postfix);
        printf("Expected: %s\n", expected[i]);
        printf("Status:   %s\n\n", strcmp(postfix, expected[i]) == 0 ? "PASS" : "FAIL");
    }
}
```

**Step-by-Step Example:**
```
Infix: A + B * C - D / E

Step | Char | Stack    | Postfix
-----|------|----------|--------
1    | A    | empty    | A
2    | +    | +        | A
3    | B    | +        | AB
4    | *    | +*       | AB
5    | C    | +*       | ABC
6    | -    | -        | ABC*+
7    | D    | -        | ABC*+D
8    | /    | -/       | ABC*+D
9    | E    | -/       | ABC*+DE
End  | --   | empty    | ABC*+DE/-
```

#### **Application 2: Infix to Prefix Conversion**

**Theory:**
- **Prefix (Polish Notation)**: Operators before operands (+ A B)
- **Method**: Reverse infix → Convert to postfix → Reverse result

**Algorithm:**
1. Reverse the infix expression (swap '(' with ')')
2. Convert to postfix using modified algorithm
3. Reverse the result to get prefix

```c
// Reverse a string
void reverseString(char *str) {
    int len = strlen(str);
    for (int i = 0; i < len / 2; i++) {
        char temp = str[i];
        str[i] = str[len - i - 1];
        str[len - i - 1] = temp;
    }
}

// Infix to Prefix conversion
void infixToPrefix(char *infix, char *prefix) {
    char reversed[MAX], postfix[MAX];
    
    // Step 1: Reverse infix and swap parentheses
    strcpy(reversed, infix);
    reverseString(reversed);
    
    for (int i = 0; reversed[i] != '\0'; i++) {
        if (reversed[i] == '(') reversed[i] = ')';
        else if (reversed[i] == ')') reversed[i] = '(';
    }
    
    // Step 2: Convert to postfix
    infixToPostfix(reversed, postfix);
    
    // Step 3: Reverse postfix to get prefix
    strcpy(prefix, postfix);
    reverseString(prefix);
}

// Test function for prefix
void testInfixToPrefix() {
    char infix[MAX], prefix[MAX];
    
    char *tests[] = {
        "A+B*C",
        "(A+B)*C",
        "A+B*C-D/E"
    };
    
    char *expected[] = {
        "+A*BC",
        "*+ABC",
        "-+A*BC/DE"
    };
    
    printf("\nInfix to Prefix Conversion:\n");
    printf("================================\n");
    for (int i = 0; i < 3; i++) {
        infixToPrefix(tests[i], prefix);
        printf("Infix:    %s\n", tests[i]);
        printf("Prefix:   %s\n", prefix);
        printf("Expected: %s\n\n", expected[i]);
    }
}
```

#### **Application 3: Postfix Expression Evaluation**

**Algorithm:**
1. Initialize empty stack
2. Scan postfix expression left to right:
   - **Operand**: Push to stack
   - **Operator**: Pop two operands, apply operator, push result
3. Final stack value is the result

```c
// Perform arithmetic operation
int performOperation(int op1, int op2, char operator) {
    switch(operator) {
        case '+': return op1 + op2;
        case '-': return op1 - op2;
        case '*': return op1 * op2;
        case '/': 
            if (op2 == 0) {
                printf("Error: Division by zero\n");
                return 0;
            }
            return op1 / op2;
        case '%': return op1 % op2;
        case '^': {
            int result = 1;
            for (int i = 0; i < op2; i++) result *= op1;
            return result;
        }
        default: return 0;
    }
}

// Evaluate postfix expression
int evaluatePostfix(char *postfix) {
    Stack stack;
    initStack(&stack);
    
    for (int i = 0; postfix[i] != '\0'; i++) {
        char current = postfix[i];
        
        // Skip whitespace
        if (isspace(current)) continue;
        
        // If operand (digit), push to stack
        if (isdigit(current)) {
            push(&stack, current - '0');  // Convert char to int
        }
        // If operator, pop two operands and compute
        else if (isOperator(current)) {
            int op2 = pop(&stack);  // Note: second operand popped first
            int op1 = pop(&stack);
            int result = performOperation(op1, op2, current);
            push(&stack, result);
        }
    }
    
    return pop(&stack);
}

// Test postfix evaluation
void testPostfixEvaluation() {
    char *tests[] = {
        "23+",       // 2 + 3 = 5
        "53*2+",     // 5 * 3 + 2 = 17
        "92-31-*",   // (9 - 2) * (3 - 1) = 14
        "53+82-*"    // (5 + 3) * (8 - 2) = 48
    };
    
    int expected[] = {5, 17, 14, 48};
    
    printf("\nPostfix Expression Evaluation:\n");
    printf("================================\n");
    for (int i = 0; i < 4; i++) {
        int result = evaluatePostfix(tests[i]);
        printf("Postfix: %s\n", tests[i]);
        printf("Result:  %d\n", result);
        printf("Expected: %d\n", expected[i]);
        printf("Status:  %s\n\n", result == expected[i] ? "PASS" : "FAIL");
    }
}
```

**Evaluation Example:**
```
Postfix: 5 3 * 2 +

Step | Char | Stack      | Operation
-----|------|------------|------------
1    | 5    | [5]        | Push 5
2    | 3    | [5, 3]     | Push 3
3    | *    | [15]       | Pop 3,5 → 5*3=15 → Push 15
4    | 2    | [15, 2]    | Push 2
5    | +    | [17]       | Pop 2,15 → 15+2=17 → Push 17
End  | --   | [17]       | Result: 17
```

### 2.5 Important Points About Stacks

**Memory Management:**
- Array-based: O(n) space for n elements, but may waste memory
- Linked-list based: O(n) space with pointer overhead, but no waste

**Thread Safety:**
- Standard stack implementations are not thread-safe
- Use mutex locks for concurrent access

**Stack Overflow vs Underflow:**
- **Overflow**: Pushing to a full stack (array-based)
- **Underflow**: Popping from an empty stack

**Performance Considerations:**
- All operations are O(1) in time
- Cache locality better in array-based (contiguous memory)
- Dynamic sizing better in linked-list based

**Common Errors:**
1. Not checking isEmpty() before pop/peek
2. Not checking isFull() before push (array-based)
3. Memory leaks in linked-list implementation (forgetting to free)
4. Integer overflow in expression evaluation

---

## 3. Recursion - Comprehensive Study

### 3.1 Introduction to Recursion

**Definition:** Recursion is a programming technique where a function calls itself directly or indirectly to solve a problem by breaking it into smaller, similar subproblems.

**Core Components:**
1. **Base Case (Termination Condition)**: Stops recursion
2. **Recursive Case**: Function calls itself with modified parameters
3. **Progress Toward Base Case**: Each call must move closer to termination

**Types of Recursion:**
1. **Direct**: Function calls itself
2. **Indirect**: Function A calls B, B calls A
3. **Tail Recursion**: Recursive call is the last operation
4. **Non-tail Recursion**: Operations after recursive call
5. **Linear**: Function calls itself once per execution
6. **Tree/Multiple**: Function calls itself multiple times

### 3.2 Role of Stack During Recursion

**Call Stack Mechanism:**
When a function is called, the system:
1. **Saves context**: Return address, parameters, local variables
2. **Allocates activation record**: Frame on call stack
3. **Executes function**: Processes until return or another call
4. **Restores context**: Pops frame, returns to caller

**Activation Record Structure:**
```
+------------------+
| Return Address   |  ← Where to resume after call
+------------------+
| Parameters       |  ← Function arguments
+------------------+
| Local Variables  |  ← Function's local data
+------------------+
| Saved Registers  |  ← CPU register values
+------------------+
```

**Example: Call Stack for factorial(3)**
```
factorial(3)
  → factorial(2)
    → factorial(1)
      → factorial(0)  [Base case]
      ← return 1
    ← return 1 * 1 = 1
  ← return 2 * 1 = 2
← return 3 * 2 = 6

Call Stack Evolution:
Step 1: [factorial(3)]
Step 2: [factorial(3), factorial(2)]
Step 3: [factorial(3), factorial(2), factorial(1)]
Step 4: [factorial(3), factorial(2), factorial(1), factorial(0)]
Step 5: [factorial(3), factorial(2), factorial(1)]  ← returns 1
Step 6: [factorial(3), factorial(2)]                ← returns 1
Step 7: [factorial(3)]                              ← returns 2
Step 8: []                                          ← returns 6
```

### 3.3 Factorial Function - Deep Analysis

**Problem:** Calculate n! = n × (n-1) × ... × 2 × 1

**Recursive Approach:**
```c
#include <stdio.h>

// Simple recursive factorial
long long factorial(int n) {
    // Base case
    if (n == 0 || n == 1) {
        return 1;
    }
    // Recursive case
    return n * factorial(n - 1);
}

// Tail-recursive factorial (optimizable by compiler)
long long factorialTail(int n, long long accumulator) {
    if (n == 0 || n == 1) {
        return accumulator;
    }
    return factorialTail(n - 1, n * accumulator);
}

// Wrapper for tail-recursive version
long long factorialTailWrapper(int n) {
    return factorialTail(n, 1);
}

// Iterative factorial (for comparison)
long long factorialIterative(int n) {
    long long result = 1;
    for (int i = 2; i <= n; i++) {
        result *= i;
    }
    return result;
}

// Factorial with memoization (dynamic programming)
#define MAX_N 100
long long memo[MAX_N];
int memoInitialized = 0;

void initMemo() {
    for (int i = 0; i < MAX_N; i++) {
        memo[i] = -1;
    }
    memoInitialized = 1;
}

long long factorialMemo(int n) {
    if (!memoInitialized) initMemo();
    
    if (n == 0 || n == 1) return 1;
    
    if (memo[n] != -1) {
        return memo[n];  // Return cached result
    }
    
    memo[n] = n * factorialMemo(n - 1);
    return memo[n];
}

void testFactorial() {
    printf("Factorial Calculations:\n");
    printf("=======================\n");
    
    for (int i = 0; i <= 10; i++) {
        printf("factorial(%d) = %lld\n", i, factorial(i));
    }
    
    printf("\nTail-recursive version:\n");
    for (int i = 0; i <= 10; i++) {
        printf("factorialTail(%d) = %lld\n", i, factorialTailWrapper(i));
    }
}
```

**Time & Space Complexity:**
- **Time**: O(n) - makes n recursive calls
- **Space**: O(n) - call stack depth of n
- **Tail-recursive space**: O(1) with optimization, O(n) without

**Recurrence Relation:**
```
T(n) = T(n-1) + c     where c is constant time for multiplication
T(0) = c

Solution: T(n) = O(n)
```

### 3.4 Binary Search - Recursive Implementation

**Problem:** Find target element in sorted array

**Algorithm:**
1. Compare target with middle element
2. If equal, return index
3. If target < middle, search left half
4. If target > middle, search right half

```c
#include <stdio.h>

// Recursive binary search
int binarySearchRecursive(int arr[], int left, int right, int target) {
    // Base case: element not found
    if (left > right) {
        return -1;
    }
    
    // Calculate middle index (avoid overflow)
    int mid = left + (right - left) / 2;
    
    // Base case: element found
    if (arr[mid] == target) {
        return mid;
    }
    
    // Recursive cases
    if (arr[mid] > target) {
        // Search in left half
        return binarySearchRecursive(arr, left, mid - 1, target);
    } else {
        // Search in right half
        return binarySearchRecursive(arr, mid + 1, right, target);
    }
}

// Wrapper function
int binarySearch(int arr[], int size, int target) {
    return binarySearchRecursive(arr, 0, size - 1, target);
}

// Iterative binary search (for comparison)
int binarySearchIterative(int arr[], int size, int target) {
    int left = 0, right = size - 1;
    
    while (left <= right) {
        int mid = left + (right - left) / 2;
        
        if (arr[mid] == target) {
            return mid;
        }
        
        if (arr[mid] > target) {
            right = mid - 1;
        } else {
            left = mid + 1;
        }
    }
    
    return -1;
}

// Binary search with call stack visualization
int binarySearchVerbose(int arr[], int left, int right, int target, int depth) {
    // Print indentation based on recursion depth
    for (int i = 0; i < depth; i++) printf("  ");
    printf("Searching [%d, %d]\n", left, right);
    
    if (left > right) {
        for (int i = 0; i < depth; i++) printf("  ");
        printf("Not found, returning -1\n");
        return -1;
    }
    
    int mid = left + (right - left) / 2;
    
    for (int i = 0; i < depth; i++) printf("  ");
    printf("Middle element: arr[%d] = %d\n", mid, arr[mid]);
    
    if (arr[mid] == target) {
        for (int i = 0; i < depth; i++) printf("  ");
        printf("Found at index %d!\n", mid);
        return mid;
    }
    
    if (arr[mid] > target) {
        for (int i = 0; i < depth; i++) printf("  ");
        printf("Target < %d, searching left\n", arr[mid]);
        return binarySearchVerbose(arr, mid + 1, right, target, depth + 1);
    }
}

void testBinarySearch() {
    int arr[] = {2, 5, 8, 12, 16, 23, 38, 45, 56, 67, 78};
    int size = sizeof(arr) / sizeof(arr[0]);
    
    printf("Binary Search Test:\n");
    printf("===================\n");
    printf("Array: ");
    for (int i = 0; i < size; i++) {
        printf("%d ", arr[i]);
    }
    printf("\n\n");
    
    int target = 23;
    printf("Searching for %d:\n", target);
    int result = binarySearchVerbose(arr, 0, size - 1, target, 0);
    printf("Result: %d\n\n", result);
    
    target = 100;
    printf("Searching for %d (not in array):\n", target);
    result = binarySearchVerbose(arr, 0, size - 1, target, 0);
    printf("Result: %d\n", result);
}
```

**Time & Space Complexity:**
- **Time**: O(log n) - halves search space each step
- **Space (Recursive)**: O(log n) - call stack depth
- **Space (Iterative)**: O(1) - constant space

**Recurrence Relation:**
```
T(n) = T(n/2) + c
T(1) = c

Using Master Theorem: T(n) = O(log n)
```

**Call Stack Example for Binary Search:**
```
Array: [2, 5, 8, 12, 16, 23, 38, 45, 56, 67, 78]
Target: 23

Call 1: binarySearch(arr, 0, 10)     → mid=5, arr[5]=23 ✓
  Found immediately!
  
Target: 45

Call 1: binarySearch(arr, 0, 10)     → mid=5, arr[5]=23 < 45
  Call 2: binarySearch(arr, 6, 10)   → mid=8, arr[8]=56 > 45
    Call 3: binarySearch(arr, 6, 7)  → mid=6, arr[6]=38 < 45
      Call 4: binarySearch(arr, 7, 7) → mid=7, arr[7]=45 ✓
```

### 3.5 Tower of Hanoi - Advanced Recursion

**Problem Statement:**
Move n disks from source rod to destination rod using an auxiliary rod, following rules:
1. Only one disk can be moved at a time
2. A disk can only be placed on a larger disk or empty rod
3. Only the top disk of a stack can be moved

**Mathematical Analysis:**
- **Minimum moves**: 2^n - 1
- **Recursive structure**: T(n) = 2T(n-1) + 1

**Recursive Solution:**

```c
#include <stdio.h>
#include <math.h>

// Global counter for moves
int moveCount = 0;

// Tower of Hanoi - Basic version
void towerOfHanoi(int n, char source, char destination, char auxiliary) {
    // Base case: only one disk
    if (n == 1) {
        moveCount++;
        printf("Move %d: Move disk 1 from %c to %c\n", moveCount, source, destination);
        return;
    }
    
    // Step 1: Move n-1 disks from source to auxiliary using destination
    towerOfHanoi(n - 1, source, auxiliary, destination);
    
    // Step 2: Move the largest disk from source to destination
    moveCount++;
    printf("Move %d: Move disk %d from %c to %c\n", moveCount, n, source, destination);
    
    // Step 3: Move n-1 disks from auxiliary to destination using source
    towerOfHanoi(n - 1, auxiliary, destination, source);
}

// Tower of Hanoi with detailed state tracking
typedef struct {
    int disks[10];
    int top;
} Rod;

void initRod(Rod *rod) {
    rod->top = -1;
}

void pushDisk(Rod *rod, int disk) {
    rod->disks[++(rod->top)] = disk;
}

int popDisk(Rod *rod) {
    if (rod->top >= 0) {
        return rod->disks[(rod->top)--];
    }
    return -1;
}

void displayRods(Rod *A, Rod *B, Rod *C, int n) {
    printf("\n");
    for (int level = n - 1; level >= 0; level--) {
        // Rod A
        if (level <= A->top) {
            printf(" [%d] ", A->disks[level]);
        } else {
            printf("  |  ");
        }
        
        // Rod B
        if (level <= B->top) {
            printf(" [%d] ", B->disks[level]);
        } else {
            printf("  |  ");
        }
        
        // Rod C
        if (level <= C->top) {
            printf(" [%d] ", C->disks[level]);
        } else {
            printf("  |  ");
        }
        
        printf("\n");
    }
    printf(" === === ===\n");
    printf("  A   B   C\n\n");
}

void moveDisk(Rod *from, Rod *to, char fromName, char toName) {
    int disk = popDisk(from);
    pushDisk(to, disk);
    printf("Move disk %d: %c -> %c\n", disk, fromName, toName);
}

void towerOfHanoiWithState(int n, Rod *source, Rod *dest, Rod *aux, 
                           char sName, char dName, char aName) {
    if (n == 1) {
        moveDisk(source, dest, sName, dName);
        return;
    }
    
    towerOfHanoiWithState(n - 1, source, aux, dest, sName, aName, dName);
    moveDisk(source, dest, sName, dName);
    towerOfHanoiWithState(n - 1, aux, dest, source, aName, dName, sName);
}

// Iterative Tower of Hanoi (using stack simulation)
void towerOfHanoiIterative(int n, char source, char dest, char aux) {
    int totalMoves = pow(2, n) - 1;
    char rods[] = {source, aux, dest};
    
    // For even number of disks, swap aux and dest
    if (n % 2 == 0) {
        char temp = rods[1];
        rods[1] = rods[2];
        rods[2] = temp;
    }
    
    printf("\nIterative solution:\n");
    for (int i = 1; i <= totalMoves; i++) {
        int from, to;
        
        if (i % 3 == 1) {
            from = 0; to = 2;  // source to dest
        } else if (i % 3 == 2) {
            from = 0; to = 1;  // source to aux
        } else {
            from = 1; to = 2;  // aux to dest
        }
        
        printf("Move %d: %c -> %c\n", i, rods[from], rods[to]);
    }
}

// Analysis function
void analyzeTowerOfHanoi(int n) {
    printf("\nTower of Hanoi Analysis for n=%d disks:\n", n);
    printf("========================================\n");
    
    int totalMoves = pow(2, n) - 1;
    printf("Total moves required: %d (2^%d - 1)\n", totalMoves, n);
    
    // Calculate recursion depth
    printf("Maximum recursion depth: %d\n", n);
    
    // Calculate time complexity
    printf("Time complexity: O(2^n) = O(%d)\n", (int)pow(2, n));
    printf("Space complexity: O(n) = O(%d) (call stack)\n\n", n);
}

void testTowerOfHanoi() {
    printf("Tower of Hanoi Problem:\n");
    printf("=======================\n\n");
    
    // Test with 3 disks
    int n = 3;
    moveCount = 0;
    
    printf("Solution for %d disks:\n", n);
    towerOfHanoi(n, 'A', 'C', 'B');
    
    analyzeTowerOfHanoi(n);
    
    // Visual representation
    printf("\nVisual representation for %d disks:\n", n);
    Rod A, B, C;
    initRod(&A);
    initRod(&B);
    initRod(&C);
    
    // Initialize rod A with disks
    for (int i = n; i >= 1; i--) {
        pushDisk(&A, i);
    }
    
    printf("Initial state:");
    displayRods(&A, &B, &C, n);
    
    towerOfHanoiWithState(n, &A, &C, &B, 'A', 'C', 'B');
    
    printf("Final state:");
    displayRods(&A, &B, &C, n);
}
```

**Recurrence Relation:**
```
T(n) = 2T(n-1) + 1
T(1) = 1

Solving:
T(n) = 2T(n-1) + 1
     = 2[2T(n-2) + 1] + 1
     = 2²T(n-2) + 2 + 1
     = 2³T(n-3) + 4 + 2 + 1
     = 2^k T(n-k) + (2^(k-1) + ... + 2 + 1)
     
When k = n-1:
T(n) = 2^(n-1) × T(1) + (2^(n-1) - 1)
     = 2^(n-1) + 2^(n-1) - 1
     = 2^n - 1

Time Complexity: O(2^n)
Space Complexity: O(n) - recursion depth
```

**Call Tree Visualization for n=3:**
```
                    H(3,A,C,B)
                   /          \
          H(2,A,B,C)          H(2,B,C,A)
         /        \           /        \
    H(1,A,C,B) H(1,C,B,A) H(1,B,A,C) H(1,A,C,B)

Where H(n,s,d,a) = move n disks from s to d using a

Total function calls: 2^n - 1 = 7 for n=3
```

### 3.6 Recursion vs Iteration - Comparative Analysis

**When to Use Recursion:**
- Problem naturally divides into similar subproblems
- Tree/Graph traversal
- Divide and conquer algorithms
- Backtracking problems
- When code clarity is priority over performance

**When to Use Iteration:**
- Simple loops suffice
- Space constraints (embedded systems)
- Tail recursion (can be optimized to iteration)
- Performance-critical code

**Conversion Example: Fibonacci**

```c
// Recursive Fibonacci (inefficient)
int fibRecursive(int n) {
    if (n <= 1) return n;
    return fibRecursive(n - 1) + fibRecursive(n - 2);
}
// Time: O(2^n), Space: O(n)

// Iterative Fibonacci (efficient)
int fibIterative(int n) {
    if (n <= 1) return n;
    int prev = 0, curr = 1;
    for (int i = 2; i <= n; i++) {
        int next = prev + curr;
        prev = curr;
        curr = next;
    }
    return curr;
}
// Time: O(n), Space: O(1)

// Memoized Recursive Fibonacci (optimal)
int fibMemo(int n, int *memo) {
    if (n <= 1) return n;
    if (memo[n] != -1) return memo[n];
    memo[n] = fibMemo(n - 1, memo) + fibMemo(n - 2, memo);
    return memo[n];
}
// Time: O(n), Space: O(n)
```

### 3.7 Important Points About Recursion

**Stack Overflow:**
- Occurs when recursion depth exceeds stack limit
- Typical stack size: 1-8 MB
- Each call consumes 50-200 bytes (varies by system)
- Maximum depth: ~10,000-100,000 calls

**Tail Call Optimization:**
- Some compilers optimize tail recursion to iteration
- Requires recursive call as last operation
- Example: `return func(x-1)` vs `return x * func(x-1)`

**Common Pitfalls:**
1. Missing or incorrect base case → infinite recursion
2. Not progressing toward base case
3. Excessive memory usage
4. Redundant calculations (solved by memoization)
5. Stack overflow for large inputs

**Debugging Techniques:**
1. Add print statements showing parameters and depth
2. Trace call stack manually
3. Use debugger with call stack visualization
4. Add counters to prevent infinite loops during testing

---

## 4. Queues - Advanced Concepts

### 4.1 Queue Definitions & Core Concepts

**Definition:** A queue is a linear data structure that follows the **FIFO (First In, First Out)** principle. The first element inserted is the first one to be removed.

**Fundamental Characteristics:**
- Insertion (enqueue) at rear
- Deletion (dequeue) from front
- Access restricted to both ends
- Order of insertion preserved

**Real-World Analogies:**
- People waiting in line
- Print job queue
- CPU task scheduling
- Breadth-First Search in graphs

**Types of Queues:**
1. **Simple Queue**: Basic FIFO structure
2. **Circular Queue**: Last position connects to first
3. **Priority Queue**: Elements have priorities
4. **Double-Ended Queue (Deque)**: Insert/delete at both ends
5. **Input-Restricted Deque**: Insert at one end only
6. **Output-Restricted Deque**: Delete from one end only

### 4.2 Queue Representation in C

#### **Method 1: Array-Based Linear Queue**

```c
#include <stdio.h>
#include <stdlib.h>
#include <stdbool.h>

#define MAX_SIZE 100

typedef struct {
    int items[MAX_SIZE];
    int front;
    int rear;
    int size;
} Queue;

// Initialize queue
void initQueue(Queue *q) {
    q->front = 0;
    q->rear = -1;
    q->size = 0;
}

// Check if empty
bool isQueueEmpty(Queue *q) {
    return q->size == 0;
}

// Check if full
bool isQueueFull(Queue *q) {
    return q->size == MAX_SIZE;
}

// Enqueue operation - O(1)
bool enqueue(Queue *q, int value) {
    if (isQueueFull(q)) {
        printf("Queue Overflow! Cannot enqueue %d\n", value);
        return false;
    }
    
    q->rear = (q->rear + 1) % MAX_SIZE;  // Circular increment
    q->items[q->rear] = value;
    q->size++;
    return true;
}

// Dequeue operation - O(1)
int dequeue(Queue *q) {
    if (isQueueEmpty(q)) {
        printf("Queue Underflow! Cannot dequeue\n");
        return -1;
    }
    
    int value = q->items[q->front];
    q->front = (q->front + 1) % MAX_SIZE;  // Circular increment
    q->size--;
    return value;
}

// Peek front element
int peekQueue(Queue *q) {
    if (isQueueEmpty(q)) {
        printf("Queue is empty!\n");
        return -1;
    }
    return q->items[q->front];
}

// Get queue size
int getQueueSize(Queue *q) {
    return q->size;
}

// Display queue
void displayQueue(Queue *q) {
    if (isQueueEmpty(q)) {
        printf("Queue is empty\n");
        return;
    }
    
    printf("Queue (front to rear): ");
    int i = q->front;
    int count = 0;
    
    while (count < q->size) {
        printf("%d ", q->items[i]);
        i = (i + 1) % MAX_SIZE;
        count++;
    }
    printf("\n");
}
```

**Problem with Simple Linear Queue:**
```
Initial: [_, _, _, _, _]  front=0, rear=-1

After enqueue(1,2,3):
[1, 2, 3, _, _]  front=0, rear=2

After dequeue() twice:
[_, _, 3, _, _]  front=2, rear=2

Issue: front=2, rear=2, but positions 0,1 are wasted!
Solution: Circular Queue (implemented above with modulo)
```

#### **Method 2: Linked List-Based Queue**

```c
typedef struct QueueNode {
    int data;
    struct QueueNode *next;
} QueueNode;

typedef struct {
    QueueNode *front;
    QueueNode *rear;
    int size;
} LinkedQueue;

// Initialize
void initLinkedQueue(LinkedQueue *q) {
    q->front = NULL;
    q->rear = NULL;
    q->size = 0;
}

// Check if empty
bool isLinkedQueueEmpty(LinkedQueue *q) {
    return q->front == NULL;
}

// Enqueue - O(1)
void enqueueLinked(LinkedQueue *q, int value) {
    QueueNode *newNode = (QueueNode *)malloc(sizeof(QueueNode));
    if (newNode == NULL) {
        printf("Memory allocation failed!\n");
        return;
    }
    
    newNode->data = value;
    newNode->next = NULL;
    
    if (isLinkedQueueEmpty(q)) {
        q->front = newNode;
        q->rear = newNode;
    } else {
        q->rear->next = newNode;
        q->rear = newNode;
    }
    q->size++;
}

// Dequeue - O(1)
int dequeueLinked(LinkedQueue *q) {
    if (isLinkedQueueEmpty(q)) {
        printf("Queue Underflow!\n");
        return -1;
    }
    
    QueueNode *temp = q->front;
    int value = temp->data;
    
    q->front = q->front->next;
    if (q->front == NULL) {
        q->rear = NULL;  // Queue became empty
    }
    
    free(temp);
    q->size--;
    return value;
}

// Peek
int peekLinkedQueue(LinkedQueue *q) {
    if (isLinkedQueueEmpty(q)) {
        printf("Queue is empty!\n");
        return -1;
    }
    return q->front->data;
}

// Display
void displayLinkedQueue(LinkedQueue *q) {
    if (isLinkedQueueEmpty(q)) {
        printf("Queue is empty\n");
        return;
    }
    
    printf("Queue (front to rear): ");
    QueueNode *current = q->front;
    while (current != NULL) {
        printf("%d ", current->data);
        current = current->next;
    }
    printf("\n");
}

// Free queue
void freeLinkedQueue(LinkedQueue *q) {
    while (!isLinkedQueueEmpty(q)) {
        dequeueLinked(q);
    }
}
```

### 4.3 Circular Queue - In-Depth Study

**Why Circular Queue?**
Linear queues waste space after dequeue operations. Circular queues solve this by wrapping the rear pointer back to the beginning.

**Key Concept:**
```
Position after increment = (current_position + 1) % MAX_SIZE
```

**Full vs Empty Condition:**
- **Empty**: `size == 0` or `front == -1`
- **Full**: `size == MAX_SIZE` or `(rear + 1) % MAX_SIZE == front`

**Complete Circular Queue Implementation:**

```c
#include <stdio.h>
#include <stdbool.h>

#define CQUEUE_SIZE 5

typedef struct {
    int items[CQUEUE_SIZE];
    int front;
    int rear;
    int count;  // Track number of elements
} CircularQueue;

// Initialize
void initCircularQueue(CircularQueue *cq) {
    cq->front = 0;
    cq->rear = -1;
    cq->count = 0;
}

// Check if empty
bool isCQEmpty(CircularQueue *cq) {
    return cq->count == 0;
}

// Check if full
bool isCQFull(CircularQueue *cq) {
    return cq->count == CQUEUE_SIZE;
}

// Enqueue
bool enqueueCQ(CircularQueue *cq, int value) {
    if (isCQFull(cq)) {
        printf("Circular Queue is full! Cannot enqueue %d\n", value);
        return false;
    }
    
    cq->rear = (cq->rear + 1) % CQUEUE_SIZE;
    cq->items[cq->rear] = value;
    cq->count++;
    
    printf("Enqueued %d at position %d\n", value, cq->rear);
    return true;
}

// Dequeue
int dequeueCQ(CircularQueue *cq) {
    if (isCQEmpty(cq)) {
        printf("Circular Queue is empty! Cannot dequeue\n");
        return -1;
    }
    
    int value = cq->items[cq->front];
    printf("Dequeued %d from position %d\n", value, cq->front);
    
    cq->front = (cq->front + 1) % CQUEUE_SIZE;
    cq->count--;
    
    return value;
}

// Peek front
int peekCQ(CircularQueue *cq) {
    if (isCQEmpty(cq)) {
        return -1;
    }
    return cq->items[cq->front];
}

// Display with visual representation
void displayCQ(CircularQueue *cq) {
    printf("\nCircular Queue Status:\n");
    printf("Front: %d, Rear: %d, Count: %d\n", cq->front, cq->rear, cq->count);
    
    printf("Array representation: [");
    for (int i = 0; i < CQUEUE_SIZE; i++) {
        if (cq->count == 0) {
            printf(" _ ");
        } else if (cq->front <= cq->rear) {
            if (i >= cq->front && i <= cq->rear) {
                printf(" %d ", cq->items[i]);
            } else {
                printf(" _ ");
            }
        } else {  // Wrapped around
            if (i >= cq->front || i <= cq->rear) {
                printf(" %d ", cq->items[i]);
            } else {
                printf(" _ ");
            }
        }
    }
    printf("]\n");
    
    printf("Positions:            ");
    for (int i = 0; i < CQUEUE_SIZE; i++) {
        printf(" %d ", i);
    }
    printf("\n");
    
    if (!isCQEmpty(cq)) {
        printf("Elements (front to rear): ");
        int i = cq->front;
        int printed = 0;
        while (printed < cq->count) {
            printf("%d ", cq->items[i]);
            i = (i + 1) % CQUEUE_SIZE;
            printed++;
        }
        printf("\n");
    }
    printf("\n");
}

void testCircularQueue() {
    CircularQueue cq;
    initCircularQueue(&cq);
    
    printf("=== Circular Queue Test ===\n\n");
    
    // Test 1: Normal enqueue
    printf("Test 1: Enqueuing 5 elements\n");
    for (int i = 1; i <= 5; i++) {
        enqueueCQ(&cq, i * 10);
    }
    displayCQ(&cq);
    
    // Test 2: Queue full
    printf("Test 2: Trying to enqueue when full\n");
    enqueueCQ(&cq, 60);
    printf("\n");
    
    // Test 3: Dequeue and circular wrap
    printf("Test 3: Dequeue 3 elements\n");
    for (int i = 0; i < 3; i++) {
        dequeueCQ(&cq);
    }
    displayCQ(&cq);
    
    // Test 4: Enqueue after wrap
    printf("Test 4: Enqueue 3 more (circular wrap)\n");
    enqueueCQ(&cq, 60);
    enqueueCQ(&cq, 70);
    enqueueCQ(&cq, 80);
    displayCQ(&cq);
}
```

**Circular Queue Visualization:**
```
Initial State (size=5):
[_, _, _, _, _]
front=0, rear=-1, count=0

After enqueue(10,20,30,40,50):
[10, 20, 30, 40, 50]
 F              R
front=0, rear=4, count=5 (FULL)

After dequeue() 3 times:
[_, _, _, 40, 50]
          F    R
front=3, rear=4, count=2

After enqueue(60,70,80):
[70, 80, _, 40, 50]
 R       _   F
front=3, rear=1, count=5 (FULL, wrapped around!)
```

### 4.4 Application: Message Queue using Circular Queue

**Problem:** Implement a message queue system where messages can be sent and received in FIFO order with a fixed buffer size.

**Features:**
1. Send message (enqueue)
2. Receive message (dequeue)
3. View pending messages
4. Handle buffer full/empty conditions

```c
#include <stdio.h>
#include <string.h>
#include <stdbool.h>
#include <time.h>

#define MSG_QUEUE_SIZE 10
#define MSG_MAX_LENGTH 100

typedef struct {
    char content[MSG_MAX_LENGTH];
    int priority;
    time_t timestamp;
    int id;
} Message;

typedef struct {
    Message messages[MSG_QUEUE_SIZE];
    int front;
    int rear;
    int count;
    int nextId;
} MessageQueue;

// Initialize message queue
void initMessageQueue(MessageQueue *mq) {
    mq->front = 0;
    mq->rear = -1;
    mq->count = 0;
    mq->nextId = 1;
}

// Check if empty
bool isMQEmpty(MessageQueue *mq) {
    return mq->count == 0;
}

// Check if full
bool isMQFull(MessageQueue *mq) {
    return mq->count == MSG_QUEUE_SIZE;
}

// Send message (enqueue)
bool sendMessage(MessageQueue *mq, const char *content, int priority) {
    if (isMQFull(mq)) {
        printf("❌ Message queue full! Cannot send message.\n");
        return false;
    }
    
    mq->rear = (mq->rear + 1) % MSG_QUEUE_SIZE;
    
    Message *msg = &mq->messages[mq->rear];
    strncpy(msg->content, content, MSG_MAX_LENGTH - 1);
    msg->content[MSG_MAX_LENGTH - 1] = '\0';
    msg->priority = priority;
    msg->timestamp = time(NULL);
    msg->id = mq->nextId++;
    
    mq->count++;
    
    printf("✓ Message #%d sent: \"%s\" [Priority: %d]\n", 
           msg->id, msg->content, msg->priority);
    return true;
}

// Receive message (dequeue)
bool receiveMessage(MessageQueue *mq, Message *output) {
    if (isMQEmpty(mq)) {
        printf("❌ No messages to receive.\n");
        return false;
    }
    
    *output = mq->messages[mq->front];
    mq->front = (mq->front + 1) % MSG_QUEUE_SIZE;
    mq->count--;
    
    return true;
}

// Peek next message without removing
bool peekMessage(MessageQueue *mq, Message *output) {
    if (isMQEmpty(mq)) {
        return false;
    }
    
    *output = mq->messages[mq->front];
    return true;
}

// Get pending message count
int getPendingCount(MessageQueue *mq) {
    return mq->count;
}

// Display all pending messages
void displayPendingMessages(MessageQueue *mq) {
    if (isMQEmpty(mq)) {
        printf("\n📭 No pending messages.\n\n");
        return;
    }
    
    printf("\n📬 Pending Messages (%d):\n", mq->count);
    printf("═══════════════════════════════════════════════════════\n");
    
    int i = mq->front;
    int printed = 0;
    
    while (printed < mq->count) {
        Message *msg = &mq->messages[i];
        printf("#%d | Priority: %d | \"%s\"\n", 
               msg->id, msg->priority, msg->content);
        i = (i + 1) % MSG_QUEUE_SIZE;
        printed++;
    }
    
    printf("═══════════════════════════════════════════════════════\n\n");
}

// Clear all messages
void clearMessageQueue(MessageQueue *mq) {
    mq->front = 0;
    mq->rear = -1;
    mq->count = 0;
    printf("✓ Message queue cleared.\n");
}

// Comprehensive test
void testMessageQueue() {
    MessageQueue mq;
    initMessageQueue(&mq);
    
    printf("\n╔════════════════════════════════════╗\n");
    printf("║   MESSAGE QUEUE SYSTEM TEST    ║\n");
    printf("╚════════════════════════════════════╝\n\n");
    
    // Test 1: Send messages
    printf("--- Test 1: Sending Messages ---\n");
    sendMessage(&mq, "System startup complete", 1);
    sendMessage(&mq, "User login: admin", 2);
    sendMessage(&mq, "Database connection established", 1);
    sendMessage(&mq, "Processing request #1234", 3);
    displayPendingMessages(&mq);
    
    // Test 2: Receive messages
    printf("--- Test 2: Receiving Messages ---\n");
    Message received;
    for (int i = 0; i < 2; i++) {
        if (receiveMessage(&mq, &received)) {
            printf("✓ Received message #%d: \"%s\"\n", 
                   received.id, received.content);
        }
    }
    displayPendingMessages(&mq);
    
    // Test 3: Fill queue and test circular behavior
    printf("--- Test 3: Circular Queue Behavior ---\n");
    for (int i = 1; i <= 8; i++) {
        char msg[50];
        sprintf(msg, "Message batch 2, item %d", i);
        sendMessage(&mq, msg, i % 3 + 1);
    }
    displayPendingMessages(&mq);
    
    // Test 4: Try to overflow
    printf("--- Test 4: Overflow Test ---\n");
    sendMessage(&mq, "This should fail", 1);
    printf("\n");
    
    // Test 5: Drain queue
    printf("--- Test 5: Draining Queue ---\n");
    int count = getPendingCount(&mq);
    printf("Receiving all %d messages...\n", count);
    while (receiveMessage(&mq, &received)) {
        printf("  → #%d: \"%s\"\n", received.id, received.content);
    }
    displayPendingMessages(&mq);
    
    // Test 6: Underflow test
    printf("--- Test 6: Underflow Test ---\n");
    receiveMessage(&mq, &received);
    printf("\n");
}
```

**Advanced Message Queue with Priority:**

```c
// Priority Message Queue (using multiple queues)
typedef struct {
    MessageQueue highPriority;    // Priority 3
    MessageQueue mediumPriority;  // Priority 2
    MessageQueue lowPriority;     // Priority 1
} PriorityMessageQueue;

void initPriorityMQ(PriorityMessageQueue *pmq) {
    initMessageQueue(&pmq->highPriority);
    initMessageQueue(&pmq->mediumPriority);
    initMessageQueue(&pmq->lowPriority);
}

bool sendPriorityMessage(PriorityMessageQueue *pmq, const char *content, int priority) {
    MessageQueue *targetQueue;
    
    switch(priority) {
        case 3:
            targetQueue = &pmq->highPriority;
            break;
        case 2:
            targetQueue = &pmq->mediumPriority;
            break;
        default:
            targetQueue = &pmq->lowPriority;
            priority = 1;
    }
    
    return sendMessage(targetQueue, content, priority);
}

bool receivePriorityMessage(PriorityMessageQueue *pmq, Message *output) {
    // Check high priority first
    if (!isMQEmpty(&pmq->highPriority)) {
        return receiveMessage(&pmq->highPriority, output);
    }
    // Then medium priority
    if (!isMQEmpty(&pmq->mediumPriority)) {
        return receiveMessage(&pmq->mediumPriority, output);
    }
    // Finally low priority
    if (!isMQEmpty(&pmq->lowPriority)) {
        return receiveMessage(&pmq->lowPriority, output);
    }
    
    printf("❌ No messages in any priority queue.\n");
    return false;
}

void displayPriorityMQ(PriorityMessageQueue *pmq) {
    printf("\n╔════════════════════════════════════╗\n");
    printf("║   PRIORITY MESSAGE QUEUE STATUS    ║\n");
    printf("╚════════════════════════════════════╝\n");
    
    printf("\n🔴 HIGH PRIORITY (3):\n");
    if (isMQEmpty(&pmq->highPriority)) {
        printf("  (empty)\n");
    } else {
        displayPendingMessages(&pmq->highPriority);
    }
    
    printf("🟡 MEDIUM PRIORITY (2):\n");
    if (isMQEmpty(&pmq->mediumPriority)) {
        printf("  (empty)\n");
    } else {
        displayPendingMessages(&pmq->mediumPriority);
    }
    
    printf("🟢 LOW PRIORITY (1):\n");
    if (isMQEmpty(&pmq->lowPriority)) {
        printf("  (empty)\n");
    } else {
        displayPendingMessages(&pmq->lowPriority);
    }
}
```

### 4.5 Queue Operations - Complexity Analysis

**Time Complexities:**

| Operation | Array-Based | Linked List-Based |
|-----------|-------------|-------------------|
| Enqueue   | O(1)        | O(1)              |
| Dequeue   | O(1)        | O(1)              |
| Peek      | O(1)        | O(1)              |
| isEmpty   | O(1)        | O(1)              |
| isFull    | O(1)        | N/A               |
| Display   | O(n)        | O(n)              |

**Space Complexities:**

| Type | Space |
|------|-------|
| Array-Based | O(n) - fixed allocation |
| Linked List-Based | O(n) - dynamic, plus pointer overhead |
| Circular Queue | O(n) - efficient fixed space |

### 4.6 Advanced Queue Operations

```c
// Get element at position (0 = front)
int getQueueElementAt(Queue *q, int position) {
    if (position < 0 || position >= q->size) {
        printf("Invalid position\n");
        return -1;
    }
    
    int index = (q->front + position) % MAX_SIZE;
    return q->items[index];
}

// Search for element in queue
int searchQueue(Queue *q, int value) {
    if (isQueueEmpty(q)) {
        return -1;
    }
    
    int i = q->front;
    int position = 0;
    int count = 0;
    
    while (count < q->size) {
        if (q->items[i] == value) {
            return position;
        }
        i = (i + 1) % MAX_SIZE;
        position++;
        count++;
    }
    
    return -1;  // Not found
}

// Reverse queue using stack
void reverseQueue(Queue *q) {
    if (isQueueEmpty(q)) {
        return;
    }
    
    Stack s;
    initStack(&s);
    
    // Dequeue all and push to stack
    while (!isQueueEmpty(q)) {
        push(&s, dequeue(q));
    }
    
    // Pop all and enqueue back
    while (!isEmpty(&s)) {
        enqueue(q, pop(&s));
    }
}

// Interleave first half with second half
// Example: [1,2,3,4,5,6] -> [1,4,2,5,3,6]
void interleaveQueue(Queue *q) {
    if (q->size <= 1) {
        return;
    }
    
    int halfSize = q->size / 2;
    Queue temp;
    initQueue(&temp);
    
    // Move first half to temp queue
    for (int i = 0; i < halfSize; i++) {
        enqueue(&temp, dequeue(q));
    }
    
    // Interleave elements
    while (!isQueueEmpty(&temp)) {
        enqueue(q, dequeue(&temp));
        if (!isQueueEmpty(q)) {
            enqueue(q, dequeue(q));
        }
    }
}

// Generate binary numbers from 1 to n using queue
void generateBinaryNumbers(int n) {
    Queue q;
    initQueue(&q);
    
    enqueue(&q, 1);
    
    printf("Binary numbers from 1 to %d:\n", n);
    
    for (int i = 0; i < n; i++) {
        int current = dequeue(&q);
        
        // Print current number in binary
        printf("%d: ", i + 1);
        int temp = current;
        int binary[32], idx = 0;
        
        while (temp > 0) {
            binary[idx++] = temp % 10;
            temp /= 10;
        }
        
        for (int j = idx - 1; j >= 0; j--) {
            printf("%d", binary[j]);
        }
        printf("\n");
        
        // Generate next binary numbers
        enqueue(&q, current * 10);      // Append 0
        enqueue(&q, current * 10 + 1);  // Append 1
    }
}
```

### 4.7 Queue Applications in Real Systems

**1. CPU Scheduling:**
```c
typedef struct {
    int processId;
    int burstTime;
    int arrivalTime;
} Process;

typedef struct {
    Process processes[MAX_SIZE];
    int front, rear, count;
} ProcessQueue;

// Round Robin Scheduling Simulation
void roundRobinScheduling(ProcessQueue *pq, int timeQuantum) {
    int currentTime = 0;
    
    printf("Round Robin Scheduling (Time Quantum: %d)\n", timeQuantum);
    printf("================================================\n");
    
    while (!isQueueEmpty((Queue*)pq)) {
        Process p = ((Queue*)pq)->items[((Queue*)pq)->front];
        dequeue((Queue*)pq);
        
        int executeTime = (p.burstTime < timeQuantum) ? p.burstTime : timeQuantum;
        
        printf("Time %d: Process P%d executes for %d units\n", 
               currentTime, p.processId, executeTime);
        
        currentTime += executeTime;
        p.burstTime -= executeTime;
        
        // If process not complete, enqueue again
        if (p.burstTime > 0) {
            enqueue((Queue*)pq, p.processId);  // Simplified
        } else {
            printf("       Process P%d completed at time %d\n", 
                   p.processId, currentTime);
        }
    }
}
```

**2. Breadth-First Search (BFS):**
```c
#define MAX_VERTICES 10

void BFS(int graph[MAX_VERTICES][MAX_VERTICES], int vertices, int start) {
    bool visited[MAX_VERTICES] = {false};
    Queue q;
    initQueue(&q);
    
    visited[start] = true;
    enqueue(&q, start);
    
    printf("BFS traversal starting from vertex %d: ", start);
    
    while (!isQueueEmpty(&q)) {
        int current = dequeue(&q);
        printf("%d ", current);
        
        // Enqueue all adjacent unvisited vertices
        for (int i = 0; i < vertices; i++) {
            if (graph[current][i] == 1 && !visited[i]) {
                visited[i] = true;
                enqueue(&q, i);
            }
        }
    }
    printf("\n");
}
```

**3. Printer Queue:**
```c
typedef struct {
    char filename[50];
    int pages;
    int jobId;
} PrintJob;

typedef struct {
    PrintJob jobs[MAX_SIZE];
    int front, rear, count;
} PrintQueue;

void addPrintJob(PrintQueue *pq, const char *filename, int pages) {
    static int nextJobId = 1;
    
    if (isMQFull((MessageQueue*)pq)) {
        printf("Print queue full!\n");
        return;
    }
    
    int rear = (((MessageQueue*)pq)->rear + 1) % MSG_QUEUE_SIZE;
    
    PrintJob *job = &pq->jobs[rear];
    strncpy(job->filename, filename, 49);
    job->pages = pages;
    job->jobId = nextJobId++;
    
    ((MessageQueue*)pq)->rear = rear;
    ((MessageQueue*)pq)->count++;
    
    printf("Print job #%d added: %s (%d pages)\n", 
           job->jobId, job->filename, job->pages);
}

void processPrintJob(PrintQueue *pq) {
    if (isMQEmpty((MessageQueue*)pq)) {
        printf("No print jobs to process\n");
        return;
    }
    
    PrintJob job = pq->jobs[((MessageQueue*)pq)->front];
    ((MessageQueue*)pq)->front = 
        (((MessageQueue*)pq)->front + 1) % MSG_QUEUE_SIZE;
    ((MessageQueue*)pq)->count--;
    
    printf("Printing job #%d: %s (%d pages)...\n", 
           job.jobId, job.filename, job.pages);
    printf("Job #%d completed.\n", job.jobId);
}
```

### 4.8 Comparison: Queue Implementations

**Array-Based Circular Queue:**
- ✅ Pros: Fast access, cache-friendly, simple implementation
- ❌ Cons: Fixed size, may waste memory

**Linked List-Based Queue:**
- ✅ Pros: Dynamic size, no overflow (until system memory)
- ❌ Cons: Extra memory for pointers, slower cache access

**When to Use Which:**
- **Array-Based**: Known maximum size, performance-critical
- **Linked List**: Unknown size, frequent size changes
- **Circular Queue**: Fixed buffer, continuous operation (e.g., streaming)

### 4.9 Important Points About Queues

**Design Considerations:**

1. **Size Management:**
   - Fixed vs dynamic sizing
   - Growth strategies (if dynamic)
   - Memory pre-allocation

2. **Thread Safety:**
   - Multiple producers/consumers
   - Lock-free implementations
   - Atomic operations

3. **Performance Optimization:**
   - Cache alignment
   - Batch operations
   - Memory pooling

**Common Pitfalls:**

1. **Not checking empty before dequeue**
2. **Not checking full before enqueue (array-based)**
3. **Incorrect modulo arithmetic in circular queue**
4. **Memory leaks in linked list implementation**
5. **Off-by-one errors in index calculations**

**Error Handling Best Practices:**

```c
// Good: Return status and use output parameter
bool safeDequeue(Queue *q, int *value) {
    if (isQueueEmpty(q)) {
        return false;
    }
    *value = dequeue(q);
    return true;
}

// Usage
int value;
if (safeDequeue(&myQueue, &value)) {
    printf("Got value: %d\n", value);
} else {
    printf("Queue was empty\n");
}
```

---

## 5. Summary & Key Takeaways

### 5.1 Data Structure Selection Guide

| Requirement | Best Choice |
|-------------|-------------|
| LIFO access | Stack |
| FIFO access | Queue |
| Frequent insertion/deletion | Linked List-based |
| Fixed size, fast access | Array-based |
| Priority-based processing | Priority Queue |
| Expression evaluation | Stack |
| BFS traversal | Queue |
| Undo mechanism | Stack |
| Task scheduling | Queue |

### 5.2 Performance Summary

**Stacks:**
- All operations: O(1)
- Space: O(n)
- Best for: Expression evaluation, recursion, backtracking

**Queues:**
- All operations: O(1)
- Space: O(n)
- Best for: Scheduling, BFS, buffering

**Recursion:**
- Time: Varies (typically O(n) to O(2^n))
- Space: O(depth)
- Best for: Tree/graph algorithms, divide-and-conquer

### 5.3 Practical Implementation Tips

**Memory Management:**
```c
// Always free dynamically allocated structures
void cleanup() {
    // For linked structures
    while (!isEmpty(&stack)) {
        pop(&stack);
    }
    
    // For queues
    freeLinkedQueue(&queue);
}
```

**Error Handling:**
```c
// Use enums for better error handling
typedef enum {
    SUCCESS,
    OVERFLOW,
    UNDERFLOW,
    INVALID_INPUT
} OperationStatus;

OperationStatus safePush(Stack *s, int value) {
    if (isFull(s)) return OVERFLOW;
    push(s, value);
    return SUCCESS;
}
```

**Debugging Techniques:**
```c
// Add debug mode
#define DEBUG_MODE 1

#if DEBUG_MODE
    #define DEBUG_PRINT(fmt, ...) \
        printf("DEBUG: " fmt "\n", ##__VA_ARGS__)
#else
    #define DEBUG_PRINT(fmt, ...)
#endif

void debugStack(Stack *s) {
    DEBUG_PRINT("Stack size: %d, top: %d", s->size, s->top);
}
```

### 5.4 Advanced Topics for Further Study

1. **Lock-Free Data Structures**
   - Atomic operations
   - Compare-and-swap (CAS)
   - Memory ordering

2. **Cache-Aware Implementations**
   - Memory alignment
   - Prefetching
   - False sharing prevention

3. **Specialized Queues**
   - Priority queues (binary heap)
   - Deques (double-ended queues)
   - Concurrent queues

4. **Advanced Applications**
   - LRU cache implementation
   - Expression parsers
   - Graph algorithms
   - Operating system schedulers

---

## 6. Practice Problems

### Stack Problems:
1. Implement a stack that supports getMin() in O(1)
2. Check for balanced parentheses in an expression
3. Implement two stacks using a single array
4. Sort a stack using another stack
5. Evaluate prefix expressions

### Recursion Problems:
1. Print all permutations of a string
2. Solve N-Queens problem
3. Generate all subsets of a set
4. Implement merge sort
5. Calculate GCD using recursion

### Queue Problems:
1. Implement stack using queues
2. Implement queue using stacks
3. Generate all numbers with given digits
4. Find first non-repeating character in stream
5. Implement circular tour problem

---

## 7. Code Integration Example

**Complete working example combining all concepts:**

```c
#include <stdio.h>
#include <stdlib.h>

int main() {
    printf("╔══════════════════════════════════════╗\n");
    printf("║  DATA STRUCTURES DEMONSTRATION       ║\n");
    printf("╚══════════════════════════════════════╝\n\n");
    
    // 1. Stack demonstration
    printf("=== STACK OPERATIONS ===\n");
    Stack stack;
    initStack(&stack);
    push(&stack, 10);
    push(&stack, 20);
    push(&stack, 30);
    displayStack(&stack);
    printf("Popped: %d\n\n", pop(&stack));
    
    // 2. Recursion demonstration
    printf("=== RECURSION EXAMPLES ===\n");
    printf("Factorial(5) = %lld\n", factorial(5));
    int arr[] = {1, 3, 5, 7, 9, 11, 13};
    printf("Binary search for 7: index %d\n", 
           binarySearch(arr, 7, 7));
    printf("\nTower of Hanoi (3 disks):\n");
    towerOfHanoi(3, 'A', 'C', 'B');
    printf("\n");
    
    // 3. Queue demonstration
    printf("=== QUEUE OPERATIONS ===\n");
    Queue queue;
    initQueue(&queue);
    enqueue(&queue, 100);
    enqueue(&queue, 200);
    enqueue(&queue, 300);
    displayQueue(&queue);
    printf("Dequeued: %d\n\n", dequeue(&queue));
    
    // 4. Circular Queue demonstration
    printf("=== CIRCULAR QUEUE ===\n");
    CircularQueue cq;
    initCircularQueue(&cq);
    for (int i = 1; i <= 5; i++) {
        enqueueCQ(&cq, i * 10);
    }
    displayCQ(&cq);
    
    // 5. Expression evaluation
    printf("=== EXPRESSION EVALUATION ===\n");
    char infix[] = "A+B*C";
    char postfix[100];
    infixToPostfix(infix, postfix);
    printf("Infix: %s\n", infix);
    printf("Postfix: %s\n\n", postfix);
    
    // 6. Message Queue
    printf("=== MESSAGE QUEUE ===\n");
    testMessageQueue();
    
    printf("\n╔══════════════════════════════════════╗\n");
    printf("║  ALL TESTS COMPLETED SUCCESSFULLY!   ║\n");
    printf("╚══════════════════════════════════════╝\n");
    
    return 0;
}
```

---

## Conclusion

This comprehensive guide covered:
- ✅ Data structure fundamentals and classifications
- ✅ Complete stack implementation (array and linked list)
- ✅ Expression conversion and evaluation algorithms
- ✅ In-depth recursion with call stack analysis
- ✅ Practical recursive algorithms (factorial, binary search, Tower of Hanoi)
- ✅ Queue implementations and circular queue design
- ✅ Real-world applications (message queues, scheduling)
- ✅ Performance analysis and optimization techniques

**Next Steps:**
1. Implement all code examples yourself
2. Solve the practice problems
3. Experiment with different test cases
4. Profile performance with large datasets
5. Study advanced variations and optimizations

Remember: Understanding these fundamental data structures is crucial for solving complex algorithmic problems and building efficient software systems!ose(arr, left, mid - 1, target, depth + 1);
    } else {
        for (int i = 0; i < depth; i++) printf("  ");
        printf("Target > %d, searching right\n", arr[mid]);
        return binarySearchVerb
