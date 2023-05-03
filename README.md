Download Link: https://assignmentchef.com/product/solved-cs0449-lab-3-linked-lists-and-the-heap
<br>
Project 2 is gonna involve linked lists, so this lab is a way for you to refresh yourself. Please make sure you understand this stuff thoroughly.

<h2>Crash course on malloc and free</h2>

We’ll learn about the heap in detail on Wednesday/Thursday. But you can get started on the lab now. Right? &#x1f642;

malloc is like C’s new. It allocates space on the heap, and gives you a pointer to that space.

But C has no garbage collection, so every piece of memory you malloc, you must ultimately free.

Using malloc is straightforward: you tell it how many bytes you need, and it returns a pointer to a brand new piece of memory that is at least that big:

// one int is sizeof(int).// multiply that by 10, and you have room for an array of 10 ints.int* array = malloc(sizeof(int) * 10);

When you’re done with a piece of memory, just pass the pointer to free:

free(array);

<h2>The lab</h2>

Here is the Node struct that you will be using:

typedef struct Node {        int value;        struct Node* next;} Node;

Here are the functions you will implement:

<ul>

 <li>Node* create_node(int value)

  <ul>

   <li>it will malloc a Node, set its value to value, set its next to NULL, and return it.

    <ul>

     <li>to malloc an instance Node, you need to allocate sizeof(Node) bytes.</li>

     <li>you can directly assign the result of malloc into a Node* variable.</li>

    </ul></li>

  </ul></li>

 <li>void list_print(Node* head)

  <ul>

   <li>head points to the head of a linked list.</li>

   <li>it will print the values of all the items in the list in this format: 5 -&gt; 8 -&gt; 2 -&gt; 1</li>

  </ul></li>

 <li>Node* list_append(Node* head, int value)

  <ul>

   <li>head points to the head of a linked list.</li>

   <li>it will go to the <strong>end</strong> of the linked list, use create_node(value), and put that new node at the end of the linked list.</li>

   <li>then it will return that new node.</li>

  </ul></li>

 <li>Node* list_prepend(Node* head, int value)

  <ul>

   <li>head points to the head of a linked list.</li>

   <li>it will use create_node(value), make that node point to head, and return that new node.</li>

   <li><strong>important:</strong> this function should return the <em>new</em> head of the list, not the old one.</li>

  </ul></li>

 <li>void list_free(Node* head)

  <ul>

   <li>head points to the head of a linked list, <strong>or NULL.</strong></li>

   <li>if head is NULL, do nothing.</li>

   <li>otherwise, free all the nodes in the list.

    <ul>

     <li><strong>do not free a node before you get its </strong><strong>next</strong><strong> field.</strong></li>

    </ul></li>

  </ul></li>

 <li>Node* list_remove(Node* head, int value)

  <ul>

   <li>head points to the head of a linked list.</li>

   <li>it will search for a node whose value == value.

    <ul>

     <li>if it finds that node, it will <strong>remove it from the list</strong> and free it.

      <ul>

       <li>there are special cases for removing the head and tail!</li>

      </ul></li>

     <li>if it doesn’t find that node, nothing happens.</li>

    </ul></li>

   <li>it will return the <strong>head of the list</strong> (which may have changed or become NULL!)</li>

  </ul></li>

</ul>

<h3>Hints:</h3>

Read these. It’s not “cheating”, it’s important information.

<ul>

 <li>Don’t write everything at once and then test it. Write one function, test that, and repeat.</li>

 <li>for loops fit together with linked lists very nicely.</li>

 <li>Write list_print as soon as you can, so you can test your code easier.</li>

 <li>Don’t worry about head == NULL for anything other than list_free.</li>

 <li>For list_remove, when you are iterating over the list, don’t move your pointer too far ahead…

  <ul>

   <li>(You have to know the previous node to remove a node.)</li>

  </ul></li>

</ul>

<h2>main</h2>

Here’s a small driver I wrote. (You can put all your code in one lab3.c file.) Feel free to comment stuff out, add more stuff etc. to test your code more thoroughly.

int main() {        // The comments at the ends of the lines show what list_print should output.        Node* head = create_node(1);        list_print(head);                  // 1        Node* end = list_append(head, 2);        list_print(head);                  // 1 -&gt; 2        end-&gt;next = create_node(3);        list_print(head);                  // 1 -&gt; 2 -&gt; 3        head = list_prepend(head, 0);        list_print(head);                  // 0 -&gt; 1 -&gt; 2 -&gt; 3        list_append(head, 4);        list_print(head);                  // 0 -&gt; 1 -&gt; 2 -&gt; 3 -&gt; 4        list_append(head, 5);        list_print(head);                  // 0 -&gt; 1 -&gt; 2 -&gt; 3 -&gt; 4 -&gt; 5         head = list_remove(head, 5);        list_print(head);                  // 0 -&gt; 1 -&gt; 2 -&gt; 3 -&gt; 4        head = list_remove(head, 3);        list_print(head);                  // 0 -&gt; 1 -&gt; 2 -&gt; 4        head = list_remove(head, 0);        list_print(head);                  // 1 -&gt; 2 -&gt; 4         list_free(head);         return 0;}

<h2>valgrind</h2>

valgrind is a helpful tool. It can analyze your program’s memory accesses, heap allocations, frees etc. at runtime to make sure you’re not doing anything weird. It can help you find memory leaks, array-out-of-bounds errors, buffer overflows, double-frees, using freed heap memory etc…

To use it, you just put valgrind before the program you want to run:

valgrind ./lab3

It will print lines that look like ==12345== that will point out issues with your program as it runs.

Common issues:

<ul>

 <li><strong>“Invalid read/write of size n”</strong>

  <ul>

   <li>you are trying to access an invalid pointer.</li>

   <li>keep reading: it will tell you <em>where</em> the read/write happens. Like, the file and line!</li>

  </ul></li>

 <li><strong>“LEAK SUMMARY”</strong>

  <ul>

   <li>you have a memory leak.</li>

   <li>basically, the problem is in list_free.

    <ul>

     <li>it says “Rerun with –leak-check=full to see details of leaked memory” but if you do that, it just shows where the memory was <em>allocated</em>, which isn’t too useful for you.</li>

    </ul></li>

  </ul></li>

 <li><strong>“Invalid free() / delete / delete[] / realloc()”</strong>

  <ul>

   <li>you are trying to call free() on something that you aren’t allowed to.</li>

   <li>like something that you already freed.</li>

   <li>or a pointer that doesn’t point to the heap.</li>

   <li>keep reading the error!!!</li>

  </ul></li>

</ul>