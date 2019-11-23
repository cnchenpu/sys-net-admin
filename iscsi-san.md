
# iSCSI SAN

![](fig/iscsi-storage.jpg)

## iSCSI SAN Terminology

Item |Description
---|---
Target | The service on an iSCSi server that gives access to backend storage devices.
Backend Storage | The storage devices on the iSCSI target that the iSCSI target component is providing access to
Initiator | The iSCSi client that connects to a target and is identified by IQN
IQN | The iSCSI qualified name. A unique name that is used for identifying targets as well as initiators
TPG | The Target Portal Group. This is the collection of the IP Address and TCP ports to which a specific iSCSI target will listen.
Portal | The IP address and port that a target or initiator uses to establish connections
ACl | The access control list that is based on the iSCSI initiator IQN and used to provide access to specific user
LUN | A Logical Unit Number. The backend storage devices that are shared through the target. This can be any device that supports read/write operations, such as disk, partitions, logical volumes, files or tape drves

## Setting Up the iSCSI Target on RHEL 7

The tool for iSCSI Target is ``targetcli``.

```=
/> ls
o- / ................................................................ [...]
  o- backstores ..................................................... [...]
  | o- block ......................................... [Storage Objects: 0]
  | o- fileio ........................................ [Storage Objects: 0]
  | o- pscsi ......................................... [Storage Objects: 0]
  | o- ramdisk ....................................... [Storage Objects: 0]
  o- iscsi ................................................... [Targets: 0]
  o- loopback ................................................ [Targets: 0]
```

1. Create the backing storage devices.
   ```=
   /> cd backstores/block 
   /backstores/block> create name=sdb dev=/dev/sdb
   ```
   ```=3
   /backstores/block> ls
   o- block .......................................... [Storage Objects: 1]
     o- sdb .................. [/dev/sdb (256.0MiB) write-thru deactivated]
       o- alua ........................................... [ALUA Groups: 1]
         o- default_tg_pt_gp ............... [ALUA state: Active/optimized]
   /backstores/block>
   ```

2. Create the IQN and default target portal group (TPG).
   
   The valid IQN must be unique in LAN. It starts with ``iqn``, which is followed by the year and month it was created and the reverse DNS name.

   ```=
   /backstores/block> cd /
   /> cd iscsi
   /iscsi> create wwn=iqn.2019-12.edu.pu:csie
   Created target iqn.2019-12.edu.pu:csie.
   Created TPG 1.
   Global pref auto_add_default_portal=true
   Created default portal listening on all IPs (0.0.0.0), port 3260.
   /iscsi>
   ```
   ```=9
   /iscsi> ls
   o- iscsi .................................................. [Targets: 1]
     o- iqn.2019-12.edu.pu:csie ................................. [TPGs: 1]
       o- tpg1 ..................................... [no-gen-acls, no-auth]
         o- acls ................................................ [ACLs: 0]
         o- luns ................................................ [LUNs: 0]
         o- portals .......................................... [Portals: 1]
           o- 0.0.0.0:3260 ........................................... [OK]
   /iscsi> 
   ```

