tags : [[Algorithms/Algorithms]]
reference : https://medium.com/@arifimran5/fast-and-slow-pointer-pattern-in-linked-list-43647869ac99

The **Fast & Slow** pointer approach, also known as the **Hare & Tortoise algorithm**, is a pointer algorithm that uses **two pointers** which move through the array (or sequence/**LinkedList**) at different speeds. This approach is quite useful when dealing with cyclic **LinkedList**s or arrays or ==finding the mid of a LinkedList==.

By moving at different speeds (say, in a cyclic **LinkedList**), the algorithm proves that the **two pointers** are bound to meet. The _fast pointer_ should catch the _slow pointer_ once both the pointers are in a cyclic loop.

![[attachments/Pasted image 20240112065141.png]]
