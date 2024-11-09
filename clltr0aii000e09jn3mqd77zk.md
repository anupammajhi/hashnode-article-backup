---
title: "LINUX: Shell Scripting Exercises"
datePublished: Thu Mar 22 2018 20:46:36 GMT+0000 (Coordinated Universal Time)
cuid: clltr0aii000e09jn3mqd77zk
slug: linux-shell-scripting-exercises
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1693328197578/65da373c-67dc-4fd4-a922-50849d12bd46.png
tags: linux, practice, shell, scripting, exercise

---

> *Note : These exercises are from “Linux Training Academy’s” Shell Scripting course.*

**Exercise 1:** Write a shell script that prints “Hello World!” to the screen.

Hint 1: Remember to make the shell script executable with the chmod command.  
Hint 2: Remember to start your script with a shebang!

```bash
#!/bin/bash
echo "Hello World!"
```

**Exercise 2:** Modify the shell script from exercise 1 to include a variable. The variable will hold the contents of the message “Hello World!”.

```bash
#!/bin/bash
THE_MESSAGE="Hello World!"
echo "$THE_MESSAGE"
```

**Exercise 3**: Store the output of the command “hostname” in a variable. Display “This script is running on *.” where “* “ is the output of the “hostname” command.

Hint: It’s a best practice to use the ${VARIABLE} syntax if there is text or characters that directly precede or follow the variable.

```bash
#!/bin/bash
THE_HOSTNAME=$(hostname)
echo "The script is running on $THE_HOSTNAME"
```

**Exercise 4**: Write a shell script to check to see if the file “/etc/shadow” exists. If it does exist, display “Shadow passwords are enabled.” Next, check to see if ou can write to the file. If you can, display “You have permissions to edit /etc/shadow.” If you cannot, display “You do NOT have permissions to edit /etc/shadow.”

```bash
#!/bin/bash
 
THE_PATH="/etc/shadow"
 
if [ -e $THE_PATH ]
then
     echo "Shadow Passwords are enabled"
 
     if [ -w $THE_PATH ]
     then
          echo "You have permission to edit $THE_PATH"
     else
          echo "Tou do NOT have permission to edit $THE_PATH"
     fi
else
     echo "Shadow Passowrds not enabled"
fi
```

**Exercise 5**: Write a shell script that displays “man”, “bear”, “pig”, “dog”, “cat”, and sheep to the screen with each appearing on a separate line. Try to do this in as few lines as possible. Hint: Loops can be used to perform repetitive tasks.

```bash
#!/bin/bash
 
ANIMALS="man bear pig dog cat sheep"
 
for ANIMAL in $ANIMALS
do
        echo "$ANIMAL"
done
```

**Exercise 6**: Write a shell script that prompts the user for a name of a file or directory and reports if it is a regular file, a directory, or other type of file. Also perform an ls command against the file or directory with the long listing option.

```bash
#!/bin/bash
 
read -p "Enter a path for file/folder : " THE_PATH
 
if [ -d $THE_PATH ]
then
        echo "$THE_PATH is a directory"
elif [ -f $THE_PATH ]
then
        echo "$THE_PATH is a simple file"
elif [ -e $THE_PATH ]
then
        echo "$THE_PATH is not a simple file"
else
        echo "$THE_PATH does NOT exist!!"
fi
 
echo "$(ls -l $THE_PATH)"
```

**Exercise 7**: Modify the previous script so that it accepts the file or directory name as an argument instead of prompting the user to enter it.

```bash
#!/bin/bash
 
THE_PATH=$1
 
if [ -d $THE_PATH ]
then
        echo "$THE_PATH is a directory"
elif [ -f $THE_PATH ]
then
        echo "$THE_PATH is a simple file"
elif [ -e $THE_PATH ]
then
        echo "$THE_PATH is not a simple file"
else
        echo "$THE_PATH does NOT exist!!"
fi
 
echo "$(ls -l $THE_PATH)"
```

**Exercise 8**: Modify the previous script to accept an unlimited number of files and directories as arguments. Hint: You’ll want to use a special variable.

```bash
#!/bin/bash
 
for THE_PATH in $@
do
if [ -d $THE_PATH ]
then
        echo "$THE_PATH is a directory"
elif [ -f $THE_PATH ]
then
        echo "$THE_PATH is a simple file"
elif [ -e $THE_PATH ]
then
        echo "$THE_PATH is not a simple file"
else
        echo "$THE_PATH does NOT exist!!"
fi
 
echo "$(ls -l $THE_PATH)"
 
done
```