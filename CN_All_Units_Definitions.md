# Computer Networks — Unit-wise Definitions (Complete & Detailed)

> Covers **Unit 1 (Introduction)**, **Unit 2 (Network Layer Design Issues & Routing Algorithms)**, **Unit 3 (Congestion Control Algorithms)**, **Unit 4 (Internetworking & Network Layer in the Internet)**, and **Unit 5 (Transport Layer Protocols)** based on the provided material.

---

# UNIT 1 — Introduction to Computer Networks

## 1. Network
A **network** is a set of devices (called **nodes**) connected by communication links so they can exchange data. A node can be a computer, printer, or any device capable of sending/receiving data.

## 2. Computer Network
A collection of **autonomous** (independent) computers interconnected by a single technology, capable of exchanging information. The connecting medium need not be copper wire — it can be fiber optics, microwaves, infrared, or satellites.

## 3. Distributed System
A collection of independent computers that **appears to its users as a single coherent system**, usually through a software layer called **middleware** sitting on top of the OS. Example: the **World Wide Web**, which runs on the Internet but presents everything as a single "document" model.
- **Key difference from a computer network:** a network is just interconnected machines; a distributed system hides this interconnection behind one unified model/paradigm.

## 4. Applications of Computer Networks
1. **Financial Services** – Electronic Funds Transfer (EFT)
2. **Electronic Messaging**
3. **Video conferencing**
4. **Online reservation services**
5. **Marketing and Sales** – collecting/exchanging/analyzing customer and product data

## 5. Characteristics of Networks
Resource Sharing, Communication speed, Backup, Scalability, Reliability, Hardware/Software sharing, Remote Access, Security.

## 6. Network Devices
| Device | Function (brief) |
|---|---|
| **Hub** | Broadcasts incoming signal to all ports (physical layer) |
| **Switch** | Forwards frames based on MAC address |
| **Router** | Forwards packets based on IP address between networks |
| **Bridge** | Connects two LAN segments, filters traffic by MAC address |
| **Gateway** | Connects networks using different protocols |
| **Modem** | Modulates/demodulates digital signals to analog and back |
| **Repeater** | Regenerates/amplifies a signal to extend distance |
| **Access Point** | Allows wireless devices to connect to a wired network |

## 7. Data Transmission Modes
- **Simplex** – Communication is **unidirectional**; only one device transmits, the other only receives (e.g., keyboard → monitor).
- **Half-Duplex** – Both devices can transmit and receive, but **not simultaneously** (e.g., walkie-talkies).
- **Full-Duplex** – Both devices can transmit and receive **at the same time** (e.g., telephone call).

## 8. Network Types (by geography)
- **LAN (Local Area Network):** Connects personal devices within a few kilometers (offices, factories).
- **MAN (Metropolitan Area Network):** Connects devices across an entire city, up to ~50 km (cable TV, telephone networks).
- **WAN (Wide Area Network):** Spans a country/continent (military, airlines, mobile operators).
- **PAN (Personal Area Network):** Very short range (~10 m); connects personal devices like phones, laptops, tablets.
- **CAN (Campus Area Network):** Interconnects multiple LANs within a college/university/corporate campus.

## 9. Data Communication — Fundamental Characteristics
For data communication to be effective, a system needs:
1. **Delivery** – data must reach the *correct* destination only.
2. **Accuracy** – data must be delivered without alteration.
3. **Timeliness** – data must arrive on time; late data (esp. audio/video) is useless. Delivering data as produced, in order, without delay = **real-time transmission**.
4. **Jitter** – the **variation in packet arrival time**. Uneven delay between consecutive packets causes uneven quality in audio/video playback.

## 10. Transmission Modes (Physical bit transmission)
- **Parallel Transmission:** Sends **n bits at once** using n wires — one wire per bit. Fast, but expensive (needs n communication lines); used over short distances.
- **Serial Transmission:** Sends **1 bit at a time** over a single channel — cheaper than parallel over long distances. Requires parallel-to-serial and serial-to-parallel conversion at the interfaces. Has 3 subtypes:
  - **Asynchronous Transmission:** Timing of the signal doesn't matter; each byte (usually 8 bits) is sent independently with a **start bit** (usually 0) before it and one or more **stop bits** (usually 1) after it, possibly with idle gaps between bytes. Synchronization is only needed for the duration of a single byte. Cheap and effective, good for low-speed communication, but slower due to overhead bits.
  - **Synchronous Transmission:** Bits are combined into longer **frames** with no gaps between bytes; the receiver groups bits itself (typically 8-bit units). No start/stop bits, so it is **faster** than asynchronous but requires precise timing/synchronization. Used for high-speed computer-to-computer transmission.
  - **Isochronous Transmission:** Guarantees data arrives at a **fixed constant rate**, with the *entire bit stream* (not just individual bytes) synchronized — essential for real-time audio/video where uneven delay is unacceptable (e.g., TV at 30 images/sec).

## 11. TCP/IP Protocol Suite
A set of protocols governing how data is packetized, addressed, transmitted, routed, and received over a network — providing **end-to-end connectivity**. It is **stateless** (each client request is treated as new, unrelated to previous ones) and follows the **client-server model**.

**Advantages:** non-proprietary, OS-independent, compatible with all hardware, highly scalable, routable.
**Disadvantages:** complex to configure, no guaranteed delivery at transport layer, hard to swap out protocols, vulnerable to SYN-flood-type attacks.

### Layers of the TCP/IP Model
1. **Application Layer** – HTTP, SNMP, SMTP, DNS, TELNET, FTP
2. **Transport Layer** – TCP, UDP
3. **Network/Internet Layer** – IP, ICMP
4. **Data Link Layer** – MAC
5. **Physical Layer**

### Key Application-Layer Protocols
- **HTTP (HyperText Transfer Protocol):** Transfers web data (text, audio, video) in a hypertext environment.
- **SNMP (Simple Network Management Protocol):** Framework for managing network devices.
- **SMTP (Simple Mail Transfer Protocol):** Sends email between servers.
- **DNS (Domain Name System):** Maps human-friendly names to IP addresses.
- **TELNET (Terminal Network):** Lets a local terminal act as a terminal on a remote system.
- **FTP (File Transfer Protocol):** Standard protocol for transferring files between computers.
- **ICMP (Internet Control Message Protocol):** Lets hosts/routers report datagram problems back to the sender.

## 12. OSI Model (Open Systems Interconnection)
A **generic, protocol-independent conceptual framework** developed by ISO in 1984 to describe all forms of network communication, using **7 layers**:
1. **Physical Layer** – transmits raw unstructured bits (voltages, cabling, pin layout, radio frequencies); devices: hubs, cables, repeaters.
2. **Data Link Layer** – node-to-node transfer, frames, error correction from physical layer. Two sub-layers:
   - **MAC (Media Access Control):** flow control & multiplexing of transmissions.
   - **LLC (Logical Link Control):** flow/error control, identifies line protocols.
3. **Network Layer** – delivers frames/packets to destinations using logical (IP) addresses; routers operate here.
4. **Transport Layer** – manages delivery, error-checking, sizing, sequencing (e.g., TCP).
5. **Session Layer** – sets up, manages, and terminates sessions/connections; handles authentication & reconnections.
6. **Presentation Layer** – formats/translates data for the application layer (also handles encryption/decryption); a.k.a. the "syntax layer."
7. **Application Layer** – direct interface between end-user software and the network (e.g., browsers).

## 13. Addressing (Four Levels in TCP/IP)
- **Physical (Link/MAC) Address:** Identifies a device on its *own* LAN/WAN; lowest-level address, e.g., a 48-bit (6-byte) Ethernet address written as 12 hex digits (`07:01:02:01:2C:4B`). Has authority only within its own LAN/WAN.
- **Logical (IP) Address:** A 32-bit (IPv4) address that identifies a host **globally** and uniquely across the Internet, independent of the underlying physical network format.
- **Port Address:** A 16-bit number identifying a **specific process/application** running on a host, so multiple processes can send/receive data independently (e.g., FTP and TELNET running simultaneously).
- **Specific Address:** User-friendly application-specific addresses, e.g., an **email address** or a **URL (Uniform Resource Locator)**.

## 14. Data Link Control (DLC)
Governs node-to-node communication procedures — mainly **framing** and **flow & error control**.

### Framing
Packing bits into distinguishable **frames** (like putting a letter into an envelope) — separates one message from the next and carries sender/receiver addresses.
- **Fixed-size framing:** frame size itself acts as the delimiter (e.g., ATM cells).
- **Variable-size framing:** needs an explicit way to mark frame boundaries:
  - **Character-Oriented (byte-oriented) Framing:** Data are 8-bit characters (e.g., ASCII); an 8-bit **flag** marks the start/end of a frame.
    - **Byte Stuffing (Character Stuffing):** Inserting a special **escape character (ESC)** before any data byte that accidentally matches the flag pattern, so it isn't mistaken for a frame delimiter.
  - **Bit-Oriented Framing:** Data is a raw sequence of bits; uses a special flag pattern `01111110` to mark frame boundaries.
    - **Bit Stuffing:** After any `0` followed by five consecutive `1`s in the data, an extra `0` bit is inserted (stuffed) to prevent the data from looking like the flag pattern; the receiver removes it.

### Flow Control
Balancing the **rate of production vs. rate of consumption** of data, so a fast sender doesn't overwhelm a slow receiver's buffer.

### Error Control
Detecting/handling corrupted frames, typically using a **CRC (Cyclic Redundancy Check)** added by the sender and verified by the receiver:
- Method 1: silently discard corrupted frames; deliver good ones.
- Method 2: silently discard corrupted frames; send an **ACK** (acknowledgment) for good ones (adds flow control too).

### Connectionless vs. Connection-Oriented DLC Protocols
- **Connectionless Protocol:** Each frame is sent independently with no relationship to other frames (no numbering/ordering). Common in LAN data-link protocols.
- **Connection-Oriented Protocol:** A logical connection is first **set up**, frames are numbered and sent in order during the **transfer phase**, then the connection is **torn down**.

