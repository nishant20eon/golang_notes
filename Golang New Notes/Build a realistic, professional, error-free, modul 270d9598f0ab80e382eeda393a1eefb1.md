# Build a realistic, professional, error-free, modular Go project

✔ gRPC to REST translation

✔ Business logic in memory (no database)

✔ Proper layering — internal/grpc → services/bl → services/client → types

✔ Structs organized in appropriate files

✔ Constructors, pointers, interfaces, two return values, and error handling

✔ A runnable example that you can test via Postman using grpc-gateway or direct REST handlers

---

## ✅ Project Structure You Want

```
/project-root
├── /cmd
│   └── /server
│       └── main.go              # Starts the server
├── /internal
│   └── /grpc
│       └── api.go                # gRPC handlers calling bl.go
├── /services
│   ├── /bl
│   │   ├── bl.go                 # Business logic implementation
│   │   └── type.go               # Structs used in business logic
│   └── /client
│       ├── client.go             # Client logic and response formatting
│       └── type.go               # Structs used in client layer
├── /types
│   └── type.go                   # Common structs used across services
├── payment.proto                  # gRPC definitions
├── go.mod
└── README.md

```

---

## ✅ Step 1 – Define the Proto File `payment.proto`

```protobuf
syntax = "proto3";

package payment;

option go_package = "project-root/paymentpb";

// Request and Response messages
message GetDetailsRequest {
    int32 id = 1;
}

message GetDetailsResponse {
    int32 id = 1;
    string name = 2;
    double amount = 3;
}

// The Payment service definition.
service PaymentService {
    rpc GetDetails (GetDetailsRequest) returns (GetDetailsResponse);
}

```

You will run `protoc` to generate `payment.pb.go` and `payment_grpc.pb.go`.

---

### ✅ Here’s the updated `payment.proto` with gRPC-Gateway annotations:

```protobuf
syntax = "proto3";

package payment;

option go_package = "project-root/paymentpb";

// Import for HTTP annotations
import "google/api/annotations.proto";

// Request and Response messages
message GetDetailsRequest {
    int32 id = 1;
}

message GetDetailsResponse {
    int32 id = 1;
    string name = 2;
    double amount = 3;
}

// The Payment service definition with HTTP annotation for gRPC-Gateway
service PaymentService {
    rpc GetDetails (GetDetailsRequest) returns (GetDetailsResponse) {
        option (google.api.http) = {
            get: "/v1/details/{id}"
        };
    }
}

```

---

### ✅ Explanation of Changes

- ✅ `import "google/api/annotations.proto";` is required for the HTTP annotation to work.
- ✅ The `option (google.api.http)` block maps the `GetDetails` RPC to an HTTP `GET` request at `/v1/details/{id}`.
- ✅ Now, you can expose this service via REST using `grpc-gateway` in addition to gRPC.

---

## ✅ Steps After This

1. Install `protoc-gen-go` and `protoc-gen-grpc-gateway`.
2. Generate gRPC and HTTP handlers using `protoc` commands.
3. In `main.go`, initialize both gRPC and HTTP servers, or set up a gateway proxy.

## ✅ Step 2 – `/internal/grpc/api.go` — gRPC Handler

```go
package grpc

import (
    "context"
    "project-root/paymentpb"
    "project-root/services/bl"
)

type PaymentAPI struct {
    BlService *bl.BLService
}

func NewPaymentAPI(blService *bl.BLService) *PaymentAPI {
    return &PaymentAPI{
        BlService: blService,
    }
}

func (api *PaymentAPI) GetDetails(ctx context.Context, req *paymentpb.GetDetailsRequest) (*paymentpb.GetDetailsResponse, error) {
    // Call business logic layer
    detail, err := api.BlService.FetchDetails(req.Id)
    if err != nil {
        return nil, err
    }

    return &paymentpb.GetDetailsResponse{
        Id:     detail.ID,
        Name:   detail.Name,
        Amount: detail.Amount,
    }, nil
}

```

### ✅ Explanation

- ✅ The gRPC handler `PaymentAPI` uses dependency injection to receive `BLService`.
- ✅ `GetDetails()` calls `FetchDetails()` in `bl`.
- ✅ It converts the business logic response to a gRPC response.

