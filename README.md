## Socket-Programing-C

A socket is the mechanism that most popular operating systems provide to give programs access to the network. It allows messages to be sent and received between applications (unrelated processes) on different networked machines.

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
