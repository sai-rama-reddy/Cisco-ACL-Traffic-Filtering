##### **ACLs Configurations:**



###### **ACL Configuration On R1:**

* access-list 110 deny ip 172.16.0.10 0.0.0.0 8.8.8.8 0.0.0.0
* access-list 110 permit ip any any
* int g2/0
* ip access-group 110 in



###### **Basic Configuration On R1:**

* int g1/0
* ip add 192.168.0.1 255.255.255.0
* no shut



* int g2/0
* ip add 172.16.0.1 255.255.0.0
* no shut



* int f0/0
* ip add 10.0.0.1 255.0.0.0
* no shut



###### **Static Routing On R1:**

* ip route 10.0.0.0 255.0.0.0 10.0.0.100
* ip route 8.8.8.8 255.255.255.255 10.0.0.100



###### **Basic Configuration On R2:**

* int g1/0
* ip add 10.0.0.100 255.0.0.0
* no shut



* int lo0
* ip add 8.8.8.8 255.255.255.255



###### **Static Routing On R2:**

* ip route 192.168.0.0 255.255.255.0 10.0.0.1
* ip route 172.16.0.0 255.255.0.0 10.0.0.1

