## Socket Programming
#### server.py
```
import socket
HOST = '127.0.0.1'
PORT = 8080
def main():
    server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    server_socket.bind((HOST, PORT))
    server_socket.listen(5)
    print("Server listening on {}:{}".format(HOST, PORT))
    client_socket, addr = server_socket.accept()
    print("Connected to", addr)
    while True:
        data = client_socket.recv(1024).decode('utf-8')
        if not data:
            break
        print("Received from client:", data)
    client_socket.close()
    server_socket.close()
if __name__ == "__main__":
    main()
```
#### client.py
```
import socket
HOST = '127.0.0.1'
PORT = 8080
def main():
    client_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    client_socket.connect((HOST, PORT))
    while True:
        message = input("Enter message to send: ")
        client_socket.sendall(message.encode('utf-8'))
        if message.lower() == 'exit':
            break
    client_socket.close()
if __name__ == "__main__":
    main()
```


## STATIC ROUTING
#### R0 (10 e 11 ta dibi, 11 e 10 er ta)
```
ip route 192.168.11.0 255.255.255.0 10.0.0.0
```
#### R1
```
ip route 192.168.10.0 255.255.255.0 10.0.0.0
```


# RIP

### Router0 Configuration Codes (DTE):

```
Router>en
Router#config terminal
Router(config)#interface gi 0/0
Router(config-if)#ip address 192.168.10.1 255.255.255.0
Router(config-if)#no shutdown
Router(config-if)#exit
Router(config)#interface se 0/1/0
Router(config-if)#ip address 10.0.0.1 255.0.0.0
Router(config-if)#no shutdown
Router(config-if)#exit
Router(config)#
Router(config)#router rip
Router(config-router)#network 192.168.10.0
Router(config-router)#network 10.0.0.0
Router(config-router)#version 2
Router(config-router)#
```
### Router1 Configuration Codes (DCE):
```
Router>en
Router#config terminal
Router(config)#interface gi 0/0
Router(config-if)#ip address 192.168.11.1 255.255.255.0
Router(config-if)#no shutdown
Router(config-if)#
Router(config-if)#exit
Router(config)#interface se 0/1/0
Router(config-if)#ip address 10.0.0.2 255.0.0.0
Router(config-if)#clock rate 64000
Router(config-if)#no shutdown
Router(config-if)#
Router(config-if)#exit
Router(config)#router rip
Router(config-router)#network 192.168.11.0
Router(config-router)#network 10.0.0.0
Router(config-router)#version 2
Router(config-router)#
```
<hr>

# EIGRP

```
 ‎  ‎ ‎ ‎  ‎‎ rou-----rou
        |        |
        sw       sw
       /  \     /  \
      pc  pc   pc  pc 
```

### IP Congiguration:
```
PC0 - 192.168.10.2 | 10.0.0.0
PC1 - 192.168.10.3 | 10.0.0.0
PC2 - 192.168.11.2 | 10.0.0.0
PC3 - 192.168.11.3 | 10.0.0.0
```

### Router0 Configuration Code (DTE):
```
Router>
Router>en
Router#config terminal
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#int gi 0/0
Router(config-if)#ip add 192.168.10.1 255.255.255.0
Router(config-if)#no shutdown

Router(config-if)#
%LINK-5-CHANGED: Interface GigabitEthernet0/0, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0, changed state to up

Router(config-if)#ex
Router(config)#int se 0/1/0
Router(config-if)#ip add 10.0.0.1 255.0.0.0
Router(config-if)#no shut

%LINK-5-CHANGED: Interface Serial0/1/0, changed state to down
Router(config-if)#
Router(config-if)#ex
Router(config)#
%LINK-5-CHANGED: Interface Serial0/1/0, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface Serial0/1/0, changed state to up

Router(config)#
Router(config)#router eigrp 3
Router(config-router)#network 192.168.10.0
Router(config-router)#network 10.0.0.0
Router(config-router)#ex
Router(config)#
%DUAL-5-NBRCHANGE: IP-EIGRP 3: Neighbor 10.0.0.2 (Serial0/1/0) is up: new adjacency

Router(config)#
```

