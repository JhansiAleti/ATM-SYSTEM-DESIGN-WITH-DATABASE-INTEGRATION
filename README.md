# ATM-SYSTEM-DESIGN-WITH-DATABASE-INTEGRATION

This project is a **console-based ATM simulation system written in C**, designed to emulate basic ATM functionalities using **RFID + PIN authentication**, **transaction processing**, and **file-based database integration**.

## Features

  - Account creation with:
  - Name, Aadhar number, contact number
  - RFID card (8-digit)
  - 4-digit PIN
  - RFID and PIN-based authentication
  - Banking operations:
  - Balance enquiry
  - Cash deposit
  - Cash withdrawal
  - Mini statement
  - PIN change
  - Account blocking/unblocking
  - Transaction history tracking (last 5)
  - File-based data storage (no SQL database)
  - UART-based interface simulation for communication with external modules (like microcontrollers)
  - Modular code using Linked Lists and structured programming

##  Modules Overview

| Module/File            | Description |
|------------------------|-------------|
| `main.c`               | Main menu and user interface |
| `Create_Account.c`     | Handles new account registration |
| `Verify_PIN.c`         | Authenticates users using PIN |
| `Verify_RFID.c`        | Validates RFID card numbers |
| `Deposit.c`            | Deposit money to an account |
| `Withdraw.c`           | Withdraw money from an account |
| `Mini_Statement.c`     | Shows last 3–5 transactions |
| `Change_Status.c`      | Allows account blocking/unblocking |
| `File_Operations.c`    | Saves/loads account data to/from files |
| `UART.c`               | Simulates UART communication |
| `header.h`             | Shared data structures, typedefs, and function declarations |

## Data Structures

- **Singly Linked List (SLL)** used for storing accounts in memory
- Each node (`struct Manager`) holds:
  - Account details
  - Transaction history (up to 5 entries)
  - Flags for block status and history overflow

## Data Persistence

Account information and transaction logs are stored in external files. This emulates a basic "database integration" without using an actual SQL database, ideal for embedded or simulation environments.

##  How to Run

###  Prerequisites
- GCC compiler
- Linux terminal (recommended due to `__fpurge` usage)
- USB-to-Serial interface (for UART testing, optional)

### Compile and Execute

 //bash
gcc main.c -o atm-system
./atm-system