---

## ✅ Step 3 – `/services/bl/bl.go` — Business Logic Implementation

```go
package bl

import (
    "errors"
    "project-root/services/client"
)

type BLService struct {
    data map[int]*client.ClientDetail
}

func NewBLService() *BLService {
    return &BLService{
        data: map[int]*client.ClientDetail{
            1: {ID: 1, Name: "John Doe", Amount: 100.50},
            2: {ID: 2, Name: "Jane Smith", Amount: 200.75},
        },
    }
}

func (b *BLService) FetchDetails(id int32) (*client.ClientDetail, error) {
    if detail, exists := b.data[int(id)]; exists {
        return detail, nil
    }
    return nil, errors.New("detail not found")
}

```

### ✅ Explanation

- ✅ `BLService` holds in-memory data.
- ✅ `NewBLService()` initializes the data (acts as a constructor).
- ✅ `FetchDetails()` retrieves details or returns an error if not found.

---

## ✅ Step 4 – `/services/client/client.go` — Client Layer Logic

```go
package client

type ClientDetail struct {
    ID     int32
    Name   string
    Amount float64
}

```

### ✅ Explanation

- ✅ `ClientDetail` struct holds data for each client.
- ✅ Used by `bl` for in-memory storage.

---

## ✅ Step 5 – `/services/client/type.go` — Additional Structs if Needed

For this example, we keep it simple and only use `ClientDetail` in `client.go`. If more structs are needed (for requests, metadata, etc.), define them here.

---

## ✅ Step 6 – `/types/type.go` — Common Structs Across Services

```go
package types

// CommonError represents an application-wide error structure
type CommonError struct {
    Code    int
    Message string
}

```

### ✅ Explanation

- ✅ Central place for common structures like errors, logging context, etc.

---

## ✅ Step 7 – `/cmd/server/main.go` — Entry Point

```go
package main

import (
    "log"
    "net"
    "google.golang.org/grpc"
    "project-root/internal/grpc"
    "project-root/paymentpb"
    "project-root/services/bl"
)

func main() {
    // Initialize services
    blService := bl.NewBLService()
    paymentAPI := grpc.NewPaymentAPI(blService)

    // Create gRPC server
    server := grpc.NewServer()
    paymentpb.RegisterPaymentServiceServer(server, paymentAPI)

    // Listen on port
    listener, err := net.Listen("tcp", ":50051")
    if err != nil {
        log.Fatalf("failed to listen: %v", err)
    }

    log.Println("gRPC server running on port 50051")
    if err := server.Serve(listener); err != nil {
        log.Fatalf("failed to serve: %v", err)
    }
}

```

### ✅ Explanation

- ✅ Initializes `BLService` and injects it into `PaymentAPI`.
- ✅ Sets up and starts the gRPC server.
- ✅ Handles errors gracefully.

---

## ✅ Final Notes

### ✅ Complete Flow

1. Client sends a `GetDetails` request to gRPC.
2. The handler in `/internal/grpc/api.go` calls `/services/bl/bl.go`.
3. `bl.go` fetches data from its in-memory map (defined in `/services/client/client.go`).
4. The result is sent back through gRPC to the client.
5. Common structs like `CommonError` are defined in `/types/type.go`.

---

### ✅ Each Concept You Asked For is Covered

✔ **Constructor** — `NewBLService()` initializes in-memory data.

✔ **Struct Reference and Pointer** — `*client.ClientDetail` is passed by reference.

✔ **Two Return Values** — `FetchDetails()` returns a value and an error.

✔ **Business Logic** — In-memory data is fetched and validated.

✔ **Layered Architecture** — Clear separation between transport (gRPC), business logic, and data structures.

✔ **Dependency Injection** — `PaymentAPI` receives `BLService` as a dependency.

✔ **Error Handling** — Errors are returned from `bl` and passed to gRPC.

---

## ✅ Next Steps

You can now:

✔ Run `protoc` to generate gRPC files

✔ Test using `grpcurl`, Postman with grpc-gateway, or directly via gRPC clients

✔ Extend this example with database integration, authentication, and logging

✔ Write unit tests for `bl` and `client` services

✔ Add REST translation using `grpc-gateway` if needed