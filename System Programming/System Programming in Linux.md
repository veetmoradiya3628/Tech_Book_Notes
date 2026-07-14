
- File descriptor (fd)
- Stream sockets
	- TCP - Transmission control protocol
	- ssh, http, telnet etc
- Datagram sockets
	- UDP - User Datagram protocol
	- tftp, dhcp, multiplayer game, streaming audio etc
	- speed
- Layer model
	- Application Layer - telnet, ftp etc
	- Host-to-host transport layer - TCP, UDP
	- Internet layer - IP and routing
	- Network Access Layer - Ethernet, wifi etc
- IPv4 
- IPv6
- Subnets
- Port
	- 16 bit number
- Big endian - Network byte order
- Little endian
- htons - host to network short
- htonl - host to network long
- host byte order - computer stores and process the data in this format and it actually  depends on the system and processer we are using
- struct addrinfo - prep the socket address structures, its linked list node
- struct sockaddr - stores socket address information for many types of sockets
- struct sockaddr_in
- NAT - network address translation
- Private IP ranges - 10.x.x.x , 192.168.x.x or 172.y.x.x y range is between 16 and 31

- System calls
getaddrinfo() & freeaddrinfo()
	- Allocates a linked list of `addrinfo` structures matching your criteria.
	- Before you can open a connection, you need to turn human-readable strings (like `"google.com"` or port `"80"`) into networking structures the kernel understands. In modern code, this handles both IPv4 and IPv6 transparently.

socket()
	- standard file descriptor (like a file handle), except instead of reading/writing to a hard drive, you are reading/writing to a network card buffer.
	- Requests a new endpoint descriptor from the operating system.

Server branch - `bind()`, `listen()`, & `accept()`
- **`bind()`** pegs your socket descriptor to a specific port on your machine
- **`listen()`** puts the port into "passive mode," allowing incoming connections to line up in a queue.
- **`accept()`** is where the magic happens. It blocks (pauses) until a client knocks on the door. It then splits off a **brand new socket descriptor** explicitly for that one client, leaving the original socket free to keep listening.

 Client branch - connect()
- Connects your open socket descriptor to a target server address.
- Clients do not typically call `bind()`. The kernel automatically assigns a random, unused local port to your client application during the `connect()` call.

send() & recv()
- **`send()`** pushes data out to the network card buffer. Crucially, **it might not send all requested bytes at once** if the buffer fills up. It returns the exact count it managed to send.
- **`recv()`** reads data out of the incoming network card buffer. It returns the number of bytes read, `-1` on error, or **`0` if the remote side gracefully closed the connection.
- **Note on Datagrams (`sendto()` / `recvfrom()`):** If you create a UDP socket (`SOCK_DGRAM`) instead of TCP, you skip `connect()` and `accept()` entirely. Because there is no persistent open pipe, you must explicitly supply the recipient's address structure on every single packet via `sendto()`, and gather the sender's origin information via `recvfrom()`.

`close()` & `shutdown()`
- `close(sockfd)` shuts down the descriptor completely, preventing further read/write actions, and frees up the descriptor number in the OS. In C++ environments, this should routinely reside in your socket container's destructor.
- `shutdown(sockfd, how)` provides surgical precision. If you want to say _"I am done sending my HTTP request, but I want to leave my socket awake to read the response,"_ you can pass `SHUT_WR` (`1`) to close down only the transmission pipeline. You still must call `close()` when completely finished.

`getpeername()` & `gethostname()`
- `getpeername()` extracts the structural IP/port data of whatever device is connected to the other side of your socket.
- `gethostname()` extracts your own machine's network identifier string, perfect for feeding straight back into `getaddrinfo()`
