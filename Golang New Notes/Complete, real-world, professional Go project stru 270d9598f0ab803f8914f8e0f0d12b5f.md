# Complete, real-world, professional Go project structure

✔ Types defined in a `types` package (`type.go`)

✔ Business logic in `bl.go`

✔ Client code in `client.go`

✔ Usage of constructors, struct references, pointers, functions with two return values, and interfaces

✔ Organized structure like a company’s project

---

## ✅ Final Project Layout

```
/project-root
├── /cmd
│   └── /app
│       └── client.go
├── /pkg
│   ├── /types
│   │   └── type.go
│   ├── /bl
│   │   └── bl.go
├── go.mod
└── README.md

```

---

## ✅ 1. `/pkg/types/type.go` — Define Common Types

```go
package types

import "fmt"

// Payment represents the payment details
type Payment struct {
    Amount   float64
    Currency string
}

// NewPayment is a constructor that creates a new Payment struct
func NewPayment(amount float64, currency string) *Payment {
    return &Payment{
        Amount:   amount,
        Currency: currency,
    }
}

// Display prints the payment details using struct reference
func (p *Payment) Display() {
    fmt.Printf("Payment: %.2f %s\n", p.Amount, p.Currency)
}

// Validate checks if the payment is valid and returns two values
func (p *Payment) Validate() (bool, error) {
    if p.Amount <= 0 {
        return false, fmt.Errorf("amount must be greater than zero")
    }
    if p.Currency == "" {
        return false, fmt.Errorf("currency must be specified")
    }
    return true, nil
}

```

---

### ✅ Explanation – `type.go`

1. ✅ The `Payment` struct holds payment details like amount and currency.
2. ✅ `NewPayment()` is a constructor that returns a pointer to `Payment`.
3. ✅ `Display()` is a method using a struct reference (`Payment`).
4. ✅ `Validate()` returns two values: a boolean and an error to check the payment’s correctness.

---

## ✅ 2. `/pkg/bl/bl.go` — Business Logic Using Types

```go
package bl

import (
    "fmt"
    "project-root/pkg/types"
)

// PaymentProcessor defines the interface
type PaymentProcessor interface {
    Process(payment *types.Payment) (string, error)
}

// SimpleProcessor implements PaymentProcessor
type SimpleProcessor struct{}

// Process applies business logic to the payment
func (sp SimpleProcessor) Process(payment *types.Payment) (string, error) {
    valid, err := payment.Validate()
    if !valid {
        return "", err
    }
    return fmt.Sprintf("Processed payment of %.2f %s successfully.", payment.Amount, payment.Currency), nil
}

// CreatePayment is a helper function acting like a constructor in business logic
func CreatePayment(amount float64, currency string) (*types.Payment, error) {
    payment := types.NewPayment(amount, currency)
    valid, err := payment.Validate()
    if !valid {
        return nil, err
    }
    return payment, nil
}

```

---

### ✅ Explanation – `bl.go`

1. ✅ `PaymentProcessor` is an interface used to decouple the implementation.
2. ✅ `SimpleProcessor` implements this interface by defining `Process`.
3. ✅ `Process()` applies validation logic and returns two values.
4. ✅ `CreatePayment()` uses the constructor from `types` and validates the payment.

---

## ✅ 3. `/cmd/app/client.go` — Client Code That Uses the Logic

```go
package main

import (
    "fmt"
    "project-root/pkg/bl"
    "project-root/pkg/types"
)

func main() {
    // Create a payment using business logic
    payment, err := bl.CreatePayment(150.0, "USD")
    if err != nil {
        fmt.Println("Error creating payment:", err)
        return
    }

    // Display payment details
    payment.Display()

    // Process payment using interface
    processor := bl.SimpleProcessor{}
    result, err := processor.Process(payment)
    if err != nil {
        fmt.Println("Error processing payment:", err)
        return
    }

    fmt.Println(result)
}

```

---

### ✅ Explanation – `client.go`

1. ✅ Calls `CreatePayment()` from `bl.go` to initialize a payment.
2. ✅ Handles errors from payment validation.
3. ✅ Uses the `Display()` method from `types`.
4. ✅ Instantiates `SimpleProcessor` and processes the payment.
5. ✅ Handles errors during processing and prints the final result.

---

## ✅ How Each Concept Is Used

### ✔ **Constructor**

- `NewPayment()` in `types/type.go` initializes a new `Payment` struct.

### ✔ **Struct Reference and Pointer**

- The method `Display()` uses `Payment` reference.
- The method `Validate()` uses `Payment` and returns two values.
- `CreatePayment()` returns `Payment`.

### ✔ **Two Return Values**

- `Validate()` and `Process()` return `(bool, error)` and `(string, error)` respectively.

### ✔ **Business Logic**

- The payment is validated and processed with proper error handling.

### ✔ **Interface and Implementation**

- `PaymentProcessor` defines the behavior.
- `SimpleProcessor` implements the logic.

### ✔ **Project Structure**

- Types are separated from business logic.
- Business logic is reusable and testable.
- Client code is clean and orchestrates the components.

---

## ✅ Final Thoughts

This complete setup mirrors how large companies structure their Go projects:

- Types are centralized and reusable.
- Business logic is abstracted and uses interfaces.
- Client code orchestrates workflows without worrying about underlying details.
- Error handling and validation are built into methods.
- Dependency injection and modular code make the project scalable and testable.

This is a fully functioning, professional-grade Go project design that incorporates everything you asked for — from constructors to interfaces, validation, error handling, and clean architecture.