# **What is `mutableListOf<Int>`?**

## **Simple Explanation:**

Imagine you have a **shopping list on a whiteboard** where you can:
1. **Add** new items
2. **Remove** items  
3. **Change** items
4. See **all items** at any time

That's what `mutableListOf<Int>()` is - a **changeable list of numbers**.

---

## **Breaking it down:**

### **1. `List`** = A collection of items
```kotlin
// A regular list (like a printed list - can't change it)
val shoppingList = listOf("Milk", "Eggs", "Bread")
```

### **2. `Mutable`** = Can be changed/modified
```kotlin
// A mutable list (like a whiteboard - can change it)
val shoppingList = mutableListOf("Milk", "Eggs", "Bread")
shoppingList.add("Butter")    // Can add
shoppingList.remove("Eggs")   // Can remove
shoppingList[0] = "Juice"     // Can change
```

### **3. `<Int>`** = Only stores whole numbers
```kotlin
// Can only put integers (whole numbers) in this list
val numbers = mutableListOf<Int>()
numbers.add(10)      // ✅ OK - 10 is an integer
numbers.add(-5)      // ✅ OK - -5 is an integer
// numbers.add(3.14) // ❌ ERROR - 3.14 is not an integer
// numbers.add("Hi") // ❌ ERROR - "Hi" is not a number
```

---

## **In Your Bank Account Code:**

```kotlin
var transactions = mutableListOf<Int>()
```
This means:
- **`transactions`** = The name of the list
- **`mutableListOf`** = A list you can modify
- **`<Int>`** = Will only store whole numbers (dollar amounts)

---

## **What You Can Do With It:**

### **Adding transactions:**
```kotlin
transactions.add(100)   // Deposit $100 (positive)
transactions.add(-50)   // Withdraw $50 (negative)
transactions.add(200)   // Deposit $200 (positive)
```

### **Seeing what's in the list:**
```kotlin
println(transactions)  // Output: [100, -50, 200]
println(transactions[0]) // Output: 100 (first item)
```

### **Other useful operations:**
```kotlin
// Get size (how many transactions)
println(transactions.size)  // Output: 3

// Check if list contains something
println(transactions.contains(100))  // Output: true

// Remove a transaction
transactions.remove(-50)  // Removes -50 from list

// Clear all transactions
transactions.clear()  // List becomes empty: []
```

---

## **Real Example in Your Bank Account:**

```kotlin
fun main() {
    val account = Account("John")
    
    // These calls add to the mutableListOf<Int>:
    account.deposit(500)   // transactions list: [500]
    account.deposit(300)   // transactions list: [500, 300]
    account.withdraw(200)  // transactions list: [500, 300, -200]
    
    // Now the list looks like this:
    println(account.transactions)  // Output: [500, 300, -200]
    
    // Calculate total: 500 + 300 - 200 = 600
    println(account.calculateBalance())  // Output: 600
}
```

---

## **Why Use `mutableListOf<Int>` Instead of Regular Array?**

| Feature | `mutableListOf<Int>()` | `Array<Int>` |
|---------|----------------------|-------------|
| **Size** | Grows automatically | Fixed size |
| **Add items** | `list.add(item)` | Can't add (fixed size) |
| **Remove items** | `list.remove(item)` | Can't remove |
| **Flexibility** | Very flexible | Less flexible |

**Example:**
```kotlin
// MutableList - flexible size
val flexibleList = mutableListOf<Int>()
flexibleList.add(1)  // Now size = 1
flexibleList.add(2)  // Now size = 2
flexibleList.add(3)  // Now size = 3

// Array - fixed size
val fixedArray = arrayOf<Int>(1, 2, 3)  // Always size = 3
// fixedArray.add(4)  // ❌ ERROR - can't add to array
```

---

## **Common Methods for `mutableListOf<Int>`:**

```kotlin
val numbers = mutableListOf<Int>(10, 20, 30)

// 1. Add elements
numbers.add(40)           // Add to end: [10, 20, 30, 40]
numbers.add(0, 5)         // Add at index 0: [5, 10, 20, 30, 40]

// 2. Remove elements
numbers.remove(20)        // Remove value 20: [5, 10, 30, 40]
numbers.removeAt(0)       // Remove first item: [10, 30, 40]

// 3. Update elements
numbers[1] = 25           // Change index 1: [10, 25, 40]

// 4. Check elements
val first = numbers[0]    // Get first: 10
val hasTen = numbers.contains(10)  // Check if contains 10: true

// 5. Loop through
for (num in numbers) {
    println(num)  // Prints: 10, then 25, then 40
}
```

---

## **Simple Analogy:**

| Programming Term | Real World Example |
|-----------------|-------------------|
| `mutableListOf<Int>()` | A shopping list on a **whiteboard** |
| `transactions.add(100)` | Writing "+$100" on the whiteboard |
| `transactions.add(-50)` | Writing "-$50" on the whiteboard |
| `transactions.remove(-50)` | Erasing "-$50" from whiteboard |
| `transactions[0]` | Looking at the first item on the list |
| `transactions.size` | Counting how many items are on the list |

So in your bank account code, `mutableListOf<Int>()` is like having a **whiteboard where you write down every deposit (positive numbers) and withdrawal (negative numbers)** that happens in the account!
