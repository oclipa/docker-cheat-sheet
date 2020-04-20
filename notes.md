# A Docker Cheat-Sheet

## Terminology
| Term | Description |
| :------- | :------- |
| Images | The blueprints of the application.  Very roughly approximates to a git registry.<br/>Containers are created from images. Images can be base images (i.e. not based on another image; typically just an OS), or child images (i.e. based on a base image; typically adds functionality to base) |
| Containers | A running instance of an application. |
| Docker File | A text file containing a list of commands for creating an image. |
| Docker Daemon | Local background service that mnages building, running and distributing of containers.  |
| Docker Client | The command line tool used to interact with the daemon (GUIs also exist). |
| Docker Hub | A registry of images.  A central registry [exists](https://hub.docker.com/search?q=&type=image), however they can also be hosted locally. |
| Flask | A micro web framework, written in Python.  Typically used for web applications. |
| <img width="350"/> | <img width="400"/> |

## CLI Commands

| Command | Action |
| :------- | :------- |
| `$ docker images`| List all local images |
| `$ docker pull [image name][:version]`| Pull an image from the Docker Hub (either latest or specified '[:version]'.<br/>To use a local registry, specify the full server name and path: `myregistry.local:5000/testing/test-image` |
| `$ docker run [-d] [-P] [--name container-name] [image-name] [command\|-it] [--rm]` | Launches a container from the specified image and runs a command or opens an `sh` shell (`-d`= run container in background; `-P` = publish any internal ports to random external ports; `--rm` = delete container on exit) |
| `$ docker run -p [external port: internal port; e.g. 8888:80] [image name]` | `-p` Specify a custom port to which the client will forward connections to the container. |
| `$ docker port [container-id]` | Display ports exposed by container |
| `$ docker stop [container-id]`| Stop a container |
| `$ docker ps [-a]`| List all running containers (`-a` = include recently stopped containers) |
| `$ docker rm [container-id]`| Delete a container |
| `$ docker container prune`| Delete all stopped containers |
| `$ docker rm $(docker ps -a -q -f status=exited)`| [legacy syntax] Delete all stopped containers |
| <img width="400"/> | <img width="400"/> |

## DockerFile Commands

| Command | Action |
| :------- | :------- |
| `FROM [base-name:version]` | The base image on which this image is based (e.g. `FROM python:3`). |
| `WORKDIR [path]` | The directory in which the app is based. |
| `COPY . .` | Copy all the files to the image' |
| `RUN [command]` | Run a command to build the environment (e.g. `RUN pip install --no-cache-dir -r requirements.txt`) | 
| `EXPOSE [port]` | The port that needs to be exposed. |
| `CMD ["executable", "arg1", "arg2", etc.]` | The command to run the application (e.g. `CMD ["python", "./app.py"]` |