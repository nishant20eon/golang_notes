# Go Interface and Error

1. ✅ **Interface syntax with methods**
2. ✅ **Implementation of interface by structs**
3. ✅ **How interfaces enable polymorphism**
4. ✅ **Project structure as per a company’s codebase with a real-world example**
5. ✅ Detailed explanation at every step

---

## ✅ 1. What is an Interface in Go?

- An **interface** in Go is a set of method signatures.
- Any struct that implements all the methods of an interface automatically satisfies that interface — **no explicit declaration is needed**.
- Interfaces allow writing flexible and reusable code.
- Interfaces help decouple implementation from usage.

---

## ✅ 2. Interface Syntax with Methods

```go
type InterfaceName interface {
    Method1(paramType) returnType
    Method2(paramType) returnType
}

```

---

## ✅ 3. Real-Time Example — Payment Processing System

Let's imagine you are building a payment processing system for a company where payments can be processed by different gateways like PayPal and Stripe.

---

### ✅ Step 1 – Define the Interface

```go
// PaymentProcessor defines the interface for processing payments
type PaymentProcessor interface {
    ProcessPayment(amount float64) string
}

```

---

### ✅ Step 2 – Implement the Interface with Different Structs

### PayPal Implementation

```go
type PayPal struct {
    Email string
}

func (p PayPal) ProcessPayment(amount float64) string {
    return fmt.Sprintf("Processed payment of $%.2f using PayPal account %s", amount, p.Email)
}

```

### Stripe Implementation

```go
type Stripe struct {
    APIKey string
}

func (s Stripe) ProcessPayment(amount float64) string {
    return fmt.Sprintf("Processed payment of $%.2f using Stripe API key %s", amount, s.APIKey)
}

```

---

### ✅ Step 3 – Using the Interface

```go
func MakePayment(p PaymentProcessor, amount float64) {
    result := p.ProcessPayment(amount)
    fmt.Println(result)
}

```

---

### ✅ Step 4 – Calling in `main`

```go
func main() {
    paypal := PayPal{Email: "user@example.com"}
    stripe := Stripe{APIKey: "sk_test_123"}

    MakePayment(paypal, 100.50)
    MakePayment(stripe, 250.75)
}

```

---

## ✅ How It Works

✔ The interface `PaymentProcessor` defines a contract

✔ Both `PayPal` and `Stripe` implement the `ProcessPayment` method

✔ The `MakePayment` function takes any type that satisfies the interface

✔ The system is extensible: you can add new payment methods without modifying existing code

---

## ✅ 4. Project Structure as per a Company

A typical company Go project is organized like this:

```
/project-root
├── /cmd
│   └── /app
│       └── main.go
├── /pkg
│   ├── /payment
│   │   ├── processor.go
│   │   └── processor_test.go
├── /internal
│   └── /config
│       └── config.go
├── go.mod
└── README.md

```

### ✅ Explanation

- `cmd/app/main.go`: Entry point for the application
- `pkg/payment/processor.go`: Contains the `PaymentProcessor` interface and its implementations
- `pkg/payment/processor_test.go`: Unit tests for payment processors
- `internal/config/config.go`: Configuration files (internal to the project, not exposed)
- `go.mod`: Dependency management
- `README.md`: Documentation

---

### ✅ Example Code Based on Structure

### `/pkg/payment/processor.go`

```go
package payment

import "fmt"

type PaymentProcessor interface {
    ProcessPayment(amount float64) string
}

type PayPal struct {
    Email string
}

func (p PayPal) ProcessPayment(amount float64) string {
    return fmt.Sprintf("Processed payment of $%.2f using PayPal account %s", amount, p.Email)
}

type Stripe struct {
    APIKey string
}

func (s Stripe) ProcessPayment(amount float64) string {
    return fmt.Sprintf("Processed payment of $%.2f using Stripe API key %s", amount, s.APIKey)
}

```

### `/cmd/app/main.go`

```go
package main

import (
    "fmt"
    "project-root/pkg/payment"
)

func MakePayment(p payment.PaymentProcessor, amount float64) {
    result := p.ProcessPayment(amount)
    fmt.Println(result)
}

func main() {
    paypal := payment.PayPal{Email: "user@example.com"}
    stripe := payment.Stripe{APIKey: "sk_test_123"}

    MakePayment(paypal, 100.50)
    MakePayment(stripe, 250.75)
}

```

---

## ✅ 5. Testing the Interface

You can create tests to ensure each implementation works as expected.

### `/pkg/payment/processor_test.go`

```go
package payment

import (
    "testing"
)

func TestPayPalProcessPayment(t *testing.T) {
    pp := PayPal{Email: "test@example.com"}
    result := pp.ProcessPayment(50.00)
    expected := "Processed payment of $50.00 using PayPal account test@example.com"
    if result != expected {
        t.Errorf("Expected %s, got %s", expected, result)
    }
}

func TestStripeProcessPayment(t *testing.T) {
    st := Stripe{APIKey: "test_key"}
    result := st.ProcessPayment(75.00)
    expected := "Processed payment of $75.00 using Stripe API key test_key"
    if result != expected {
        t.Errorf("Expected %s, got %s", expected, result)
    }
}

```

---

## ✅ Key Takeaways

✔ Interfaces are contracts that define method signatures

✔ Structs automatically implement interfaces by having the required methods

✔ Interfaces enable writing functions that work with any implementation

✔ Interfaces promote code reuse and extensibility

✔ Real-world projects organize interfaces and implementations in packages

✔ Unit tests ensure that each implementation behaves as expected

---

This complete explanation now includes:

✅ Interface definition and method signatures

✅ Implementation by multiple structs

