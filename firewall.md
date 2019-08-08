# Firewall
## firewalld v.s. iptables

### firewalld

### firewall-cmd

```bash
$ firewall-cmd --state
```

### zone
id|zone|comment
---|---|---
1|public|For use in public areas. You do not trust the other computers on the network. Only selected incoming connections are accepted.
2|external|Public area, for NAT.





* block
* dmz
* drop
* external
* home
* internal
* public
* trusted
* work

```bash
$ firewall-cmd --get-active-zone

$ firewall-cmd --get-zone

$ firewall-cmd --zone=public --list-all
```