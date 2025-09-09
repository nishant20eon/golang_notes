# Go Control Statements

**if, switch, for (classic, while, infinite, range)**

### 1️⃣ If Statement

```go
a := 10

if a > 5 {
    fmt.Println("a is greater than 5")
} else if a == 5 {
    fmt.Println("a is 5")
} else {
    fmt.Println("a is less than 5")
}
```

**Output:**

```
a is greater than 5
```

---

### 2️⃣ Switch Statement

```go
day := 3

switch day {
case 1:
    fmt.Println("Monday")
case 2:
    fmt.Println("Tuesday")
case 3:
    fmt.Println("Wednesday")
default:
    fmt.Println("Other day")
}
```

**Output:**

```
Wednesday
```

- **Tip:** `fallthrough` can be used to continue to the next case.

---

### 3️⃣ For Loops

**Classic For Loop**

```go
for i := 0; i < 3; i++ {
    fmt.Println("i =", i)
}
```

**Output:**

```
i = 0
i = 1
i = 2
```

**While-like Loop**

```go
n := 0
for n < 3 {
    fmt.Println("n =", n)
    n++
}
```

**Output:**

```
n = 0
n = 1
n = 2
```

**Infinite Loop**

```go
count := 0
for {
    fmt.Println("count =", count)
    count++
    if count == 3 {
        break
    }
}
```

**Output:**

```
count = 0
count = 1
count = 2
```

**Range Loop**

```go
nums := []int{10, 20, 30}
for i, v := range nums {
    fmt.Println("Index:", i, "Value:", v)
}
```

**Output:**

```
Index: 0 Value: 10
Index: 1 Value: 20
Index: 2 Value: 30
```

### ✅ Go Example: Using `range` for slices, maps, and strings

```go
package main

import "fmt"

func main() {
    fmt.Println("=== Slice Example ===")
    // Slice of integers
    nums := []int{10, 20, 30, 40}
    for i, n := range nums {
        fmt.Printf("Index: %d, Value: %d\n", i, n)
    }

    fmt.Println("\n=== Map Example ===")
    // Map of string to int
    scores := map[string]int{"Alice": 90, "Bob": 85, "Charlie": 95}
    for name, score := range scores {
        fmt.Printf("%s scored %d\n", name, score)
    }

    fmt.Println("\n=== String Example ===")
    // String iteration (Unicode safe)
    word := "GoLang"
    for i, ch := range word {
        fmt.Printf("Index: %d, Character: %c\n", i, ch)
    }

    fmt.Println("\n=== Modify Slice Elements Example ===")
    // Important: range gives a copy of element
    nums2 := []int{1, 2, 3, 4}
    for i, n := range nums2 {
        n *= 2          // this modifies copy, not original slice
        fmt.Printf("Modified value at index %d: %d\n", i, n)
    }
    fmt.Println("Original slice:", nums2)

    // Correct way to modify the slice
    for i := range nums2 {
        nums2[i] *= 2
    }
    fmt.Println("After modification:", nums2)
}

```

---

### ✅ Output

```
=== Slice Example ===
Index: 0, Value: 10
Index: 1, Value: 20
Index: 2, Value: 30
Index: 3, Value: 40

=== Map Example ===
Alice scored 90
Bob scored 85
Charlie scored 95

=== String Example ===
Index: 0, Character: G
Index: 1, Character: o
Index: 2, Character: L
Index: 3, Character: a
Index: 4, Character: n
Index: 5, Character: g

=== Modify Slice Elements Example ===
Modified value at index 0: 2
Modified value at index 1: 4
Modified value at index 2: 6
Modified value at index 3: 8
Original slice: [1 2 3 4]
After modification: [2 4 6 8]

```

---

### ✅ Key Takeaways

1. `range` works on **slices, arrays, maps, strings, and channels**.
2. `range` gives **copies** of elements for slices/arrays, so modifying the loop variable **does not modify the original slice**.
3. Use `for i := range slice { slice[i] = ... }` to modify in-place.
4. `_` can be used to ignore **index/key** if not needed.