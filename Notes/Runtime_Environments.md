# Runtime Environments

## Runtime Environments

The runtime environment is the state of the target machine when a program is executing. It includes the organization of the memory, the layout of activation records, and the mechanisms for memory allocation and deallocation.

### Runtime Organization
*   **Static Allocation:** Memory for data objects is allocated at compile time and remains fixed throughout program execution. Suitable for global variables and constants.
*   **Stack Allocation:** Memory is allocated and deallocated in a LIFO (Last-In, First-Out) manner. Used for local variables and parameters of procedures/functions. Activation records are pushed onto and popped from the stack.
*   **Heap Allocation:** Memory is allocated and deallocated dynamically at runtime. Used for data whose size or lifetime cannot be determined at compile time (e.g., objects created with `new` in Java/C++).

### Activation Records
An activation record (or stack frame) is a block of information pushed onto the stack when a procedure is called and popped when the procedure returns. Its typical contents include:
*   **Return Value:** Space for the function's return value.
*   **Actual Parameters:** Values or addresses of arguments passed to the function.
*   **Control Link (Dynamic Link):** A pointer to the activation record of the caller.
*   **Access Link (Static Link):** A pointer to the activation record of the non-local scope (lexical parent) that defines the current procedure, used for accessing non-local variables in languages with nested procedures.
*   **Saved Machine Status:** Register values and program counter saved before the call.
*   **Local Data:** Space for local variables of the procedure.
*   **Temporaries:** Space for intermediate results of expression evaluation.

### Stack Allocation
Procedures are activated and deactivated in a LIFO manner, making a stack a natural choice for managing activation records.
*   When a procedure is called, its activation record is pushed onto the stack.
*   When it returns, its activation record is popped.
*   The stack pointer (SP) typically points to the top of the current activation record.

### Heap Management
The heap is a region of memory used for dynamic memory allocation.
*   **Memory Allocation:** When a program requests memory (e.g., `malloc`, `new`), the heap manager finds a suitable block of free space.
    *   **First Fit:** Allocates the first free block that is large enough.
    *   **Best Fit:** Allocates the smallest free block that is large enough.
*   **Memory Deallocation:** When memory is no longer needed, it is returned to the free list. This can lead to fragmentation (small, unusable free blocks).

### Garbage Collection
Garbage collection is an automatic memory management technique that reclaims memory occupied by objects that are no longer reachable by the program.
*   **Reference Counting:** Each object maintains a count of references pointing to it. When the count drops to zero, the object is considered garbage and its memory is reclaimed.
    *   **Problem:** Cannot reclaim cyclic data structures (where objects refer to each other but are not reachable from the program's roots).
*   **Mark-and-Sweep:** A two-phase algorithm:
    1.  **Mark Phase:** Starts from root objects (global variables, stack variables) and recursively marks all reachable objects.
    2.  **Sweep Phase:** Iterates through the entire heap, reclaiming memory from unmarked (unreachable) objects.
*   **Copying Collectors (e.g., Cheney's Algorithm):** Divides the heap into two semispaces (From-space and To-space). During collection, reachable objects are copied from From-space to To-space, compacting memory and eliminating fragmentation. After copying, the roles of From-space and To-space are swapped.
*   **Incremental Garbage Collection:** Designed to reduce pause times by performing garbage collection in small steps, interleaving it with program execution. This is crucial for interactive applications.

### Access Links and Displays
In languages that allow nested procedures (lexical scoping), a procedure might need to access non-local variables defined in an enclosing scope.
*   **Access Link (Static Link):** A pointer in an activation record that points to the activation record of the lexically enclosing (static) scope. Following a chain of access links allows a procedure to find non-local data.
*   **Display:** An array of pointers, where each entry `display[i]` points to the activation record of the most recent activation of a procedure at lexical level `i`. This provides faster access to non-local data compared to traversing access links.

### Summary
Runtime environments define how a program's memory is organized and managed during execution, primarily through static, stack, and heap allocation. Stack allocation uses activation records to manage procedure calls, with components like control and access links facilitating execution flow and scope access. Heap allocation supports dynamic memory needs, managed by strategies like First Fit or Best Fit. Crucially, garbage collection automates memory deallocation, employing techniques such as reference counting (prone to cycles), mark-and-sweep (two-phase collection), and copying collectors (for compaction). Incremental garbage collection minimizes pauses in interactive systems. Access links and displays are mechanisms to resolve non-local variable references in lexically scoped languages.