
- Communication between processes over the network, or inter process communication (IPC) is at the heart of a distributed systems.
- OSI / IP layers in the networking

##### Reliable links
- TCP
	- connection lifecycle
	- TCP 3 way handshake
	- flow control
	- congestion control
- UDP - Non reliable

##### Secure Links
- TLS - transport layer security
	- encryption
	- integrity
	- TLS handshake (TLS 1.2 & TLS 1.3)
- CA certification

##### Discovery
- DNS (_Domain name system_)
	- TLD - top level domain
	- authoritative name server
	- TTL - time to live
	- DNS cache at different levels

#### APIs
- Application programming interface
- direct communication vs. indirect communication
- Direct communication
	- request-response model
	- IPC technologies
		- gRPC, REST, GraphQL
	- HTTP
		- stateless
		- REST - representational state transfer
		- Http methods (GET, POST, PUT, DELETE etc)
		- Http Response status codes (401, 403, 503, 200, 201 etc)
		- OpenAPI specification with swagger