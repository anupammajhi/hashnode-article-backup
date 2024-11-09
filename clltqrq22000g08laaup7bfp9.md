---
title: "WINDOWS: Deleting Orphaned DFS Share"
datePublished: Tue Jul 28 2015 08:22:00 GMT+0000 (Coordinated Universal Time)
cuid: clltqrq22000g08laaup7bfp9
slug: windows-deleting-orphaned-dfs-share
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1693326746943/0595a8bf-864c-43ce-bd3d-d08cfb7c3913.png
tags: error, windows, dfs, 2008, 2012

---

Often stuck with an orphaned shared folder? I did, many times. Moreover, when I go ahead and try to delete it, I am hit with an error message saying the share must be removed from the distributed file system before it can be deleted.

There is an easy way out:

Either find the registry entry for that DFS under **HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Microsoft\\DFSRoots\\Domain**   
and delete the key named with the DFS name.

OR

If you are in love with the command line like me, run the command

```powershell
dfsutil /clean /server: /share:
```

The next step is to go to services and restart the service called “Server”. The “DFS Namespaces” service is dependent on it, so go ahead and accept the prompt that says this will restart DFS Namespaces.  
 Now go back to the share and remove it without any problems.