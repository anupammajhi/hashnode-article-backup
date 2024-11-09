---
title: "LINUX: locale & localectl — The Linux Locale Settings"
datePublished: Mon May 16 2016 16:27:51 GMT+0000 (Coordinated Universal Time)
cuid: clltrjyrp00040ami8w1g91h8
slug: linux-locale-localectl-the-linux-locale-settings
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1693088522552/4204a7a2-fe7d-4380-b29a-030a0a87da17.jpeg
tags: linux, bash, command-line

---

You can get or set the **locale** settings in a linux environment (tried in fedora) by using the command :

```bash
localectl
```

You can remember it as **Locale Control**

You can also get more locale information by simply executing the command:

```bash
locale
```

To learn all the possible locale languages type this in terminal:

```bash
locale -a
```

To set locale (to change locale language setting), you can do something like this example:

```bash
localectl set-locale LANG=fr_FR.utf8
```

Also to temporarily to execute any command in a different language, prefix the command with the LANG=&lt;appropriate languagr&gt; &lt;command&gt;. For example:

```bash
LANG=fr_FR.utf8 date
```

To learn more about locale always take help of documentations.