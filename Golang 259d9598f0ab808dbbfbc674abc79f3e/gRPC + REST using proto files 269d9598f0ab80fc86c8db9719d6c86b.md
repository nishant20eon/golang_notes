# gRPC + REST using proto files

- Grpc + proto file code
    
    ## **1️⃣ Project Structure (Production Style)**
    
    ```
    my-grpc-rest-app/
    ├── go.mod
    ├── main.go
    ├── pkg/
    │   ├── api/
    │   │   └── customer.proto           # proto definition
    │   ├── models/
    │   │   └── customer.go              # struct and business logic
    │   ├── services/
    │   │   └── customer_service.go      # gRPC service implementation
    │   └── utils/
    │       └── logger.go
    ├── gen/                             # generated grpc code
    │   ├── customer.pb.go
    │   ├── customer_grpc.pb.go
    │   └── customer.pb.gw.go            # grpc-gateway (REST) code
    
    ```
    
    ---
    
    ## **2️⃣ Proto File (`pkg/api/customer.proto`)**
    
    ```protobuf
    syntax = "proto3";
    
    package api;
    
    import "google/api/annotations.proto";
    
    service CustomerService {
      // Create customer endpoint
      rpc CreateCustomer (CreateCustomerRequest) returns (CreateCustomerResponse) {
        option (google.api.http) = {
          post: "/v1/customers"
          body: "*"
        };
      }
    }
    
    // Request & Response
    message CreateCustomerRequest {
      string name = 1;
      double balance = 2;
      int32 age = 3;
    }
    
    message CreateCustomerResponse {
      int32 id = 1;
      string message = 2;
    }
    
    ```
    
    **Notes:**
    
    - `option (google.api.http)` → grpc-gateway allows REST calls to hit gRPC server.
    - `/v1/customers` → REST endpoint.
    - This proto generates both **gRPC server code** and **REST gateway** code.
    
    ---
    
    ## **3️⃣ Generate gRPC & REST code**
    
    ```bash
    # Generate grpc server code
    protoc -I pkg/api --go_out=gen --go-grpc_out=gen pkg/api/customer.proto
    
    # Generate grpc-gateway (REST to gRPC) code
    protoc -I pkg/api --grpc-gateway_out=gen pkg/api/customer.proto
    
    ```
    
    - After this, `gen/` folder will contain:
        - `customer.pb.go` → messages
        - `customer_grpc.pb.go` → gRPC service interfaces
        - `customer.pb.gw.go` → REST gateway
    
    ---
    
    ## **4️⃣ Implement gRPC Service (`pkg/services/customer_service.go`)**
    
    ```go
    package services
    
    import (
    	"context"
    	"fmt"
    	"my-grpc-rest-app/gen/api"
    	"my-grpc-rest-app/pkg/models"
    	"my-grpc-rest-app/pkg/utils"
    )
    
    type CustomerServiceServer struct {
    	api.UnimplementedCustomerServiceServer
    	customers map[int]*models.Customer
    	nextID    int
    }
    
    func NewCustomerServiceServer() *CustomerServiceServer {
    	return &CustomerServiceServer{
    		customers: make(map[int]*models.Customer),
    		nextID:    1,
    	}
    }
    
    // gRPC Method Implementation
    func (s *CustomerServiceServer) CreateCustomer(ctx context.Context, req *api.CreateCustomerRequest) (*api.CreateCustomerResponse, error) {
    	utils.Log(fmt.Sprintf("CreateCustomer called: %v", req))
    
    	cust := &models.Customer{
    		Id:      s.nextID,
    		Name:    req.Name,
    		Balance: req.Balance,
    		Age:     int(req.Age),
    	}
    	s.customers[s.nextID] = cust
    	s.nextID++
    
    	return &api.CreateCustomerResponse{
    		Id:      int32(cust.Id),
    		Message: "Customer created successfully",
    	}, nil
    }
    
    ```
    
    **Notes:**
    
    - `CustomerServiceServer` implements the generated gRPC interface.
    - Logic to store customers in-memory for demo.
    - `utils.Log()` → helps debug flow.
    
    ---
    
    ## **5️⃣ Main Entry (`main.go`)**
    
    ```go
    package main
    
    import (
    	"context"
    	"flag"
    	"fmt"
    	"log"
    	"net"
    	"net/http"
    
    	"my-grpc-rest-app/gen/api"
    	"my-grpc-rest-app/pkg/services"
    
    	"github.com/grpc-ecosystem/grpc-gateway/v2/runtime"
    	"google.golang.org/grpc"
    )
    
    func main() {
    	grpcPort := flag.Int("grpc-port", 50051, "gRPC port")
    	httpPort := flag.Int("http-port", 8080, "HTTP REST port")
    	flag.Parse()
    
    	// 1️⃣ Start gRPC server
    	lis, err := net.Listen("tcp", fmt.Sprintf(":%d", *grpcPort))
    	if err != nil {
    		log.Fatal(err)
    	}
    
    	grpcServer := grpc.NewServer()
    	customerService := services.NewCustomerServiceServer()
    	api.RegisterCustomerServiceServer(grpcServer, customerService)
    
    	go func() {
    		log.Printf("gRPC server listening at :%d", *grpcPort)
    		if err := grpcServer.Serve(lis); err != nil {
    			log.Fatal(err)
    		}
    	}()
    
    	// 2️⃣ Start REST gateway
    	ctx := context.Background()
    	mux := runtime.NewServeMux()
    	opts := []grpc.DialOption{grpc.WithInsecure()}
    
    	err = api.RegisterCustomerServiceHandlerFromEndpoint(ctx, mux, fmt.Sprintf("localhost:%d", *grpcPort), opts)
    	if err != nil {
    		log.Fatal(err)
    	}
    
    	log.Printf("REST server listening at :%d", *httpPort)
    	if err := http.ListenAndServe(fmt.Sprintf(":%d", *httpPort), mux); err != nil {
    		log.Fatal(err)
    	}
    }
    
    ```
    
    ---
    
    ## **6️⃣ Flow of Code When REST Endpoint Hit**
    
    **Scenario:** Client calls `POST /v1/customers`
    
    **Flow:**
    
    1. **REST Call**
        - Client sends HTTP POST request to `/v1/customers`.
    2. **gRPC-Gateway**
        - `customer.pb.gw.go` intercepts HTTP request.
        - Converts JSON → gRPC request (`CreateCustomerRequest`).
    3. **gRPC Server**
        - Calls `CustomerServiceServer.CreateCustomer()` with the converted request.
        - Business logic executes → stores customer in-memory.
    4. **Response**
        - gRPC server returns `CreateCustomerResponse`.
        - gRPC-gateway converts response → JSON HTTP response.
        - Client receives `{ "id": 1, "message": "Customer created successfully" }`.
    5. **Debug / Logs**
        - `utils.Log()` prints messages showing each step.
    
    ---
    
    ### ✅ **Summary (Production-style flow)**
    
    | Step | Component | Action |
    | --- | --- | --- |
    | 1 | Client | REST POST /v1/customers |
    | 2 | gRPC-Gateway | Converts REST JSON → gRPC message |
    | 3 | gRPC Server | Executes method logic (`CreateCustomer`) |
    | 4 | Response | gRPC → gRPC-Gateway → HTTP JSON |
    | 5 | Logging | Logs each step for traceability |
    
    ---
    
    ### ✅ **Key Points**
    
    - `.proto` file is **single source of truth** → defines both gRPC and REST endpoints.
    - `gen/` folder contains **generated code** → don’t edit manually.
    - REST calls are handled by **grpc-gateway**, which internally calls gRPC server.
    - Business logic lives in `services/` → clean separation.
    - `main.go` wires everything together → starts gRPC + REST servers.