### Router1 Configuration Code (DCE):
```
Router>
Router>en
Router#config t
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#int gi 0/0
Router(config-if)#ip add 192.168.11.1 255.255.255.0
Router(config-if)#no shut

Router(config-if)#
%LINK-5-CHANGED: Interface GigabitEthernet0/0, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0, changed state to up

Router(config-if)#ex
Router(config)#int se 0/1/0
Router(config-if)#ip add 10.0.0.2 255.0.0.0
Router(config-if)#clock rate 64000
Router(config-if)#no shut

Router(config-if)#
%LINK-5-CHANGED: Interface Serial0/1/0, changed state to up

Router(config-if)#ex
Router(config)#
Router(config)#
%LINEPROTO-5-UPDOWN: Line protocol on Interface Serial0/1/0, changed state to up

Router(config)#
Router(config)#router eigrp 3
Router(config-router)#network 192.168.11.0
Router(config-router)#network 10.0.0.0
Router(config-router)#
%DUAL-5-NBRCHANGE: IP-EIGRP 3: Neighbor 10.0.0.1 (Serial0/1/0) is up: new adjacency

Router(config-router)#
Router(config-router)#ex
Router(config)#
```
<hr>

# DHCP


```
 ‎  ‎ ‎ ‎   ‎‎rou
         |       
	 sw
     /   |   \    
   pc   pc    pc  
```

```
Press RETURN to get started!
Router>en
Router#config terminal
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#interface gi 0/0
Router(config-if)#ip address 192.168.10.1 255.255.255.0
Router(config-if)#no shutdown

Router(config-if)#
%LINK-5-CHANGED: Interface GigabitEthernet0/0, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0, changed state to up

Router(config-if)#exit
Router(config)#ip dhcp pool a
Router(dhcp-config)#network 192.168.10.0 255.255.255.0
Router(dhcp-config)#default-router 192.168.10.1
Router(dhcp-config)#dns 8.8.8.8
Router(dhcp-config)#exit
Router(config)#
```
<hr>

# OSPF


```
 ‎  ‎ ‎ ‎  ‎rou------rou
        |        |
	       sw       sw
       /  \     /  \
      pc  pc   pc  pc 
```

### Configuration Codes for Router 0 (DTE):

```
Router>en
Router#config terminal
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#interface gi 0/0
Router(config-if)#ip address 192.168.10.1 255.255.255.0
Router(config-if)#no shutdown

Router(config-if)#
%LINK-5-CHANGED: Interface GigabitEthernet0/0, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0, changed state to up

Router(config-if)#exit
Router(config)#interface se 0/1/0
Router(config-if)#ip address 10.0.0.1 255.0.0.0
Router(config-if)#no shutdown

%LINK-5-CHANGED: Interface Serial0/1/0, changed state to down
Router(config-if)#exit
Router(config)#
%LINK-5-CHANGED: Interface Serial0/1/0, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface Serial0/1/0, changed state to up

Router(config)#
Router(config)#router ospf 2
Router(config-router)#network 192.168.10.0 0.0.0.255 area 1
Router(config-router)#network 10.0.0.0 255.255.255.255 area 1
Router(config-router)#exit
Router(config)#
00:06:19: %OSPF-5-ADJCHG: Process 2, Nbr 192.168.11.1 on Serial0/1/0 from LOADING to FULL, Loading Done
```


### Configuration Codes for Router 1 (DCE):
```
Router>en
Router#config terminal
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#ip address 192.168.11.1 255.255.255.0
                   ^
% Invalid input detected at '^' marker.
	
Router(config)#interface gi 0/0
Router(config-if)#ip address 192.168.11.1 255.255.255.0
Router(config-if)#no shutdown

Router(config-if)#
%LINK-5-CHANGED: Interface GigabitEthernet0/0, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0, changed state to up

Router(config-if)#exit
Router(config)#interface se 0/1/0
Router(config-if)#ip address 10.0.0.2 255.0.0.0
Router(config-if)#no shutdown

Router(config-if)#
%LINK-5-CHANGED: Interface Serial0/1/0, changed state to up

Router(config-if)#exit
Router(config)#interfac
%LINEPROTO-5-UPDOWN: Line protocol on Interface Serial0/1/0, changed state to up
% Incomplete command.
Router(config)#
Router(config)#interface se 0/1/0
Router(config-if)#clock rate 64000
Router(config-if)#exit
Router(config)#
Router(config)#router ospf 3
Router(config-router)#network 192.168.11.0 0.0.0.255 area 1
Router(config-router)#network 10.0.0.0 255.255.255.255 area 1
Router(config-router)#
Router(config-router)#exit
Router(config)#
Router(config)#
00:06:14: %OSPF-5-ADJCHG: Process 3, Nbr 192.168.10.1 on Serial0/1/0 from LOADING to FULL, Loading Done
```

