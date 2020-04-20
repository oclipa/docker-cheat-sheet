# A Docker Cheat-Sheet

Largely derived from [docker-curriculum.com](https://docker-curriculum.com/).

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
| `$ docker run -p [external port:internal port; e.g. 8888:80] [image name]` | `-p` Specify a custom port to which the client will forward connections to the container. |
| `$ docker port [container-id]` | Display ports exposed by container |
| `$ docker stop [container-id]`| Stop a container |
| `$ docker ps [-a]`| List all running containers (`-a` = include recently stopped containers) |
| `$ docker rm [container-id]`| Delete a container |
| `$ docker container prune`| Delete all stopped containers |
| `$ docker rm $(docker ps -a -q -f status=exited)`| [legacy syntax] Delete all stopped containers |
| <img width="400"/> | <img width="400"/> |
| `$ docker build -t [username/appname] [directory containing docker file]`| Create an image based on the DockerFile |
| `$ docker login [server name]`| Login to [Docker Hub](https://hub.docker.com/) (or, optionally, another server) |
| `$ docker push [username/appname]`| Push the new image to the repository |
| <img width="400"/> | <img width="400"/> |

## DockerFile Commands

| Command | Action |
| :------- | :------- |
| `FROM [base-name:version]` | The base image on which this image is based (e.g. `FROM python:3`). |
| `WORKDIR [path]` | The directory in which the app is based. |
| `COPY . .` | Copy all the files to the image |
| `RUN [command]` | Run a command to build the environment (e.g. `RUN pip install --no-cache-dir -r requirements.txt`) | 
| `EXPOSE [port]` | The port that needs to be exposed. |
| `CMD ["executable", "arg1", "arg2", etc.]` | The command to run the application (e.g. `CMD ["python", "./app.py"]`) |

## Deploying to AWS Electric Beanstalk

[Documentation](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/single-container-docker-configuration.html#create_deploy_docker_image_dockerru)

*Example Dockerrun.aws.json*

```json
{
  "AWSEBDockerrunVersion": "1",
  "Image": {
    "Name": "[image-name; e.g. oclipa/catnip]",
    "Update": "true"
  },
  "Ports": [
    {
      "ContainerPort": 5000,
      "HostPort": 8000
    }
  ],
  "Logging": "/var/log/nginx"
}
```

## Deploying to Google Compute Engine

[Documentation](https://cloud.google.com/compute/docs/containers/deploying-containers)

## Deploying to Azure App Service

[Documentation](https://docs.microsoft.com/en-us/learn/modules/deploy-run-container-app-service/)

## Deploying to Digital Ocean

[Documentation](https://stackabuse.com/deploying-a-node-js-app-to-a-digitalocean-droplet-with-docker/)

1. Create SSH keypair: `$ ssh-keygen -t rsa -b 4096`
1. Copy the public key to Digital Ocean account (Security -> Add SSH Key)
1. Obtain a Droplet configured for Docker (may need to search in the Marketplace)
1. Choose plan, region, SSH key and create Droplet
1. Identify the IP address of the Droplet
1. SSH to the Droplet: `$ ssh -i [path/to/private/key] root@ip-address]`
1. Run the Docker image: `$ docker run -p [external-port]:[internal-port] [container-name]`
1. Access the web page at: `http://[ip-address]:[external-port]/`
