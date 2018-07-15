# Socket-Programming-C

A socket is the mechanism that most popular operating systems provide to give programs access to the network. It allows messages to be sent and received between applications (unrelated processes) on different networked machines.

Sockets are the low level endpoint used for processing information across a network. common networking protocols like HTTP, and FTP rely on  sockets underneath to make connections.

The sockets mechanism has been created to be independent of any specific type of network. IP, however, is by far the most dominant network and the most popular use of sockets. This tutorial provides an introduction to using sockets over the IP network (IPv4).

# Topics TCP/IP sockets
There are a few steps involved in using sockets:

- Create the socket.
- Identify the socket.
- On the server, wait for an incoming connection.
- On the client, connect to the server's socket.
- Send and receive messages.
- Close the socket.

### Client Socket Workflow

* Create a socket using the socket() function;
* Connect the socket to the address of the server using the connect() function;
* Send and receive data by means of the read() and write() functions.

> socket() ------->  connect() -------> recv()

### Server Socket Workflow

* Create a socket with the socket() function;
* Bind the socket to an address using the bind() function;
* Listen for connections with the listen() function;
* Accept a connection with the accept() function system call. This call typically blocks until a client connects with the server.
* Send and receive data by means of send() and receive().

> socket() ------->  bind() -------> listen() -------> accept()

| Function  | Description |
| ------------- | ------------- |
| socket()  | Specifying the type of communication protocol (TCP based on IPv4, TCP based on IPv6, UDP). Where family specifies the protocol family (AF_INET for the IPv4 protocols), type is a constant described the type of socket (SOCK_STREAM for stream sockets and SOCK_DGRAM for datagram sockets.The function returns a non-negative integer number, similar to a file descriptor, that we define socket descriptor or -1 on error. |

| Content Cell  | Content Cell  |

## HTTP Client 

```
#include <stdio.h>
#include <stdlib.h>

#include <sys/socket.h>
#include <sys/types.h>

#include <netinet/in.h>
#include <arpa/inet.h>

int main(int argc, char *argv[]) {

	char *address;
	address = argv[1];

	int client_socket;
	client_socket = socket(AF_INET, SOCK_STREAM, 0);

	// connect to an address
	struct sockaddr_in remote_address;
	remote_address.sin_family = AF_INET;
	remote_address.sin_port = htons(80);
	inet_aton(address, &remote_address.sin_addr.s_addr);

	connect(client_socket, (struct sockaddr *) &remote_address, sizeof(remote_address));

	char request[] = "GET / HTTP/1.1\r\n\r\n";
	char response[4096];

	send(client_socket, request, sizeof(request), 0);
	recv(client_socket, &response, sizeof(response), 0);

	printf("response from the server: %s\n", response);
	close(client_socket);

	return 0;


}

```
## HTTP Server

```
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#include <sys/socket.h>
#include <sys/types.h>

#include <netinet/in.h>

int main() {

	// open a file to serve
	FILE *html_data;
	html_data =  fopen("index.html", "r");

	char response_data[1024];
	fgets(response_data, 1024, html_data);

	char http_header[2048] = "HTTP/1.1 200 OK\r\n\n";
	strcat(http_header, response_data);

	// create a socket 
	int server_socket;
	server_socket = socket(AF_INET, SOCK_STREAM, 0);

	
	// define the address
	struct sockaddr_in server_address;
	server_address.sin_family = AF_INET;
	server_address.sin_port = htons(8001);
	server_address.sin_addr.s_addr = INADDR_ANY;

	bind(server_socket, (struct sockaddr *) &server_address, sizeof(server_address));

	listen(server_socket, 5);

	int client_socket;
	while(1) {
		client_socket = accept(server_socket, NULL, NULL);
		send(client_socket, http_header, sizeof(http_header), 0);
		close(client_socket);
	}
	
	return 0;
}

```

## TCP Client

```
#include <stdio.h>
#include <stdlib.h>
#include <sys/types.h>
#include <sys/socket.h>

#include <netinet/in.h>
int main() {
	
	char request[256] = "Test Data";
	
	
	// create the socket
	int sock;
	sock = socket(AF_INET, SOCK_STREAM, 0);
	
	//setup an address
	struct sockaddr_in server_address;
	server_address.sin_family = AF_INET;
	server_address.sin_addr.s_addr = INADDR_ANY;
	server_address.sin_port = htons(3001);

	connect(sock, (struct sockaddr *) &server_address, sizeof(server_address));
	
	send(sock, request, sizeof(request), 0);
	
	//recv(sock, &buf, sizeof(buf), 0);
	
	printf("\n %s \n", buf);
	close(sock);

	return 0;
}

```

## TCP Server

```
#include <stdio.h>
#include <stdlib.h>

#include <sys/socket.h>
#include <sys/types.h>

#include <netinet/in.h>

int main() {

	char server_message[256] = "You have reached the server!";
	
	// create the server socket
	int server_socket;
	server_socket = socket(AF_INET, SOCK_STREAM, 0);

	
	// define the server address
	struct sockaddr_in server_address;
	server_address.sin_family = AF_INET;
	server_address.sin_port = htons(9002);
	server_address.sin_addr.s_addr = INADDR_ANY;

	// bind the socket to our specified IP and port
	bind(server_socket, (struct sockaddr*) &server_address, sizeof(server_address));

	listen(server_socket, 5);

	int client_socket;
	client_socket = accept(server_socket, NULL, NULL);
	
	// send the message
	send(client_socket, server_message, sizeof(server_message), 0);

	// close the socket
	close(server_socket);
	
	return 0;
}
```