- Diagram - 1
    
    # 🟢 **REST to gRPC Flow – Customer Example**
    
    ```
    CLIENT (REST)
      │
      │ POST /v1/customers  { "name":"Alice", "balance":1000, "age":30 }
      ▼
    ────────────────────────────────────────────
    |          gRPC-Gateway (customer.pb.gw.go) |
    |-------------------------------------------|
    | - Intercepts REST HTTP request            |
    | - Converts JSON → gRPC CreateCustomerRequest |
    | - Calls gRPC server                       |
    ────────────────────────────────────────────
      │
      ▼
    ────────────────────────────────────────────
    |          gRPC Server (CustomerServiceServer) |
    |----------------------------------------------|
    | - Receives gRPC request (CreateCustomerRequest) |
    | - Logs request (utils.Log)                    |
    | - Calls business logic                        |
    |   e.g., create customer in-memory map        |
    | - Prepares gRPC response (CreateCustomerResponse) |
    ────────────────────────────────────────────
      │
      ▼
    ────────────────────────────────────────────
    |          Business Logic / Services Layer    |
    |--------------------------------------------|
    | - CustomerServiceServer.CreateCustomer      |
    |   → generates new Customer struct           |
    |   → stores in map[int]*Customer             |
    | - Accesses Models (pkg/models/customer.go) |
    |   → Customer struct: Id, Name, Balance, Age |
    ────────────────────────────────────────────
      │
      ▼
    ────────────────────────────────────────────
    |          gRPC Response                      |
    |--------------------------------------------|
    | - Response: CreateCustomerResponse         |
    | - grpc-gateway converts gRPC → JSON         |
    ────────────────────────────────────────────
      │
      ▼
    CLIENT RECEIVES RESPONSE
      {
        "id": 1,
        "message": "Customer created successfully"
      }
    
    ```
    
    ---
    
    ### ✅ **Detailed Explanation of Each Component**
    
    1. **CLIENT**
        - Sends HTTP POST request with JSON body.
        - Doesn’t know about gRPC internally.
    2. **gRPC-Gateway**
        - Auto-generated from `.proto` (`customer.pb.gw.go`).
        - Converts JSON → gRPC request.
        - Acts as a REST-to-gRPC translator.
    3. **gRPC Server**
        - Implements the `CustomerServiceServer` interface.
        - Receives gRPC request and executes server-side method.
        - Handles all business logic through service layer.
    4. **Business Logic / Service Layer**
        - `CreateCustomer` method:
            - Generates new `Customer` struct.
            - Stores in-memory or database.
            - Updates fields like Id, Name, Balance, Age.
    5. **Models**
        - Struct definitions (`Customer`) live here.
        - Provides data structure for business logic and storage.
    6. **Response Flow**
        - gRPC response sent back to gRPC-gateway.
        - gRPC-gateway converts gRPC → JSON.
        - Client receives a standard HTTP JSON response.
    
    ---
    
    ### ✅ **Key Takeaways for Production**
    
    - **Proto file** → single source of truth for gRPC + REST endpoints.
    - **gRPC-Gateway** → handles REST → gRPC translation automatically.
    - **Services layer** → contains all business logic, can interact with database or in-memory storage.
    - **Models layer** → data definitions only.
    - **Main.go** → wires gRPC + REST servers and services together.
    - **Logging / Tracing** → `utils.Log` or similar for debugging request flow.
    
    ---
    
    ### 🔹 **Extra Tips for Understanding Flow in Debugging**
    
    1. Always start from **main()** → see how gRPC + REST servers are wired.
    2. Follow REST request through **gRPC-gateway** → shows how JSON is converted.
    3. Trace gRPC method call → see how it interacts with services/models.
    4. Response flow is reversed → gRPC → REST → Client.
    5. Use logs at each step to see **exact order of execution**.
