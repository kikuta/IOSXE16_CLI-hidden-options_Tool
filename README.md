# 1. Overview: Enjoy IOS XE hidden options
* According to the release note of 16.8.1a, "Accessing Hidden Commands" is documented as a new feature. Now hidden command options will be exposed if "service internal" is configured.
* [Release note](https://www.cisco.com/c/en/us/td/docs/switches/lan/catalyst9300/software/release/16-8/release_notes/ol-16-8-9300.html#id_65400)
* "service internal" should not be configured as a default because it is almost for testing, trouble shooting, or evaluation.
* This script is for us who are just interested in hidden options. Easy to know how many and what kind of hidden options are available with the IOS XE booted.
* Having fun is important eventhough it is meaningless technically. If it is hidden, we want to know what it is. Enjoy!

# 2. Accessing Hidden Commands feature - 16.8.1a
## Example
### 2.1 Normal view
7 options are found for "show processes" IOS command.
```
Cat9300-01#show processes ?
  <1-2147483647>  IOS(d) Process Number
  cpu             Show CPU usage per IOS(d) process
  heapcheck       Show IOS(d) scheduler heapcheck configuration
  history         Show ordered IOS(d) process history
  memory          Show memory usage per IOS(d) process
  platform        Show information per IOS-XE process
  timercheck      Show IOS(d) processes configured for timercheck
  |               Output modifiers
  <cr>            <cr>
```

### 2.2 Configure "service internal" for "Accessing Hidden Commands"
```
Cat9300-01(config)#service internal
```

### 2.3 Some hidden options are exhibited
11 options are found after "service internal" is configured. So 4 options are IOS hidden options.
```
Cat9300-01#show processes ?
  <1-2147483647>  IOS(d) Process Number
  all-events      Show all notifications
  bootup-init     Show system init time
  bootup-time     Show system bootup time
  cpu             Show CPU usage per IOS(d) process
  events          Show events for which IOS(d) processes want notification
  heapcheck       Show IOS(d) scheduler heapcheck configuration
  history         Show ordered IOS(d) process history
  memory          Show memory usage per IOS(d) process
  platform        Show information per IOS-XE process
  timercheck      Show IOS(d) processes configured for timercheck
  |               Output modifiers
  <cr>            <cr>
```

# 3. How this script works
With this script, it is easy to find how many and what kind of options for IOS CLI are available. The script works on IOS XE Guestshell, so IOS XE Guestshell must be configured and the script should be on the  flash of routers/switches.

## Prerequisite: IOS XE Guestshell
IOS Guestshell must be enabled as follow previously. hiddenop.py are put on flash of Cat9300 for this example. Python2.7 is provided as default with IOS XE Guestshell so we do not have to install any program.

```
Cat9300-01#sh ver | i IOSXE  
Cisco IOS Software [Fuji], Catalyst L3 Switch Software (CAT9K_IOSXE), Version 16.8.1a, RELEASE SOFTWARE (fc1)
*    1 40    C9300-24P          16.8.1a           CAT9K_IOSXE           INSTALL
Cat9300-01#
Cat9300-01#guestshell run bash
[guestshell@guestshell ~]$
[guestshell@guestshell ~]$ uname -a
Linux guestshell 4.4.111 #1 SMP Wed Feb 14 20:30:50 PST 2018 x86_64 x86_64 x86_64 GNU/Linux
[guestshell@guestshell ~]$
[guestshell@guestshell ~]$ python -V
Python 2.7.5
[guestshell@guestshell ~]$ cd /flash/kikuta
[guestshell@guestshell kikuta]$
[guestshell@guestshell kikuta]$ ls
hiddenop.py
```

## 3.1 Example: Running script from guestshell
The script is executed on the guestshell directly as follow.
```
[guestshell@guestshell kikuta]$ python hiddenop.py
Please input exec command with ? >show ip ospf ?    

 *** Hidden CLI Information - ###  show ip ospf ?  ### ***

bad-checksum
delete-list
internal
maxage-list
route-list

 ***************
Number of Hidden Command in exec mode is : 5
 ***************
[guestshell@guestshell kikuta]$
```

## 3.2 Example: Running script from IOS XE
The script is also executed from IOS XE exec mode and it might be more practical.

```
Cat9300-01#guestshell run python hiddenop.py
Please input exec command with ? >show ip bgp ?

 *** Hidden CLI Information - ###  show ip bgp ?  ### ***

aspath
attr
dampened-paths
flap-statistics
internal
per-ce-label-internal
rcache
rrinfo

 ***************
Number of Hidden Command in exec mode is : 8
 ***************

Cat9300-01#
```

## Reference
Japanese article is also available on [Qiita](https://qiita.com/kikuta1978/items/12eb9aeec3ec6444a73a).
