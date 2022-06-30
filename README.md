# Net_Practice

## Basics
For this project we only use IPv4, so i won't talk about IPv6.<br>
An IPv4-adress is a 32-bit number divided into 4 "blocks", each 8 bits.<br>
i.e.:<br>
`192.168.100.1` turns into `11000000.10101000.01100100.00000001`<br>
So the min. value of one "block" is `0` and the max. value is `255`.<br>
The same logic applies to the network-mask:<br>
`255.255.255.0` turns into `11111111.11111111.11111111.00000000`<br>
Special to the mask is, after one bit was `0` there can't be any `1` bit's anymore.<br>
so the only available numbers are:


- `255`
- `254`
- `252`
- `248`
- `240`
- `224`
- `192`
- `128`
- `0`


Through which `255.255.255.0` is a valid mask<br>
and `255.255.128.128` is **not** a valid mask.<br>
<br>
In order to have the ability to send packages between two IP-addresses they either need to be part of the same network or they need to be connected by a router which is part of both subnets.



## Special IP-ranges

The following special address-ranges are reserved for Private Networks:<br>
`10.0.0.0 – 10.255.255.255`<br>
`172.16.0.0 – 172.31.255.255`<br>
`192.168.0.0 – 192.168.255.255`<br>

The following address-range is reserved for so called loopback addresses:<br>
`127.0.0.0 – 127.255.255.255`


There is some more special ip-ranges, but for this project, you only need to remember those above.


## Masks

The network-mask, subnet-mask or in our project only called mask is there to decide which range of ip-adresses are part of the same subnet.<br>
There is 2 different ways of writing the mask:

- "Dot-decimal notation": `255.255.255.0`
- "Class Inter-Domain Routing" or "CIDR": `/24`


The more usable ip-addresses you need in one subnet, the less subnets you will be able to create.<br>
To help you understanding it, i found this table very helpfull:


| CIDR | Dot-decimal | Number of IP-addresses<br /> per subnet | Usable IP-addresses <br /> per subnet | Number of subnets |
| :---: | :-----------: | :---: | :---: | :---: |
| /32 | 255.255.255.255 | 1 | 0 | 256 |
| /31 | 255.255.255.254 | 2 | 0 | 128 |
| /30 | 255.255.255.252 | 4 | 2 | 64 |
| /29 | 255.255.255.248 | 8 | 6 | 32 |
| /28 | 255.255.255.240 | 16 | 14 | 16 |
| /27 | 255.255.255.224 | 32 | 30 | 8 |
| /26 | 255.255.255.192 | 64 | 62 | 4 |
| /25 | 255.255.255.128 | 128 | 126 | 2 |
| /24 | 255.255.255.0 | 256 | 254 | 1 |


The number of usable IP-addresses per subnet is lower than the total number of IP's because the first address is reserved as the network-address of the subnet and the last address is reserved as a broadcast-adress.<br>
i.e. for mask `255.255.255.252`:<br>
network: `190.3.2.252`<br>
broadcast: `190.3.2.255`<br>
usable IP's: `190.3.2.253`, `190.3.2.254`


## Switches

A switch will enable you to connect more than two devices to the same network.<br>
It's only purpose is to distribute packages to its network.<br>
To see a working example, you can take a look at [Level 3](https://github.com/tblaase/Net_Practice/blob/main/my_solutions/Level_3.png).<br>

## Routers

As previously mentioned a router is a interface which enables communication between different networks.<br>
A router has the ability to be part of multiple networks, in Netpractice this is visualized by the so called `Interface`.<br>
If routers and switches are still magic to you, i suggest looking deeper [into it](https://www.youtube.com/watch?v=Vc16CCAAz7Q) yourself, as their basic understanding is crucial to succeed in this project.



## Routing Table


The routing table is there to store all the different paths to all the networks, the device is part of.<br>
In Net_Practice the routing table consists of two elements, the **destination** and the **next hop**<br>
The **destination** consists of the network-address that you want to send a package to, combined with the CIDR of that network: `190.3.2.252/30`. If you don't want to specify a destination, you can just set it to `default` or `0.0.0.0/0`.<br>
The **next hop** is the address of the next router that you need to send the packages to in order to reach the destination-network.<br>


## Network

And now to connect all of the above mentioned topics.<br>
In order to have a functioning network, you now need to apply all of the parts talked about earlier.<br>
If there should be a working connection in a network, the devices somehow need to be connected, either directly or by the help of routers which are part of both networks.


Now you may ask, how do i know if two devices are part of the same network?<br>
For this you need to combine the IP-address and the mask of the devices in order to get the network-adress, that device is part of.<br>
By combining i mean, doing a bit-by-bit-AND-opperation.<br>
For that we first need to translate the IP and the mask to binary.<br>
i.e.:<br>
IP: `192.168.100.1` in binary: `11000000.10101000.1100100.00000001`<br>
MASK: `255.255.255.0` in binary: `11111111.11111111.11111111.00000000`<br>
Now you just combine the two bit by bit, if both bits are a `1` the corresponding bit of the network-address is `1`, in any other case the corresponding bit is `0`.


By doing that to the mentioned example, you should get the network-address of<br>`11000000.10101000.1100100.00000000` in binary or `192.168.100.0` in dot-decimal.<br>
If two devices share the same network-address, they are part of the same network and communication is enshured.


