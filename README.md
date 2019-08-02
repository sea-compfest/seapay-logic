 # Seapay
 
Seapay is a fintech app consists of 4 different services
  - API gateway
    - API gateway service routes all requests to other services
  - User account
    - User account service takes care of user management: user creation, get user data, etc
  - Wallet
    - Wallet service handles all wallet functionalities: update balance, get balance, create wallet, etc
  - Transaction
    - Transaction service manage all transaction data

The project itself has 4 modules
 - seapay-api
   - a module that abstracts interface for all services
 - seapay-common
   - a module that groups all common functionalities used by all services
 - seapay-domain
   - the bussiness logic for all services goes here
 - seapay-monolith
   - an entry point of our monolithic app, including all handlers
  
 # How to use

 ### Dependencies
 ```
 brew cask install java
 ```

 ### How to build

 ```
 make all
 ```

 ### How to run
 ```
 make run
 ```
 
 # Logic Sequence Diagram
 This script can be shown as Sequence Diagram on `https://mermaidjs.github.io/mermaid-live-editor`

 ### Register Customer
```mermaid
sequenceDiagram
    participant Customer
    participant Gateway
    participant Wallet
    participant UserAccount

    Customer->>Gateway: Register Customer
    Gateway->>UserAccount: Create Customer Account
    UserAccount->>Gateway:Customer Account Created
    Gateway->>Wallet: Create Wallet
    Wallet->>Gateway: Wallet Created
    Gateway->>Customer: Customer Registered
```

### Register Merchant
```mermaid
sequenceDiagram
    participant Admin
    participant Gateway
    participant Wallet
    participant UserAccount

    Admin->>Gateway: Register Merchant
    Gateway->>UserAccount: Create Merchant Account
    UserAccount->>Gateway:Merchant Account Created
    Gateway->>Wallet: Create Wallet
    Wallet->>Gateway: Wallet Created
    Gateway->>Admin: Merchant Registered
```

### Topup
```mermaid
sequenceDiagram
    participant Bank
    participant Gateway
    participant Wallet
    participant Transaction

    Bank->>Gateway: Topup

    Gateway->>Wallet: Get Wallet by UserID
    Wallet->>Gateway: WalletID

    Gateway->>Transaction: Create Topup Transaction
    Transaction->>Gateway: Topup Transaction Created

    Gateway->>Wallet: Topup Wallet
    Wallet->>Gateway: Wallet Topup Success 

    Gateway->>Bank: Topup Success
```

### Pay
```mermaid
sequenceDiagram
    participant Merchant
    participant Gateway
    participant Wallet
    participant Transaction

    Merchant->>Gateway: Pay

    Gateway->>Wallet: Get Wallet by MerchantID
    Wallet->>Gateway: Merchant WalletID

    Gateway->>Wallet: Get Wallet by CustomerID
    Wallet->>Gateway: Customer WalletID

    Gateway->>Transaction: Create Pay Transaction
    Transaction->>Gateway: Pay Transaction Created

    Gateway->>Wallet: Transfer Balance
    Wallet->>Gateway:  Balance Transferred

    Gateway->>Merchant: Payment Success
```

 # Common Sequence Diagram

 ### Create Entity Flow
```mermaid
sequenceDiagram
    participant Client
    participant Service
    participant Entity
    participant Repository
    participant Database
    
   Client->>Service: Create

   Service->>Entity: Initiate new Entity
   Entity->>Service: Entity

   Service->>Repository: Insert Entity

   Repository->>Database: SQL [Insert into xx table]
   Database->>Repository: Data inserted

   Repository->>Service: Entity Inserted

   Service->>Client: Created
```

  ### Find Entity Flow
```mermaid
sequenceDiagram
    participant Client
    participant Service
    participant Repository
    participant Mapper
    participant Entity
    participant Database
    
   Client->>Service: getDataByID

   Service->>Repository: Find Entity by ID

   Repository->>Database: SQL [Select * from xx table]
   Database->>Repository: Database Data return

   Repository->>Mapper: Convert Database Data to Entity
   Mapper->>Entity: Initiate Entity from Data
   Entity->>Mapper: Entity
   Mapper->>Repository: Entity

   Repository->>Service: Entity

   Service->>Client: Data
```
