---
title: "HADOOP: Basic Architecture (Hadoop 2.x)"
datePublished: Wed Sep 05 2018 12:07:35 GMT+0000 (Coordinated Universal Time)
cuid: clltqxe7w000a08l9fhuwfaoc
slug: hadoop-basic-architecture-hadoop-2x
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1693329485075/e5d7dc91-3e4d-43f4-b7ee-e2f189ebd006.png
tags: hadoop, big-data, architecture, apache, node

---

Apache Hadoop has earned quite a traction in the recent past. It is a framework for processing of large datasets in a distributed computing environment.  
   
 All this started back in 2003 when Google released its paper on Google File System. Later the Hadoop project started taking that as a blueprint and it has reached here today.  
   
 Hadoop 1.x had quite a different structure and had certain limitations, which have been addressed in Hadoop 2.x versions with a different architecture with different components to enable more reliable, distributed, parallel processing of Big Data.  
   
 Hadoop comprises of 2 main core components:  
 1. HDFS (Hadoop Distributed File System) — Storage Unit  
 2. YARN (Yet Another Resource Negotiator) — Processing Unit

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1693088558134/8eb1c296-5372-47b4-8181-31b0b0942985.png align="center")

### HDFS

HDFS comprises of 3 components.

**Name Node**

The name Node is the ‘master’ component of HDFS system. It stores the metadata of HDFS, which is essentially the directory tree of the files in HDFS, and a track of all the files across the cluster.

The name node does not store the actual data. It knows the list of blocks and locations for any given file.

When the name node is down, the cluster itself is considered down as the cluster becomes inaccessible.

It stores fsimage(snapshot of the filesystem at start-up) and edit logs (sequence of changes made to the filesystem after start-up).

**Secondary Name Node**  
   
The secondary Name node is NOT a backup name node as the name might suggest. It is more of an assistive node.  
   
It is responsible for taking checkpoints of the file system metadata present in the name node. It does this by checkpointing fsimage. It gets the edit logs from the name node in regular intervals and creates the checkpoint. Then it copies this fsimage back to the name node.  
   
The name node uses this fsimage in the next start-up so that it reduces start-up time.

**Data Node**  
   
This is where the actual data is stored. The name node keeps track of each block that is stored in these data nodes. It is also known as the ‘slave’.  
   
When it starts up, it announces its presence to name node and also the data blocks it is responsible for. This is the workhorse of Hadoop HDFS.

### YARN

YARN comprises of 2 components:  
   
**Resource Manager**  
   
The resource Manager is responsible for taking inventory of available resources and running critical services, the most critical of which is the Scheduler.  
   
The scheduler allocates resources. It negotiates the available resources in the cluster and manages the distributed processing. It works along with the Node Manager to attain this.

**Node Manager**  
   
The node manager acts as the ‘slave’ to the Resource Manager.

It keeps track of the tasks and jobs that are being deployed to the data nodes. It helps the Resource manager keep track of available space and processing power. Memory, bandwidth, etc. so that tasks can be distributed to the data nodes.