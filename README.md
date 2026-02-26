# P2P Gossip Network Implementation

**Instructor:** Nitin Awathare  
**Course:** Distributed Systems  
**Language Used:** Python  
**Development Environment:** Windows 10/11  
**Team Members:**  
- Vadlamudi Jyothsna (B23CS1076)  
- Jandhyam Sai Sriya (B23CS1021)  

---

# 1. Introduction

Peer-to-Peer (P2P) systems are decentralized distributed systems where nodes communicate directly without relying on a central coordinator. Such systems are widely used in applications such as BitTorrent, blockchain networks, distributed databases, and content distribution systems.

This project implements a **Gossip-based Peer-to-Peer Network** in Python using TCP sockets and multithreading. The system demonstrates decentralized communication, dynamic peer membership management, failure detection, and scalable message dissemination using a gossip protocol.

The implementation simulates a distributed network environment on localhost (127.0.0.1) using multiple terminal instances on Windows.

---

# 2. Objectives

The primary objectives of this project are:

1. Design and implement a decentralized P2P overlay network.
2. Implement a Gossip Protocol for reliable and scalable message dissemination.
3. Simulate realistic network topology using power-law degree distribution.
4. Implement heartbeat-based liveness detection using PING messages.
5. Handle dynamic peer join and failure detection.
6. Use multithreading for concurrent communication handling.

---

# 3. System Architecture

The system consists of two types of nodes:

## 3.1 Seed Nodes

Seed nodes act as lightweight membership servers.

### Responsibilities:
- Maintain a list of active peers.
- Handle new peer registrations.
- Provide peer list to joining peers.
- Remove dead peers when reported.
- Log membership changes.

### Important:
Seed nodes do NOT participate in gossip forwarding.  
They are only used for peer discovery and membership management.

---

## 3.2 Peer Nodes

Peer nodes form the actual decentralized P2P overlay network.

### Responsibilities:
- Register with seed nodes.
- Obtain peer list.
- Select neighbors using power-law distribution.
- Establish TCP connections with neighbors.
- Generate gossip messages.
- Forward received gossip messages.
- Avoid duplicate message forwarding.
- Perform periodic liveness detection.
- Report failed peers to seed nodes.

Peers communicate directly with each other, forming a true decentralized system.

---

# 4. Gossip Protocol Design

## 4.1 Gossip Message Format

Example:
[2026-02-26 19->39.24]:127.0.0.1: Message 0 [Hello from jyothsna]

### Format Breakdown:

- `[YYYY-MM-DD HH->MM.SS]` → Timestamp
- `127.0.0.1` → Sender IP
- `Message 0` → Gossip sequence number
- `[Hello from <peer>]` → Message content

Timestamp is generated using Python's `datetime` module.

---

## 4.2 Gossip Behavior

1. Each peer generates 10 gossip messages.
2. Messages are generated every 5 seconds.
3. On generation:
   - Message is sent to all neighbors.
4. On first receipt:
   - Message is stored locally.
   - Forwarded to all neighbors except sender.
5. On duplicate receipt:
   - Message is ignored.

### Properties Achieved:

- No infinite forwarding loops
- No duplicate flooding
- Eventual message propagation to all connected peers

---

# 5. Power-Law Degree Distribution

To simulate real-world P2P networks, peer connectivity follows a power-law distribution:

- A few peers have high connectivity.
- Most peers have fewer connections.

This mimics real decentralized networks such as BitTorrent and blockchain overlays.

The neighbor selection algorithm probabilistically assigns higher degree to fewer nodes.

---

# 6. Liveness Detection Mechanism

Each peer performs heartbeat-based failure detection.

### Process:

- Every 13 seconds, a peer sends a `PING` message to each neighbor.
- If a neighbor fails to respond to 3 consecutive PINGs:
  - The neighbor is marked as dead.
  - A report is sent to seed nodes.
- Seed removes the failed peer from membership list.

### Advantages:

- Detects failures dynamically.
- Maintains active membership.
- Ensures network stability.

---

# 7. Multithreading Design

Python `threading` module is used for concurrency.

Each peer runs multiple threads:

- Listener thread (incoming connections)
- Gossip generation thread
- Ping monitoring thread
- Peer communication handler threads

Thread synchronization is ensured using `threading.Lock` for:

- Shared peer lists
- Logging operations

---

# 8. Implementation Details

## 8.1 Communication

- Protocol: TCP
- Address: 127.0.0.1
- Port: Dynamically assigned per peer
- Data exchange: UTF-8 encoded messages

## 8.2 Logging

All major activities are logged in `outputfile.txt`:

- Peer registration
- Gossip generation
- Gossip reception
- Gossip forwarding
- PING and PING_REPLY
- Dead peer detection

---

# 9. Testing and Evaluation

## 9.1 Peer Join

- Multiple peers were started.
- Each peer successfully registered with seed.
- Peer list was distributed correctly.

## 9.2 Gossip Propagation

- Messages generated by one peer propagated across the network.
- No duplicates were forwarded.
- Message dissemination was successful.

## 9.3 Failure Simulation

- A peer was terminated manually using Ctrl + C.
- Neighbor peers detected 3 missed PING responses.
- Dead peer was reported to seed.
- Seed removed the peer successfully.
- Remaining network continued functioning.

---

# 10. Assumptions

- All nodes run on localhost.
- No encryption is implemented.
- Network communication is reliable.
- Honest peer assumption (no malicious behavior).
- No persistent storage.

---

# 11. Limitations

- Runs only on a single machine.
- No security layer (TLS/SSL).
- No authentication mechanism.
- No persistent membership storage.
- Limited scalability testing.

---

# 12. Future Improvements

- Deployment across multiple machines.
- Secure communication using TLS.
- Peer authentication.
- Persistent distributed storage.
- Network visualization dashboard.
- Performance metric analysis (latency, bandwidth, convergence time).

---

# 13. Conclusion

This project successfully demonstrates:

- Decentralized peer-to-peer communication.
- Gossip-based message dissemination.
- Power-law overlay network formation.
- Heartbeat-based failure detection.
- Dynamic peer membership management.
- Multithreaded socket programming in Python.

The implementation accurately simulates a distributed P2P gossip network and reflects core distributed systems principles including decentralization, scalability, and fault tolerance.

This project provides a strong practical understanding of gossip protocols and P2P system design.