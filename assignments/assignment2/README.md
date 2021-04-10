# Project#2: Routing and Forwarding

Through this assignment, you will realize the concepts of routing and forwarding - the
main function of a router.  You will program a router to deliver information to the right
destination in a network with other nodes based on the destination IP address.

This is a group project up to two members. **You are not allowed to copy or look at code
from other groups.** However, you are welcome to discuss the assignments with
other students without sharing code.

The programming assignments are designed to be challenging but manageable in
the time allotted. If you have questions, want to suggest clarifications, or
are struggling with any of the assignments this semester, please come to office
hours, ask questions on Discussions forum, or talk to the instructor before or after class.

## Router Operations

As discussed in lectures, the main functions of a router are routing and forwarding.
You will write (1) 2 pairs of TCP client and server programs for
sending and receiving text messages over the Internet, and (2) a router program for
processing the incoming packets and determining the respective destination. You are
provided with the instructor's sample solutions for Project 1 in C, Python, and Go
languages, which you may use and modify to complete Project #2.

The client, server, and router programs should meet the following specifications. Be
sure to read these meticulously before and after programming to make sure your
implementation fulfills them:

## Client specification

* Each client program should contact a server through a router. Here, you may consider
  the router as the gateway.
* Each client program should read a message from stdin (standard input, i.e., keyboard),
  send the message, wait for the server to send the message back, and exit.
* Each client should read and send the message *exactly* as it appears in stdin
  until reaching an EOF (end-of-file).
* Each client should take four command-line arguments: the IP address of the
  server, the port number of the server, the IP address of the router, and the port
  number of the router.
**NOTE:** in essence, the router does not carry its own port numbers. Because we are
  simulating the function of a hardware router through our router program, we need to
  provide the port numbers for the router process.
* For the client socket, it should only connect to the router using the router's IP
  address and port number. <ins>The server's IP address and port number are embedded in
  the message sent out to the router</ins>.
* Each client should gracefully handle error values potentially returned by
  socket programming library functions.

## Server specification

* Each server program should listen on a socket, wait for a client to connect,
  receive a message from the client, print the message to stdout (standard output,
  i.e., the screen), and then wait for the next client **indefinitely**.
* Each server should take three command-line arguments: the port number to listen
  on for client connections, the gateway's IP address and port number.
**NOTE:** in essence, the router does not carry its own port numbers. Because we are
  simulating the function of a hardware router through our router program, we need to
  provide the port numbers for the router process.
**NOTE:** for simplicity, we make the clients, servers, and router all run on the same
  machine, thus the same IP address is used among all of them. As presented in class,
  the routers in real world would connect multiple networks and carry multiple IP
  addresses.
* Each server should accept and process client communications in an infinite
  loop, allowing multiple clients to send messages to the same server. The
  server should only exit in response to an external signal (e.g. SIGINT from
  pressing `ctrl-c`).
**NOTE:** similar to the client, the server socket should only receive the packet
  from the router and send the packet to the router.
* Each server should maintain a short (5-10) client queue and handle multiple
  client connection attempts sequentially. In real applications, a TCP server
  would fork a new process to handle each client connection concurrently, but
  that is **not necessary** for this prroject.
* Each server should gracefully handle error values potentially returned by
  socket programming library functions (see specifics for each language below).
  Errors related to handling client connections should not cause the server to
  exit after handling the error; all others should.

## Router specification

* As mention earlier, in this project, we make the client, server, and router all
  run on the same machine with the same IP address for simpli*city.
* The router, in a sense very similar to the server, should listen on a socket, wait
  for the connection from either a client or server, and receive the message.
* The router should take one command-line argument: the port number to listen
  on for connections.
* The router should carry a forwarding table (hard-coded in the program), which shoud
  cover the client and server networks.
**NOTE:** for simplicity, we do not have dynamic forwarding table but just a
  hard-coded/static one.
* The router should open the message and check the destination IP address and port
  number.
* The router then creates a new socket and connects to the server. The same message
  received would be sent out to the server (the content is not changed).

## Programming language options

* You can use any one of the three languages to complete this project: C, Python, Go.
* Only submit the project written in ONE language.

## Grading rubrics

* 20 pts: Client can successfully send a message to the router. This message includes
  (client's IP address, port number, the server's IP address, and port number)
* 20 pts: Router can successfully forward the received message from the client to
  the server. Note: new socket should be created, and the message received should be
  checked but NOT modified.
* 10 pts: The server verifies the destination IP address and port number <ins>IN THE
  MESSAGE</ins>. If both of them are correct, prints out the message; otherwise, prints
  out an ERROR message.
* 10 pts: a report. This report should include, but not limited to, (1) team members,
  (2) task distribution - who did what, (3) challenges encountered, (4) how you tackled
  the challenges, (5) the resources used to do the project and solve the problems, (6)
  lessons learned from this project, (7) anything you think can improve the project
  (or other fun ideas you would like to implement in the project).
* +5 pts: implement the forwarding table in the router
* +5 pts: add MAC addresses to all devices, and add the mechanism to change the
  MAC addresses with respect to the links
* +5 pts: once the server receives the message from the client, sends a response message
  back by adding a prefix "[SERVER RECEIVED]: " to the message. Note: don't forget that
  in the response message, the sender is now the server and the receiver is the client.
  The server would get the client's IP addrress and port number from the received message.
* -25 pts: code that does not compile and run is graded harshly; if you want partial credits,
*make sure your programs compile and run!*
