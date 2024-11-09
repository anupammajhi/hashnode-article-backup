---
title: "LINUX: useradd & usermod : The Linux User Administration"
datePublished: Mon May 16 2016 17:19:30 GMT+0000 (Coordinated Universal Time)
cuid: clltrg8gw000109mr4g3b6mzm
slug: linux-useradd-usermod-the-linux-user-administration
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1693327590398/344eaff8-980f-45f8-914f-98006df50325.png
tags: ubuntu, linux, command-line, access-control, user-management

---

Adding user to a Linux environment is pretty easy!  
Just type **useradd** followed by username. Example:

```bash
useradd anupam
```

But!! the user cannot login yet, because the password needs to be set.  
The password must follow the suggested password rule. To set password, just type **passwd** followed by username and enter the new password ( *needs to be executed as root or SU privileges*). If you are not a SU then you can only change your password with this command.

You can find the password rules, aging rules etc in the file:

```bash
/etc/login.defs
```

To modify existing user use the command **usermod**. It has a lot of options like modifying primary group, supplementary group, lock and unlock user account, move or update user home directory etc. Example, to lock user account:

```bash
usermod -L anupam
```

And finally to delete a user, use the command **userdel**

Pretty interesting eh!