✅ Usage patterns with functions

✅ Real-time example (payment processors)

✅ Project directory structure

✅ How to test interface implementations

---

---

1. ✅ **Error handling in interface methods**
2. ✅ **Dependency Injection using interfaces**
3. ✅ **How interfaces support scalability and testing in large systems**

I'll build upon the **Payment Processor** example and include advanced practices used in professional projects.

---

## ✅ 1. Error Handling in Interface Methods

In real-world applications, payment processing might fail, so functions should return an error along with the result.

---

### ✅ Updated Interface with Error Handling

```go
package payment

type PaymentProcessor interface {
    ProcessPayment(amount float64) (string, error)
}

```

---

### ✅ Implementation with Error Handling

### PayPal Implementation

```go
import (
    "errors"
    "fmt"
)

type PayPal struct {
    Email string
}

func (p PayPal) ProcessPayment(amount float64) (string, error) {
    if amount <= 0 {
        return "", errors.New("invalid amount for PayPal payment")
    }
    return fmt.Sprintf("Processed payment of $%.2f using PayPal account %s", amount, p.Email), nil
}

```

### Stripe Implementation

```go
import (
    "errors"
    "fmt"
)

type Stripe struct {
    APIKey string
}

func (s Stripe) ProcessPayment(amount float64) (string, error) {
    if s.APIKey == "" {
        return "", errors.New("missing API key for Stripe payment")
    }
    if amount <= 0 {
        return "", errors.New("invalid amount for Stripe payment")
    }
    return fmt.Sprintf("Processed payment of $%.2f using Stripe API key %s", amount, s.APIKey), nil
}

```

---

### ✅ How to Use It with Error Handling

```go
func MakePayment(p PaymentProcessor, amount float64) {
    result, err := p.ProcessPayment(amount)
    if err != nil {
        fmt.Println("Payment failed:", err)
        return
    }
    fmt.Println(result)
}

```

---

### ✅ Calling from `main`

```go
func main() {
    paypal := PayPal{Email: "user@example.com"}
    stripe := Stripe{APIKey: "sk_test_123"}

    MakePayment(paypal, 100.50)
    MakePayment(stripe, -5.00) // Invalid amount, will produce an error
}

```

---

## ✅ 2. Dependency Injection with Interfaces

Dependency injection (DI) is a technique where you pass dependencies (like the payment processor) into functions or structs instead of hardcoding them.

This allows your code to be more flexible and testable.

---

### ✅ Service that Depends on PaymentProcessor

```go
type PaymentService struct {
    Processor PaymentProcessor
}

func (ps *PaymentService) Pay(amount float64) {
    result, err := ps.Processor.ProcessPayment(amount)
    if err != nil {
        fmt.Println("Error:", err)
        return
    }
    fmt.Println(result)
}

```

---

### ✅ Injecting Dependencies

```go
func main() {
    paypal := PayPal{Email: "user@example.com"}
    service := PaymentService{Processor: paypal}

    service.Pay(200)

    stripe := Stripe{APIKey: "sk_test_456"}
    service.Processor = stripe
    service.Pay(300)
}

```

---

### ✅ Benefits of Dependency Injection

✔ Allows switching implementations at runtime

✔ Facilitates unit testing by injecting mocks or stubs

✔ Promotes loose coupling between components

✔ Encourages interface-based design

---

## ✅ 3. Project Structure in a Scalable Application

```
/project-root
├── /cmd
│   └── /app
│       └── main.go
├── /pkg
│   ├── /payment
│   │   ├── processor.go
│   │   ├── processor_test.go
│   │   └── service.go
├── /internal
│   └── /config
│       └── config.go
├── go.mod
└── README.md

```

### ✅ `/pkg/payment/service.go`

```go
package payment

import "fmt"

type PaymentService struct {
    Processor PaymentProcessor
}

func (ps *PaymentService) Pay(amount float64) {
    result, err := ps.Processor.ProcessPayment(amount)
    if err != nil {
        fmt.Println("Error:", err)
        return
    }
    fmt.Println(result)
}

```

### ✅ `/cmd/app/main.go`

```go
package main

import (
    "fmt"
    "project-root/pkg/payment"
)

func main() {
    paypal := payment.PayPal{Email: "user@example.com"}
    stripe := payment.Stripe{APIKey: "sk_test_456"}

    service := payment.PaymentService{Processor: paypal}
    service.Pay(200)

    service.Processor = stripe
    service.Pay(300)

    fmt.Println("Payments processed successfully.")
}

```

---

## ✅ 4. Testing with Dependency Injection

In your tests, you can create mock processors to simulate various scenarios.

### ✅ Mock Implementation

```go
type MockProcessor struct{}

func (m MockProcessor) ProcessPayment(amount float64) (string, error) {
    return "mock payment processed", nil
}

```

### ✅ Using Mock in Tests

```go
func TestPaymentService(t *testing.T) {
    mock := MockProcessor{}
    service := PaymentService{Processor: mock}

    service.Pay(100)
    // Assertions can be added here if needed
}

```

---

## ✅ Summary

### ✔ Interfaces define behavior without implementation

### ✔ Methods can return errors to handle real-world scenarios

### ✔ Dependency injection allows flexibility, testability, and scalability

### ✔ Interfaces decouple components and promote clean architecture

### ✔ Project structures should separate concerns logically

### ✔ Tests can be written easily by injecting mocks

---

This advanced guide covers:

✅ Interface with methods returning errors

✅ How to implement error handling in structs

✅ Dependency injection pattern in Go

✅ Structuring projects like professional organizations

✅ Writing tests with mock objects

✅ Real-world example — payment processing system