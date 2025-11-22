# **TFTP File Transfer System (Client–Server Architecture)**

This project implements a fully functional **TFTP (Trivial File Transfer Protocol)** system in Java, including:

* A multi-threaded **TFTP server**
* A command-line **TFTP client**
* A complete **binary message encoder/decoder**
* A full **protocol implementation**
* Support for file upload, download, delete, directory listing, and error handling
* A reusable, generic server framework (connections, handlers, protocols)

This system was built as part of a university networks assignment, with a strong emphasis on:

✔ Network programming
✔ Binary protocols
✔ Stateful message handling
✔ Clean multi-threaded design
✔ Client–server architecture

---

## 📌 **Project Overview**

The project reproduces the core functionality of the Trivial File Transfer Protocol (TFTP).
Both client and server communicate through a custom binary protocol using:

* OPCODE-based messaging
* Packets for read/write requests
* Data packets (block-numbered)
* Acknowledgments
* Error packets
* Directory request & broadcast messages

The system is generic and modular:
the networking framework (connections, handlers, server base classes) is **independent** of TFTP, meaning new protocols can be plugged in easily.

---

## 🧱 **Architecture**

The code is divided into several main components:

---

## **1. Networking Framework (Generic)**

### **Server.java / BaseServer.java**

A generic, extensible server supporting:

* Thread-per-client (TPC) mode
* Multi-threaded execution
* Custom protocol + encoder/decoder injection

### **BlockingConnectionHandler.java**

Handles communication with one client:

* Reads & decodes messages
* Passes messages to protocol
* Encodes & sends protocol responses
* Runs in its own thread

### **Connections.java / ConnectionsImpl.java**

A connection manager that:

* Stores all active clients
* Sends messages to specific clients
* Broadcasts messages to all clients (used by TFTP DIRQ ACK)

---

## **2. Messaging Infrastructure (Generic)**

### **MessagingProtocol.java / BidiMessagingProtocol.java**

Define the lifecycle of a connection:

* `process(message)`
* `continueProcessing()`
* Bidirectional messaging
* Ability to push messages back to the client

### **MessageEncoderDecoder.java**

Generic interface for:

* Converting byte streams → messages
* Converting messages → byte arrays

---

## **3. TFTP Protocol Implementation**

### **TftpProtocol.java**

Implements the **full TFTP protocol logic**, including:

* RRQ (Read Request)
* WRQ (Write Request)
* DATA packets (512-byte chunks)
* ACK packets
* ERROR packets
* DIRQ (directory request)
* DELRQ (delete request)
* LOGRQ (login)
* DISC (disconnect)
* BCAST (broadcasts on file creation/deletion)

Manages connection state per client (logged in, reading/writing file, current block number, etc).

---

## **4. TFTP Encoder/Decoder**

### **TftpEncoderDecoder.java**

Handles binary protocol transformation:

#### Incoming bytes → TftpMessage

* accumulates bytes
* identifies OPCODE
* parses filenames, data blocks, packet lengths

#### TftpMessage → byte[]

* encodes responses
* builds binary packets per TFTP spec

---

## **5. TFTP Client**

### **TftpClient.java**

Implements a fully interactive client with:

* Keyboard thread for user commands
* Listener thread for server responses
* Support for commands:

  * `RRQ <filename>`
  * `WRQ <filename>`
  * `DELRQ <filename>`
  * `DIRQ`
  * `LOGRQ <username>`
  * `DISC`

File reading/writing is fully supported.

### **KeyboardThread.java**

Reads user commands, encodes them, sends to server.

### **ListeningThread.java**

Receives server packets, handles:

* File writes
* File reads
* Directory listing output
* Broadcast notifications

---

# 🔌 **Supported TFTP Features**

✔ Login
✔ Read file
✔ Write file
✔ Delete file
✔ Directory listing
✔ Server broadcasts file creation/deletion
✔ Error messages (illegal operation, file exists, access denied)
✔ Clean disconnect
✔ 512-byte chunked data transfer
✔ Reliable block-numbered transmission

---

## 📂 **Project Structure**

```
├── server/
│   ├── Server.java
│   ├── BaseServer.java
│   ├── BlockingConnectionHandler.java
│   ├── Connections.java
│   ├── ConnectionsImpl.java
│   ├── TftpServer.java
│
├── protocol/
│   ├── MessagingProtocol.java
│   ├── BidiMessagingProtocol.java
│   ├── TftpProtocol.java
│
├── encoding/
│   ├── MessageEncoderDecoder.java
│   ├── TftpEncoderDecoder.java
│
├── client/
│   ├── TftpClient.java
│   ├── KeyboardThread.java
│   ├── ListeningThread.java
│
└── README.md
```

---

## 🚀 **How to Run the Project**

### **Start the Server**

```bash
java -cp bin TftpServer <port>
```

Example:

```
java -cp bin TftpServer 7777
```

### **Start the Client**

```bash
java -cp bin TftpClient <server-ip> <port>
```

### **Client Commands**

Inside the client:

```
LOGRQ <username>
RRQ <filename>
WRQ <filename>
DIRQ
DELRQ <filename>
DISC
```

---

## **In a Nutshell**

What was implemented:

* The entire networking architecture (connections, handlers, base server)
* The full TFTP protocol logic
* Complete encoder/decoder for binary messages
* Multithreaded client (keyboard + listener threads)
* File I/O and correct block-based transfer logic
* Error handling and BCAST functionality
* All TFTP opcodes and state machine transitions

This project demonstrates:

✔ Low-level networking in Java
✔ Binary protocols
✔ Multi-threaded systems
✔ Client–server architecture
✔ Clean separation of concerns
✔ Protocol parsing & stateful logic

---

## 📜 **License**

This project is for educational and portfolio use.


Just tell me!
