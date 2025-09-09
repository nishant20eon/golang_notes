# Go Range

# 🔹 `range` in Go

- `range` is used in **for loops** to iterate over:
    - Arrays
    - Slices
    - Maps
    - Strings
    - Channels

---

## 1️⃣ Range with Slice / Array

```go
package main

import "fmt"

func main() {
    nums := []int{10, 20, 30}

    // index and value
    for i, v := range nums {
        fmt.Println("Index:", i, "Value:", v)
    }

    // only value (ignore index with _)
    for _, v := range nums {
        fmt.Println("Value:", v)
    }
}

```

**Output:**

```
Index: 0 Value: 10
Index: 1 Value: 20
Index: 2 Value: 30
Value: 10
Value: 20
Value: 30

```

---

## 2️⃣ Range with Map

```go
func main() {
    myMap := map[string]int{"apple": 10, "banana": 20}

    for k, v := range myMap {
        fmt.Println("Key:", k, "Value:", v)
    }
}

```

**Output:**

```
Key: apple Value: 10
Key: banana Value: 20

```

⚠️ Order of maps is **not guaranteed**.

---

## 3️⃣ Range with String

```go
func main() {
    word := "GoLang"

    for i, ch := range word {
        fmt.Printf("Index: %d, Char: %c\n", i, ch)
    }
}

```

**Output:**

```
Index: 0, Char: G
Index: 1, Char: o
Index: 2, Char: L
Index: 3, Char: a
Index: 4, Char: n
Index: 5, Char: g

```

---

## 4️⃣ Range with Channel

```go
func main() {
    ch := make(chan int, 3)
    ch <- 1
    ch <- 2
    ch <- 3
    close(ch)

    for v := range ch {
        fmt.Println("Value from channel:", v)
    }
}

```

**Output:**

```
Value from channel: 1
Value from channel: 2
Value from channel: 3

```

---

✅ So basically, `range` in Go is like Java’s **for-each loop** but more powerful (works on strings, maps, channels too).

Do you want me to prepare a **Java vs Go comparison table** for `for-each (range)` so you can relate quickly?

### You said:

ok

### ChatGPT said:

Great 👍 here’s a quick **Java vs Go comparison for `for-each (range)`** so you can see the similarities and differences clearly:

---

# 🔹 Java vs Go: For-each / Range

| Feature | **Java (for-each)** | **Go (range)** |
| --- | --- | --- |
| **Syntax** | `for (Type var : collection) { }` | `for index, value := range collection { }` |
| **Works on** | Arrays, Lists, Sets, Maps (with entrySet) | Arrays, Slices, Maps, Strings, Channels |
| **Index access** | Need a normal `for (int i=0...)` loop | `for i, v := range arr {}` gives index + value |
| **Ignore index/value** | Not directly possible | Use `_` placeholder → `for _, v := range arr {}` |
| **Strings iteration** | Iterates by characters (char, UTF-16 code units) | Iterates by runes (UTF-8 safe) |
| **Maps iteration** | `for (Map.Entry<K,V> e : map.entrySet())` | `for k, v := range map {}` |
| **Channels** | Not available | `for v := range ch {}` until channel closed |
| **Order guarantee** | Lists keep order, Sets/Maps may not | Maps: no guaranteed order |
| **Example** | `java for(int n : nums) { System.out.println(n); }` | `go for _, n := range nums { fmt.Println(n) }` |

---

✅ **Biggest difference:**

- Java’s for-each is **only for collections/arrays**.
- Go’s `range` is **more universal** (works on arrays, slices, maps, strings, and channels).