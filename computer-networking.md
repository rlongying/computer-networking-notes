# Computer Networking

> How computers communicate with each other - A course ([The bits and bytes of computer networking - coursera](https://www.coursera.org/learn/computer-networking/home/welcome)) note 

### TCP/IP Five-Layer Network Model

<img src="computer-networking/images/image-20200531212326401.png" alt="image-20200531212326401" style="zoom:67%;" />

- Physical layer
  - represent the physical devices that interconnect computers
  - specifications for cable, sending signals
- Data Link
  - defining a common way of interpreting these signals so network devices can communicate
  - protocol: Ethernet - getting data to node on the same network or link
- Network
  - allow different devices to communicate with each other through devices known as routers
  - get data across networks, links
  - IP 
- Transport
  - sorts out which client or server to send or receive data
  - TCP - reliable, UDP - not reliable
- Application
  - application specific protocols

### The Basics of Networking Devices

#### Cables

- copper
  - electrical voltages - 0/1
  - Cat5, Cat5e, Cat6 - how twisted pairs are arranged inside them can affect how quickly data can be sent and how resistant these signals are to outside interference.
  - Crosstalk - when an electrical pulse on one wire is accidentally detected on another wire. 
  - cat5e, cat6 - more strict specifications to reduce crosstalk
  - cat6 -  more reliable and faster, but shorter distance  
- fiber
  - pulses of light - 0/1
  - resistant to electronmagnetic interference, faster, but fragile

#### Hubs and Switches

- single network - LAN

- hub - ```a physical layer device``` that allows for connections from many computers at once
  - broadcast
  - collision domain - a network segment where only one device can communicate at a time because electrical pulses will interfere with each other
- switch - `a data link layer device` that allows for connections from many computers at once
  - only send data to intended system

#### Routers

- a device that forward data between independent networks
- `a network layer device`
- home/office LAN - ISP (core router) - internet
- BGP(*Border Gateway Protocol*) - routers share data with each other via this protocol, which lets them learn about most optimal paths to forward traffic

#### Servers and Clients

- nodes - devices that can communicate with each other
- server - provide data (a node or an applicaiton inside a node)
- client - receive data
- a node can be a server or a client or both, eg. email server is a server but also a client to DNS server

### The Physical Layer

#### Moving Bits Across the Wire

- sending 0/1 bits
- modulation / line coding - a way of varying the voltage of this charge moving across the cable<img src="computer-networking/images/image-20200531225656715.png" alt="image-20200531225656715" style="zoom:50%;" />

#### Twisted Pair Cabling and Duplexing

- pairs of twisted copper wires 
- cat 6 - standard 4 twisted pair per jacket 
- duplex communication - information can flow in both directions across the cable
  - reserving some pairs for one direction and some pairs for another
  - full-duplex - simultanneous bidirectional communicaiton
  - half-duplex - take turns
- simplex - unidirectinoal

#### Network Ports and Patch Panels

- most common - RJ-45 (*Registered jack 45*) <img src="computer-networking/images/image-20200531230514651.png" alt="image-20200531230514651" style="zoom:33%;" /> <img src="computer-networking/images/image-20200531230815225.png" alt="image-20200531230815225" style="zoom:33%;" />
- Network ports - are generally directly attached to the devices that make up a computer network
- Patch Panel - a device that only has many network ports



### The Data Link Layer

> Abstract away the need for any other layers to care about the physical layer and what hardware is in use

#### Ethernet and MAC Address

- CSMA/CD - used to determine when the communications channels are clear, and when a device is free to transmit data
  - a node can send data if it detects no devices are sending data 
  - wait random intervals if more than one devices are sending data, which resulted in a collision
- MAC (`media access control`) address 
  - global unique identifier attached to an individual network interface
  - 48-bit number -  6 groupings of 2 hex
  - two sections - OUI + Vendor Assigned, <img src="computer-networking/images/image-20200531232449663.png" alt="image-20200531232449663" style="zoom:50%;" />
  - Ethernet uses MAC to specify sender and receiver

#### Unicast, Multicast, and BroadCast

- a `unicast` transmission is alwasy meant for just one receiving address
  - if th least significant bit in the first octet of a destination address is set to <u>zero</u>, it means that ethernet frame is intended for only the destination address
  - e.g. 00:01:44:55:66:77 -> `00` which is `0000 0000`, the rightmost bit is `0`
- `multicast` - send to all devices BUT will be accpeted or discarded by each device depending on criteria from their own MAC address, eg. networks interfaces can be configured to accept a list of configured multicast addresses
  - if th least significant bit in the first octet of a destination address is set to <u>one</u>, it means that ethernet frame is intended for only the destination address
  - e.g.  01:00:CC:CC:DD:DD -> `01` is the first octet, convert it to binary `0000 0001` the rightmost bit is 1 
- `broadcast` - send to every single device on a LAN
  - ip broadcast address <img src="computer-networking/images/image-20200531234141940.png" alt="image-20200531234141940" style="zoom:50%;" /> (reverse mask = ~subnet mask)
  - broadcast mac address: FF:FF:FF:FF:FF:FF

#### Dissecting an Ethernet Frame

- `data packet` : an all-encompassing term that represents any single set of binary data being sent across a network link
- ethernet frames - data packet at ethernet level
  - <img src="computer-networking/images/image-20200531234641561.png" alt="image-20200531234641561" style="zoom:50%;" />
  - Preamble
    - first 7 bytes - act as a buffer between frames and can also be used to synchronize the internal clock the network interfaces use
    - last byte - SFD - start frame delimiter - signal to a reveving device that the preamble is over and actual frame contents will now follow
  - Destination MAC address 
  - Source MAC address
  - EtherType field 
    - VLAN (`Virtual LAN`) Tag
      - a technique that lets you have multiple logical LANs operating on the same physical equipment
      - indicate that the frame itself is a VLAN frame
      - will only be delivered out of a switch that configured to relay that specific tag
    - Ether-type - 16 bits long and used to describe the protocol of the contents of the frame
  - Payload
    - actual data - 46 to 1500 bytes long
    - all data from upper layers
  - Frame Check Sequence - a 4 byte number that represents a checksum value for the entire frame
    - CRC(`Cyclical Redundancy Check`)

### The Network Layer

> Allow data to cross many networks facilitating communications over great distances

#### IP Adress

- 32  bit  - 4 octet
- belong to networks not attached to devices
- DHCP - Dynamic Host Configuration Protocol - aumatically assign ip address

#### IP Datagrams and Encapsulation

- IP header datagram <img src="computer-networking/images/image-20200601221842126.png" alt="image-20200601221842126" style="zoom:50%;" />  <img src="computer-networking/images/image-20200601223501596.png" alt="image-20200601223501596" style="zoom:50%;" />
- Version - what version of internet protocol, e.g. IPv4
- Header Length - lenght of the header
  - almost always 20 bytes for IPv4
  - 20 bytes is the min length for a ip header
- Service Type - details about quality of service, Qos
- Total Length - total length the IP datagram
- Identification - group message together
  - max size of a single datagram is 2^16 - 1 = 65535 bytes
  - large amount of data will be divided into smaller packets to fit in the max size
  - packets with the same identification will be identified by the receiver as a part of the same transmission
- Flags
  - to indicate if the datagram is fragmented or not
  - Fragmentation - talking a single IP datagram and splitting it up into several smaller datagrams
- Fragment offset - help to put all fragments back together
- TTL - time to live - how many router hops a datagram can traverse before it's thrown away
- Protocol - transport layer protocol
- Header Checksum 
- Source IP Address
- Dest IP - Address
- Options - set special characteristics for datagrams primarily used for testing purposes
- Padding - a series of zeros

#### IP Address Classes

<img src="computer-networking/images/image-20200601224707342.png" alt="image-20200601224707342" style="zoom:50%;" />  *class c ends with 223* not 224

- Class A
  - first octet is the network ID, and last three are used for host ID
  - starts with 0
- Class B
  - first two octet are network ID
  - starts with 10
- Class C
  - first 2 octet are network ID
  - starts with 110
- Class D
  - starts with 1110
  - used for multicasting
- Class E
  - unassigned
  - testing 

#### Address Resolution Protocol - ARP

- discover the hardware address of a node with a certain IP address
- look at the local ARP table
-  if not found, send broadcast ARP message<img src="computer-networking/images/image-20200601225905306.png" alt="image-20200601225905306" style="zoom:33%;" />
-  receive the ARP response containing the MAC address<img src="computer-networking/images/image-20200601225905306.png" alt="image-20200601225905306" style="zoom:33%;" />

### Subnetting

#### Subnetting

- A gateway router - entry / exit of a network
- split a large network into smaller ones

#### Subnet Masks

- subnet ID - part of host ID
- 32 bits
- 9.100.100.100 - 0000 1001   0110 0100   0110 0100   0110 0100 -  IP address
- 255.255.255.0 - 1111 1111  1111 1111   1111 1111   0000 0000 - subnet mask - a subnet of 256 ip addresses, but normally 254 (1-254) hosts, as 0 is not used, and 255 is reserved for broadcast
- another notation, 9.100.100.100/24 - 24 means all first 24 bits are used as a 'network ID (Class A network ID + sub network ID)' - which indicates that the submask should have all first 24 bits of 1s and left are 0s
- a subnet mask: all `1s`  tells use what part we can ignore to compute a host ID, follows all `0s`tell us what to keep
- (class network + sub network) network ID  = (IP) & (Subnet mask)

#### CIDR - Classes Inter-Domain Routing

- Demarcation point - to describe where one network or system ends and another one begins
- CIDR notation:  9.100.100.100/24
- more flexiable network sizes <img src="computer-networking/images/image-20200601233914469.png" alt="image-20200601233914469" style="zoom:40%;" />

### Routing

#### Basic Routing Concepts

- router - a device with two network interfaces (because it connects with two networks) that forward traffic depending on the destination address
- <img src="computer-networking/images/image-20200601234409267.png" alt="image-20200601234409267" style="zoom:33%;" />
- <img src="computer-networking/images/image-20200601234515359.png" alt="image-20200601234515359" style="zoom:33%;" />
- A node network A 192.169.1.100 sends packet to  node B 10.0.0.10
  1. source MAC: MAC A ,  destination MAC: MAC of router, send packet to router
  2. router look at the routing table,  decrement the TTL 
  3. soruce MAC: MAC of router, destination MAC: MAC B, send packet to B

#### Routing Tables

- destination network - CIDR or IP with subnet masks
- Next hop - next router ip or state no additional hops needed
- Total hops - shortest path
- Interface - which interface to go out

#### Interior Gateway Protocols

- routers share information within a single autonomous system
  - autonomous system: a collection of networks that all fall under the control of a single network operator
- distance-vector protocol
  - a router sends its routing table to every neighbouring router
  - <img src="computer-networking/images/image-20200602142354277.png" alt="image-20200602142354277" style="zoom:33%;" />
    - at the beginning, a path for A to X is 1-2-3-4
    - B shares its routing table to A
    - A finds out from B to X is 2 hops away, it's still shorter adding the path from A to B
    - A adjust its path to X, A -> B -> 1-> 2
    - cons: react slowly to the changes of networks far away itself
  - link state routing protocol
    - <img src="computer-networking/images/image-20200602142820473.png" alt="image-20200602142820473" style="zoom:33%;" />
    - each router shares its info of each of its interface
    - information about each router is propagated to every other router in the autonomous system
    - each router runs complicated algorithms to determine the best path

#### Exterior Gateway Protocols

- IANA (`Internet Assigned Numbers Authority`): A non-profit organization that helps manage things like IP address allocation
  - also response for ASN (`Autonomous System Number`) allocation
    - ASN: 32 bits  - IBM: AS19604 - 9.0.0.0/8 
    - core routers(ISP) update ASN to find Autonomous system

#### Non-Routable Address Space

- RFC (`Request for Comments`)
- Exterior Gateway Protocols will not route those addresses, but Interior Gateway Protocols can route those address so they can be used within an autonomous system
- 10.0.0.0/8
- 172.16.0.0/12
- 192.168.0.0/16

### The Transport Layer

> Allow traffic to be directed to specific network applications

#### Transport Layer

- multiplexing - nodes on the network have the ability to direct traffic toward many different receiving services.
- demultiplexing - take traffic that's all aimed at the same node and delivering it to the proper receiving service.
- port - 16-bit number that used to direct traffic to specific services running on a netwroked computer
  - socket address/socket number - ip:port e.g. 10.0.0.100:80

#### Dissection of a TCP Segment

- TCP segment = TCP header + data section(payload from appication layer)
- <img src="computer-networking/images/image-20200602194527202.png" alt="image-20200602194527202" style="zoom:50%;" />
- Source Port - a high numbered port chosen from a special section of ports known as ephemeral ports
- Destination Port
- Sequence number - used to keep track of where in a sequence of TCP segments this one is expected to be
- Acknowledgement number - the number of next expected segment
- Header length - length of this tcp header so the receiving ends knows where the payloads start
  - measured in 32-bit multiples
  - e.g. the value is 7(0111), the length of header is 7 * 32 / 8 = 28 bytes
- Control flags
- Window - specifies the range of sequence numbers that might be sent before an acknowledgement is required
- Checksum
- Urgent - used in conjunction with one of the tcp control flags to point out particular segments that might be more important than others
- Options - sometimes used for more complicated protocols
- Padding

#### TCP Control Flags and the Three-way Handshake

- URG(urgent) - A value of `1` here indicates that the segment is considered urgent and the urgent pointer field more has data about this
- ACK(acknowledgement) - A value of `1` in this field means that the acknowledgement number field should be examined
- PSH(push) - The transmitting device want s the receiving device to push currently-buffered data to the application on the receiving end as soon as possible
- RST(reset) - one of the sides in a tcp connection hasn't been able to properly recover from a series of missing or malformed segment
- SYN - it's used when first establishing a tcp connection and makes sure the receiving end knows to examine the sequence number field
- FIN(finish) - `1`means transmitting computer doesn't have any more data to send and the connection can be closed.
- establish conection: Three-Way Handshake <img src="computer-networking/images/image-20200602201130107.png" alt="image-20200602201130107" style="zoom:33%;" />
  - A sends SYN (seq = x)
  - B receives SYN (seq = x) and responses ACK and SYN (seq = y, ack = x + 1)
  - A receives and sends ACK(ack=y+1)
- close connection: Four-Way Handshake <img src="computer-networking/images/image-20200602201900738.png" alt="image-20200602201900738" style="zoom:33%;" />
  - 

#### TCP Socket States

- a socket is the instaniation of an endpoint in a potential TCP connection
- LISTEN - a TCP socket is ready and listening for incoming connections (server side)
- SYN-SENT - a synchronized request has been sent, but the connection hasn't been established yet (client side)
- SYN-RECEIVED - a socket previously in a LISTEN state has received a synchronization request and sent a SYN/ACK back (server side)
- ESTABLISHED - The TCP connection is in working order and both side are free to send each other data (both sides)
- FIN_WAIT - a FIN has been sent, but the corresponding ACK from the other end hasn't been received yet (both sides)
- CLOSE_WAIT - the connection has been closed at the TCP layer, but that the application that opened the socket hasn't released its hold on the socket yet
- CLOSED - connecton fully terminated
- socket state definitions can vary from system to system

#### Connection-oriented and Connectionless Protocols

- connection-oriented
  - tcp
  - reliable, but having overhead of establishment, ackownledgement of data segments and tire down of the connection
- connectionless 
  - UDP
  - streaming videos
  - doesn't resent lost data, doesn't ensure sequence

#### Firewalls

- most commonly used at Transportation Layer, could be used at other layers
- blocking or allowing traffic through certain ports

### The Application Layer

> Allows applications to communicate in a way they understand

#### The Application Layer and the OSI Model

- <img src="computer-networking/images/image-20200602205823543.png" alt="image-20200602205823543" style="zoom:50%;" />
- Session Layer - facilitating the communication between actual applications and the transport layer
- Presentation Layer - responsible for making sure that the unencapsulated applicaiton layer data is able to be understood by the application in question

#### All the Layer Working in Union

- <img src="computer-networking/images/image-20200602210939472.png" alt="image-20200602210939472" style="zoom:33%;" /> computer 1 find out the destination is not in its network, it tries to reach the gateway router A, which it has been configured with(it knows the ip of the gateway), but faild to find the mac (needed to contruct the ethernet frame) on local ARP table, so it sends out a ARP request
- <img src="computer-networking/images/image-20200602210958991.png" alt="image-20200602210939472" style="zoom:33%;" />  the router responses with its MAC address
- <img src="computer-networking/images/image-20200602211055747.png" alt="image-20200602211055747" style="zoom:33%;" /> Computer 1 open an ephemeral port for browser
- <img src="computer-networking/images/image-20200602211143403.png" alt="image-20200602211143403" style="zoom:33%;" /> construct tcp segment
- <img src="computer-networking/images/image-20200602212225357.png" alt="image-20200602212225357" style="zoom:33%;" /> contruct ip datagram
- <img src="computer-networking/images/image-20200602211247761.png" alt="image-20200602211247761" style="zoom:33%;" /> contruct ethernet frame
- <img src="computer-networking/images/image-20200602211412026.png" alt="image-20200602211412026" style="zoom:33%;" /> sends out as modulaiton of 0/1 to physical link, the switches will ensure it gets sent out of the interface that the router A connectd to



