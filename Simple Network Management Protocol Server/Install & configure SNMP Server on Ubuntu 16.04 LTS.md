
## 1. SNMP Server on Ubuntu 16.04 LTS

## Installation

**1.** Update Ubuntu, download and install SNMP:

    sudo apt-get update
    sudo apt-get install snmp snmpd
_**Note** Current distributions, for example Debian - Ubuntu, incorporate and the net-snmp package so that its installation is much simpler ._
## Configurations

The file containing the settings is the snmpd.conf file.

  **1.** Open the **snmpd.conf** file from file from **/etc/snmp/**
  
      sudo nano /etc/snmp/snmpd.conf
     
 
 
  **2.** Search in the **agents section** the following configuration:
    
  * You'll see this snippet part:
    ```
      # AGENT BEHAVIOUR
        ...
         #  Listen for connections from the local system only
     agentAddress  udp:127.0.0.1:161
       #  Listen for connections on all interfaces (both IPv4 *and* IPv6)
     #agentAddress udp:161,udp6:[::1]:161
        ...
    ```
        
  * Then, you have comment **agentAddress  udp:127.0.0.1:161** and uncomment **agentAddress udp:161,udp6:[::1]:161**, you'll have then this snippet configuration:
        
    ```
      # AGENT BEHAVIOUR
        ...
         #  Listen for connections from the local system only
     #agentAddress  udp:127.0.0.1:161
       #  Listen for connections on all interfaces (both IPv4 *and* IPv6)
     agentAddress udp:161,udp6:[::1]:161
        ...
    ```
  **3.** Search in the **access control section** the following configuration:
    
  * You'll see this snippet part:
    ```
      # ACCESS CONTROL
        ...
                                    # system + hrSystem groups only
            view   systemonly  included   .1.3.6.1.2.1.1
            view   systemonly  included   .1.3.6.1.2.1.25.1
        ...
    ```
        
  * Then, add the new view for all groups, you'll have then this snippet configuration:
        
    ```
      # ACCESS CONTROL
        ...
                                    # system + hrSystem groups only
            view   systemonly  included   .1.3.6.1.2.1.1
            view   systemonly  included   .1.3.6.1.2.1.25.1
            view    all     included      .1
        ...
    ```
    
    **4.** In the same section **access control**:
    
  * You'll see this snippet part:
    ```
      # ACCESS CONTROL
        ...
            #rocommunity public  localhost
                                                 #  Default access to basic system info
      # rocommunity public  default    -V systemonly
                                                 #  rocommunity6 is for IPv6
      # rocommunity6 public  default   -V systemonly
        ...
    ```
        
  * Then, add the new community, you'll have then this snippet configuration:
        
    ```
      # ACCESS CONTROL
        ...
            #rocommunity public  localhost
                                                 #  Default access to basic system info
      # rocommunity public  default    -V systemonly
                                                 #  rocommunity6 is for IPv6
      # rocommunity6 public  default   -V systemonly
            
            rocommunity publica  default    -V all
            rocommunity publica  127.0.0.1
            rocommunity publica  192.168.0.0/24
        ...
    ```
* 127.0.0.1 for localhost
* 192.168.0.0/24 to enable access to all the network

 **5.** Search in the **system information section** the following configuration:
    
  * You'll see this snippet part:
    ```
      # SYSTEM INFORMATION
        ...
            sysLocation    Unknown
      sysContact     Me <me@exaple.org>
        ...
    ```
        
  * Then, add your location and contact, you'll have then this snippet configuration:
        
    ```
      # SYSTEM INFORMATION
        ...
            sysLocation    I'm the SERVER
      sysContact     Admin <master@snmp.mx>
        ...
    ```

**6.** Save and restart service
 
    sudo service snmpd restart
_Note Always restart service if you modified the snmpd.conf file._

**7.** Test
 
    snmpwalk -v 2c -c publica -O e localhost > Desktop/test.txt
    
* publica: community name
* localhost: machine ip


_Note You will a new file in your desktop named test.txt, these file contains response from machine._

##
### For more help, you can consult the documentation and the manuals:
* [snmpd.conf](https://linux.die.net/man/5/snmpd.conf)
* [net-snmp](http://www.net-snmp.org/)
