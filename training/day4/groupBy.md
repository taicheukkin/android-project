# Kotlin groupBy() Function

The `groupBy()` function in Kotlin groups collection elements by a specified key.

## Basic Example

```kotlin
fun main() {
    // A list of string numbers for demonstration
    val numbers = listOf("one", "two", "three", "four", "five")
    println("Original list: $numbers")
}
```
**Output:**
```
[one, two, three, four, five]
```

---

## Basic `groupBy()` - Group by First Character

```kotlin
val groupedByFirstChar = numbers.groupBy { element ->
    element.first().uppercase()  // Key: First character (uppercased)
}

println("groupBy { it.first().uppercase() }:")
groupedByFirstChar.forEach { (key, values) ->
    println("  Key '$key' → $values")
}
```
**Output:**
```
Key 'O' → [one]
Key 'T' → [two, three]
Key 'F' → [four, five]
```
Full map structure: `{O=[one], T=[two, three], F=[four, five]}`

### Visual Representation of Grouping Process
```
Element: "one"   → Key: "O" → Map: "O" → ["one"]
Element: "two"   → Key: "T" → Map: "T" → ["two"]
Element: "three" → Key: "T" → Map: "T" → ["two", "three"]
Element: "four"  → Key: "F" → Map: "F" → ["four"]
Element: "five"  → Key: "F" → Map: "F" → ["four", "five"]
```

---

## `groupBy()` with Key & Value Transform

```kotlin
val groupedWithTransform = numbers.groupBy(
    keySelector = { element -> 
        element.first()  // Key: First character (lowercase)
    },
    valueTransform = { element ->
        element.lowercase()  // Value: Entire string converted to lowercase
    }
)

println("groupBy(keySelector={it.first()}, valueTransform={it.lowercase()}):")
groupedWithTransform.forEach { (key, values) ->
    println("  Key '$key' → $values")
}
```
**Output:**
```
Key 'o' → [one]
Key 't' → [two, three]
Key 'f' → [four, five]
```
Full map structure: `{o=[one], t=[two, three], f=[four, five]}`

---

## Comparing the Two Results

| Basic `groupBy()` | Advanced `groupBy()` |
|-------------------|----------------------|
| **Keys:** `[O, T, F]` (uppercase) | **Keys:** `[o, t, f]` (lowercase) |
| **Values type:** `ArrayList` | **All values lowercase:** `[four, five]` |

---

## More Practical Examples

### 1. Group by String Length
```kotlin
val groupedByLength = numbers.groupBy { it.length }
println("1. Group by length:")
groupedByLength.forEach { (length, words) ->
    println("   $length-letter words: $words")
}
```
**Output:**
```
3-letter words: [one, two]
5-letter words: [three]
4-letter words: [four, five]
```

### 2. Group by Whether Contains 'e'
```kotlin
val groupedByContainsE = numbers.groupBy { it.contains('e') }
println("2. Group by contains 'e':")
groupedByContainsE.forEach { (hasE, words) ->
    println("   Contains 'e' = $hasE: $words")
}
```
**Output:**
```
Contains 'e' = true: [one, three, five]
Contains 'e' = false: [two, four]
```

### 3. Group with Complex Key (Pair)
```kotlin
val groupedByFirstAndLast = numbers.groupBy { 
    Pair(it.first(), it.last())
}
println("3. Group by (firstChar, lastChar):")
groupedByFirstAndLast.forEach { (pair, words) ->
    println("   Key (${pair.first}, ${pair.second}) → $words")
}
```
**Output:**
```
Key (o, e) → [one]
Key (t, o) → [two]
Key (t, e) → [three]
Key (f, r) → [four]
Key (f, e) → [five]
```

---

## Real-World Examples

### Group Users by Department
```kotlin
data class Employee(val name: String, val department: String, val salary: Int)

val employees = listOf(
    Employee("Alice", "Engineering", 85000),
    Employee("Bob", "Sales", 65000),
    Employee("Charlie", "Engineering", 95000),
    Employee("Diana", "Marketing", 70000),
    Employee("Eve", "Sales", 60000)
)

val employeesByDept = employees.groupBy { it.department }

println("Employees grouped by department:")
employeesByDept.forEach { (dept, empList) ->
    val names = empList.joinToString { it.name }
    val avgSalary = empList.map { it.salary }.average().toInt()
    println("   $dept Department: $names (Avg salary: \$$avgSalary)")
}
```
**Output:**
```
Engineering Department: Alice, Charlie (Avg salary: $90000)
Sales Department: Bob, Eve (Avg salary: $62500)
Marketing Department: Diana (Avg salary: $70000)
```

