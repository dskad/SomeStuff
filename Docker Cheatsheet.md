# **Docker Cheatsheet**

## Images

<https://docs.docker.com/engine/reference/commandline/image>

| Description               | Command                                          |
| ------------------------- | ------------------------------------------------ |
| List downloaded images    | ```docker images```                              |
| List downloaded images    | ```docker image ls```                            |
| Delete image              | ```docker image rm <image name or image hash>``` |
| Delete all unussed images | ```docker image prune -a```                      |
| Pull image from registry  | ```docker pull <image name>```                   |

---

## Run a container

<https://docs.docker.com/engine/reference/commandline/run>

### Basic command structure

```shell
docker [options] [image name or hash] [optional command to run inside container]
```

### Option Summary

| Option                                    | Description                                                                  |
| ----------------------------------------- | ---------------------------------------------------------------------------- |
| -p [outside port]:[inside port]           | Map a port inside the container to outside the container                     |
| -v [host directory]:[container directory] | Map contents of local directory into a directory inside the container        |
| --rm                                      | Remove the container after the container stops                               |
| --env or -e VAR1=value1                   | Set an environment variable inside the container                             |
| -i -t  or -it                             | Open an interactive terminal inside the container                            |
| -d                                        | Detach and run container in the back ground. (use `docker logs` to see logs) |

### Examples

- **Run a basic Ubuntu container and open a command prompt inside the container**

  ```bash
  docker run --rm -it ubuntu bash
  ```

- **Run an Nginx web server**
  - map the html folder in the current directory into /usr/share/nginx/html in the container
  - Make port 80 inside the container available as port 8888 on the host

  ```bash
  docker run --rm -p 8888:80 -v (pwd)/html:/usr/share/nginx/html -d nginx
  ```

