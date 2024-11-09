---
title: "DOCKER: The ENTRYPOINT and CMD"
datePublished: Thu May 17 2018 18:30:00 GMT+0000 (Coordinated Universal Time)
cuid: clpjq3zq7000h09jqcuq078sd
slug: docker-the-entrypoint-and-cmd
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1701259379999/cda70c79-87d0-42ed-b262-815c3012ceae.png
tags: docker, containers, dockerfile, cmd, entrypoint

---

In a Dockerfile, the use of `ENTRYPOINT` and `CMD` is crucial in defining the way a container behaves. However, it does confuse a lot of us regarding the usage, best practices, and key considerations when using these instructions.

## **ENTRYPOINT vs CMD:**

### ENTRYPOINT:

The `ENTRYPOINT` instruction sets the primary command that will executed by the container when it starts. It can be specified with or without an executable and also allows for arguments.

```Dockerfile
ENTRYPOINT ["nginx", "-g", "daemon off;"]
```

`ENTRYPOINT` command cannot be overridden by appending a command while executing 'docker run' at runtime. However, the arguments can be appended.

Example Dockerfile:

```dockerfile
FROM ubuntu
ENTRYPOINT ["echo", "Hello"]
```

Run the container with a command:

```bash
docker run my-image Goodbye
```

Output:

```bash
# output
Hello Goodbye
```

If we really want to overwrite the command in `ENTRYPOINT`, we need to use the '--entrypoint' with docker run:

```bash
# Example
docker run -it --entrypoint /bin/sh my_custom_image
```

### CMD:

The `CMD` instruction sets the default command for the container. If used without an `ENTRYPOINT`, it becomes the default command and arguments that will be used when the container starts. Whereas, when used with an `ENTRYPOINT`, it provides default arguments for the entry point.

This can be overridden though at runtime by passing a command when running the container using 'docker run'.

Example:

```Dockerfile
CMD ["nginx", "-g", "daemon off;"]
```

`CMD` command can be overridden by appending a command while executing 'docker run' at runtime.

Example Dockerfile:

```dockerfile
FROM ubuntu
CMD ["echo", "Hello"]
```

Run the container with a command:

```bash
docker run my-image Goodbye
```

Output:

```bash
# output
Goodbye
```

## **Best Practices:**

We should prefer the exec form of the `ENTRYPOINT` and `CMD` to avoid shell processing.

Example:

```Dockerfile
ENTRYPOINT ["echo", "Hello, $USER"]
```

If we use the insecure way of using shell form instead, it might introduce a security vulnerability. The shell form can be abused to perform shell injection. Additionally, the shell form becomes dependent on the particular shell in use and if a different shell is being used in a distribution, it might not be consistent.

```dockerfile
# An example where $USER can be used for shell injection vulnerability.
# Malicious code can be introduced if the $USER variable is not sanitized.
FROM ubuntu
CMD echo "Hello, $USER" 
```

## **Choosing Between CMD and ENTRYPOINT**

* Use `CMD` when you want to provide default values for the command and allow them to be easily overridden at runtime.
    
* Use `ENTRYPOINT` when you want to set a fixed primary command and allow users to append additional arguments.
    

In many cases, combining both instructions offers a balance of flexibility and definition. Understanding these use cases will empower you to create more versatile and user-friendly Docker images.

## **ENTRYPOINT as a Script:**

If we have a scenario where we want to run a script instead of any particular command, and maybe pass a default argument, we can do it seamlessly with ENTRYPOINT and using CMD along with it.

```dockerfile
FROM node

# Set the working directory
WORKDIR /app

# Copy the application files
COPY . .

# Set the script as the ENTRYPOINT
ENTRYPOINT ["./entrypoint.sh"]

# Default argument 'start' to pass to the script if nothing mentioned in run.
CMD ["start"]
```

## Conclusion:

Mastering Dockerfile entry points empowers you to create efficient, modular, and customizable Docker images. Whether you're building a simple web server or a complex microservices architecture, understanding how to leverage `ENTRYPOINT` and `CMD` is essential for Dockerfile success.

Happy Dockerizing!

---