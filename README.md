## Socket-Programing-C

A socket is the mechanism that most popular operating systems provide to give programs access to the network. It allows messages to be sent and received between applications (unrelated processes) on different networked machines.

Sockets are the low level endpoint used for processing information across a network. common networking protocols like HTTP, and FTP rely on  sockets underneath to make connections.

The sockets mechanism has been created to be independent of any specific type of network. IP, however, is by far the most dominant network and the most popular use of sockets. This tutorial provides an introduction to using sockets over the IP network (IPv4).

## Topics TCP/IP sockets
There are a few steps involved in using sockets:

- Create the socket.
- Identify the socket.
- On the server, wait for an incoming connection.
- On the client, connect to the server's socket.
- Send and receive messages.
- Close the socket.

## Prerequisites

- Basic Understanding of the C Programming language.
- Basic Knowledge of a Linux command line, editing text files on a linux system.

## Setup Enviornment

- Debian 7 Linux Machine or Windows System With GCC Compiler.
- Vim or Code Editor for Editing C Code.

## Client Socket Workflow

The client socket is created with a socket() call, and the connected to aremote address with the connect() call,  and then finally can retrieve data with the recv() call.

> socket() ------->  connect() -------> recv()

<a href="http://www.youtube.com/watch?feature=player_embedded&v=YOUTUBE_VIDEO_ID_HERE
" target="_blank"><img src="http://img.youtube.com/vi/YOUTUBE_VIDEO_ID_HERE/0.jpg" 
alt="IMAGE ALT TEXT HERE" width="240" height="180" border="10" /></a>

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
