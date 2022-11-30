# ONTAP Storage Architecture
  - ONTAP is NetApp's proprietary operating system used in stotage arrays and cloud volumes
  - ONTAP stand for Open Network Technology for Appliance Products (known as Clustered Data)

### Components
  - <b>LUNs</b> (Logical Unit Number): Specific to SAN protocols, goes into Volume or QTree level.
  - <b>QTrees</b>: Used to configure quotas, optional to configure, and goes in volumes.
  - <b>Volume</b>: Lowest level that the clients can access their data, you can have a single volume or multiple volumes in aggregates.
  - <b>Aggregates</b>: A collection of drives, attributes; RAID groups (parity, data).
  - <b>Disks</b>: Physcial hard drives.
 
![ONTAP](https://user-images.githubusercontent.com/111991325/204441334-44c88af7-640d-4ecd-b73e-c37b5cfbfad9.png)

### Storage Virtual Machines (VServers)
 ![SVM](https://user-images.githubusercontent.com/111991325/204441665-ad35b5e8-7c90-4a27-b344-f8fcdacef56e.png)

# Administration Components
### Node Root Volume 
  - Uses <i>Aggr0</i> and <i>Vol0</i>, exists on every node in the cluster.
  - System information is replicated between node over the cluster network
  - No user data is stored on Vol9, used for system information only

## Replicated DataBase (RDB)
  - Consist of 5 components:
    - Management Gateway
      - Provide the management CLI
      - Cluster is managed as a whole, configuring nodes individually is not needed.
    - Volume Location Database
      - Lists which aggregate contains each volume and which node contains each aggregate.
      - Keeps track of where volumes are located.
    - Virtual Interface Manager
      - IP addresses live on a logical interface (LIF)
      - Lists which pyshical interface the logical interfaces are on.
      - Durning a failover, LIFs can move to a different physical interface.
    - Blocks Configuration and Operations Management (BCOM)
      - BCOM store information for SAN protocols, includes info on LUNs and iGroups
    - Configuration Replication Service (CRS)
      - Used by MetroCluster to replicate configuration and operational data to the remote secondary cluster
  - One node in the cluster will be elected the replication master for each of the RDB units.
  - The same node will be master for all units, but can change in the event of any node failover.

## Admin SVM
  - This is created during system set up, purely for management access.
  - Admin SVM owns the Cluster Management LIF, this is the "global" address.
  - Usually a one-time use to access the device and configure a cluster.

## Node SVM
  - Node Management use for management access and create during system setup.
  - Node SVM owns the Node Management LIF for the node, can be used to manage the entire cluster.

# Bootup Process 
## The Boot Device Image
- Primary Image, the primary bootup process. 
- Secondary Image, serves as a backup image to rollback to in the bootloader menu.

## Boot Order
1. Firmware Loader, you can break into the firmware CLI at this point.
2. Data ONTAP System Image from CompactFlash, you can break into DataONTAP Boot Menu at this point.
3. System configuration from Vol0 in Aggr0
4. Boot Complete
- Summary: Firmware -> OS (ONTAP) -> Configurations -> Complete