3. Configure one or more ACLs for the TPG.
   
   ```=
   /iscsi> cd iqn.2019-12.edu.pu:csie/
   /iscsi/iqn.2019-12.edu.pu:csie> ls
   o- iqn.2019-12.edu.pu:csie ................................... [TPGs: 1]
     o- tpg1 ....................................... [no-gen-acls, no-auth]
       o- acls .................................................. [ACLs: 0]
       o- luns .................................................. [LUNs: 0]
       o- portals ............................................ [Portals: 1]
         o- 0.0.0.0:3260 ............................................. [OK]
   /iscsi/iqn.2019-12.edu.pu:csie>
   ```
   ```=10
   /iscsi/iqn.2019-12.edu.pu:csie> cd tpg1/acls 
   /iscsi/iqn.20...sie/tpg1/acls> ls
   o- acls ...................................................... [ACLs: 0]
   ```
   ```=13
   /iscsi/iqn.20...sie/tpg1/acls> create wwn=iqn.2019-12.edu.pu:node1
   Created Node ACL for iqn.2019-12.edu.pu:node1
   /iscsi/iqn.20...sie/tpg1/acls>
   ```
   ```=16
   /iscsi/iqn.20...sie/tpg1/acls> ls
   o- acls ...................................................... [ACLs: 1]
     o- iqn.2019-12.edu.pu:node1 ......................... [Mapped LUNs: 0]
   /iscsi/iqn.20...sie/tpg1/acls>
   ```
   ```=20
   /iscsi/iqn.20...sie/tpg1/acls> cd /
   /> ls
   o- / ............................................................. [...]
     o- backstores .................................................. [...]
     | o- block ...................................... [Storage Objects: 1]
     | | o- sdb .............. [/dev/sdb (256.0MiB) write-thru deactivated]
     | |   o- alua ....................................... [ALUA Groups: 1]
     | |     o- default_tg_pt_gp ........... [ALUA state: Active/optimized]
     | o- fileio ..................................... [Storage Objects: 0]
     | o- pscsi ...................................... [Storage Objects: 0]
     | o- ramdisk .................................... [Storage Objects: 0]
     o- iscsi ................................................ [Targets: 1]
     | o- iqn.2019-12.edu.pu:csie ............................... [TPGs: 1]
     |   o- tpg1 ................................... [no-gen-acls, no-auth]
     |     o- acls .............................................. [ACLs: 1]
     |     | o- iqn.2019-12.edu.pu:node1 ................. [Mapped LUNs: 0]
     |     o- luns .............................................. [LUNs: 0]
     |     o- portals ........................................ [Portals: 1]
     |       o- 0.0.0.0:3260 ......................................... [OK]
     o- loopback ............................................. [Targets: 0]
   />
   ```

4. Create LUNs to provide access to the backing storage devices.
   
   ```=
   /> cd iscsi/iqn.2019-12.edu.pu:csie/tpg1/luns 
   /iscsi/iqn.20...sie/tpg1/luns> ls
   o- luns ...................................................... [LUNs: 0]
   ```
   ```=4
   /iscsi/iqn.20...sie/tpg1/luns> create /backstores/block/sdb 
   Created LUN 0.
   Created LUN 0->0 mapping in node ACL iqn.2019-12.edu.pu:node1
   /iscsi/iqn.20...sie/tpg1/luns>
   ```
   ```=8
   /iscsi/iqn.20...sie/tpg1/luns> ls
   o- luns ...................................................... [LUNs: 1]
     o- lun0 .................... [block/sdb (/dev/sdb) (default_tg_pt_gp)]
   /iscsi/iqn.20...sie/tpg1/luns>
   ```
   ```=12
   /iscsi/iqn.20...sie/tpg1/luns> exit
   Global pref auto_save_on_exit=true
   Last 10 configs saved in /etc/target/backup/.
   Configuration saved to /etc/target/saveconfig.json
   ```

5. To open port 3260 in the firewall for iSCSI connections.
   
   ```=
   # firewall-cmd --add-port=3260/tcp --permanent
   # firewall-cmd --reload
   ```

6. Start and enable target service

   ```=
   # systemctl start target
   # systemctl enable target
   ```

## Setting Up the iSCSI Initiator on RHEL 6

The tool for iSCSI Initiator is ``iscsiadm`` which is in the ``iscsi-initiator-utils`` RPM.

1. Setting the iSCSI Initiator name in ``/etc/iscsi/initiatorname.iscsi``
   
   ```
   InitiatorName=iqn.2019-12.edu.pu:node1
   ```

2. Start ``iscisd`` if it is not started.

3. Discover the target.
   ```
   # iscsiadm -m discovery -t st -p <server-IP>
   ```
   ```=
   # iscsiadm -m discovery -t st -p 192.168.0.163
   192.168.0.163:3260,1 iqn.2019-12.edu.pu:csie
   ```

4. Login the target.
   ```=
   # iscsiadm -m node -l
   Logging in to [iface: default, target: iqn.2019-12.edu.pu:csie, portal: 192.168.0.163,3260] (multiple)
   Login to [iface: default, target: iqn.2019-12.edu.pu:csie, portal: 192.168.0.163,3260] successful.
   ```

5. Use ``fdisk -l`` to get the LUN.   

  