- Diagram - 2
    
    # 🟢 **REST → gRPC Flow Diagram (Customer Example)**
    
    ```
    ┌───────────────┐
    │   CLIENT      │
    │ REST POST     │
    │ /v1/customers │
    │ {JSON Body}   │
    └───────┬───────┘
            │
            ▼
    ┌─────────────────────────────┐
    │  gRPC-Gateway               │
    │ (customer.pb.gw.go)         │
    │ - Intercepts REST request   │
    │ - Converts JSON → gRPC      │
    │ - Calls gRPC server method  │
    └───────────┬─────────────────┘
                │
                ▼
    ┌─────────────────────────────┐
    │  gRPC Server                │
    │ (CustomerServiceServer)     │
    │ - Receives gRPC request     │
    │ - Executes method           │
    │ - Calls Business Logic      │
    └───────────┬─────────────────┘
                │
                ▼
    ┌─────────────────────────────┐
    │ Business Logic / Services   │
    │ - CreateCustomer()          │
    │ - Generates Customer struct │
    │ - Stores in memory / DB     │
    └───────────┬─────────────────┘
                │
                ▼
    ┌─────────────────────────────┐
    │ Models / Data Layer         │
    │ (pkg/models/customer.go)    │
    │ - Customer struct           │
    │   {Id, Name, Balance, Age}  │
    └───────────┬─────────────────┘
                │
                ▼
    ┌─────────────────────────────┐
    │ gRPC Response               │
    │ - CreateCustomerResponse    │
    │ - grpc-gateway converts     │
    │   gRPC → JSON               │
    └───────────┬─────────────────┘
                │
                ▼
    ┌───────────────┐
    │   CLIENT      │
    │ Receives JSON │
    │ Response      │
    │ {id, message} │
    └───────────────┘
    
    ```
    
    ---
    
    ### ✅ **How to Read**
    
    1. Client hits **REST endpoint**.
    2. **gRPC-gateway** converts REST → gRPC request.
    3. **gRPC server** executes method.
    4. **Business logic / services** handle the request.
    5. **Models** define data structures.
    6. Response flows back: gRPC → gRPC-gateway → REST → Client.
    
    ---
    
    ### ✅ **Tips for Debugging**
    
    - Put logs in **gRPC server methods** and **business logic**.
    - Logs in **gRPC-gateway** help trace REST → gRPC conversion.
    - Follow the flow top-down to understand the request lifecycle.
    - You can attach debugger breakpoints at **CreateCustomer** method and see request data in real time.