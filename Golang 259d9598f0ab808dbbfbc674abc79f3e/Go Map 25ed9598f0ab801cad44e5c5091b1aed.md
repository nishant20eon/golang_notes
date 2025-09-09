# Go Map

# 🔹 Go Map Basics

### 📌 What is a Map?

- A **map** is a **key–value** data structure (just like Java’s `HashMap<K,V>`).
- Keys must be **unique**.
- Values can be any type.

---

### 🔹 Syntax

```go
var m map[KeyType]ValueType
```

Or using `make` (recommended):

```go
m := make(map[string]int)
```

---

### 🔹 Example 1: Create and Use a Map

```go
package main

import "fmt"

func main() {
    // Declare and initialize map
    m := make(map[string]int)

    // Insert values
    m["Alice"] = 25
    m["Bob"] = 30

    // Access values
    fmt.Println("Alice:", m["Alice"])
    fmt.Println("Bob:", m["Bob"])

    // Update value
    m["Alice"] = 26
    fmt.Println("Updated Alice:", m["Alice"])

    // Delete key
    delete(m, "Bob")

    // Length
    fmt.Println("Length:", len(m))
}
```

**Output:**

```
Alice: 25
Bob: 30
Updated Alice: 26
Length: 1
```

---

### 🔹 Example 2: Map Literal (shortcut like Java `Map.of(...)`)

```go
students := map[int]string{
    1: "Nishant",
    2: "Hege",
    3: "Amit",
}
fmt.Println(students)
```

---

### 🔹 Example 3: Check if Key Exists

```go
age, exists := m["Alice"]
if exists {
    fmt.Println("Alice exists, age:", age)
} else {
    fmt.Println("Alice not found")
}
```

---

# 🔑 Key Points vs Java

| Feature | Java `HashMap` | Go `map` |
| --- | --- | --- |
| Initialization | `new HashMap<>()` | `make(map[K]V)` |
| Insert / Update | `put(key, val)` | `m[key] = val` |
| Get | `get(key)` | `val := m[key]` |
| Check existence | `containsKey(key)` | `val, ok := m[key]` |
| Remove | `remove(key)` | `delete(m, key)` |
| Iteration | `for(Map.Entry..)` | `for k,v := range m` |

## 🔹 Iterating over Maps in Go

### Example 1: Basic Iteration

```go
package main

import "fmt"

func main() {
    students := map[int]string{
        1: "Nishant",
        2: "Hege",
        3: "Amit",
    }

    // Iterate using range
    for id, name := range students {
        fmt.Println("ID:", id, "Name:", name)
    }
}

```

✅ Output (order may vary, since Go maps are **unordered**):

```
ID: 1 Name: Nishant
ID: 2 Name: Hege
ID: 3 Name: Amit

```

---

### Example 2: Iterate Only Keys

```go
for id := range students {
    fmt.Println("ID:", id)
}

```

---

### Example 3: Iterate Only Values

```go
for _, name := range students {
    fmt.Println("Name:", name)
}

```

---

### Example 4: Sorted Iteration (like Java’s TreeMap)

By default, Go **does not keep order**. If you want sorted iteration:

```go
import (
    "fmt"
    "sort"
)

func main() {
    students := map[int]string{
        3: "Amit",
        1: "Nishant",
        2: "Hege",
    }

    // Collect keys
    keys := make([]int, 0, len(students))
    for k := range students {
        keys = append(keys, k)
    }

    // Sort keys
    sort.Ints(keys)

    // Iterate in sorted order
    for _, k := range keys {
        fmt.Println("ID:", k, "Name:", students[k])
    }
}

```

✅ Output:

```
ID: 1 Name: Nishant
ID: 2 Name: Hege
ID: 3 Name: Amit

```

---

⚡ So:

- `range` = like Java’s `for-each` on `entrySet()`.
- But Go maps are **unordered** (unlike `LinkedHashMap`).
- For order → collect keys → sort → iterate.

## 🔹 Map with Struct Values

### Example 1: Simple `map[int]Person`

```go
package main

import "fmt"

// Define a struct
type Person struct {
    Name   string
    Age    int
    Job    string
    Salary int
}

func main() {
    // Map with int key and Person value
    employees := map[int]Person{
        1: {"Nishant", 31, "Developer", 25000},
        2: {"Hege", 29, "Manager", 50000},
    }

    // Iterate
    for id, emp := range employees {
        fmt.Println("ID:", id, "->", emp.Name, emp.Age, emp.Job, emp.Salary)
    }
}

```

✅ Output (unordered):

```
ID: 1 -> Nishant 31 Developer 25000
ID: 2 -> Hege 29 Manager 50000

```

---

### Example 2: Map with Struct Pointers (`map[int]*Person`)

This is like storing references in Java (`Map<Integer, Person>` by reference).

```go
func main() {
    employees := map[int]*Person{
        1: {"Nishant", 31, "Developer", 25000},
        2: {"Hege", 29, "Manager", 50000},
    }

    // Update directly (since it's a pointer)
    employees[1].Salary = 30000

    for id, emp := range employees {
        fmt.Println("ID:", id, "->", emp.Name, emp.Salary)
    }
}

```

✅ Output:

```
ID: 1 -> Nishant 30000
ID: 2 -> Hege 50000

```

👉 Difference:

- `map[int]Person` → **copies struct**, updates won’t reflect outside.
- `map[int]*Person` → **references struct**, updates reflect everywhere (like Java object reference).

---

### Example 3: Nested Map (`map[string]map[string]Person`)

```go
departments := map[string]map[string]Person{
    "IT": {
        "E101": {"Nishant", 31, "Developer", 25000},
        "E102": {"Hege", 29, "Tester", 20000},
    },
    "HR": {
        "E201": {"Amit", 28, "HR Manager", 30000},
    },
}

for dept, emps := range departments {
    fmt.Println("Department:", dept)
    for empID, emp := range emps {
        fmt.Println("   ID:", empID, "->", emp.Name, emp.Job)
    }
}

```

✅ Output:

```
Department: IT
   ID: E101 -> Nishant Developer
   ID: E102 -> Hege Tester
Department: HR
   ID: E201 -> Amit HR Manager

```

---

⚡ So in Go:

- `map[int]Person` → value copy (like Java storing object, but immutable effect).
- `map[int]*Person` → reference (like Java storing reference).
- Can do nested maps, just like nested HashMaps in Java.