## 15. Data-Link Layer Protocols (Finite State Machine based)
- **FSM (Finite State Machine):** A model with a finite number of states; each event triggers a set of actions and/or a state transition; one state is the initial state.
- **Simple Protocol:** No flow or error control — receiver is assumed to never be overwhelmed. Sender/receiver each have only one "ready" state.
- **Stop-and-Wait Protocol:** Uses both flow and error control. Sender sends **one frame** at a time and waits for an **ACK** before sending the next; uses a **CRC** for error detection and a **timer**; if the timer expires before ACK arrives, the frame is resent.
  - **Sequence Numbers & Acknowledgment Numbers:** Added to data frames/ACK frames (alternating 0/1) to prevent duplicate frames from being delivered twice.
- **Piggybacking:** Combining the **acknowledgment of received data** with the **outgoing data** in the opposite direction, to make bidirectional communication more efficient (adds complexity, so it's uncommon).

## 16. HDLC (High-Level Data Link Control)
A **bit-oriented** protocol for point-to-point and multipoint communication; implements Stop-and-Wait.

### Transfer Modes
- **NRM (Normal Response Mode):** *Unbalanced* configuration — one primary station (sends commands), multiple secondary stations (only respond). Used for point-to-point and multipoint links.
- **ABM (Asynchronous Balanced Mode):** *Balanced* configuration — point-to-point link where each station acts as both primary and secondary (peers). This is the common mode today.

### HDLC Frame Types
1. **I-frames (Information frames):** Carry user data + control info (piggybacking).
2. **S-frames (Supervisory frames):** Carry only control information (flow/error control), no data.
3. **U-frames (Unnumbered frames):** Used for system/link management.

### HDLC Frame Fields
Flag (sync pattern `01111110`), Address, Control, Information, FCS (Frame Check Sequence — 2 or 4 byte CRC), Flag.

### Control Field Details
- **I-frame control field:** first bit = 0; **N(S)** (3 bits) = sequence number; **N(R)** (3 bits) = piggybacked ACK number; **P/F bit** = Poll (primary→secondary) or Final (secondary→primary).
- **S-frame control field:** first 2 bits = `10`; **N(R)** = ACK/NAK number; 2-bit **code** field defines 4 types:
  - **RR (Receive Ready):** code = 00 — acknowledges safe receipt.
  - **RNR (Receive Not Ready):** code = 10 — acknowledges receipt but signals receiver is busy (a congestion-control mechanism).
  - **REJ (Reject):** code = 01 — a NAK used in Go-Back-N ARQ.
  - **SREJ (Selective Reject):** code = 11 — a NAK used in Selective Repeat ARQ.
- **U-frame control field:** used for connection establishment/release, e.g. **SABM** (Set Asynchronous Balanced Mode — request connection), **UA** (Unnumbered Acknowledgment), **DISC** (Disconnect).

## 17. PPP (Point-to-Point Protocol)
The most common **point-to-point** data-link protocol, e.g. used by home users connecting to an ISP over a telephone line. Uses **character-oriented framing**.

### PPP Frame Fields
- **Flag:** 1-byte, `01111110`, marks start/end.
- **Address:** constant `11111111` (broadcast).
- **Control:** constant `00000011` (mimics unnumbered frames). PPP does **not** provide flow control; error control is limited to detection only.
- **Protocol:** defines what the data field carries.
- **Payload field:** the actual data (default max 1500 bytes); byte-stuffed if the flag pattern appears inside it.
- **FCS:** 2 or 4 byte CRC.
- **Byte Stuffing in PPP:** escape byte `01111101` is inserted before any accidental flag-like pattern in the data (and the escape byte itself is escaped if it appears in the data).

### PPP Transition Phases (FSM)
**Dead** (no carrier) → **Establish** (negotiate options, optionally authenticate) → **Open** (data transfer) → **Terminate** → back to **Dead**.

## 18. Media Access Control (MAC) — Multiple Access Protocols
Needed when many nodes share one **broadcast/multipoint link**, to coordinate who gets to transmit.

### Channelization Protocols
- **FDMA (Frequency Division Multiple Access):** Bandwidth split into separate **frequency bands**; each station permanently owns one band (e.g., FM radio).
- **TDMA (Time Division Multiple Access):** Bandwidth split into **time slots**; each station transmits only during its assigned slot (e.g., a scheduled test).
- **CDMA (Code Division Multiple Access):** All stations transmit **simultaneously over the entire frequency**; each is distinguished by a unique **code sequence**.

### Random Access (Contention) Protocols
Stations are equal (no controller); each decides on its own, based on medium state, whether to transmit. Collisions can happen.
- **ALOHA:** the original, simplest multiple-access protocol — transmit whenever you have data (no medium sensing).
- **CSMA (Carrier Sense Multiple Access):** "Sense before transmit" / "listen before talk" — a station checks if the medium is idle before sending. Reduces but doesn't eliminate collisions.
- **CSMA/CD (Collision Detection):** Adds monitoring of the medium *during/after* transmission to detect a collision; if detected, the frame is resent. Used in wired Ethernet.
  - **Minimum Frame Size requirement:** frame transmission time (Tfr) must be **at least 2× the maximum propagation time (Tp)**, so the sender is still transmitting when a collision signal could return.
- **CSMA/CA (Collision Avoidance):** Designed for **wireless** networks (can't reliably detect collisions there). Uses three strategies:
  - **IFS (Interframe Space):** a waiting period even after sensing the channel idle, to let far-away signals arrive first.
  - **Contention Window:** a randomly chosen number of time slots to wait, following **binary exponential backoff** (window doubles each time the channel is found busy after IFS).
  - **Acknowledgment:** a positive ACK + timeout confirms successful, uncorrupted receipt.
  - **RTS/CTS Handshake:** sender sends **RTS (Request To Send)**; receiver replies **CTS (Clear To Send)**; then data is sent; then an ACK. Uses **DIFS** and **SIFS** interframe spacing.
  - **NAV (Network Allocation Vector):** a timer other (nearby) stations set based on the duration info in an overheard RTS, telling them how long to defer before checking the channel again — this is the core mechanism that achieves collision avoidance.
  - **Hidden-Station Problem:** solved via the RTS/CTS handshake — a station (C) that can't hear the sender's RTS can still hear the receiver's CTS (which carries the duration) and thus knows to stay silent.

---

# UNIT 2 — Network Layer Design Issues & Routing Algorithms

## 1. Network Layer — Role
The **network layer** is responsible for getting packets from the **source all the way to the destination**, possibly across many intermediate router hops. This contrasts with the **data link layer**, whose job is only to move frames across *one* link (one wire). Thus, the network layer is the **lowest layer dealing with end-to-end transmission**.

To do this, the network layer must:
1. Know the network's **topology** (all routers and links) and choose good paths through it, even at large scale.
2. **Avoid overloading** some lines/routers while others sit idle.
3. Handle the extra problems that appear when source and destination are on **different networks**.

## 2. Store-and-Forward Packet Switching
The basic model of most WANs: a host sends its packet to the **nearest router** (on its own LAN, or a point-to-point link into the ISP). The packet is **stored** at that router until it has **fully arrived** and its **checksum** has been verified, and only then is it **forwarded** to the next router — hop by hop — until it reaches the destination. This overall mechanism is called **store-and-forward packet switching**.
- **DSL Modem (Digital Subscriber Line modem):** a device connecting a computer/router to a DSL internet connection via a telephone line, allowing simultaneous high-speed internet + voice calls.
- In the model, the **ISP's equipment** (routers + transmission lines) is distinguished from the **customer's own equipment** (e.g., a customer-owned router with a leased line into the ISP).

## 3. Services Provided to the Transport Layer
The network layer offers services to the transport layer at their shared interface. Good network-layer service design should ensure:
1. Services are **independent of router technology**.
2. The transport layer is **shielded from the number, type, and topology** of routers.
3. Network addresses use a **uniform numbering plan**, even across LANs/WANs.

### Two Philosophies (the "Great Debate")
1. **Internet Community view:** Routers should only **move packets** — nothing else. Hosts should do their own **error control** and **flow control**. The network service should be **connectionless**, with simple primitives like `SEND PACKET`/`RECEIVE PACKET`; no packet ordering or flow control at the network layer, since hosts do it anyway. Every packet must carry the **full destination address** since each is routed independently.
2. **Telephone Company view:** The network should provide a **reliable, connection-oriented** service, because **quality of service (QoS)** is paramount — especially hard to achieve for real-time voice/video without connections.

## 4. Implementation of Connectionless Service (Datagram Networks)
- **Datagram:** an individually routed packet, injected into the network with **no advance setup**, in analogy to a telegram.
- **Datagram Network:** a network offering connectionless service — each datagram is routed **independently** of others.
- **How it works:** The sending transport layer prepends a header and hands data to the network layer, which may split a long message into multiple packets, sending each individually via a point-to-point protocol (e.g., **PPP**) to the first router.
- **Routing Table:** an internal table each router keeps, pairing a **destination** with the **outgoing line** to use for it (only directly-connected lines/neighbors can be listed).
- Different packets of the *same* message **can take different routes** (e.g., if a router updates its table due to a detected traffic jam partway through sending a message) — this is a defining trait of connectionless/datagram networks.
- **Routing Algorithm:** the algorithm that manages/updates these tables and makes the routing decisions.
- **IP (Internet Protocol)** is the dominant real-world example of connectionless network service — every packet carries a full destination IP address so routers can forward it independently.

## 5. Implementation of Connection-Oriented Service (Virtual-Circuit Networks)
- **Virtual Circuit (VC):** a means of transporting data over a packet-switched network where a **virtual path is pre-established** between source and destination before data flows — avoiding the need to choose a fresh route for every packet.
- **Virtual-Circuit Network:** a network where a route is chosen **once, at connection setup**, stored in router tables, and reused for **all traffic on that connection** — just like the telephone system. The circuit is released (torn down) when the connection ends.
- Every packet on a VC carries a **connection identifier** telling which virtual circuit it belongs to (instead of a full destination address on every packet).
- **Label Switching:** the ability of a router to **replace/remap the connection identifier** in outgoing packets — needed because two different source hosts might independently pick the *same* connection identifier for their own connections, causing a naming conflict downstream that the router must resolve by assigning a different outgoing identifier.
- **MPLS (MultiProtocol Label Switching):** the key real-world example of a connection-oriented network service, used within ISP networks — IP packets are wrapped in an MPLS header carrying a **20-bit label** (connection identifier).

## 6. Comparison: Virtual Circuit vs. Datagram Subnets
| Issue | Datagram Subnet | Virtual-Circuit Subnet |
|---|---|---|
| Circuit setup | Not needed | Required |
| Addressing | Each packet carries full source & destination address | Each packet carries only a short VC number/label |
| State information | Routers hold no per-connection state | Each VC requires router table space per connection |
| Routing | Each packet routed independently | Route chosen once at setup, then fixed |
| Effect of router failure | Minimal (only packets in transit at the router are lost) | Serious (all VCs through that router terminate) |
| Congestion control | Harder | Easier if enough buffers can be pre-allocated per VC |
| Quality of service | Difficult | Easier to guarantee |

## 7. Routing Algorithms — Core Concepts
- **Routing Algorithm (defined):** the part of the network-layer software responsible for deciding which **outgoing line** an incoming packet should be sent on.
- In **datagram** networks, this decision must be made **fresh for every packet** (best route may have changed).
- In **virtual-circuit** networks, the routing decision is made only **once, at VC setup** — subsequent data just follows the established route. This is sometimes called **session routing**, since the route remains fixed for an entire session.
- **Routing vs. Forwarding:**
  - **Routing:** deciding *which* routes to use (updating routing tables) — the job of the routing algorithm.
  - **Forwarding:** what happens when a packet *arrives* — looking up the outgoing line in the (already built) routing table.

### Desirable Properties of a Routing Algorithm
**Correctness, Simplicity, Robustness, Stability, Fairness, Efficiency.** It should cope with topology/traffic changes without aborting jobs on hosts.

### Nonadaptive vs. Adaptive Routing
- **Nonadaptive (Static) Routing:** Routes are computed **in advance, offline**, and downloaded to routers at boot — do **not** respond to real-time measurements of topology/traffic. Useful only where the best routing choice is already clear and unlikely to need to change.
- **Adaptive (Dynamic) Routing:** Routing decisions **change** to reflect real topology/traffic changes. These algorithms differ in:
  - **Where** they get information (locally, from adjacent routers, or from all routers).
  - **When** they update routes (on topology change, or periodically every ΔT seconds).
  - **What metric** they optimize (distance, hop count, estimated transit time, etc.).

## 8. Shortest Path Routing (Dijkstra's Algorithm)
A technique for computing optimal paths given a **complete picture** of the network, modeled as a **graph**: each **node** = a router, each **edge** = a communication link, and each edge has a **label/weight** (which can represent hop count, geographic distance, bandwidth, average traffic, cost, measured delay, or a combination).
- **Shortest Path:** the path with the minimum total weight between two nodes, according to whatever metric is chosen.
- **Dijkstra's Algorithm (1959):** Finds shortest paths from one source node to **all** other nodes.
  - Every node gets a **label** = current best known distance from the source; initially all labels are **infinity** (except the source).
  - A label is either **tentative** (may still improve) or **permanent** (proven to be the true shortest distance — never changes again).
  - **Steps:**
    1. Mark the source node **permanent**.
    2. Examine all nodes adjacent to the current **working node**; relabel each with the shorter of its old label or (working node's label + edge weight); also record the predecessor node for path reconstruction.
    3. Among all **tentatively** labeled nodes in the whole graph, pick the one with the **smallest label**, make it permanent — it becomes the new working node.
    4. Repeat steps 2–3 until all nodes are permanent.
  - Because each node stores its *predecessor* (working backward toward the source), the final path is reconstructed **in reverse** and then flipped to produce the correct forward order.

## 9. Flooding
A simple **local** routing technique: every incoming packet is sent out on **every outgoing line except the one it arrived on**. This generates a huge (potentially infinite) number of duplicate packets unless damped.
- **Damping with a Hop Counter:** a counter in the packet header is decremented at each hop; the packet is discarded when it reaches zero. Ideally initialized to the known/expected path length, or to the network's full **diameter** as a worst case.
- **Damping with Sequence Numbers:** the source router puts a **sequence number** in each packet; every router keeps a per-source list of sequence numbers **already seen/flooded**, and does not re-flood duplicates.
- **Uses of Flooding:**
  1. Guarantees delivery to **every node** — good for broadcasting.
  2. Extremely **robust** — even if many routers are destroyed, flooding will find a path if one exists (useful for military/war-zone networks).
  3. Requires minimal setup — routers only need to know their **neighbors**.
  4. Used as a **benchmark metric** against which other routing algorithms are compared, since flooding always finds the shortest path (it tries every path in parallel).
  5. In wireless networks, flooding happens somewhat naturally since all stations in radio range receive every transmission.

## 10. Distance Vector Routing
Each router maintains a **table (vector)** giving its **best known distance to each destination** and **which link to use** to get there. Routers exchange this information with their **neighbors** periodically, until every router knows the best link for every destination.
- Also called the **Distributed Bellman-Ford Routing Algorithm** (after Bellman 1957; Ford & Fulkerson 1962) — this was the **original ARPANET routing algorithm**.
- **Routing table entry structure:** for each destination — the **preferred outgoing line** + an **estimated distance** to it (hops, or another metric like propagation delay, which can be measured directly using special **ECHO packets** that are timestamped and bounced back).
- **Update process:** Every `T` msec, each router sends **its own distance vector** (estimated delay to every destination) to each of its neighbors, and also receives the same from each neighbor. If a router knows its delay to neighbor X is `m` msec, and X claims a delay of `Xi` to reach destination `i`, then the router can reach `i` via X in `Xi + m` msec. It repeats this calculation for **every neighbor** and picks the **best (smallest)** result as its new estimate and route for that destination. (The **old** routing table is not used in this calculation — only the fresh neighbor reports.)

### The Count-to-Infinity Problem
A fundamental **flaw** of distance vector routing: when a network topology change (especially a **link/router going down**) occurs, the "bad news" propagates **very slowly** — routers keep believing they have valid (but actually invalid, looping) routes to a destination through each other, incrementing distance estimates step-by-step ("counting up") toward infinity rather than immediately recognizing the destination is unreachable. This can cause persistent **routing loops**, network instability, wasted traffic, and slow **convergence**.
- **Convergence:** the state where all routers in the network have settled onto consistent, correct best-path information (share the same topological view/understanding).
- **Key asymmetry:** distance vector routing reacts **quickly to good news** (a shorter route appearing propagates in one exchange) but **very slowly to bad news** (a route disappearing can take many exchanges to fully propagate — proportional to the network's longest path length, and can be far worse, effectively counting to infinity, in failure cases with loops).
- This was the **primary reason** the ARPANET replaced distance vector routing with **link state routing** in 1979.

## 11. Link State Routing
Introduced to fix distance vector's slow-convergence/count-to-infinity problems. The complete network **topology is distributed to every router**, and each router runs **Dijkstra's algorithm locally** to compute shortest paths to everyone else. Widely used variants: **IS-IS (Intermediate System to Intermediate System)** and **OSPF (Open Shortest Path First)**.

### The Five Steps Each Router Performs
1. **Discover its neighbors** and learn their network addresses.
2. **Set the distance/cost metric** to each neighbor.
3. **Construct a packet** describing everything it just learned (a **Link State Packet**, LSP).
4. **Send this packet to, and receive packets from, all other routers** (via flooding).
5. **Compute the shortest path** to every other router (using Dijkstra's algorithm on the now-complete topology).

### Step 1 — Learning about Neighbors
A booting router sends a **HELLO packet** on each point-to-point line; the router on the other end replies with its (globally unique) name.
- **Broadcast Links (e.g., switched Ethernet):** Modeling every pair of routers on a shared LAN as separate point-to-point links would bloat the topology graph. Instead, the LAN itself is modeled as an **artificial extra node "N"**, with each attached router connected to N — with one **designated router** on the LAN playing the role of N in the protocol.

### Step 2 — Setting Link Costs
The cost/metric for a link can be set automatically or configured manually.
- A common choice: cost **inversely proportional to bandwidth** (e.g., 1-Gbps link = cost 1; 100-Mbps link = cost 10), favoring higher-capacity paths.
- For geographically spread networks, **delay** may be factored in (measured via round-trip **ECHO packets**, dividing the round-trip time by two).

### Step 3 — Building Link State Packets (LSPs)
An LSP contains: the sender's **identity**, a **sequence number**, an **age**, and a **list of neighbors with costs**. Built either **periodically** or **on significant events** (a line/neighbor going up/down, or a notable property change).

### Step 4 — Distributing Link State Packets (via Flooding)
All routers must reliably and quickly receive **all** LSPs, or inconsistent topology views cause loops/unreachable destinations.
- Each LSP carries an incrementing **sequence number**; routers track `(source router, sequence number)` pairs already seen, forwarding only **new** packets and discarding duplicates/obsolete ones (lower sequence number than already seen).
- **Problems & fixes:**
  - Sequence number **wraparound:** solved using a **32-bit** sequence number (takes ~137 years to wrap at 1 LSP/sec).
  - A **crashed router restarting at sequence 0:** its packets would wrongly be rejected as duplicates/obsolete.
  - A **corrupted sequence number** (e.g., a 1-bit error turning 4 into 65,540) could wrongly reject many subsequent legitimate packets.
  - **Solution to all three:** include an **Age field**, decremented once per second (and also decremented during flooding, so a packet can't live forever); when age hits zero, that router's LSP data is discarded — so bad state naturally times out (normally new LSPs arrive every ~10 sec, so this only triggers on real router failure or major packet loss).
- **Refinements:** incoming LSPs are held briefly (in case more updates are coming); duplicate LSPs are compared by sequence number (equal = discard duplicate; different = discard the older); LSPs are individually **acknowledged** to guard against link errors.
- Each router keeps a data structure per received-but-not-fully-processed LSP, tracking: origin, sequence, age, data, and per-link **send flags** (must still forward here) and **acknowledgment flags** (must still ACK here).

### Step 5 — Computing New Routes
Once a router has a **full set** of LSPs, it can reconstruct the entire network graph (every link appears **twice**, once per direction — and different directions can even have different costs, so shortest paths can be asymmetric). It then runs **Dijkstra's algorithm locally** to compute shortest paths to every destination and installs the results into its routing table.
- **Trade-off vs. distance vector:** link state needs **more memory and computation** (memory ∝ `k·n` for `n` routers with `k` neighbors each), but does **not suffer from the slow-convergence problem**, so it converges much faster and more reliably.
- **IS-IS vs. OSPF:** both are very similar (OSPF borrowed many IS-IS innovations); the key difference is **IS-IS can carry information for multiple network-layer protocols simultaneously** (e.g., IP, IPX, AppleTalk), while OSPF cannot — an advantage for large multiprotocol environments.
- **General routing-robustness caveat:** Any bug/failure at even a small number of routers (false/forgotten links, corrupted forwarding, memory exhaustion, calculation errors) can seriously disrupt the whole network's routing — a growing risk as networks scale to tens/hundreds of thousands of nodes.

## 12. Hierarchical Routing
As networks grow, keeping a full routing-table entry for **every** router becomes infeasible (memory, CPU, bandwidth all suffer). The solution, as used in the telephone network, is **hierarchical routing**:
- Routers are grouped into **regions**; each router knows full details for routing **within its own region**, but nothing about the internal structure of other regions (only how to reach them in aggregate).
- Very large networks may need more levels: regions → **clusters** → **zones** → **groups**, etc.
- **Trade-off:** hierarchical routing **saves table space** dramatically, but **increases path length** in some cases (since the "best" route condensed into one entry per remote region isn't necessarily optimal for every specific destination inside that region).
- **Kamoun & Kleinrock (1979) result:** for a network of `N` routers, the **optimal number of hierarchy levels is ln N**, requiring about `e·ln N` routing-table entries per router — and the resulting increase in mean effective path length is small enough to usually be acceptable.

## 13. Broadcast Routing
**Broadcasting** = sending a packet to **all** destinations in the network simultaneously (e.g., weather updates, stock feeds, live radio).

### Method 1 — Send a Separate Packet to Each Destination
Simplest approach; the source sends one distinct packet per destination.
- **Drawbacks:** wastes bandwidth, is slow, and requires the source to know **every** destination address.

### Method 2 — Multi-Destination Routing
Each packet carries **a list (or bit-map) of destinations**. At each router, the destinations are checked against the routing table to determine which **output lines** are needed (an output line is needed if it's the best route to at least one listed destination); the router makes one copy of the packet per needed output line, each copy carrying only the subset of destinations relevant to that line — the destination set is progressively **partitioned** across the network.
- **Better than Method 1** (network bandwidth used more efficiently — packets that must follow the same route share the "cost" of just one packet), but the source must **still know all destinations**, and routers still do meaningful per-packet destination-list processing.

### Method 3 — Reverse Path Forwarding (RPF)
An elegant, simple technique: when a broadcast packet **arrives** at a router, the router checks whether it arrived on the **link the router would normally use to send packets back toward the source**.
- **If yes:** this is very likely the *first* copy to arrive via the best path — the router forwards it onto **all other links** (except the one it arrived on).
- **If no:** the packet is almost certainly a **duplicate** — it is **discarded**.
- **Advantages:** efficient (sends a broadcast packet over each link only once per direction, like flooding), simple to implement, requires **no sequence numbers**, **no full destination lists** in the packet, and only requires routers to know how to reach destinations (normal routing knowledge) — nothing extra.
- **Spanning Tree:** a subset of the network graph including **all routers but containing no loops**. If a router knows which of its lines belong to the spanning tree, it can broadcast-forward incoming packets onto just those lines (excluding the arrival line) for **maximum bandwidth efficiency** — generating the theoretical minimum number of packets needed.
- **Sink Tree:** a spanning tree rooted at a particular destination router, representing the best paths from all other routers *to* that one node — reverse path forwarding approximates broadcasting along the sink tree of the source.

## 14. Multicast Routing
**Multicasting** = sending a message to a **well-defined subset (group)** of receivers — large enough that per-destination unicast is expensive, but small enough (relative to the whole network) that plain broadcasting would be wasteful (or even inappropriate, if non-members shouldn't see the data).
- Requires some mechanism to **create/destroy groups** and identify **which routers have group members** — typically via a **multicast address**, with routers aware of which groups they belong to (mechanism details are outside the routing algorithm itself).
- **Multicast Spanning Tree:** built by starting from a broadcast spanning tree and **pruning away links that lead to no group members** (Deering & Cheriton, 1990) — resulting in an efficient tree specific to each group (different groups get different pruned trees, generally with fewer links than the full broadcast tree).
- **Pruning with Distance Vector Routing:** based on reverse path forwarding — a router with **no interested hosts and no other routers depending on it** for a given group sends a **PRUNE message** back, telling its neighbor to stop sending it multicasts for that group. This process happens **recursively**, progressively trimming the tree to only the links actually needed to reach group members.
  - **DVMRP (Distance Vector Multicast Routing Protocol):** a real-world multicast protocol that works exactly this way. Produces efficient trees, but at the cost of significant router workload, especially at large scale.

---

# UNIT 3 — Congestion Control Algorithms

## 1. Congestion
The state when network nodes/links are **overloaded with data**, degrading performance and increasing delay. Causes: high demand, insufficient bandwidth, or inefficient routing.

### Effects of Congestion
1. **Increased Latency** – delays hurt real-time apps (video calls, gaming).
2. **Packet Loss** – packets dropped due to limited buffer space, causing retransmissions.
3. **Reduced Throughput** – less bandwidth available per user/device.
4. **Unfair Resource Allocation** – some flows dominate bandwidth at others' expense.

## 2. General Principles of Congestion Control
- **Open Loop Solutions:** Solve congestion through **good design up front**; no adjustments once running. Decisions are made **without regard to current network state** (e.g., deciding when to accept traffic, which packets to discard, scheduling policy).
- **Closed Loop Solutions:** Based on a **feedback loop** with 3 steps:
  1. **Monitor** the system to detect congestion (metrics: % packets discarded, average queue length, # timeouts/retransmissions, average delay, delay standard deviation).
  2. **Pass information** about congestion to where action can be taken.
  3. **Adjust operation** to fix the problem.
- **Explicit Feedback Algorithms:** Packets are sent back from the congestion point to explicitly warn the source.
- **Implicit Feedback Algorithms:** The source *infers* congestion from local observations (e.g., time taken for ACKs to return), without an explicit warning packet.

### Ways to Address Congestion
- **Increase resources:** e.g., temporarily add bandwidth via dial-up lines, boost transmission power, split traffic over multiple routes, bring backup routers online.
- **Decrease load:** deny service to some users, degrade service, or have users schedule demand more predictably.

## 3. Congestion Prevention Policies (Open-Loop, by layer)
- **Data Link Layer:**
  - **Retransmission Policy:** how quickly a sender times out and what it retransmits (Go-Back-N is heavier load than Selective Repeat).
  - **Buffering Policy:** whether out-of-order packets are discarded (causing retransmission) or buffered.
  - **Acknowledgement Policy:** immediate ACKs add traffic; piggybacked ACKs may cause extra timeouts.
  - **Flow control window size:** a small (tight) window reduces data rate, helping fight congestion.
- **Network Layer:**
  - **Virtual circuits vs. datagrams** — many congestion algorithms need virtual circuits.
  - **Packet queueing/service policy** — queue per input line vs. per output line, and processing order.
  - **Discard Policy:** rule for which packet to drop when there's no buffer space.
  - **Routing algorithm quality** — spreading traffic vs. overloading congested lines.
  - **Packet lifetime management** — how long a packet may live before being discarded.
- **Transport Layer:** Same issues as data link, plus **harder timeout determination** since transit time across a whole network is less predictable than over a single wire.

## 4. Congestion Control in Virtual-Circuit Subnets
- **Admission Control:** Once congestion is signaled, **no new virtual circuits are set up** until the problem clears — crude but simple and effective.
- **Alternative:** allow new circuits but **route them around congested areas**.
- **Traffic/Resource Negotiation Agreement:** when a VC is set up, host and subnet negotiate traffic volume/shape and QoS; the subnet **reserves resources** (buffers, bandwidth) in advance so congestion is less likely — though this can **waste bandwidth** if not all circuits transmit simultaneously.

## 5. Congestion Control in Datagram Subnets
- Each router monitors output-line **utilization (u)**, a value between 0.0–1.0, updated periodically using a smoothing constant `a` that determines how fast recent history is "forgotten."
- If `u` exceeds a threshold, the line enters a **"warning" state**.

### The Warning Bit
A bit set in a packet's header by a congested router; the destination copies it into the next ACK sent back to the source; the source reduces its transmission rate as long as it keeps seeing the bit set, and increases when it stops (used in old DECNET, and frame relay).

### Choke Packets
A router experiencing congestion sends a **choke packet** directly back to the source (an explicit, more direct method than the warning bit). The source must reduce traffic to that destination by X percent, then **ignore further choke packets** for that destination for a fixed interval (since more are already in flight); if more choke packets arrive after that interval, it reduces the rate again (typically first reduction to 0.50 of previous rate, then 0.25, etc.); if none arrive, flow may increase again (in smaller increments).

### Hop-by-Hop Choke Packets
At high speed/long distance, sending the choke packet all the way back to the *source* reacts too slowly (too much data already "in the pipe"). Instead, the choke packet **takes effect at every hop it passes through** — each intermediate router immediately reduces flow to the next one, giving quicker relief at the congestion point at the cost of extra buffer usage upstream (no packets are lost this way).

## 6. Load Shedding
When nothing else works, routers simply **throw away packets** they can't handle.
- **Wine policy:** for file transfer, **old** packets are more valuable than new ones (dropping old creates a bigger retransmission gap).
- **Milk policy:** for multimedia, **new** packets are more valuable than old ones.
- **Intelligent discard / priority marking:** applications mark packets by priority class so routers drop the lowest-priority ones first.

## 7. Random Early Detection (RED)
A proactive algorithm that discards packets **before** buffer space is fully exhausted (based on the idea that acting early is more effective). Routers maintain a **running average queue length**; if it exceeds a threshold, the line is "congested" and a packet (chosen at random from the triggering queue) is dropped — usually **silently** (no choke packet), relying on the source noticing the missing ACK and slowing down (works only when sources respond to lost packets by slowing down, e.g., TCP over wired links; doesn't work well in wireless where losses are usually due to noise).

## 8. Jitter Control
- **Jitter:** the variation (standard deviation) in packet arrival times. High jitter degrades audio/video quality even if average delay is fine.
- Controlled by computing the **expected transit time per hop** — packets ahead of schedule are held briefly; packets behind schedule are rushed, reducing overall jitter.
- **Buffering at the receiver** (e.g., ~10 sec for streaming media) can eliminate jitter for non-interactive apps, but isn't acceptable for real-time interactive apps (telephony, videoconferencing) due to the added delay.

## 9. Quality of Service (QoS)
### Flow
A stream of packets from a source to a destination. In connection-oriented networks, all packets of a flow follow the same route; in connectionless networks they may differ.

### The Four QoS Parameters
1. **Reliability** – no bits delivered incorrectly (verified via checksums).
2. **Delay (Latency)** – time for a packet to go from source to destination; caused by propagation, switching, routing, buffering.
3. **Jitter** – variation in delay.
4. **Bandwidth** – data-carrying capacity needed.

### Jitter vs. Latency (Key Differences)
1. Latency = delay in departure→arrival; Jitter = *variation* in that delay.
2. Congestion causes jitter; latency comes from propagation/switching/routing/buffering.
3. Jitter is countered with **timestamps**; latency is reduced via **multiple connections**.

### ATM QoS Traffic Categories
1. **Constant Bit Rate (CBR):** e.g., telephony — simulates a wire (uniform bandwidth/delay).
2. **Real-time Variable Bit Rate (rt-VBR):** e.g., compressed videoconferencing.
3. **Non-real-time Variable Bit Rate (nrt-VBR):** e.g., streaming a movie.
4. **Available Bit Rate (ABR):** e.g., file transfer — not sensitive to delay/jitter.

## 10. Techniques for Achieving Good QoS
1. **Overprovisioning:** Provide so much capacity/bandwidth/buffers that congestion basically never happens. Simple but expensive.
2. **Buffering:** Buffer flows at the receiver **before** delivering them; doesn't help reliability/bandwidth, increases delay, but **smooths jitter** — key technique for audio/video-on-demand.
3. **Traffic Shaping:** Regulating the **average rate and burstiness** of data leaving the *sender* (as opposed to the receiver-side buffering above). A **Service Level Agreement (SLA)** defines the agreed transmission shape; the carrier monitors compliance — this monitoring is called **Traffic Policing**.

### The Leaky Bucket Algorithm
An analogy: a bucket with a small hole lets water (packets) **out at a constant rate**, no matter how bursty the input; excess input when the bucket is full simply **spills over and is lost**. Implemented as a finite queue: incoming packets are queued if there's room, else discarded; exactly one packet (or a fixed byte quota) is released per clock tick, enforcing a smooth, even output rate. It **strictly disallows bursts** in the output.

### The Token Bucket Algorithm
Tokens are generated at a constant rate (one every ΔT) and accumulate in a bucket (up to a max, `n`). A packet can only be transmitted if it **captures/destroys one token**. Unlike leaky bucket:
- It **allows saving up** unused capacity (tokens) — enabling **bursts** of up to `n` packets when traffic picks up, giving faster response to sudden bursts.
- It **discards excess tokens** (not packets) when the bucket is full — so it **never discards data** by itself, unlike leaky bucket which discards packets.
- A byte-count variant lets each token represent `k` bytes rather than one packet.

### Resource Reservation
Reserving actual capacity along a flow's fixed route. Three reservable resource types:
1. **Bandwidth** – don't oversubscribe any output line.
2. **Buffer space** – reserve buffers for a flow so it never has to compete with others for them.
3. **CPU cycles** – ensure the router's processing capacity isn't overloaded (even small idle-time losses can create backlogs, since routers rarely run at 100% efficiency).

### Admission Control
A router's decision (based on capacity vs. existing commitments) to **admit or reject** a new flow. Uses a **Flow Specification** — a set of negotiable parameters describing a flow, typically including:
1. **Token bucket rate** – max sustained bytes/sec (long-term average).
2. **Token bucket size** – max burst capacity in bytes.
3. **Peak data rate** – max rate tolerated even briefly; never to be exceeded.
4. **Minimum packet size** – matters because processing each packet has a fixed per-packet cost.
5. **Maximum packet size** – bounded by internal network limits (e.g., Ethernet's 1500-byte cap).
- Routers can only **reduce** the flow spec as it propagates, never increase it.

### Proportional Routing
Instead of sending all traffic for a destination over one "best" path, **split traffic across multiple paths** (e.g., proportional to link capacity) using locally available information, since routers usually lack a full network-wide view.

### Packet Scheduling
Prevents one aggressive flow from starving others.
- **Fair Queueing:** Each output line has a separate queue per flow; the router **round-robins** across queues so each of n competing hosts gets 1/n of the bandwidth (regardless of how many packets it sends).
- **Byte-by-byte round robin (Demers et al.):** improves fair queueing by simulating scanning byte-by-byte rather than packet-by-packet, preventing large-packet flows from getting an unfair advantage.
- **Weighted Fair Queueing (WFQ):** an extension where different flows/hosts get **different priority weights** (e.g., more bandwidth for video servers than file servers).

## 11. Integrated Services (IntServ)
An IETF (1995–1997) architecture for guaranteed, per-flow QoS over IP, for real-time/multimedia traffic (unicast and multicast). Based on **reserving resources in advance** for each individual flow. Documented in RFCs 2205–2210.
- **IETF (Internet Engineering Task Force):** the open international body that develops Internet standards.
- **RFC (Request for Comments):** the documents through which Internet standards/protocols are proposed and published.

### RSVP (Resource reSerVation Protocol)
The main IntServ signaling protocol (RFC 2205) — used **only to make reservations**; actual data uses other protocols. Allows multiple senders to multicast to multiple receiver groups, lets receivers freely switch channels, and reserves bandwidth using **multicast spanning trees**:
- Each multicast group has an address; the routing algorithm (not part of RSVP) builds a spanning tree covering group members.
- Any receiver can send a **reservation message up the tree** toward the sender (using **reverse path forwarding**); each hop along the way reserves the necessary bandwidth (or reports failure if insufficient).
- Reservations from different receivers for the *same* source can **share** already-reserved capacity along common path segments; the reservation is sized to satisfy the **greediest receiver**.

## 12. Differentiated Services (DiffServ)
A simpler, **class-based** (not flow-based) alternative to IntServ, implementable **locally at each router** without advance per-flow setup, path-wide negotiation, or per-flow state — making it far more scalable.
- Offered by a domain of routers (e.g., an ISP); packets carry a **Type of Service** field indicating their class (e.g., premium vs. regular); the operator can charge more for premium service.

### Flow-Based vs. Class-Based QoS (Comparison)
| Aspect | Flow-Based (IntServ) | Class-Based (DiffServ) |
|---|---|---|
| Granularity | Per-individual-flow | Per-class (groups of flows) |
| State | Maintains per-flow state in routers | No per-flow state — simplified management |
| Complexity | High (extra memory/processing) | Lower, more scalable |
| Control | Fine-grained | Coarser, less granular |
| Best for | VoIP, videoconferencing (strict per-flow guarantees) | Prioritizing traffic types generally (e.g., mission-critical vs. bulk) |

### Expedited Forwarding (EF)
The simplest DiffServ class: two service levels — **regular** and **expedited**. A small fraction of traffic is expedited and should transit the network as if no other traffic exists; implemented with **two output queues per line** and a scheduling ratio (e.g., WFQ) so expedited traffic always sees an effectively unloaded network.

### Assured Forwarding (AF)
A more elaborate DiffServ scheme with **4 priority classes × 3 discard probabilities (low/medium/high)** = **12 total AF classes**, marked via the **DSCP (Differentiated Services Code Point)** field in the IP header:
- AF11–AF13 (Class 1, low→high drop), AF21–AF23 (Class 2), AF31–AF33 (Class 3), AF41–AF43 (Class 4).
- **3-step process:** (1) **Classify** packets into a priority class (best done at the sending host, which has the most flow information); (2) **Mark** packets using the Type-of-Service field (6 bits per RFC 2597); (3) **Shape/drop** via a filter (e.g., leaky/token buckets) to conform each stream to its acceptable shape.

---

# UNIT 4 — Internetworking & The Network Layer in the Internet

## PART I: Internetworking

## 1. Internetworking
The practice of **connecting multiple different computer networks together** to form a larger network of networks (the Internet being the prime example), enabling communication between devices on different networks despite differing underlying technologies.

### How Networks Can Differ
Different modulation techniques, frame formats, addressing schemes, ordering guarantees (connection-oriented vs. connectionless), and QoS support levels — all can cause interoperability problems at network boundaries.

### How Networks Can Be Connected — Two Approaches
1. **Translation devices:** convert packets from one network's format directly into another's.
2. **Common layer of indirection:** build a universal layer (this is what **IP** does) recognized by every router, riding on top of each individual network's own technology.

## 2. Key Terms in Internetworking
- **MPLS (Multiprotocol Label Switching):** A protocol operating between Layer 2 and Layer 3, used to improve efficiency of forwarding in WANs/service-provider networks; **connection-oriented** (needs a virtual circuit set up), unlike connectionless 802.11/Ethernet.
- **802.11:** IEEE standards for **Wireless LAN (WLAN)** communication (e.g., 802.11a in 5 GHz up to 54 Mbps; 802.11b in 2.4 GHz up to 11 Mbps).
- **Multiprotocol Router:** A router that can handle **multiple network-layer protocols**, either translating between them or leaving conversion to a higher layer.
- **Router vs. Switch/Bridge:** A **router** extracts the packet from the frame and uses the *network address* to decide where to send it (understands the network-layer protocol). A **switch/bridge** forwards the *entire frame* based on its MAC address, without needing to understand the network layer protocol at all.

### Common Problems Connecting Different Networks
1. **Addressing & Routing** – incompatible addressing schemes/subnetting.
2. **Network Protocols & Standards** – e.g., TCP/IP vs. IPX/SPX vs. AppleTalk incompatibility.
3. **Security & Access Control** – differing policies/authentication across networks.
4. **Network Performance** – differing latency/bandwidth across joined networks.
5. **Network Management & Monitoring** – harder with disparate technologies.
6. **Quality of Service (QoS)** – maintaining consistent QoS across networks with different capabilities.
7. **Firewalls & NAT (Network Address Translation)** – can obstruct interconnectivity if misconfigured.
8. **Network Stability & Reliability** – more potential points of failure.
9. **Protocol Translation** – needs intermediary devices, adding complexity/failure points.

## 3. Connectionless Internetworking (Datagram Model)
The network layer only offers the ability to **inject datagrams and hope for the best** — no virtual circuit concept exists at the network layer. Packets from the same connection may travel **different routes** and may **arrive out of order** (or not at all). Achieves higher potential bandwidth (multiple routes) at the cost of ordering guarantees.
- **SNA (Systems Network Architecture):** IBM's proprietary architecture (1970s) for connecting mainframes — used as an example of an incompatible addressing scheme vs. IP.
- Building a **universal packet format** that all routers recognize (i.e., **IP**) is the practical solution to the addressing mismatch problem, though achieving universal adoption is hampered by commercial interests in proprietary formats (e.g., the IPv4 vs. IPv6 divide).

### Comparison — Concatenated Virtual-Circuit Model vs. Datagram Model
- **Virtual-circuit model:** buffers reservable in advance, guaranteed sequencing, shorter headers, avoids delayed-duplicate issues — BUT needs per-connection table space in routers, no alternate routing around congestion, vulnerable to router failure along the path, and doesn't work over unreliable datagram-only subnets.
- **Datagram model:** more potential for congestion but more adaptability to it, robust to router failures, needs longer headers. Works over LANs/mobile networks/WANs that don't internally support virtual circuits (a case where virtual-circuit internetworking would fail entirely).

## 4. Tunneling
A technique to **transmit data securely/compatibly across an untrusted or incompatible network** by **encapsulating** packets of one protocol inside packets of another, creating a virtual "tunnel."
- **Classic use case:** two networks of the *same* type (e.g., IPv6) connected via a *different* intervening network (e.g., the IPv4 Internet). The router at each end wraps/unwraps the IPv6 packet inside an IPv4 packet; intermediate routers and the end hosts never need to understand both protocols — only the multiprotocol routers at the tunnel's ends do.

### Network Overlay
A **virtual network built on top of an existing physical network**, enabling communication between nodes that aren't directly connected at the physical level. Example: a **VPN (Virtual Private Network)** is an overlay creating a secure virtual network over the public Internet.
- **Advantages:** flexibility/agility, virtualization & multi-tenancy, enhanced security, scalability independent of physical infra, protocol translation, easier deployment/management.
- **Disadvantages:** added overhead/complexity (encapsulation), risk of overlapping IP address spaces, dependency on the underlying network's reliability, network fragmentation as overlays multiply, cross-vendor compatibility issues, variable performance.

## 5. Internetwork Routing (Interdomain Routing)
The process of forwarding packets across multiple interconnected networks/domains, requiring exchange of routing info between different **Autonomous Systems (ASes)**.

### Why It's Harder Than Single-Network Routing
- Different networks may use **different internal routing algorithms** (link state vs. distance vector).
- Different **operators** may value different metrics (delay vs. monetary cost) — costs aren't comparable across networks.
- Operators may not want to **reveal internal path details** (competitive/business reasons).
- The Internet as a whole is **larger** than any single constituent network, needing hierarchical scaling.

### Two-Level Routing Algorithm
Splits routing into two levels for scalability:
1. **Intra-domain Routing (IGP – Interior Gateway Protocol):** Routing *within* a single Autonomous System. Examples:
   - **OSPF (Open Shortest Path First):** link-state protocol.
   - **RIP (Routing Information Protocol):** distance-vector protocol using hop count.
2. **Inter-domain Routing (EGP – Exterior Gateway Protocol):** Routing *between* different Autonomous Systems. Main example: **BGP (Border Gateway Protocol)**.

## 6. Fragmentation
Every network/link has a maximum packet size (**MTU**) due to hardware limits, OS buffer sizes, protocol length fields, standards compliance, error-retransmission concerns, or channel-hogging prevention. Common MTUs: **1500 bytes (Ethernet)**, **2272 bytes (802.11)**; IP itself allows up to 65,515 bytes.

- **MTU (Maximum Transmission Unit):** The largest packet size a given network link can carry without fragmenting it.
- **Packet Fragmentation:** Breaking a large packet into smaller pieces to fit within the outgoing link's MTU. Steps: (1) check packet size vs. MTU; (2) fragment if needed (copying header info to each fragment); (3) transmit fragments independently (possibly via different paths, out of order); (4) **reassemble** at the destination using header info.
- **Nontransparent Fragmentation:** The **sending host** is responsible for fragmenting; intermediate routers just forward fragments as independent packets without knowledge of the original packet.
- **Transparent Fragmentation:** **Intermediate routers** detect oversized packets and fragment/reassemble them along the path, entirely hidden from sender and receiver.
- **Path MTU Discovery (PMTUD):** A technique to dynamically discover the smallest MTU along an entire path, avoiding fragmentation altogether.
  - Sender sets the **DF (Don't Fragment)** flag and sends a packet at a guess size (e.g., 1500 bytes for IPv4, 1280 bytes minimum for IPv6).
  - If a router's outgoing link MTU is too small, it **drops** the packet and returns an **ICMP "Fragmentation Needed"** message specifying the max MTU it can forward.
  - Sender reduces packet size and retries, iterating until it finds the path's smallest MTU (the **Path MTU**), then uses that size for all future packets on that path.
  - **Advantages:** less fragmentation overhead, improved performance, more efficient (larger) payloads.
  - **Disadvantages:** ICMP filtering by firewalls can break PMTUD; misconfiguration risk; **PMTUD Black Hole** (an ICMP message gets lost/blocked, so the sender never learns to shrink packets, causing repeated silent packet loss); slight additional signaling overhead.
  - **Why fragmentation is avoided generally:** it adds overhead, requires *all* fragments to arrive for reassembly, firewalls sometimes drop fragments, and losing even one fragment forces costly retransmission of the whole packet.

## PART II: The Network Layer in the Internet

## 7. IP (Internet Protocol)
A fundamental **Network Layer (Layer 3)** protocol that provides a standardized way to route packets from source to destination across different networks/devices. IP is **connectionless** and **best-effort** — it does not guarantee delivery or reliability; higher layers (TCP, UDP) build on it for reliability. Works with companion protocols **ICMP** (error reporting) and is complemented by **TCP** (reliable) and **UDP** (lightweight).

### IP Addresses
An IP address technically identifies a **network interface**, not a host — a multi-homed host needs multiple IP addresses; routers (with multiple interfaces) always have multiple IP addresses.
- **IPv4:** 32-bit address space (~4.3 billion addresses); dotted-decimal notation (e.g., `192.168.1.1`); addresses are now scarce.
- **IPv6:** 128-bit address space (~340 undecillion addresses); hexadecimal-colon notation (e.g., `2001:0db8:85a3:0000:0000:8a2e:0370:7334`); designed to solve IPv4 exhaustion plus add security/efficiency improvements.

## 8. Internet Control Protocols

### ICMP (Internet Control Message Protocol)
Used by routers/hosts to report unexpected events during packet processing back to the sender, and to test the network. Each message is carried inside an IP packet. Key message types:
| Type | Meaning |
|---|---|
| **Destination Unreachable** | Router can't locate the destination, or a DF-flagged packet can't get through a "small-packet" network |
| **Time Exceeded** | Packet dropped because its **TTL (Time to Live)** reached zero — indicates possible routing loops |
| **Parameter Problem** | An illegal value found in a header field — indicates a software bug |
| **Source Quench** | (Largely obsolete) told a host to slow down due to congestion |
| **Redirect** | Tells the sending host to use a better route |
| **Echo / Echo Reply** | Used by `ping` to check reachability/liveness of a destination |
| **Timestamp Request / Reply** | Like Echo, but records send/receive times — used to measure performance |
| **Router Advertisement / Solicitation** | Lets hosts discover nearby routers |
| **Address Mask Request/Reply** | Determines a network's subnet mask (older ICMP usage) |

### ARP (Address Resolution Protocol)
Resolves an **IP address into a physical (MAC/Ethernet) address**, since NICs understand only physical addresses, not IP addresses.
- **How it works:** A host broadcasts "who owns IP address X?" on the local network; the owning host replies with its MAC address; the requester **caches** the mapping to avoid repeat broadcasts.
- **Default Gateway:** the router a host sends all "off-network" traffic to (by convention, often the lowest address on the subnet); the host resolves the gateway's own MAC address via ARP too.
- **Gratuitous ARP:** A device **proactively broadcasts its own IP-to-MAC mapping** (not in response to a request) — used to announce/update its presence on the network.
- **Proxy ARP:** A router answers ARP requests **on behalf of another host** (giving its own MAC address), letting a host on a different network appear to be local — useful e.g. for mobile hosts that aren't on their home network.

### DHCP (Dynamic Host Configuration Protocol)
Automatically assigns IP addresses and other configuration parameters (subnet mask, default gateway, DNS server) to hosts, avoiding tedious/error-prone manual configuration.
- **Process:** Host broadcasts a **DHCP DISCOVER** (identified by its MAC address, since it has no IP yet) → server responds with a **DHCP OFFER** containing a free IP address (relayed via routers if the server isn't on the local network).
- **Leasing:** IP addresses are assigned for a **fixed time period**; the host must request a **renewal** before the lease expires, or it loses the address — prevents addresses being permanently lost if a host leaves without releasing its address.
- **Advantages:** simplified network management, IP address pooling, centralized configuration, address conservation, adapts to dynamic network changes. Largely replaced older protocols **RARP** and **BOOTP**.

## 9. OSPF (Open Shortest Path First) — Interior Gateway Protocol
A **link-state** intradomain (interior gateway) routing protocol, standardized in 1990, drawing on the **IS-IS (Intermediate-System to Intermediate-System)** ISO protocol. Replaced older distance-vector protocols like **RIP**, which suffered from the **count-to-infinity problem** and slow convergence.
- **Autonomous System (AS):** a collection of IP networks/routers under a single administrative organization's control, using one internal routing policy.
- OSPF models the network as a **directed graph** with weighted arcs (delay, distance, etc.); every router computes shortest paths using the **link-state method**.
- **ECMP (Equal Cost MultiPath):** if multiple equally-short paths exist, OSPF remembers all of them and **splits traffic across them** to balance load.
- **Areas:** OSPF divides a large AS into numbered **areas** (a network or contiguous set of networks) to improve scalability — a router only needs full topology detail within its own area; other areas are visible only via summarized destination info.
  - **Area 0 / Backbone Area:** every AS has one; all other areas connect to it (directly or via tunnels), so any two areas can communicate through the backbone.
  - **Internal Router:** lies wholly within one area.
  - **Area Border Router:** connects two or more areas (and is also part of the backbone); summarizes destinations/costs from one area into others.
  - **Stub Area:** an area with only a single border router — routes out of it are simply "go to the border router," no detailed summary needed.
  - **AS Boundary Router:** injects routes to external destinations (outside the AS) into the area.
- **Designated Router:** On a broadcast LAN, one router is elected to be "adjacent" to (i.e., exchange info with) all others, avoiding an inefficient all-to-all exchange; a **backup designated router** is kept ready in case it fails.
- **OSPF Message Types:**
  1. **Hello:** discovers/establishes neighbor relationships.
  2. **Database Description (DBD):** exchanges a summary list of Link State Advertisements (LSAs) held.
  3. **Link State Request (LSR):** requests specific missing LSAs.
  4. **Link State Update (LSU):** carries the actual requested LSAs.
  5. **Link State Acknowledgment (LSAck):** confirms receipt of LSAs.
- **LSA (Link State Advertisement):** a packet of information describing network topology, flooded to all routers so each can build a consistent view and compute shortest paths.

## 10. BGP (Border Gateway Protocol) — Exterior Gateway Protocol
The standard **inter-domain (between-AS)** routing protocol; unlike OSPF/RIP (which only need to move packets efficiently), BGP must also handle **routing policy/politics** (economic, security, political constraints on which traffic flows where).

### Key BGP Concepts
- **Transit Service:** A customer AS pays a provider AS to carry its traffic to/from anywhere on the Internet; the provider advertises **all** Internet routes to the customer, and the customer advertises **only its own routes** to the provider.
- **Peering:** Two ASes agree to exchange traffic **directly and for free** (not paying a transit provider) — but peering is **not transitive**; an AS won't carry traffic between two other ASes it merely peers with unless compensated.
- **IXP (Internet eXchange Point):** A shared facility where many ISPs connect to each other, commonly used to implement peering/transit links.
- **Multihoming:** A company network connects to **multiple ISPs** for redundancy; it typically must run BGP itself to tell other ASes which addresses go via which ISP link.
- **Path Vector Protocol:** BGP is a form of **distance vector protocol**, but instead of just a cost, each router tracks the actual **AS Path** — the full sequence of ASes a route has traversed (in reverse order) plus the next-hop router. This makes it easy to detect/avoid routing loops (a router discards any advertisement that already includes its own AS number).
- **eBGP (External BGP):** used *between* different Autonomous Systems.
- **iBGP (Internal BGP):** used *within* the same AS, to propagate externally-learned routes consistently to all of that AS's boundary routers.
- **BGP Route Selection Strategies (typical policies):**
  1. Prefer routes via **peers** (free) over routes via **transit providers** (paid).
  2. Prefer routes to **paying customers** highest.
  3. Prefer **shorter AS paths** (a common tiebreaker, though not always truly shortest in practice).
  4. Prefer the route with **lowest cost within one's own ISP** — a strategy called **Early Exit / Hot-Potato Routing** (hand the packet off to the next AS as quickly as possible rather than carrying it further internally). This strategy tends to create **asymmetric routes** (different paths for the two directions of a conversation).
- BGP routers communicate over **TCP connections** for reliable, hidden-network-detail communication.

## 11. IPv6 (Internet Protocol version 6)
The next-generation IP designed to replace IPv4, chiefly to solve **IPv4 address exhaustion** using **128-bit addresses** (vs. IPv4's 32-bit), while also improving security, configuration simplicity, and efficiency.

### Advantages of IPv6
1. **Vast address space** — enough for all future connected devices.
2. **Improved network efficiency** — simplified header, more efficient routing.
3. **Enhanced security** — **IPsec** built in as a standard feature.
4. **Autoconfiguration** — reduces manual/DHCP-dependent setup.
5. **Support for emerging technologies** — IoT, mobile networks, etc.
6. **Multicast efficiency** — better multicast support.

### Disadvantages of IPv6
1. **Transition complexity** from IPv4.
2. **Compatibility issues** with older devices/software.
3. **Lack of immediate incentive** while IPv4 + NAT still "work."
4. **Learning curve** for network admins.
5. **New security challenges** as adoption grows.

### IPv6 Header Fields
| Field | Size | Meaning |
|---|---|---|
| **Version** | 4 bits | Always `6` (`0110`) for IPv6 |
| **Traffic Class (Differentiated Services)** | 8 bits | Priority/class of the packet — helps routers decide what to drop first under congestion; distinguishes congestion-controlled traffic (0–7) from uncontrolled traffic like audio/video (8–15, given higher priority) |
| **Flow Label** | 20 bits | Labels packets of the *same flow* so routers can give them special/consistent handling (e.g., QoS); set to 0 if unsupported/default handling |
| **Payload Length** | 16 bits | Size of the payload (data + extension headers); if payload > 65,535 bytes, this is set to 0 and a **jumbogram** option is used instead |
| **Next Header** | 8 bits | Identifies the type of the next extension header, or the upper-layer protocol (TCP=6, UDP=17, ICMPv6=58, etc.) |
| **Hop Limit** | 8 bits | Same role as IPv4's **TTL** — decremented at each hop; packet discarded when it hits 0 (max 255 hops) |
| **Source Address** | 128 bits | Sender's IPv6 address |
| **Destination Address** | 128 bits | Recipient's IPv6 address |

### IPv6 Extension Headers (optional, in preferred order if multiple present)
1. **Hop-by-Hop Options Header** — options examined by *every* router on the path (e.g., router alert, jumbogram size, multicast listener discovery).
2. **Routing Header** — specifies the route the packet should take (e.g., Type 0 = strict source route, Type 2 = loose source route).
3. **Fragmentation Header** — used because in IPv6, **only the sender** fragments packets (routers do not); carries reassembly info.
4. **Authentication Header (AH)** — provides data integrity + authentication (ensures the packet wasn't modified and confirms sender identity).
5. **Encapsulating Security Payload (ESP)** — provides encryption, confidentiality, and authentication for the payload.
6. **Destination Options Header** — carries options meant only for the final destination node (can appear twice: before Routing Header, examined by all listed intermediate nodes; or just before the upper layer, examined only by the destination).
7. **Mobility Headers** — support Mobile IPv6 for devices that move between networks while keeping the same IP.

---

# UNIT 5 — The Internet Transport Protocols

## 1. Overview
The Internet has two complementary main transport-layer protocols:
- **UDP (User Datagram Protocol):** Connectionless — does almost nothing beyond delivering packets to the right application; apps must build their own reliability if needed.
- **TCP (Transmission Control Protocol):** Connection-oriented — handles connection setup, reliability (retransmission), flow control, and congestion control on the application's behalf.

## 2. UDP (User Datagram Protocol)
A **connectionless transport protocol** — sends encapsulated IP datagrams without setting up a connection first.

### UDP Segment Structure
An **8-byte header** followed by the payload:
- **Source Port & Destination Port:** identify the sending/receiving process/application on each machine; without them, the transport layer wouldn't know which app should get an incoming packet. The Source Port lets a reply be addressed back correctly.
- **UDP Length field:** total length (header + data); minimum 8 bytes, maximum 65,515 bytes (bounded by the IP packet size limit).
- **Checksum (optional):** covers the header, data, and a conceptual **pseudoheader**; if the checksum isn't computed, the field is set to all-zero/all-one (due to one's-complement arithmetic quirks).
- **Pseudoheader:** contains source/destination IPv4 addresses, the UDP protocol number (17), and the segment's byte count — included in the checksum to catch **misdelivered packets**, even though this technically breaks strict layering (IP info being used inside a UDP-layer computation). TCP uses an analogous pseudoheader.

### What UDP Does NOT Do
No flow control, no congestion control, no automatic retransmission of lost/bad segments — all left to the application. UDP's only real jobs: **demultiplexing** via ports, and **optional end-to-end error detection** via the checksum.

## 3. TCP (Transmission Control Protocol)
Designed to provide a **reliable, end-to-end byte stream** over an unreliable internetwork (which may have wildly varying topology, bandwidth, delay, and packet sizes across its parts). Each machine runs a **TCP transport entity** (in the kernel, a library, or a user process) that breaks application data into pieces (max 64 KB in theory, often ~1460 bytes in practice to fit one Ethernet frame) and sends each as a separate IP datagram; it also reassembles incoming datagrams back into the byte stream.

## 4. TCP Service Model
- **Socket:** An endpoint for communication, identified by an **(IP address, port)** pair. A **port** is TCP's name for a **TSAP (Transport Service Access Point)**.
- Connections are identified by the pair **(socket1, socket2)** — no separate virtual-circuit numbers are used.
- **Well-Known Ports:** ports below 1024, reserved for standard services, usually startable only by privileged users (e.g., port 143 for IMAP).
- **Registered/Ephemeral Ports:** 1024–49151, can be registered with **IANA**, though apps often just pick their own (e.g., BitTorrent unofficially uses 6881–6887).
- **inetd (Internet daemon):** a UNIX process that listens on multiple ports and only forks off the actual service daemon **when there's an incoming connection**, so idle daemons don't consume resources.
- **Full Duplex, Point-to-Point:** every TCP connection carries traffic in both directions simultaneously, and connects exactly two endpoints (no TCP multicast/broadcast).
- **Byte Stream, not Message Stream:** TCP does not preserve the original write-boundary structure of the data — the same data can arrive at the receiver split/merged into a totally different chunking pattern than it was sent in; TCP treats data purely as an undifferentiated sequence of bytes.
- **PUSH flag:** an application can request TCP not delay/buffer transmission (implementation-specific mechanisms exist, e.g., `TCP_NODELAY`), for cases needing immediate delivery (e.g., interactive game updates).
- **Urgent Data (URGENT flag):** lets an application flag high-priority data (e.g., a break signal like Ctrl-C) for immediate transmission and interrupt-style delivery at the receiver; largely fallen into disuse due to inconsistent implementations.

## 5. TCP Protocol Mechanics
- Every byte on a TCP connection has its own **32-bit sequence number**.
- A **TCP segment** = fixed 20-byte header + optional options + 0 or more data bytes.
- **Segment size limits:**
  1. Must fit within the 65,515-byte IP payload limit.
  2. Must fit within each link's **MTU** (typically 1500 bytes for Ethernet) to avoid IP-level fragmentation, which degrades performance.
- TCP performs **Path MTU Discovery** (using ICMP messages) to size segments so as to avoid fragmentation altogether.
- Basic mechanism: **sliding window protocol with a dynamic window size**. Sender starts a timer per sent segment; the receiver ACKs with the next expected sequence number plus its remaining window size; if the timer expires before an ACK, the segment is retransmitted.

## 6. TCP Segment Header — Field-by-Field
| Field | Size | Purpose |
|---|---|---|
| **Source Port / Destination Port** | 16 bits each | Identify local endpoints of the connection; together with IPs, form the connection's **5-tuple** (protocol, src IP, src port, dst IP, dst port) |
| **Sequence Number** | 32 bits | The byte-offset of the first data byte in this segment |
| **Acknowledgement Number** | 32 bits | The next **in-order** byte expected (a **cumulative acknowledgement** — doesn't reflect out-of-order data received beyond a gap) |
| **TCP Header Length** | 4 bits | Number of 32-bit words in the header (needed because Options is variable-length) |
| **(Reserved)** | 4 bits | Unused (historically) |
| **CWR / ECE** | 1 bit each | Congestion signaling for **ECN (Explicit Congestion Notification)**: **ECE** tells the sender to slow down (receiver got a congestion signal); **CWR** tells the receiver the sender has already slowed down |
| **URG** | 1 bit | Indicates the **Urgent pointer** field is valid |
| **ACK** | 1 bit | Indicates the Acknowledgement Number field is valid (set on nearly all packets) |
| **PSH** | 1 bit | Requests the receiver deliver data to the application immediately, without buffering |
| **RST** | 1 bit | Abruptly resets a confused/invalid connection, or rejects a connection attempt |
| **SYN** | 1 bit | Used to establish connections (SYN=1,ACK=0 = connection request; SYN=1,ACK=1 = connection accepted) |
| **FIN** | 1 bit | Sender has no more data to send (used to release the connection) |
| **Window Size** | 16 bits | How many bytes may be sent starting from the acknowledged byte — implements flow control (a window of 0 means "stop sending for now") |
| **Checksum** | 16 bits | Covers header, data, and a TCP pseudoheader (protocol number 6); **mandatory** in TCP (unlike UDP) |
| **Urgent Pointer** | 16 bits | Byte offset (from current sequence number) marking where urgent data is |
| **Options** | variable | Extra negotiable facilities (see below), Type-Length-Value encoded, up to 40 bytes |

### Key TCP Options
- **MSS (Maximum Segment Size):** each side announces the largest segment it's willing to accept during connection setup; default 536 bytes if unspecified (so all hosts must accept at least 536+20=556-byte segments).
- **Window Scale Option:** negotiates a scale factor to shift the 16-bit Window field up to 14 bits left, allowing windows up to 2³⁰ bytes — needed for high-bandwidth, high-delay ("long fat") links where a 64 KB window would leave the sender idle most of the time waiting for ACKs.
- **Timestamp Option:** carries a sender timestamp echoed by the receiver, used to compute round-trip time samples and to extend the sequence number logically (avoiding wraparound confusion via **PAWS – Protection Against Wrapped Sequence numbers**).
- **SACK (Selective Acknowledgement):** lets the receiver report the specific ranges of sequence numbers it has actually received (beyond just the cumulative ACK number), so the sender knows precisely what to retransmit after a loss.

## 7. TCP Connection Establishment — The Three-Way Handshake
1. **Client → Server:** SYN segment (SYN=1, ACK=0), specifying max segment size and optional data.
2. **Server → Client:** SYN+ACK segment, if a process is listening on that port and accepts.
3. **Client → Server:** final ACK — connection is now **ESTABLISHED**.
- If no process is listening, the server replies with **RST** to reject.
- **Simultaneous Open:** if both sides try to connect to each other at the same moment, only **one** connection results (identified by the endpoint pair), not two.
- Initial sequence numbers are chosen to **cycle slowly** (not a fixed value like 0), to protect against delayed duplicate packets from earlier connections.
- **SYN Flood Attack:** a malicious sender floods a host with SYN segments, never completing the handshake, exhausting the listener's memory (which must remember sequence numbers for half-open connections).
  - **SYN Cookies (defense):** instead of remembering the sequence number, the host derives it via a cryptographic function of known inputs (peer's IP/port + a local secret) and can **regenerate and verify it later** without having stored anything, defeating the resource-exhaustion attack (though this loses support for some TCP options).

## 8. TCP Connection Release
Each TCP connection is conceptually a **pair of simplex connections**, released independently.
- Either side sends a **FIN** segment (no more data to send); once ACKed, that direction is shut. Data can still flow in the *other* direction.
- Connection fully closes once **both directions** are shut. Normally requires 4 segments (FIN + ACK each way), but the first ACK and second FIN can sometimes combine, reducing it to 3.
- **Simultaneous FINs:** both sides can send FIN at the same time — handled just like sequential release.
- **Two-Army Problem:** the theoretical impossibility of a perfectly reliable close protocol — solved practically using **timers**: if no response to a FIN arrives within **two maximum packet lifetimes**, the connection is released anyway.

### TCP Connection States (Finite State Machine)
11 states total, e.g. **CLOSED**, **LISTEN**, **SYN SENT**, **SYN RCVD**, **ESTABLISHED**, **FIN WAIT 1/2**, **CLOSE WAIT**, **LAST ACK**, **CLOSING**, **TIME WAIT**. A connection begins in **CLOSED**, moves through setup states to **ESTABLISHED**, and through teardown states back to **CLOSED**. Each transition is triggered by an event (system call like CONNECT/LISTEN/SEND/CLOSE, an arriving segment type, or a timeout) and results in an action (sending a control segment, or nothing) — client-side and server-side paths through the diagram differ (**active open** vs. **passive open/LISTEN**).

## 9. TCP Timer Management
- **RTO (Retransmission TimeOut):** the timer started per sent segment; if it expires before the ACK arrives, the segment is retransmitted.
  - **SRTT (Smoothed Round-Trip Time):** an **EWMA (Exponentially Weighted Moving Average)** estimate of round-trip time, updated as: `SRTT = α·SRTT + (1−α)·R` (typically α = 7/8), where R is a newly measured round-trip sample.
  - **RTTVAR (Round-Trip Time Variation):** an EWMA of the *variability* in RTT: `RTTVAR = β·RTTVAR + (1−β)·|SRTT − R|` (typically β = 3/4) — proposed by **Jacobson** to make the timeout adapt to variance, not just the mean.
  - **RTO formula:** `RTO = SRTT + 4×RTTVAR` — chosen because multiplying by 4 is a cheap bit-shift and covers >99% of late packets. RTO also has a minimum floor of **1 second** to avoid spurious retransmissions.
  - **Karn's Algorithm:** don't use round-trip samples from **retransmitted** segments to update SRTT/RTTVAR (since it's ambiguous whether the ACK corresponds to the original or the retransmission); also **double the timeout** on each successive retransmission until the segment gets through.
- **Persistence Timer:** prevents a deadlock where a receiver sends a 0-size window (telling sender to wait) and the later window-update packet is lost — both sides then wait forever. When this timer fires, the sender sends a **probe** to check the current window size; repeats if still 0.
- **Keepalive Timer:** after a long idle period, checks whether the other side of the connection is still present; terminates the connection if there's no response (controversial, since it adds overhead and might kill a still-healthy but temporarily-partitioned connection).
- **TIME WAIT Timer:** runs for **twice the maximum packet lifetime** after both directions close, to guarantee any lingering packets from the old connection have fully died off before the connection record is deleted.

---

## Quick Cross-Unit Glossary (Frequently Confused Terms)

| Term | One-line meaning |
|---|---|
| **Latency vs. Jitter** | Latency = one-way delay; Jitter = *variation* in that delay across packets |
| **Flow Control vs. Congestion Control** | Flow control = protecting the *receiver* from being overwhelmed; congestion control = protecting the *network* from being overwhelmed |
| **Leaky Bucket vs. Token Bucket** | Leaky bucket forces a strictly constant output rate (no bursts, discards excess data); token bucket allows saved-up bursts (discards excess tokens, not data) |
| **IntServ vs. DiffServ** | IntServ = per-flow reservation (RSVP), doesn't scale; DiffServ = per-class marking (DSCP), scales well |
| **Datagram vs. Virtual-Circuit Subnet** | Datagram = connectionless, each packet routed independently; Virtual Circuit = connection-oriented, one fixed route set up in advance and reused |
| **Routing vs. Forwarding** | Routing = deciding which routes to use (building tables); Forwarding = looking up the outgoing line for an arriving packet |
| **Distance Vector vs. Link State Routing** | Distance vector shares full distance tables with neighbors only (slow convergence, count-to-infinity); link state floods link info to *all* routers and each runs Dijkstra locally (faster convergence, more memory/compute) |
| **Broadcast vs. Multicast Routing** | Broadcast = send to *everyone*; Multicast = send to a *defined subset (group)* only, typically via a pruned spanning tree |
| **OSPF vs. BGP** | OSPF = intradomain (inside one AS), link-state; BGP = interdomain (between ASes), path-vector, policy-driven |
| **TCP vs. UDP** | TCP = connection-oriented, reliable, ordered, flow/congestion-controlled; UDP = connectionless, best-effort, minimal overhead |
| **ARP vs. DHCP** | ARP maps IP→MAC address; DHCP assigns an IP address (and config) to a host in the first place |
| **Fragmentation (IPv4) vs. IPv6** | IPv4 allows routers to fragment in-flight; IPv6 requires the *sender* to fragment (uses PMTUD to avoid it) |
