# A Docker Cheat-Sheet

## Terminology
| Command | Action |
| :------- | :------- |
| Images | The blueprints of the application.  Very roughly approximates to a git registry.  Containers are created from images. Images can be base images (i.e. not based on another image; typically just an OS), or child images (i.e. based on a base image; typically adds functionality to base) |
| Containers | A running instance of an application. |
| Docker Daemon | Local background service that mnages building, running and distributing of containers.  |
| Docker Client | The command line tool used to interact with the daemon (GUIs also exist). |
| Docker Hub | A registry of images.  A central registry [exists](https://hub.docker.com/search?q=&type=image), however they can also be hosted locally. |
| <img width="400"/> | <img width="400"/> |

## Commands

| Command | Action |
| :------- | :------- |
| `$ docker images`| List all local images |
| `$ docker pull [image name][:version]`| Pull an image from the Docker Hub (either latest or specified '[:version]'.  To use a local registry, specify the full server name and path: `myregistry.local:5000/testing/test-image` |
| `$ docker run [-d] [-P] [--name [container name]] [image name] [command|-it] [--rm]`| Launches a container from the specified image and runs a command or opens an `sh` shell [`-d`= run container in background; `-P` = publish any internal ports to random external ports; `--rm` = delete container on exit) |
| `$ docker run -p [external port: internal port; e.g. 8888:80] [image name]` | `-p` Specify a custom port to which the client will forward connections to the container. |
| `$ docker port [container name]` | Display ports exposed by container |
| `$ docker stop [container id]`| Stop a container |
| `$ docker ps [-a]`| List all running containers (`-a` = include recently stopped containers) |
| `$ docker rm [container id]`| Delete a container |
| `$ docker container prune`| Delete all stopped containers |
| `$ docker rm $(docker ps -a -q -f status=exited)`| [legacy syntax] Delete all stopped containers |
| <img width="400"/> | <img width="400"/> |

