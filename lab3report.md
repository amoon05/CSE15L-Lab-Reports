Part 1: I will be identifying a bug in the LinkedListExample code 

```
import java.util.NoSuchElementException;

class Node {
    int value;
    Node next;
    public Node(int value, Node next) {
        this.value = value;
        this.next = next;
    }
}
class LinkedList {
    Node root;
    public LinkedList() {
        this.root = null;
    }
    /**
     * Adds the value to the _beginning_ of the list
     * @param value
     */
    public void prepend(int value) {
        // Just add at the beginning
        this.root = new Node(value, this.root);
    }
    /**
     * Adds the value to the _end_ of the list
     * @param value
     */
    public void append(int value) {
        if(this.root == null) {
            this.root = new Node(value, null);
            return;
        }
        // If it's just one element, add if after that one
        Node n = this.root;
        if(n.next == null) {
            n.next = new Node(value, null);
            return;
        }
        // Otherwise, loop until the end and add at the end with a null
        while(n.next != null) {
            n = n.next;
            n.next = new Node(value, null);
        }
    }
    /**
     * @return the value of the first element in the list
     */
    public int first() {
        return this.root.value;
    }
    /**
     * @return the value of the last element in the list
     */
    public int last() {
        Node n = this.root;
        // If no such element, throw an exception
        if(n == null) { throw new NoSuchElementException(); }
        // If it's just one element, return its value
        if(n.next == null) { return n.value; }
        // Otherwise, search for the end of the list and return the last value
        while(n.next != null) {
            n = n.next;
        }
        return n.value;
    }
    /**
     * @return a string representation of the list
     */
    public String toString() {
        Node n = this.root;
        String s = "";
        while(n != null) {
            s += n.value + " ";
            n = n.next;
        }
        return s;
    }
    /**
     * @return the number of elements in the list
     */
    public int length() {
        Node n = this.root;
        int i = 0;
        while(n != null) {
            i += 1;
            n = n.next;
        }
        return i;
    }
}
```

- Failure Inducing JUnit Test

```
import static org.junit.Assert.*;
import org.junit.*;

public class LinkedListTest {
    public LinkedListTest() {

    }
    @Test
    public void testFail() {
        LinkedList temp = new LinkedList();
        temp.prepend(1);
        temp.prepend(2);
        temp.prepend(3);
        assertEquals(3, temp.last());
    }
}
```

- Success Inducing JUnit Test

```
import static org.junit.Assert.*;
import org.junit.*;

public class LinkedListTest {
    public LinkedListTest() {

    }
    @Test
    public void testSuccess() {
        LinkedList temp = new LinkedList();
        temp.append(1);
        temp.append(2);
        temp.append(3);
        assertEquals(3, temp.last());
    }
}
```

- Results Screenshots
  - The results for the failed test: ![Image](lab3failed.png)
  - The results for the successful test: ![Image](lab3success.png)

- The bug is in this section
Before: 
```
public void append(int value) {
        if(this.root == null) {
            this.root = new Node(value, null);
            return;
        }
        // If it's just one element, add if after that one
        Node n = this.root;
        if(n.next == null) {
            n.next = new Node(value, null);
            return;
        }
        // Otherwise, loop until the end and add at the end with a null
        while(n.next != null) {
            n = n.next;
            n.next = new Node(value, null);
        }
    }
```
After: 
```
public void append(int value) {
        if(this.root == null) {
            this.root = new Node(value, null);
            return;
        }
        // If it's just one element, add if after that one
        Node n = this.root;
        while (n.next != null) {
          n = n.next;
        }
        n.next = new Node(value, null)
    }
```

- The bug occurs because when a new node is being added, the `next` reference is incorrectly overwritten. This is fixed now when using a while loop as it iterates through the list until reaching the last node then assigning the `next` to the node.


Part 2: I utilized ChatGPT to find me four alternate ways to use `grep`
The prompt I utilized was "what are four alternate ways to use grep"
- Using Regular Expressions
- Counting Matches
- Displaying Line Numbers
- Searching Multiple Files
