# Lamport Ticket Reservation Server
A ticket reservation system for a movie theatre with fault-tolerance using Lamport's Mutex Algorithm.

## Details
A client program takes the commands (requests) from the user and communicates with servers using sockets. The system behaves correctly in presence of multiple concurrent client and ensures that the reservations are identical at all servers.

Any update to the system is done in a mutually exclusive fashion using Lamport’s algorithm. Lamport requires the messages between servers be delivered in FIFO order so we use TCP protocol. We assume:
* The system has “perfect” failure detection, i.e., a server does not respond in the timeout interval if and only if it has crashed
* There is at least one server is available (although not necessarily the same server always)

When a server comes up again, it synchronizes with existing servers to ensure the consistency of the data.

## Usage
The movie theatre has c total seats and there are n servers that keep the current reservation of the seats. Accepts the following requests from a client:
```
reserve <name> <count> 
```
If sucessful, client gets assigned seat numbers. Search for confirmed reservation:
```
search <name>
```
Free up the seats assigned a name:
```
delete <name>
```

### Note:
Each server and client is started in a seperate JVM process, not just seperate threads.