<hr>

# ACL


```

            rou------------------rou
         /      \                 |
 ‎  ‎ ‎ ‎  ‎‎ /        \                |
        |        |                |
	sw       sw              ser
       /  \     /  \
      pc  pc   pc  pc 
```


## IP Configuration:
```
PC0 - 192.168.10.2 | 10.0.0.0
PC1 - 192.168.10.3 | 10.0.0.0
PC2 - 192.168.11.2 | 10.0.0.0
PC3 - 192.168.11.3 | 10.0.0.0
Server0 - 192.168.12.2 | 10.0.0.0
```

## Router0 Configuration Code:
```
Router>
Router>en
Router#config terminal
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#interface gi 0/0
Router(config-if)#ip address 192.168.10.1 255.255.255.0
Router(config-if)#no shutdown

Router(config-if)#
%LINK-5-CHANGED: Interface GigabitEthernet0/0, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0, changed state to up

Router(config-if)#exit
Router(config)#
Router(config)#interface gi 0/1
Router(config-if)#ip address 192.168.11.1 255.255.255.0
Router(config-if)#no shutdown

Router(config-if)#
%LINK-5-CHANGED: Interface GigabitEthernet0/1, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/1, changed state to up

Router(config-if)#exit
Router(config)#
Router(config)#interface se 0/1/0
Router(config-if)#ip address 10.0.0.1 255.0.0.0
Router(config-if)#clock rate 64000
Router(config-if)#no shutdown

%LINK-5-CHANGED: Interface Serial0/1/0, changed state to down
Router(config-if)#exit
Router(config)#
Router(config)#
%LINK-5-CHANGED: Interface Serial0/1/0, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface Serial0/1/0, changed state to up

Router(config)#
Router(config)#router eigrp 3
Router(config-router)#network 192.168.10.0
Router(config-router)#network 10.0.0.0
Router(config-router)#exit
Router(config)#
Router(config)#router eigrp 3
Router(config-router)#network 192.168.11.0
Router(config-router)#network 10.0.0.0
Router(config-router)#exit
Router(config)#
Router(config)#
%DUAL-5-NBRCHANGE: IP-EIGRP 3: Neighbor 10.0.0.2 (Serial0/1/0) is up: new adjacency

Router(config)#
Router(config)#
Router(config)#
Router(config)#config terminal
%Invalid hex value
Router(config)#exit
Router#
%SYS-5-CONFIG_I: Configured from console by console

Router#
Router#config terminal
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#access-list 2 deny 192.168.10.0 0.0.0.255
Router(config)#access-list 2 permit any
Router(config)#interface se 0/1/0
Router(config-if)#ip access-group 2 out
Router(config-if)#
```
## Router1 Configuration Code:
```
Router>
Router>en
Router#config terminal
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#interface gi 0/0
Router(config-if)#ip address 192.168.12.1 255.255.255.0
Router(config-if)#no shutdown

Router(config-if)#
%LINK-5-CHANGED: Interface GigabitEthernet0/0, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0, changed state to up

Router(config-if)#
Router(config-if)#exit
Router(config)#
Router(config)#interface se 0/1/0
Router(config-if)#ip address 10.0.0.2 255.0.0.0
Router(config-if)#no shutdown

Router(config-if)#
%LINK-5-CHANGED: Interface Serial0/1/0, changed state to up

Router(config-if)#
Router(config-if)#exit
Router(config)#
Router(config)#
%LINEPROTO-5-UPDOWN: Line protocol on Interface Serial0/1/0, changed state to up

Router(config)#
Router(config)#
Router(config)#router eigrp 3
Router(config-router)#network 192.168.12.0
Router(config-router)#network 10.0.0.0
Router(config-router)#
%DUAL-5-NBRCHANGE: IP-EIGRP 3: Neighbor 10.0.0.1 (Serial0/1/0) is up: new adjacency

Router(config-router)#exit
Router(config)#
Router(config)#access-list 112 deny tcp 192.168.10.0 0.0.0.255 192.168.12.2 0.0.0.0 eq 80
Router(config)#access-list 112 permit ip any any
Router(config)#interface gi 0/0
Router(config-if)#ip access-group 112 out
Router(config-if)#
```
<hr>

