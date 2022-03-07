# Network Attacks and Defenses

MAC (Media Access Control) Address: a device's physical address

## Ethernet
- frames are addressed by MAC
- no guarantee that a frame sent to one MAC isnt accessible to others
- no confidentiality, integrity, authentication

## ARP (Address Resolution Protocol)
- Allows hosts to find other MAC addresses on LAN
- host broadcasts a message to all MACs, asking which MAC has the desired IP
- the device with the desired IP can respond with their MAC, which will be added to the ARP table of the original host sender
- exploited because anyone can send the response message claiming they have the desired IP even if they don't

## IP (Internet Protocol)
- Packets have a non-cryptographic checksum, which attackers can modify if they want to modify the packet (in the middle)
- source address is set by sender, so an attacker can pretend to send packets from someone else

## BGP (Border Gateway Protocol)
- ISPs announce their presence on internet
- no authentication, so you can announce someone elses network presence
- attacker can advertise their network as something that it isn't

## TCP (Transmission Control Protocol)
- reliable stream of data on top of unreliable lower layers (IP, Ethernet, etc)
- sent as limited size segments and then reconstructed to a byte stream by receiver
- endpoints send acknowledgement of received data (if you send packet 3, you'll get back ACK 4 asking for the next packet)
- lost packets are retransmitted if ACK times out

### TCP Handshake
- initiator = client
- listener = server
- initiator sends a SYN with a window size and sequence number
- listener sends back an ACK with the initial sequence number +1
- lisener sends a SYN with a different sequence number
- initiator sends ACK with listener's different sequence number +1
- initial sequence number is now randomized to make sequence number spoofing realistically impossible

## DNS
- DNS responses are cached
- DNS responses are trusted directly without verification
- a compromised DNS can enable a MITM attack

### DNS Cache Poisoning
- targeting a specific name server
- attacker can try to visit a malicious website
- have a malicious name server for that site send a false IP address for a different website when the recursive resolver reaches it
- that false IP will be stored in the DNS cache
- future users of the name server will be told to go to that false IP, allowing the attacker to exploit them
- solution: only accept additional records (IP associated with a omain) for sites that are within the name server's domain. this means the targeted name server would just throw away the false IP sent by the malicious name server

### DNS Spoofing
- victim queries a certain website to a local DNS resolver
- local DNS resolver sends a request to figure out the IP of that certain website
- attacker spoofs a response to the local DNS resolver, sending a false IP for that certain webite
- now when the victim visits that website they will actually be sent to the attacker's IP
- solved by using secure DNS where responses are verified through a signature

### DNS Rebinding
- victim visits malicious site with malicious DNS server
- malicious DNS server sends a very short lived correct IP to the victim
- malicious DNS server then sends the victim's local IP
- malicious website can then access other devices on the victims network without breaking CORS, since the victim thinks the malicious website's IP is on their local network

## Denial of Service
- DoS Bug: design flaw that allows attacker to easily disrupt a service
- DoS Flood: send a lot of requests to a service
### SYN flood attack
-  service stores sequence numbers for every connection
-  send a bunch of SYN packets to a service, all with random source IPs
-  this will fill up the server's memory
-  solved by SYN cookies: instead of server storing and sending a random seq number, they generate one based on a counter that increments every minute and check that the response is the same as the counter

### Amplification attack
- attacker sends one request that gets amplified into many more requests by DNS server
- DNS query "ANY"