## Socket-Programing-C

A socket is the mechanism that most popular operating systems provide to give programs access to the network. It allows messages to be sent and received between applications (unrelated processes) on different networked machines.

The sockets mechanism has been created to be independent of any specific type of network. IP, however, is by far the most dominant network and the most popular use of sockets. This tutorial provides an introduction to using sockets over the IP network (IPv4).

This tutorial will not try to cover the entire topic of sockets. There are tutorials on the web that delve into far greater detail. On-line manual pages will provide you with the latest information on acceptable parameters and functions. The interface described here is the system call interface provided by the OS X, Linux, and Solaris operating systems and is generally similar amongst all Unix/POSIX systems (as well as many other operating systems).

## Programming with TCP/IP sockets
There are a few steps involved in using sockets:

*Create the socket.
*Identify the socket.
*On the server, wait for an incoming connection.
*On the client, connect to the server's socket.
*Send and receive messages.
*Close the socket.
