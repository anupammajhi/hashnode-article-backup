---
title: "HIVE: Basic Architecture and Components"
datePublished: Wed Jul 19 2017 18:55:44 GMT+0000 (Coordinated Universal Time)
cuid: clltr2r0i000f09jnd9v72xky
slug: hive-basic-architecture-and-components
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1693088551585/7313a92a-58b8-435a-9edd-43f4c179b07a.jpeg
tags: big-data, architecture, components, basics, hive

---

### Hive Architecture

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1693088549570/6a60d389-644e-4ef2-9c78-bfcc1d42a98b.jpeg align="center")

The Hive Architecture comprises 3 main components:

1. **Hive Client**  
     This is where the applications get an interface to interact with hive.
    
2. **Hive Services**  
     Hive services enable the hive interactions by passing them through the hive driver which in turn uses MapReduce.
    
3. **Compute and Storage**  
     This is the workhorse of the Hive ecosystem which includes the Metastore DB and HDFS storage.
    

### About the Components

**Hive Client**

Hive client is the interface for different applications and clients which makes it possible for different applications to communicate with Hive.

Hive client comprises Thrift Client, JDBC Client and ODBC Client.

Thrift Client enables Thrift based applications to communicate with Hive. JDBC and ODBC enable several applications and languages to make connections and process with hive.

These clients in turn connect to the Hive Server to make this possible.

**Hive Services**

Client interactions are made possible by Hive services.

CLI service enables us to interact with Hive via command line.

Web based service makes it possible to have a web based interaction with Hive.

The Hive server service is one of the most important and most used service which most of the Client services connect to.

**Hive Storage and Compute**

Hive storage used MapReduce in the background which makes it possible for hive to process Big Data.

Hive stores meta information in the Hive Metastore which is the backbone of Hive.

By default, Hive uses Derby DB for storing metadata, but any other RDBMS can be used for this purpose.

However, the data resides on either Local storage or HDFS.