# Static NAT




```
 ‎  ‎ ‎ ‎  ‎‎ S--------S
        |        |
	       sw       ser
       /  \     
      pc  pc   
```
### IP Congihuration:
```
PC0 - 192.168.10.2 | 10.0.0.0
PC1 - 192.168.10.3 | 10.0.0.0
Server0 - 192.168.11.2 | 10.0.0.0
```



### Router0 Configuration Code:
```
Router>
Router>en
Router#config terminal
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#interface gi 0/0
Router(config-if)#ip address 192.168.10.1 255.255.255.0
Router(config-if)#no shutdown

Router(config-if)#
%LINK-5-CHANGED: Interface GigabitEthernet0/0, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0, changed state to up

Router(config-if)#exit
Router(config)#interface se 0/1/0
Router(config-if)#ip address 10.0.0.1 255.0.0.0
Router(config-if)#no shutdown

%LINK-5-CHANGED: Interface Serial0/1/0, changed state to down
Router(config-if)#exit
Router(config)#
%LINK-5-CHANGED: Interface Serial0/1/0, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface Serial0/1/0, changed state to up

Router(config)#
Router(config)#router eigrp 3
Router(config-router)#network 192.168.10.0
Router(config-router)#network 10.0.0.0
Router(config-router)#exit
Router(config)#
Router(config)#
%DUAL-5-NBRCHANGE: IP-EIGRP 3: Neighbor 10.0.0.2 (Serial0/1/0) is up: new adjacency

Router(config)#
Router(config)#ip nat inside source static 192.168.11.2 10.0.0.1	
Router(config)#interface gi 0/0
Router(config-if)#ip nat inside
Router(config-if)#exit
Router(config)#interface se 0/1/0
Router(config-if)#ip nat outside
Router(config-if)#exit
Router(config)#
Router(config)#
Router(config)#
Router(config)#exit
Router#
%SYS-5-CONFIG_I: Configured from console by console

Router#
Router#show ip nat translations
Pro  Inside global     Inside local       Outside local      Outside global
---  10.0.0.1          192.168.11.2       ---                ---

Router#
```
### Router1 Configuration Code:
```
Router>
Router>en
Router#config terminal
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#interface gi 0/0
Router(config-if)#ip address 192.168.11.1 255.255.255.0
Router(config-if)#no shutdown

Router(config-if)#
%LINK-5-CHANGED: Interface GigabitEthernet0/0, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0, changed state to up

Router(config-if)#exit
Router(config)#interface se 0/1/0
Router(config-if)#ip address 10.0.0.2 255.0.0.0
Router(config-if)#clock rate 64000
Router(config-if)#no shutdown

Router(config-if)#
%LINK-5-CHANGED: Interface Serial0/1/0, changed state to up

Router(config-if)#exit
Router(config)#
%LINEPROTO-5-UPDOWN: Line protocol on Interface Serial0/1/0, changed state to up

Router(config)#
Router(config)#
Router(config)#router eigrp 3
Router(config-router)#network 192.168.11.0
Router(config-router)#network 10.0.0.0
Router(config-router)#
%DUAL-5-NBRCHANGE: IP-EIGRP 3: Neighbor 10.0.0.1 (Serial0/1/0) is up: new adjacency

Router(config-router)#exit
Router(config)#
Router(config)#
```

<hr>

