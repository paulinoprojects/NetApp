# NetApp Hardware Platforms
  - <b>FAS (Fabic Attached Storage)</b>: Hybrid storage; supports SSD and HDs
  - <b>AFF (All Flash FAS)</b>: SSD only
  - Both platforms can be included in the same cluster and are managed the same way.
  - External disk shelves can be added to FAS and AFF for capacity expansion.

# Clustered ONTAP Hardware Atchitecture
![Clustered (2)](https://user-images.githubusercontent.com/111991325/204424966-c753891e-523d-48d0-a11e-3923da8a6294.png)

  - A cluster is managed as a single system, called SVM (Storage Virtual Machines or Vservers).
  - The cluster can be virtualized into different storage systems, SVMs appear as a single system to clients.
  - Data processesing is spread throughout the different nodes in the cluster.
  - Data can be mirrored or cached across multiple nodes in the cluster.
  
# NetApp Storage System Networks
![NetApp Networks](https://user-images.githubusercontent.com/111991325/204425945-db34eeaf-438d-47f7-b568-7235e71e7146.png)

### Cluster Network
  - Cluster network is used for internal system communications between nodes in the cluster.
  - System configurations changes is replicated on all nodes over the cluster network.
  - Uses IP over Ethernet
### Management Network
  - Management network is used for administration of the cluster
  - Uses SSH for CLI and HTTPS for GUI
  - Uses IP over Ethernet and port e0M
### Data Network
  - Data network is used for client access to the cluster
  - Protocol could be SMB, NFS, Fiber CHannel, iSCSI, FCoE, NVMeOF
  - Physical infrastructure will be Ethernet or Fiber
  - <b>Data Protection traffic </b>: replication of data to another cluster for backup and disaster recvoery
### HA (High Avalability) Connection
  - HA Connection inside the chassis that sends keepalives and NVRAM mirroring between the controllers
