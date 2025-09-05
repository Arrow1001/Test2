
## ✅ **IB Computer Science – Paper 2 Style Exam Question**
### **Topic: OOP, Linked Lists, Stacks, and Data Structure Selection**

---
### Part A
### **Question 1 – Customer Discount Correction System**
**[Total: 17 marks]**

A retail store uses a transaction system to record each customer's purchase at checkout. Each customer has a name, purchase amount, loyalty tier, and the hour of purchase (in 24-hour format). The store operates from 9:00 AM to 9:00 PM (21:00).

Due to a software bug, a **flat 5% discount** was mistakenly applied to all customers during the **last hour of operation (20:00–21:00)**. The correct discount policy is based on loyalty:

- **Bronze**: 5%
- **Silver**: 10%
- **Gold**: 25%

The system currently stores customers in a **singly linked list** in the order they checked out.

---

### **Given Classes:**

```java
class Customer {
    private String name;
    private double lastPurchase;  // already reduced by 5% due to bug
    private String loyaltyTier;   // "Bronze", "Silver", "Gold"
    private int hour;             // 9 to 21 (24-hour format)

    public Customer(String name, double lastPurchase, String loyaltyTier, int hour) {
        this.name = name;
        this.lastPurchase = lastPurchase;
        this.loyaltyTier = loyaltyTier;
        this.hour = hour;
    }

    // Getters
    public String getName() { return name; }
    public double getLastPurchase() { return lastPurchase; }
    public String getLoyaltyTier() { return loyaltyTier; }
    public int getHour() { return hour; }

    // Setter
    public void setLastPurchase(double amount) { this.lastPurchase = amount; }

    // Returns correct discount rate
    public double getCorrectDiscountRate() {
        switch(loyaltyTier) {
            case "Gold": return 0.25;
            case "Silver": return 0.10;
            case "Bronze": return 0.05;
            default: return 0.05;
        }
    }
}
```

```java
class TransactionLog {
    private Node head;

    private class Node {
        Customer customer;
        Node next;

        public Node(Customer customer) {
            this.customer = customer;
            this.next = null;
        }
    }

    public TransactionLog() {
        head = null;
    }

    // Adds customer to the end of the list (chronological order)
    public void addCustomer(Customer customer) {
        Node newNode = new Node(customer);
        if (head == null) {
            head = newNode;
        } else {
            Node current = head;
            while (current.next != null) {
                current = current.next;
            }
            current.next = newNode;
        }
    }
}
```

---

### **(a)**
Write a method in the `TransactionLog` class called:
```java
//public void correctLastHourDiscounts()
```
This method must:
- Traverse the **entire linked list**
- Identify customers who checked out between **20:00 and 21:00 (inclusive)**
- For each such customer:
    - **Reverse the incorrect 5% discount**
    - Apply the **correct discount** based on loyalty tier
    - Update the `lastPurchase` with the corrected value


**[8 marks]**

public void correctLastHourDiscounts(){
  Node current = head;
  while (current != null){
    current = current.next;
    if(current.customer.getHour() >= 20 && current.customer.getHour() <= 21){
      int original = current.customer.getPurchase() * 1.05;
      int correct = 0;
      switch(current.customer.getLoyaltyTier()){
        case "Gold": correct = original * 0.75;
        break;
        case "Silver": correct = original * 0.9;
        break;
        case "Bronze": correct = original * 0.95;
        break;
      }
    }
  current.customer.setLastPurchase(corrected);
  }
  current = current.next;
}
      


### **(b)**
Explain why using a **singly linked list** is **inefficient** for this type of operation — especially if the store needs to **frequently correct the most recent transactions**.

Suggest a **better data structure** for storing transactions when **recent entries need to be accessed or modified quickly**, and justify your choice.

**[4 marks]**

singly linked list only stores a reference to the next node, not the previous one. Also the traversal is O(n) in time complexity. A much better choice could be a doubly linked list which stores references for both the next and previous node. It also has a time complexity of O(1).

---

### **(c)**
Describe how the **stack** data structure follows the **LIFO principle**, and explain why this makes it **particularly suitable** for modeling **transaction undo systems** or **rollback operations** in point-of-sale software.

**[3 marks]**

A stack structures follows the LIFO principle (Last in First Out) and insertion/deletion can only be done at one end called the 'top'. This makes it particularly suitable for the transaction undo systems and rollback operations as the last transaction is usually the one which needs to be corrected; A stack is able to give quick access to the most recent transaction which makes it great for this operation.

---

### **(d)**
State **one disadvantage** of using a stack for this transaction system if the store also needs to **generate daily reports sorted by time** or **search for a customer by name**.

**[2 marks]**

A disadvantage of using a stack for this transaction system is that it can only quickly access the most recent transaction, meaning if older transactions need to be reached, you would need to pop through the entire stack.

---



### **Task 1 (c): Binary Search Application**

The store now wants to **quickly find a customer by name** in a **separate sorted array** of customers (not the linked list). The array is sorted alphabetically by name.

Write a **binary search method** (not in the list class) that searches for a customer by name and returns the index, or -1 if not found.

Assume you are given:
```java
Customer[] sortedCustomers; // sorted by name
```

Write the method:
```java
//public int binarySearchCustomer(Customer[] arr, String targetName)
```

You may assume `Customer` has a `getName()` method.

**[6 marks]**

> 🔎 *Note: This tests understanding of binary search preconditions (sorted data) and implementation.*

public int binarySearchCustomer(Customer[] arr, String targetName) {
int low = 0;
int high = arr.length - 1;
    while (low <= high) {
        int mid = (low + high) / 2; 
        String midName = arr[mid].getName();
        int comparison = targetName.compareTo(midName);
        if (comparison == 0) {
            return mid; 
        } else if (comparison < 0) {
            high = mid - 1; 
        } else {
            low = mid + 1;
        }
    }
    return -1;
}


---

## **Question 2: Stack Applications**

### **Standard Stack Operation**

You are given a **stack of integers**. Write a method that **reverses the stack** using **only one queue** as auxiliary storage.

You may only use standard stack and queue operations (`push`, `pop`, `peek`, `enqueue`, `dequeue`, `isEmpty`).

Write the method:
```java
//public void reverseStack(Stack<Integer> stack)
```

**[6 marks]**

> 💡 *Hint: Use the queue to temporarily store elements, then re-push them back.*

public void reverseStack(Stack<Integer> stack) {
Queue<Integer> queue = new LinkedList<>(); 
    while (!stack.isEmpty()) {
        queue.add(stack.pop());
    }
    while (!queue.isEmpty()) {
        stack.push(queue.remove());
     }
}
---