### Group with Value Transformation
```kotlin
val namesByDept = employees.groupBy(
    keySelector = { it.department },
    valueTransform = { it.name }  // Store only names, not full objects
)
println("Only employee names grouped by department: $namesByDept")
```
**Output:**
```
{Engineering=[Alice, Charlie], Sales=[Bob, Eve], Marketing=[Diana]}
```

---

## Related Grouping Functions

### `groupByTo()` - Group into Existing Mutable Map
```kotlin
val targetMap = mutableMapOf<String, MutableList<String>>()
numbers.groupByTo(targetMap) { it.first().uppercase() }
println("groupByTo (appends to existing map): $targetMap")
```

### `groupingBy()` - More Advanced Grouping
```kotlin
val grouping = numbers.groupingBy { it.first().uppercase() }
println("groupingBy() creates a Grouping object:")
println("  Group sizes: ${grouping.eachCount()}")  // {O=1, T=2, F=2}
```

---

## `groupBy()` vs `associateBy()` - Important Difference!

```kotlin
// groupBy() → Map<Key, List<Value>> (multiple values per key)
val groupByResult = numbers.groupBy { it.first().uppercase() }
println("groupBy(): $groupByResult")  // {O=[one], T=[two, three], F=[four, five]}

// associateBy() → Map<Key, SingleValue> (last value wins if duplicate keys)
val associateByResult = numbers.associateBy { it.first().uppercase() }
println("associateBy(): $associateByResult")  // {O=one, T=three, F=five}
```

**Key difference:**
- `groupBy()` collects ALL values with same key into a List
- `associateBy()` keeps ONLY LAST value when keys duplicate

---

## Real-World Use Cases

### 1. Categorizing Products
```kotlin
val products = listOf("Laptop", "Mouse", "Keyboard", "Monitor", "Motherboard")
val byCategory = products.groupBy {
    when {
        it.startsWith("M") -> "M-Peripherals"
        else -> "Other"
    }
}
println("1. Products by category: $byCategory")
```

### 2. Grouping Events by Month
```kotlin
data class Event(val name: String, val date: String) // YYYY-MM-DD format

val events = listOf(
    Event("Conference", "2024-03-15"),
    Event("Workshop", "2024-03-20"),
    Event("Meetup", "2024-04-05"),
    Event("Seminar", "2024-03-25")
)

val eventsByMonth = events.groupBy { event ->
    event.date.substring(0, 7) // Extract YYYY-MM (year-month)
}

println("2. Events grouped by month:")
eventsByMonth.forEach { (month, eventList) ->
    println("   $month: ${eventList.joinToString { it.name }}")
}
```

### 3. Score Distribution
```kotlin
val scores = listOf(85, 92, 78, 95, 88, 72, 91, 84, 79, 96)
val scoresByRange = scores.groupBy { score ->
    when (score) {
        in 90..100 -> "A (90-100)"
        in 80..89 -> "B (80-89)"
        in 70..79 -> "C (70-79)"
        else -> "Below 70"
    }
}

println("3. Score distribution:")
scoresByRange.forEach { (range, scoreList) ->
    println("   $range: ${scoreList.size} students")
}
```

---

## Quick Reference Guide

### `groupBy()` Function Signatures

1. **Basic:**
   ```kotlin
   collection.groupBy { keySelector }
   // Returns: Map<Key, List<Element>>
   ```

2. **With value transform:**
   ```kotlin
   collection.groupBy(keySelector, valueTransform)
   // Returns: Map<Key, List<TransformedValue>>
   ```

### Key Concepts
- Creates a Map where keys come from keySelector
- Values are Lists of elements (or transformed elements)
- Elements with same key are grouped together in a list
- Original collection order is preserved within groups

### Common Use Cases
1. **Categorization** (group by category/tag/type)
2. **Aggregation** (count, sum, average within groups)
3. **Partitioning** (split data into logical groups)
4. **Data summarization** (create summary by key)

### Performance
- **Time complexity:** O(n) - processes each element once
- **Space complexity:** O(n) - creates new map and lists
- For large datasets, consider using sequences

### Alternatives
- **`groupingBy()`:** More advanced operations (fold, reduce, aggregate)
- **`associateBy()`:** When you need one-to-one mapping (keys unique)
- **`partition()`:** For binary grouping (true/false conditions)
