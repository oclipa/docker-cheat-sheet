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

### Management Commands (since v1.13+)

| Command | Action |
| :------- | :------- |
| _Repository Commands_ | <img width="400"/> |
| `$ docker login [server name]`| Login to [Docker Hub](https://hub.docker.com/) (or, optionally, another server) |
| `$ docker search [search-term]`| Search the Docker Hub for images |
| _Image Commands_ | <img width="400"/> |
| `$ docker image ls`| List all local images |
| `$ docker image pull [image name][:version]`| Pull an image from the Docker Hub (either latest or specified '[:version]'.<br/>To use a local registry, specify the full server name and path: `myregistry.local:5000/testing/test-image` |
| `$ docker image build -t [username/appname] [directory containing docker file]`| Create a new image based on the DockerFile |
| `$ docker image push [username/appname]`| Push an image to the repository |
| _Container Commands_ | <img width="400"/> |
| `$ docker container run [-d] [-P\|-p [external port:internal port]] -e "[key=value]" --net [network-name] [--name container-name] [image-name] [command\|-it] [--rm]` | Launches a container from the specified image and runs a command or opens an `sh` shell<br/>`-d` run container in background<br/>`-P` Map internal ports to random external ports<br/>`-p` Specify custom internal/external port mapping<br/>`-e` Set an environment variable<br/>`--net` Specify the network to join<br/>`--rm` Delete container on exit |
| `$ docker container logs [container-name]` | Display logs for the specified container |
| `$ docker container port [container-name]` | Display ports exposed by container |
| `$ docker container stop [container-name]`| Stop a container |
| `$ docker container ls [-a]`| List all running containers (`-a` = include recently stopped containers) |
| `$ docker container rm [container-name]`| Delete a container |
| `$ docker container prune`| Delete all stopped containers |
| _Network Commands_ | <img width="400"/> |
| `$ docker network ls`| List all networks (by default, Docker creates: "bridge", "host" and "none"; "bridge" is the network in which containers run by default). |
| `$ docker network inspect [network-name]`| Inspect the state of a network. |
| `$ docker network create [network-name]`| Create a new network. |
| <img width="400"/> | <img width="400"/> |

### Legacy Commands (may be deprecated in the future)

| Command | Action |
| :------- | :------- |
| _Image Commands_ | <img width="400"/> |
| `$ docker images`| List all local images |
| `$ docker pull [image name][:version]`| Pull an image from the Docker Hub (either latest or specified '[:version]'.<br/>To use a local registry, specify the full server name and path: `myregistry.local:5000/testing/test-image` |
| `$ docker build -t [username/appname] [directory containing docker file]`| Create an image based on the DockerFile |
| `$ docker push [username/appname]`| Push the new image to the repository |
| _Container Commands_ | <img width="400"/> |
| `$ docker run [-d] [-P\|-p [external port:internal port]] -e "[key=value]" --net [network-name] [--name container-name] [image-name] [command\|-it] [--rm]` | Launches a container from the specified image and runs a command or opens an `sh` shell<br/>`-d` run container in background<br/>`-P` Map internal ports to random external ports<br/>`-p` Specify custom internal/external port mapping<br/>`-e` Set an environment variable<br/>`--net` Specify the network to join<br/>`--rm` Delete container on exit |
| `$ docker logs [container-name]` | Display logs for the specified container |
| `$ docker port [container-name]` | Display ports exposed by container |
| `$ docker stop [container-name]`| Stop a container |
| `$ docker ps [-a]`| List all running containers (`-a` = include recently stopped containers) |
| `$ docker rm [container-name]`| Delete a container |
| `$ docker rm $(docker ps -a -q -f status=exited)`| Delete all stopped containers |
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

## Commonly Used Images

* Ubuntu - simple but flexible base image (use apt-get to install dependencies) - [example](https://github.com/prakhar1989/FoodTrucks)
* Python - base image with python pre-installed - [example](https://github.com/prakhar1989/docker-curriculum/tree/master/flask-app)
* Busybox - lightweight command line tools
* Elasticsearch - open source search and analytics engine

## Example Script to Launch Multi-Container Environment

```bash
#!/bin/bash

# build the flask container
docker image build -t oclipa/foodtrucks-web .

# create the network
docker network create foodtrucks-net

# start the ES container
docker container run -d --name es --net foodtrucks-net -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" docker.elastic.co/elasticsearch/elasticsearch:6.3.2

# start the flask app container
docker container run -d --net foodtrucks-net -p 5000:5000 --name foodtrucks-web oclipa/foodtrucks-web
```
Relates to the following docker image: https://hub.docker.com/repository/docker/oclipa/foodtrucks-web

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

## Additional References

* [Offical Docker Reference Documentation](https://docs.docker.com/reference/)
* [Get Started with Docker](https://www.docker.com/get-started)
* [Docker for Beginners](https://docker-curriculum.com/)
* [Docker Networks](https://docs.docker.com/network/)

