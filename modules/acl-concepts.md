# ACL Concepts

**ACL** (Access Control List) is a sequential list of permit/deny statements used to filter packets based on their header
information. By default, routers don't have any ACLs configured and therefore all packets are permitted.

ACL consists of 1 or more **ACEs** (Access Control Entries, also referred to as *ACL statements*). ACEs are checked in sequence,
from top to bottom, and the first match is applied. This process is called **packet filtering**.

ACLs can:
- Limit network traffic to increase network performance
- Provide traffic flow control
- Provide basic level of security
- Filter traffic based on traffic type
- Screen hosts to permit/deny access
- Provide priority to certain types of traffic

Packet filtering can be done on:
- Layer 3 of OSI model (Network layer) - IP addresses, protocols, etc. (= *Standard ACLs*)
- Layer 4 of OSI model (Transport layer) - TCP/UDP ports, etc. (= *Extended ACLs*)

ACL is only applied to incoming and outgoing traffic, rules are not applied to traffic originating from the router itself.
Processing of inbound traffic is a little bit more efficient as the router doesn't have to route the packet before applying
the ACL (it gets discarded immediately).

### ACL wildcard traffic matching

**Wildcard mask** - Checks IPv4 address on binary level based on inverse of the IP (= wildcard).

> **Example of an ACL that only permits host with IPv4 address 192.168.1.1**
> 
> ... = ACL 10:
> `access-list 10 permit 192.168.1.1 0.0.0.0`
>> = Permitted IP `11000000.10101000.00000001.00000001` in binary mask form.

> We can also match for example whole /24 subnet by using a wildcard mask:
> 
> `access-list 10 permit 192.168.1.1 0.0.0.255`
> would match all IPs in `192.168.1.0-255`.

We can use alternative keywords to specify the mask:
- `host` - Specifies a single host = substitute for mask `0.0.0.0`
- `any` - Matches any IP address = substitute for mask `255.255.255.255`


### Guidelines for ACL creation

- Start with ACLs on security policies.
- Write down all the ACL requirements and targets they are supposed to achieve.
- Prepare ACLs in advance in a text editor, then copy-paste them into the router. (Save the configuration in text editor as well!)
- Document ACLs using `remark` command.
- Test the ACLs before production deployment.

> Entering ACLs directly can be risky, as a mistake can result in locking yourself out of the router, or for example
> cause a broadcast storm that can mess up other parts of the network and result in more required troubleshooting.


### Numbered vs Named ACLs

We can divide **numbered ACLs** into 2 categories:
- **Standard ACLs** - ACL with numbers from **1-99** and **1300-1999**. They filter based on source IP address only. They do not consider other
  parameters like destination address, protocol, or port numbers.
- **Extended ACLs** - ACL with numbers from **100-199** and **2000-2699**. They can include filtering based on protocols (TCP, UDP, ICMP...),
  port numbers and even certain flags.

Extended ACLs should be located as close to the source of traffic as possible, while standard ACLs should be located as close
to the destination as possible, to ensure maximum efficiency and good performance.

>Standard ACL example:
> 
>![img.png](../images/acl_ex_1.png)
>
>Extended ACL example:
> 
> ![img.png](../images/acl_ex_2.png)


**Named ACLs** are more flexible and easier to manage. They can be edited without changing the sequence number, and can be applied to
specific interfaces. They are also more descriptive and easier to understand. The `ip access-list` command is used to create named ACLs.
![img.png](../images/named_acl.png)


## Configuring ACLs on Cisco routers



