# ATM System Design with Database Integration

This project simulates a secure ATM system that uses **RFID card authentication**, **PIN validation**, and **real-time transaction communication** between a **microcontroller (LPC2148)** and a **PC application (Linux, C)**. It emulates the entire ATM workflow, including authentication, balance inquiry, deposits, withdrawals, mini statements, and PIN changes, with a structured banking database maintained using **C data structures and file handling**.

##  Objective

To develop a **secure, embedded ATM simulation** using:

- RFID-based authentication
- PIN entry via keypad
- LCD for real-time display
- UART for MCU ↔ PC communication
- PC-side banking database simulation using linked lists and file I/O in C


##  System Architecture

### 1. Front-End – **Microcontroller (LPC2148)**

Handles user interactions:
- **RFID Reader** for card detection
- **Keypad** for PIN and menu inputs
- **LCD Display** for instructions and output
- **UART0/1** for serial communication with the PC

**Modules Implemented:**
- `lcd.c` / `lcd.h` – LCD control
- `keypad.c` / `keypad.h` – Keypad scanning
- `uart.c` / `uart.h` – UART communication
- `delay.c` – Software delays
- `main.c` – Integration of all modules with transaction flow

### 🔹 2. Back-End – **PC Application (Linux, C)**

Simulates the backend banking database:
- Stores account and transaction data using structs and linked lists
- Uses `users.txt` and `transactions.txt` for file persistence
- Listens for UART commands from MCU and processes them

---

## 🔐 Functionalities

- RFID Card Verification
- 4-digit PIN Verification (3 attempts max)
- Account Blocking after failed attempts
- Balance Inquiry
- Cash Deposit / Withdrawal
- PIN Change
- Mini Statement (Last 3 transactions)
- UART-based communication protocol
- File handling for persistent data storage


## 🔄 Command Protocol (MCU ↔ PC)

| Transaction       | Command Sent from MCU                         | Expected PC Response                      |
|-------------------|-----------------------------------------------|-------------------------------------------|
| Card Verification | `#C:<RFID>$`                                  | `@OK:ACTIVE:<Name>$` / `@ERR:INVALID$`    |
| PIN Verification  | `#V:<RFID>:<PIN>$`                            | `@OK:MATCHED$` / `@ERR:WRONG$`            |
| Withdrawal        | `#A:WTD:<RFID>:<Amount>$`                     | `@OK:DONE$` / `@ERR:LOWBAL$`              |
| Deposit           | `#A:DEP:<RFID>:<Amount>$`                     | `@OK:DONE$`                               |
| Balance Inquiry   | `#A:BAL:<RFID>$`                              | `@OK:BAL=<Amount>$`                       |
| Mini Statement    | `#A:MST:<RFID>:<TxNo>$`                       | `@TXN:<Type>:<Date>:<Amount>$`            |
| PIN Change        | `#A:PIN:<RFID>:<NewPIN>$`                     | `@OK:DONE$`                               |
| Block Card        | `#A:BLK:<RFID>$`                              | `@OK:DONE$`                               |
| Line Check        | `#X:LINEOK$`                                  | `@X:LINEOK$`                              |
| Exit              | `#Q:SAVE$`                                    | (No response expected)                    |


## Project Workflow

### 1️ Initialization
- MCU boots up and initializes peripherals
- Displays "Welcome To ATM"
- Tests PC connection via UART

### 2️ Card Authentication
- RFID scanned and sent to PC
- Valid: Show account name
- Invalid/Blocked: Show error and restart

### 3️ PIN Verification
- 3 attempts max, then card blocked
- Successful match leads to menu

### 4️ Menu-Driven Options (via keypad)
- `1`: Balance
- `2`: Deposit
- `3`: Withdraw
- `4`: PIN Change
- `5`: Mini Statement
- `6`: Exit

### 5️ Transactions
- Each action sends a formatted UART request
- PC responds with balance, status, or transaction data
- LCD displays result to the user

### 6️ Session End
- Manual exit or timeout (30s inactivity)
- MCU resets to welcome screen


##  PC-Side Implementation (Linux C)

### Modules:
- `main.c` – Menu and control loop
- `account_creation.c` – Creates and inserts account into linked list
- `transaction.c` – Deposit, Withdraw, Mini-statement
- `uart_handler.c` – Reads UART commands and responds accordingly
- `file_handler.c` – Loads/saves data to files
- `account_utils.c` – Comparison, validation, and display helpers

### Data Structures:
- `struct Manager` – Holds account data and transaction history
- `struct Trsnc` – Individual transaction records
- Linked List used to store all accounts

### File Storage:
- `users.txt` – All account details
- `transactions.txt` – Transaction logs (optional)
- Auto-updated after each transaction


##  LPC2148 Hardware Interfacing

### LCD 16x2
- Data Lines: `P1.16` to `P1.23`
- Control Lines: `P0.16 (RS)`, `P0.17 (RW)`, `P0.26 (EN)`

###  Keypad (4x4)
- Rows: `P1.24` to `P1.27`
- Columns: `P1.28` to `P1.31`

### RFID Reader
- TX (output) → `P0.9` (UART1 RX)

###  UART (DB9 to PC)
- MCU TX (P0.0) ↔ DB9 RX (Pin 2)
- MCU RX (P0.1) ↔ DB9 TX (Pin 3)


##  How to Build and Run

### Requirements
- GCC compiler (Linux)
- LPC2148 with USB-UART setup (for full hardware integration)

### PC Side:
```bash
gcc main.c -o atm_system
./atm_system

