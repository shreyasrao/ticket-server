#Lamport Ticket Reservation Server
A ticket reservation system with fault-tolerance for a movie theatre using Lamport's Mutex Algorithm.

##Details
A client program takes the commands (requests) from the user and communicates with servers using sockets. When a client is started, it first reads a file with the IP addresses and port numbers of all servers and randomly connects to one of the servers. If the chosen server is not available (i.e., after the timeout), the client contacts some other server.

The movie theatre has c total seats and there are n servers that keep the current reservation of the seats. The reservation system accepts the following requests from a client:
    (a) reserve <name> <count> – reserves the number, count, of seats for the name.
        • If the theater does not have enough seats, then the request is rejected and the client shows this message: “Failed: only <# seats left> seats left but <count> seats are requested.”
        • If a reservation has been made using the name, then the client shows the message: “Failed: <name> has booked the following seats: <seat-number>,...,<seat-number>.” Here, the seat numbers are the numbers that are previously reserved for the name.
        • Otherwise, the specified seats are assigned to the name and the client shows the mes- sage: “The seats have been reversed for <name>: <seat-number>,...,<seat-number>.”
    (b) search <name> – returns the seat numbers that are reserved for the name. If no reservation is found for the name, then the client shows the message: “Failed: no reservation is made by <name>.”
    (c) delete <name> – frees up the seats that are assigned to the name. The clients shows the message: “<# seats released> seats have been released. <# seat left> seats are now available.” If no reservation is found for the name, the client shows the message: “Failed: no reservation is made by <name>.”

The system behaves correctly in presence of multiple concurrent clients and ensures that the reservation is identical at all servers; any update to the system is done in a mutually exclusive fashion using Lamport’s algorithm. Since Lamport’s algorithm requires the messages between servers are delivered in FIFO order, the implementation uses TCP protocol. All servers know the IP addresses of other servers; every server has a file that holds its IP address and port number as well as those of other servers.

The cluster of servers gives the user illusion of a single immortal server (so far as serving the requests are concerned). We assume that the system has “perfect” failure detection, i.e., a server does not respond in the timeout interval (5 seconds) if and only if it is crashed. We assume that there is at least one server is available. However, we DO NOT assume that it is the same server that is always up. When a server comes up again, it synchronizes with existing servers to ensure the consistency of the data.

###Note:
Each server and client is started in a seperate JVM process instead of a thread.

