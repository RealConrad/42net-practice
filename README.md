<div align="center">
  <h1>
    📗 Net Practice
  </h1>
  <p>
    <b><i>Basic networking exercises.</i></b>
  </p>
</div>

<br />

# Explanations:
> NOTE: You do not need to know everything below to pass net practice. As long as you understand the basics of subnetting you would be good to go. I would recommend to learn the [Subnet Mask Chart](#subnet-mask-chart)
### Introduction
This guide explains the basics of IP addressing and subnetting, including how to convert IP addresses to binary, calculate network bits, host bits, and the number of available addresses in a subnet.

### What is an IP?
<div align="center">
  <img width="307" alt="image" src="https://github.com/RealConrad/42net-practice/assets/79513076/00ee28a4-6ed1-4c45-87c7-62195dbc6681">
</div>

An IP (Internet Protocol) address is a unique identifier assigned to each device connected to a computer network that uses the Internet Protocol for communication. It has two main purposes: network interface identification and location addressing. An IP address contains four octets (in IPv4), which can range from 0 to 255 (e.g., 192.168.1.1). This numerical label enables devices to locate and communicate with each other on a network. There is also IPv6, but that is beyond the scope of the project.

### What is a subnet mask?
<div align="center">
  <img width="307" alt="image" src="https://github.com/RealConrad/42net-practice/assets/79513076/8254ba72-d41c-4e51-9ce5-e2d730b5c24d">
</div>
A subnet mask is a 32-bit number that masks an IP address and divides the IP address into network address and host address. Subnet masks are used to designate which portion of an IP address is allocated to the network and which part is available for host use. This creates subnetworks within a larger network, which basically does the following:

  - Routing efficiency
  - Security
  - Management of IP address allocations.

A subnet mask looks very similar to an IP address (e.g., 255.255.255.0).

When the subnet mask is applied (via a bitwise AND operation) to an IP address, the network portion of the IP address is known. The remaining part, which corresponds to the host on that network, determines the number of hosts that can be assigned in the subnet. A common subnet mask is 255.255.255.0, which allows for 254 host addresses within a single network segment.

### Switch
<div align="center">
  <img width="302" alt="image" src="https://github.com/RealConrad/42net-practice/assets/79513076/cdfe2af8-62e9-4d69-8a6d-2a7f4762e307">
</div>
A switch allows multiple devices to connect with one another in a local network (LAN). It cannot deliver packages outside of its own network.

### Router
<div align="center">
  <img width="627" alt="image" src="https://github.com/RealConrad/42net-practice/assets/79513076/5a72997b-1788-4313-b0cc-2ff624347f43">
</div>
The router connects multiple networks. Unlike the switch, the router has an interface for each network it connects to. Make sure the IP's do not overlap when assigning IP's.


### Converting IP Addresses to Binary and Decimal
To convert an IP address we will be using the following cheat table:
   | 128 | 64  | 32  | 16  | 8   |  4  | 2   | 1   |
   |-----|-----|-----|-----|-----|-----|-----|-----|
   | 2^7 | 2^6 | 2^5 | 2^4 | 2^3 | 2^2 | 2^1 | 2^0 |

#### **From Decimal to Binary:**
Example of IP address in bits: `11000000.10101000.00000001.00010101`. \
Place the 8 bits on our table and check the following: When theres a `1`, add all the numbers.

So we take the first octet (`11000000`) and place it on our table:
| 1   | 1   | 0   | 0   | 0   | 0   | 0   | 0   |
|-----|-----|-----|-----|-----|-----|-----|-----|
| 128 | 64  | 32  | 16  | 8   |  4  | 2   | 1   |

The above gives us, `128 + 64 = 192`. \
When we do this for the rest of the octets, you get: 
```
192.168.1.21
```

#### **From binary to decimal:**
IP Address: `172.16.34.3` \
Take the individual octests and ask: Can we subtract the current octet from the table?
  - _Yes_: Place 1 and use the result from the subtraction and move on.
  - _No_: Place 0 and move on to the next one.

Taking the first octet, `172` and place it on our table:
| 172 | 44  | 44  | 12  | 12  | 4   | 0   | 0   |
|-----|-----|-----|-----|-----|-----|-----|-----|
| 128 | 64  | 32  | 16  | 8   |  4  | 2   | 1   |
| 1   | 0   | 1   | 0   | 1   |  1  | 0   | 0   |

From the above we get our first binary octet: 10101100. \
Again, we first check, can we subtract 128 from 172 (basically is `172 > 128`)? Yes, `172-128 = 44`. So then we place a `1` and move on to the next one, but now with the result (`44`).

If you do this for all of them you get the following: 
```
10101100.00010000.00100010.00000011
```

#### **What does the binary tell us?**
We can calculate the number of network/host bits when converting a subnet mask to binary (see this [chart](#subnet-mask-chart) for all the different combinations).
  - 1 = Network bits
  - 0 = Host bits

Example of sub net mask:
- Decimal: 255.255.255.0
- Binary: 11111111.11111111.11111111.00000000

The number of 0's (in binary!) = number of hosts. So we can use the following to calculate the number of possible IP's in the the network:
```
2^(# of 0's) - 2
```
So given our example above, we can do:
```
2^8 = 256
```
This means we can have a total of 256 different IP addresses in the current network.

**HOWEVER**: 2 IP's are reserved:
-  The first IP = subnet address.
-  The last IP = broadcast address.

So our new total would be 256 - 2 = 254 useable addresses.

### Subnet Mask Chart

| Subnet Mask       | CIDR | Binary                              | Network Bits | Host bits | Available addresses (Group size) |
|-------------------|------|-------------------------------------|--------------|-----------|-----------------------------------|
| 255.255.255.255   | /32  | 11111111 11111111 11111111 11111111 | 32           | 0         | 1                                 |
| 255.255.255.254   | /31  | 11111111 11111111 11111111 11111110 | 31           | 1         | 2                                 |
| 255.255.255.252   | /30  | 11111111 11111111 11111111 11111100 | 30           | 2         | 4                                 |
| 255.255.255.248   | /29  | 11111111 11111111 11111111 11111000 | 29           | 3         | 8                                 |
| 255.255.255.240   | /28  | 11111111 11111111 11111111 11110000 | 28           | 4         | 16                                |
| 255.255.255.224   | /27  | 11111111 11111111 11111111 11100000 | 27           | 5         | 32                                |
| 255.255.255.192   | /26  | 11111111 11111111 11111111 11000000 | 26           | 6         | 64                                |
| 255.255.255.128   | /25  | 11111111 11111111 11111111 10000000 | 25           | 7         | 128                               |
| 255.255.255.0     | /24  | 11111111 11111111 11111111 00000000 | 24           | 8         | 256                               |
| 255.255.254.0     | /23  | 11111111 11111111 11111110 00000000 | 23           | 9         | 512                               |
| 255.255.252.0     | /22  | 11111111 11111111 11111100 00000000 | 22           | 10        | 1,024                              |
| 255.255.248.0     | /21  | 11111111 11111111 11111000 00000000 | 21           | 11        | 2,048                              |
| 255.255.240.0     | /20  | 11111111 11111111 11110000 00000000 | 20           | 12        | 4,096                              |
| 255.255.224.0     | /19  | 11111111 11111111 11100000 00000000 | 19           | 13        | 8,192                              |
| 255.255.192.0     | /18  | 11111111 11111111 11000000 00000000 | 18           | 14        | 16,384                             |
| 255.255.128.0     | /17  | 11111111 11111111 10000000 00000000 | 17           | 15        | 32,768                             |
| 255.255.0.0       | /16  | 11111111 11111111 00000000 00000000 | 16           | 16        | 65,536                             |
| 255.254.0.0       | /15  | 11111111 11111110 00000000 00000000 | 15           | 17        | 131,072                            |
| 255.252.0.0       | /14  | 11111111 11111100 00000000 00000000 | 14           | 18        | 262,144                            |
| 255.248.0.0       | /13  | 11111111 11111000 00000000 00000000 | 13           | 19        | 524,288                            |
| 255.240.0.0       | /12  | 11111111 11110000 00000000 00000000 | 12           | 20        | 1,048,576                           |
| 255.224.0.0       | /11  | 11111111 11100000 00000000 00000000 | 11           | 21        | 2,097,152                           |
| 255.192.0.0       | /10  | 11111111 11000000 00000000 00000000 | 10           | 22        | 4,194,304                           |
| 255.128.0.0       | /9   | 11111111 10000000 00000000 00000000 | 9            | 23        | 8,388,608                           |
| 255.0.0.0         | /8   | 11111111 00000000 00000000 00000000 | 8            | 24        | 16,777,216                          |
