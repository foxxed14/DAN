 (in progress)
# What is DAN ?

## DAN (Dynamic Adaptive Network) is a protocol for organizing access to a transmission medium based on dynamic TDMA, allowing nodes in the network to automatically rebuild when the number of devices changes. 

The essence of the idea:
We divide the general frame (for example, 100 ms) into N+1N+1 slots, where NN is the current number of nodes, and "+1" is a reserved (guard) slot.
Each node gets its own slot for transmission (broadcast). In this slot, it sends its data (coordinates, service information, etc.).
Guard slot (silence period) is used for new nodes that want to "join" the network. If a new node hears on the air that NN nodes, then it waits for this "quiet slot" and transmits a "join request".
The remaining nodes (having received a join request) increase NN by 1, and, starting from the next frame, all are rebuilt: now the frame is divided into (N+1)+1=N+2(N+1)+1=N+2 slots, and the new node gets “its” slot and can transmit.

Thus, the system automatically adapts when the number of nodes in the network increases/decreases.

 # 2. Differences between DAN and classical schemes
    2.1. Comparison with fixed TDMA
    Fixed TDMA (Static TDMA): usually we know the number of nodes NN in advance and divide the frame into NN (or N+kN+k) slots.
    Problem: if the number of nodes changes, manual reconfiguration is required. If there are fewer nodes, some slots are wasted; if there are more, there are not enough slots and collisions occur.
    DAN: slots are recalculated “on the fly” when adding/removing nodes, which provides flexibility.

    2.2. Comparison with CSMA/CA (or ALOHA)
    CSMA-like protocol (Carrier Sense Multiple Access) does not require slot allocation, nodes try to transmit, avoiding collisions using "listen before sending" methods.
    Problem: Under heavy load, the probability of collisions increases, there may be unpredictable delays.
    DAN: guaranteed no collisions (each node has its own clear slot), and at the same time new nodes are added automatically.
    
    2.3. Comparison with Master-Slave (centralized) systems
    Centralized (for example, one "master" issues a poll, "slaves" respond in turn).
    Here everything depends on the main node. If it disconnects, the network will collapse.
    DAN: decentralized. There is no obvious "master": each node "knows" NN and its number. When a new node appears, all nodes jointly increase NN.

# 3. Advantages of DAN

Decentralized logic: no leading node. Each node independently calculates which slot to transmit into.
No collisions: clear time slots exclude simultaneous transmissions if time synchronization is agreed upon.
Automatic adaptation to a new node: when a new module appears on the air, old nodes simply increase NN, and from the next frame everyone knows that there are now N+1N+1 slots.
Expandability: you can add/remove nodes without interrupting the network; the "guard slot" allows you to "wedge in" at the right moment.
Low airtime load: each node transmits strictly into its slot, there is no excess traffic.

# 4. Disadvantages of DAN

Accurate synchronization of all nodes in time is required. The TDMA approach always requires agreement on the "start of the frame", otherwise the slots will "slide" and collisions are possible. Either a periodic "beacon" message is needed, or external synchronization (UWB tags with precise timing), or GPS, etc. Reducing slot duration as nodes grow: if there are many nodes, TframeN+1N+1Tframe​ may become too short, perhaps the node will not have enough time to send a packet. You need to either increase the total TframeTframe​ frame, or set a limit on NN.
Guard slot may cause collisions if several new nodes try to connect at the same time. Usually, a small random delay is added so that they do not conflict (CSMA-like).
Removing nodes: if a node "left", you need to understand (the other nodes) that it is gone (by "silence" in the slot) and rebuild NN (perhaps you need a timer so as not to remove a node that just has a temporary failure).

# 5. Conclusion

DAN is a dynamic TDMA scheme that provides:

Automatic addition/removal of nodes to the network,
Decentralization (no master),
No collisions (each node has a slot),

But it also requires:

Time synchronization,
Deterioration of the "slot duration" with a large NN,
Special logic of the "guard slot" and removal of "missing" modules.

Thus, DAN is a flexible and effective solution for a number of tasks (UWB networks, robotics, IoT, tracking), especially if it is important to dynamically connect new nodes and at the same time maintain a TDMA approach without a centralized "master".
