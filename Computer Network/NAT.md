# Network Address Traslation

In a local network (Home, office, school or campus network, etc.), a device are configured to use it's private IP address, which is reserved for internal use and are not globally unique. As private IP address are not routable on the public internet, external IP address (globally unique) can not directly access it.

So, how can a device with private IP addresses within a local network access resources on the internet ?

The answer is NAT, which stands for **Network Address Translation**

## What is NAT ?

NAT (Network Address Translation) is a technique used in networking to enable a divice within a local network to use a single public IP address when communicating with external networks, such as the internet.

## How many types of NAT ?

NAT is classified into one-to-one NAT and one-to-many NAT. Let's break them down.

### one-to-one NAT

Also called **Basic NAT**, the one to one NAT map one private IP address with a unique public IP address.

On the other hand, if you have an number of devices on your internal network, you need a corresponding number of public IP addresses in order for all of them to access the internet.

Here's the visual illustrating how one-to-one NAT operates:

![1696410120187](image/NAT/1696410120187.png)

When a host in private network access a web server on the Internet, one-to-one NAT process in a NAT device (router, gateway device, etc.) as follows:

1. **Sending Packet**: The host sends a packet to the router, which contains:
   * Source IP address and destination IP address (within the IP header)
   * Source port number and destination port number (within the TCP header). We will not use this in this type of NAT.
2. **Source Network Address Translation (SNAT)**: After receiving the packet, the router only replaces the source IP address (in the IP header) with a unique public IP address (assigned by the Internet Service Provider, or ISP).
3. **Updating Translation Table**: The router then creates a mapping record of private IP address and corresponding public IP address in the NAT translation table and forwards the packet to the web server on the Internet.
4. **Destination Network Address Translation (DNAT)**: After receiving the response packet from the server, router searches the NAT translation table for record inserted in previous step, replaces the destination IP in the IP header of the packet with the corresponding private IP found in the table, and sends the packet back to the host device.

#### Use cases:

* Basic NAT is suitable for scenarios where two IP networks with incompatible addressing need to interconnect.
* It allows communication between devices in a private network with non-routable addresses and external networks with routable addresses.

However, one-to-one NAT cannot resovle the IPv4 address exhaustion. That's why the next type of NAT is more popular.

### one-to-many NAT

Other names of this NAT include *port address translation* (PAT), *network address and port translation* (NAPT), *IP masquerading* , *NAT overload* and *many-to-one NAT.* 

This technique allows multiple private IP addresses to be mapped to a single public IP address. Each connection is uniquely identified by a combination of the public IP address and a port number dynamically assigned by the NAT device.

Here's the visual illustrating how one-to-many NAT operates:

In this scenario, the router only need one public IP addresss. As you see in the figure above, the public IP is 50.50.50.1.

When a host in private network access a web server on the Internet, one-to-many NAT process in a NAT device (router, gateway device, etc.) as follows:

1. **Sending Packet**: The host sends a packet to the router, which contains:

   * Source IP address and destination IP address (within the IP header)
   * Source port number and destination port number (within the TCP header). We will use this port in this type of NAT.
2. **Source Network Address Translation (SNAT) and Port Address Translation (PAT)**: After receiving the packet, the router do following steps:

   * Replaces the source IP address (in the IP header) with the public IP address.
   * Replaces the source port number (in the TCP header) with an unique port number which assigned by router to identify this communication session (I call it **session port**).
3. **Updating Translation Table**: The router then creates an entry in the translation table containing source IP address, original source port number, public IP address, and translated port number. Then, it forwards the packet to the web server on the Internet.
4. **Destination Network Address Translation (DNAT)**: After receiving the response packet from the server, router searches the NAT translation table based on the **destination port** in the packet header. If a match found, the destination IP address and port number is replaced with the values found in the table and the packet is forwarded to the inside network. Otherwise, if the destination port number of the incoming packet is not found in the translation table, the packet is dropped or rejected because the PAT device doesn't know where to send it.

By using one-to-many NAT, all devices in a private network can access internet using a single public IP address. 

## Reference

* [Network address translation - Wikipedia](https://en.wikipedia.org/wiki/Network_address_translation)
* [Tìm hiểu cơ chế hoạt động của NAT (Network Address Translation) (Phần 1) (quantrimang.com)](https://quantrimang.com/cong-nghe/network-address-translation-nat-hoat-dong-nhu-the-nao-phan-1-118495)
* [What Is NAT? What Are the NAT Types? - Huawei](https://info.support.huawei.com/info-finder/encyclopedia/en/NAT.html)
* [Difference between IP address and Port Number - GeeksforGeeks](https://www.geeksforgeeks.org/difference-between-ip-address-and-port-number/)

Thanks a bunch for taking the time to read my article! If you have any thoughts or questions, I'm all ears. 

Cheers! 👻👻👻