# PAT
```
 ‎  ‎ ‎ ‎  ‎‎ S--------S
        |        |
	     sw       ser
       /  \     
      pc  pc   
```

### Functional Ports:
```
PC0 - 
192.168.10.2 (ipv4)
255.255.255.0 (subnet)
10.0.0.0 (default gateway)

PC1 - 
192.168.10.3 (ipv4)
255.255.255.0 (subnet)
10.0.0.0 (default gateway)

Server0 - 
192.168.11.2 (ipv4)
255.255.255.0 (subnet)
10.0.0.0 (default gateway)
```
### Router0 Configuration Code (DTE) [Left Side with More Devices]: 
```
Router>
Router>en
Router#config terminal
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#interface gi 0/0
Router(config-if)#ip address 192.168.10.1 255.255.255.0
Router(config-if)#no shutdown
Router(config-if)#
%LINK-5-CHANGED: Interface GigabitEthernet0/0, changed state to up
%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0, changed state to up
Router(config-if)#exit
Router(config)#
Router(config)#interface se 0/1/0
Router(config-if)#ip address 10.0.0.1 255.0.0.0
Router(config-if)#no shutdown
%LINK-5-CHANGED: Interface Serial0/1/0, changed state to down
Router(config-if)#
Router(config-if)#exit
Router(config)#
Router(config)#
Router(config)#
%LINK-5-CHANGED: Interface Serial0/1/0, changed state to up
%LINEPROTO-5-UPDOWN: Line protocol on Interface Serial0/1/0, changed state to up
Router(config)#
Router(config)#
Router(config)#router eigrp 3
Router(config-router)#network 192.168.10.0
Router(config-router)#network 10.0.0.0
Router(config-router)#exit
Router(config)#
Router(config)#
%DUAL-5-NBRCHANGE: IP-EIGRP 3: Neighbor 10.0.0.2 (Serial0/1/0) is up: new adjacency
Router(config)#
Router(config)#
Router(config)#
Router(config)#access-list 2 permit 192.168.10.0 0.0.0.255
Router(config)#
Router(config)#
Router(config)#
Router(config)#
Router(config)#
Router(config)#ip nat inside source list 2 interface se0/1/0 overload
Router(config)#
Router(config)#
Router(config)#
Router(config)#interface gi 0/0
Router(config-if)#ip nat inside
Router(config-if)#exit
Router(config)#
Router(config)#
Router(config)#interface se 0/1/0
Router(config-if)#ip nat outside
Router(config-if)#
Router(config-if)#exit
Router(config)#exit
Router#
%SYS-5-CONFIG_I: Configured from console by console
Router#
Router#show ip nat translations
```
[Nothing was shown because nothing was pinged /  No packet was sent]

OVER HERE OPEN PCO OR PC1 AND WRITE IN COMMAND PROMPT "ping 192.168.11.2" THEN RUN "show ip nat translations"
```
Router#
Router#
Router#show ip nat translations
Pro  Inside global     Inside local       Outside local      Outside global
icmp 10.0.0.1:14       192.168.10.2:14    192.168.11.2:14    192.168.11.2:14
icmp 10.0.0.1:15       192.168.10.2:15    192.168.11.2:15    192.168.11.2:15
icmp 10.0.0.1:16       192.168.10.2:16    192.168.11.2:16    192.168.11.2:16
icmp 10.0.0.1:17       192.168.10.2:17    192.168.11.2:17    192.168.11.2:17
Router#
Router#
Router#
Router#show ip nat translations
Pro  Inside global     Inside local       Outside local      Outside global
tcp 10.0.0.1:1025      192.168.10.2:1025  192.168.11.2:80    192.168.11.2:80
Router#
```
### Router1 Configuration Code (DCE):
```
Router>
Router>en
Router#config terminal
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#interface gi 0/0
Router(config-if)#ip address 192.168.11.1 255.0.0.0
Router(config-if)#no shutdown
Router(config-if)#
%LINK-5-CHANGED: Interface GigabitEthernet0/0, changed state to up
%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0, changed state to up
Router(config-if)#exit
Router(config)#
Router(config)#interface se 0/1/0
Router(config-if)#ip address 10.0.0.2 255.0.0.0
Router(config-if)#clock rate 64000
Router(config-if)#no shutdown
Router(config-if)#
%LINK-5-CHANGED: Interface Serial0/1/0, changed state to up
Router(config-if)#
Router(config-if)#exit
Router(config)#
%LINEPROTO-5-UPDOWN: Line protocol on Interface Serial0/1/0, changed state to up
Router(config)#
Router(config)#
Router(config)#
Router(config)#router eigrp 3
Router(config-router)#network 192.168.11.0
Router(config-router)#network 10.0.0.0
Router(config-router)#
%DUAL-5-NBRCHANGE: IP-EIGRP 3: Neighbor 10.0.0.1 (Serial0/1/0) is up: new adjacency
Router(config-router)#exit
Router(config)#
Router(config)#
```

