P2P Network with Gossip Protocol (C++ Implementation)
Distributed Systems Assignment

Instructor: Nitin Awathare

👩‍💻 Team Members

Vadlamudi Jyothsna (B23CS1076)

Jandhyam Sai Sriya (B23CS1021)

📌 Project Overview

This project implements a Peer-to-Peer (P2P) Network in C++ using:

🔁 Gossip Protocol for message dissemination

📊 Power-law degree distribution for peer connectivity

📡 Ping-based liveness detection

🌱 Seed-based peer discovery

The system simulates a distributed network using TCP sockets on localhost (127.0.0.1).

Seed nodes manage membership, while peer nodes exchange gossip messages directly.

🏗️ System Architecture

The network consists of two types of nodes:

🔹 1️⃣ Seed Nodes (seed.cpp)

Seed nodes are responsible for:

Maintaining a list of active peers

Handling new peer registrations

Providing peer lists to joining peers

Removing dead peers when reported

Logging membership changes

Seed nodes do NOT participate in gossip message forwarding.

They are only used for peer discovery and membership management.

🔹 2️⃣ Peer Nodes (peer.cpp)

Peer nodes are responsible for:

Registering with seed nodes

Fetching peer lists

Selecting neighbors using power-law distribution

Establishing TCP connections with other peers

Generating gossip messages

Forwarding received gossip messages

Maintaining a message list to avoid duplicates

Performing liveness detection using PING messages

Reporting dead peers to seed nodes

Logging all major activities

Peers communicate directly with each other (true P2P).

📂 Project Structure
📦 P2P-Gossip-Network
 ├── seed.cpp
 ├── peer.cpp
 ├── config.txt
 ├── outputfile.txt
 ├── README.md
📜 File Description
1️⃣ seed.cpp

Implements seed node functionality:

TCP server for peer registration

Maintains peer list

Shares peer list with new peers

Removes failed peers

Logs seed activities

2️⃣ peer.cpp

Implements peer node functionality:

Registers with seeds

Connects to other peers

Uses power-law distribution for neighbor selection

Generates gossip messages

Forwards gossip messages

Sends periodic PING messages (every 13 seconds)

Detects peer failure after 3 consecutive failed PINGs

Logs activities in outputfile.txt

3️⃣ config.txt

Contains seed node IP and Port details.

Example:

127.0.0.1:5002
127.0.0.1:7234
127.0.0.1:6071

Each line represents one seed node.

4️⃣ outputfile.txt

Stores logs including:

Peer registration

Gossip generation

Gossip reception

Gossip forwarding

PING and PING_REPLY messages

Dead peer detection and removal

This file is used for verification and debugging.

⚙️ Compilation Instructions

Compile using:

g++ seed.cpp -o seed -pthread
g++ peer.cpp -o peer -pthread

Note:

-pthread links the POSIX thread library.

Required because the project uses multithreading.

🚀 Running the Project
Step 1️⃣: Start Seed Nodes
./seed

All seed nodes specified in config.txt will start.

Step 2️⃣: Start Peer Nodes

In separate terminals:

./peer

For each peer:

Enter a unique peer name

Peer registers with seed nodes

Peer connects to other peers

Gossip starts after 30 seconds

Each peer generates 10 gossip messages

Gossip messages are generated every 5 seconds

🔁 Gossip Protocol
📌 Message Format Used in Implementation

Example:

[2026-02-26 19->39.24]:127.0.0.1: Message 0 [Hello from jyothsna]
Format Explanation:

[YYYY-MM-DD HH->MM.SS] → Timestamp

127.0.0.1 → Sender IP address

Message 0 → Gossip message number

[Hello from jyothsna] → Message content

This format is generated using system time functions in C++.

🔁 Gossip Behavior

When a peer generates a message → it sends to all neighbors

On first receipt → store message and forward to neighbors (except sender)

On duplicate receipt → ignore

This ensures:

No infinite loops

No duplicate flooding

Reliable message dissemination

📡 Liveness Detection

Every 13 seconds, peers send a PING message to neighbors.

If a peer fails to respond to 3 consecutive PINGs:

It is considered dead.

A report is sent to seed nodes.

Seed removes the dead peer from the network.

This ensures dynamic network maintenance.

📊 Power-Law Degree Distribution

Peer connections follow a power-law distribution:

A few peers have many connections.

Most peers have fewer connections.

This simulates real-world P2P networks.

🧪 Testing Procedure
Simulating Peer Failure

Start multiple peer nodes.

Stop one peer using Ctrl + C.

Other peers detect 3 failed PING attempts.

Seed removes the dead peer.

Network continues functioning normally.

⚠️ Assumptions

System runs on localhost (127.0.0.1)

No encryption implemented

No persistent storage

Reliable network communication

Honest majority assumption

🔮 Future Improvements

Multi-machine deployment

Secure communication using TLS

Authentication mechanism for peers

Persistent storage of peer list

Improved failure handling

Performance metrics analysis

✅ Conclusion

This project successfully demonstrates:

Gossip-based message dissemination

Power-law P2P overlay formation

Distributed peer discovery

Multithreaded socket programming in C++

Liveness detection using heartbeat (PING)

Dynamic peer joining and removal

The implementation accurately simulates a decentralized P2P gossip network using C++.