<hr>

# VLAN


Diagram: DHCP with 1 extra PC

### IP Configuration:
```
PC0 - 192.168.10.2 | 10.0.0.0
PC1 - 192.168.10.3 | 10.0.0.0
PC2 - 192.168.11.2 | 10.0.0.0
PC3 - 192.168.11.3 | 10.0.0.0
```

### Switch Connfiguration:
```
Switch>
Switch>en
Switch#config terminal
Enter configuration commands, one per line.  End with CNTL/Z.
Switch(config)#vlan 2
Switch(config-vlan)#name CSE
Switch(config-vlan)#exit
Switch(config)#vlan 3
Switch(config-vlan)#name IT
Switch(config-vlan)#exit
Switch(config)#interface range fa0/2-3
Switch(config-if-range)#switchport mode access
Switch(config-if-range)#switchport access vlan 2
Switch(config-if-range)#exit
Switch(config)#interface range fa0/4-5
Switch(config-if-range)#switchport mode access
Switch(config-if-range)#switchport access vlan 3
Switch(config-if-range)#exit
Switch(config)#
%LINK-5-CHANGED: Interface GigabitEthernet0/1, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/1, changed state to up

Switch(config)#
```
Configure Router In this step Then follow the Rest of the switch code...
```
Switch(config)#
Switch(config)#interface gi 0/1
Switch(config-if)#switchport mode trunk

Switch(config-if)#
%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/1, changed state to down

%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/1, changed state to up

Switch(config-if)#
Switch(config-if)#exit
Switch(config)#exit
Switch#
%SYS-5-CONFIG_I: Configured from console by console

Switch#
Switch#show vlan brief

VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    Fa0/1, Fa0/6, Fa0/7, Fa0/8
                                                Fa0/9, Fa0/10, Fa0/11, Fa0/12
                                                Fa0/13, Fa0/14, Fa0/15, Fa0/16
                                                Fa0/17, Fa0/18, Fa0/19, Fa0/20
                                                Fa0/21, Fa0/22, Fa0/23, Fa0/24
                                                Gig0/2
2    CSE                              active    Fa0/2, Fa0/3
3    IT                               active    Fa0/4, Fa0/5
1002 fddi-default                     active    
1003 token-ring-default               active    
1004 fddinet-default                  active    
1005 trnet-default                    active    
Switch#
```
### Router Configuration:
```
Router>
Router>en
Router#config terminal
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#interface gi 0/0
Router(config-if)#no shutdown

Router(config-if)#
%LINK-5-CHANGED: Interface GigabitEthernet0/0, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0, changed state to up

Router(config-if)#interface gi 0/0.1
Router(config-subif)#
%LINK-5-CHANGED: Interface GigabitEthernet0/0.1, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0.1, changed state to up

Router(config-subif)#encapsulation dot1q 2
Router(config-subif)#ip address 192.168.10.1 255.255.255.0
Router(config-subif)#exit
Router(config)#interface gi 0/0.2
Router(config-subif)#
%LINK-5-CHANGED: Interface GigabitEthernet0/0.2, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0.2, changed state to up

Router(config-subif)#encapsulation dot1q 3
Router(config-subif)#ip address 192.168.11.1 255.255.255.0
Router(config-subif)#exit
Router(config)#
```
Now go back and complete switch code
