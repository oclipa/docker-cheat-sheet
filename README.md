<div style="display: inline-block;">
<a class="link" href="http://oclipa.github.io/">&lt; home</a>
<a class="link" href="http://oclipa.github.io/toolbox.html">&lt; toolbox</a>
</div> 

## Terminology

| Term | Description |
| :------- | :------- |
| Images | The blueprints of the application.  Very roughly approximates to a git registry.<br/>Containers are created from images. Images can be base images (i.e. not based on another image; typically just an OS), or child images (i.e. based on a base image; typically adds functionality to base) |
| Containers | A running instance of an application. |
| DockerFile | A tool for simplifying the reployment of multi-container applications. |
| Docker Compose | A text file containing a list of commands for creating an image. |
| Docker Daemon | Local background service that mnages building, running and distributing of containers.  |
| Docker Client | The command line tool used to interact with the daemon (GUIs also exist). |
| Docker Hub | A central registry of images maintained by Docker: [https://hub.docker.com/](https://hub.docker.com/) <br/> Repositories can also be hosted locally. |
| Flask | A micro web framework, written in Python.  Typically used for web applications. |
| <img width="300"/> | <img width="400"/>  |

<p style="page-break-before: always"/> 

## CLI Commands

### Management Commands (since v1.13+)

| Command | Action |
| :------- | :------- |
| _Repository Commands_ | <img width="400"/> |
| `$ docker login [server name]`| Login to the Docker Hub<br/>(or, optionally, another server) |
| `$ docker search [search-term]`| Search the Docker Hub for images |
| _Image Commands_ |  |
| `$ docker image ls`| List all local images |
| `$ docker image pull [image name][:version]`| Pull an image from the Docker Hub<br/>(either latest or specified '[:version]'.<br/>To use a local registry, specify<br/>the full server name and path:<br/> `myregistry.local:5000/testing/test-image` |
| `$ docker image build -t [username/appname] [directory containing docker file]`| Create a new image based on the DockerFile |
| `$ docker image push [username/appname]`| Push an image to the repository |
| _Container Commands_ | <img width="400"/>  |
| `$ docker container run [-d] [-P\|-p [external port:internal port]] -e "[key=value]" --net [network-name] [--name container-name] [image-name] [command\|-it] [--rm]` | Launches a container from the specified image and<br/>runs a command or opens an `sh` shell<br/>`-d` run container in background<br/>`-P` Map internal ports to random external ports<br/>`-p` Specify custom internal/external port mapping<br/>`-e` Set an environment variable<br/>`--net` Specify the network to join<br/>`--rm` Delete container on exit |
| `$ docker container logs [container-name]` | Display logs for the specified container |
| `$ docker container port [container-name]` | Display ports exposed by container |
| `$ docker container stop [container-name]`| Stop a container |
| `$ docker container ls [-a]`| List all running containers<br/>`-a` = include recently stopped containers |
| `$ docker container rm [container-name]`| Delete a container |
| `$ docker container prune`| Delete all stopped containers |
| _Network Commands_ |  |
| `$ docker network ls`| List all networks.<br/>By default, Docker creates: "bridge", "host" and "none",<br/>"bridge" is the default network for containers. |
| `$ docker network inspect [network-name]`| Inspect the state of a network. |
| `$ docker network create [network-name]`| Create a new network. |
| `$ docker network rm [network-name]`| Remove a network. |
| <img width="300"/> |  |

<p style="page-break-before: always"/> 

### Legacy Commands (may be deprecated in the future)

| Command | Action |
| :------- | :------- |
| _Image Commands_ | <img width="400"/>  |
| `$ docker images`| List all local images |
| `$ docker pull [image name][:version]`| Pull an image from the Docker Hub<br/>(either latest or specified '[:version]'.<br/>To use a local registry, specify the full server name and<br/>path: `myregistry.local:5000/testing/test-image` |
| `$ docker build -t [username/appname] [directory containing docker file]`| Create an image based on the DockerFile |
| `$ docker push [username/appname]`| Push the new image to the repository |
| _Container Commands_ |  |
| `$ docker run [-d] [-P\|-p [external port:internal port]] -e "[key=value]" --net [network-name] [--name container-name] [image-name] [command\|-it] [--rm]` | Launches a container from the specified image and runs a command or opens an `sh` shell<br/>`-d` run container in background<br/>`-P` Map internal ports to random external ports<br/>`-p` Specify custom internal/external port mapping<br/>`-e` Set an environment variable<br/>`--net` Specify the network to join<br/>`--rm` Delete container on exit |
| `$ docker logs [container-name]` | Display logs for the specified container |
| `$ docker port [container-name]` | Display ports exposed by container |
| `$ docker stop [container-name]`| Stop a container |
| `$ docker ps [-a]`| List all running containers (`-a` = include recently stopped containers) |
| `$ docker rm [container-name]`| Delete a container |
| `$ docker rm $(docker ps -a -q -f status=exited)`| Delete all stopped containers |
| <img width="300"/> |  |

<p style="page-break-before: always"/> 

## DockerFile

| Command | Action |
| :------- | :------- |
| `FROM [base-name:version]` | The base image on which this image is based <br/> e.g. `FROM python:3` |
| `WORKDIR [path]` | The directory in which the app is based. |
| `ADD file.xyz /file.xyz` | Copy all the files to the image |
| `COPY . .` | Copy all the files to the image |
| `COPY --chown=user:group host_file.xyz /path/container_file.xyz`| Copy a specific file to the image, with specific access permissions |
| `VOLUME ["/data"]` | Specification for mount point | 
| `ENV APP_HOME /myapp` or `ARG APP_HOME="/myapp"` | Set environment variables |
| `EXPOSE [port]` | The port that needs to be exposed. |
| `RUN ["executable", "arg1", "arg2", "etc."]` | Mainly used to install a new package on top of the base image <br/> e.g. `RUN ["pip", "install", "--no-cache-dir", "-r", "requirements.txt"` |
| `CMD ["executable", "arg1", "arg2", "etc."]` | A default command to be run when `docker run` is called <br/> e.g. `CMD ["python", "./app.py"]`.  If the "executable" is left out, the arguments are appended to the `ENTRYPOINT` command. `CMD` is ignored if a command is specified on the command line. |
| `ENTRYPOINT ["executable", "arg1", "arg2", "etc."]` | This should be used if the container is to be used as an executable.  The command will always run, regardless of any command line arguments.  Any command line arguments will be appended to the `ENTRYPOINT` arguments. |
| `ONBUILD [command]` | To be run if another image is built on top of this image. |
| `LABEL <key>=<value> <key>=<value> <key>=<value> ...` | Set metadata as key-value pairs. |
| <img width="300"/> | <img width="400"/>  |

For further discussion of the difference between `RUN`, `CMD` and `ENTRYPOINT, see 
   * [https://aboullaite.me/dockerfile-run-vs-cmd-vs-entrypoint/](https://aboullaite.me/dockerfile-run-vs-cmd-vs-entrypoint/)

<div id="multi">   
<button type="button" class="collapsible">+ Example Script to Launch Multi-Container Environment</button>
<div class="content" style="display: none;" markdown="1">

```bash
#!/bin/bash

# build the flask container
docker image build -t oclipa/foodtrucks-web .

# create the network
docker network create foodtrucks-net

# start the ES container
docker container run -d --name es --net foodtrucks-net \ 
   -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" \
   docker.elastic.co/elasticsearch/elasticsearch:6.3.2

# start the flask app container
docker container run -d --net foodtrucks-net -p 5000:5000 \
   --name foodtrucks-web oclipa/foodtrucks-web
```
Relates to the following git repo: [https://github.com/oclipa/food-trucks](https://github.com/oclipa/food-trucks)

</div>
</div>
<p style="page-break-before: always"/> 

## Docker-Compose

A tool for managing a collection of containers as if they were a single app.  

It is configured using a YAML file: `docker-compose.yml`, of which the following is an example:

```yaml
version: "3"
services:
  es:
    image: docker.elastic.co/elasticsearch/elasticsearch:6.3.2
    container_name: es
    environment:
      - discovery.type=single-node
    ports:
      - 9200:9200
    volumes:
      - esdata1:/usr/share/elasticsearch/data
  web:
    #build: . # builds a new image
    image: oclipa/foodtrucks-web # uses an existing image
    command: python app.py
    depends_on:
      - es
    #environment:
    #  - DEBUG=True # set an env var for flask
    ports:
      - 5000:5000
    volumes:
      - ./flask-app:/opt/flask-app
volumes:
  esdata1:
    driver: local
```

This file defines two services `es` (the elasticsearch service) and `web` (the web app).  The details are broadly the same as those in the DockerFile, however note that `depends-on` property, which indicates that the es service must be started before the web service.  The `volumes` properties are particularly useful for logging.

`docker-compose` commands must be run in the same directory as the `docker-compose.yml` file.

If problems are experienced, might need to run `docker-compose up -d --build`.

| Command | Action |
| :------- | :------- |
| _Image Commands_ | <img width="400"/>  |
| `$ docker-compose up [-d] [--build]` | Launch all of the services and connect them to the same network.<br/>`-d` Detached mode.<br/>`--build` Rebuild the local image. |
| `$ docker-compose ps` | List all services. |
| `$ docker-compose down [-v]` | Shutdown all of the services. `-v` Destroy all data volumes. |
| `$ docker-compose run [service-id] [command]` | Run a command in the context of the specified service. |
| <img width="300"/> |  |

<p style="page-break-before: always"/> 

<div id="aws">   
<button type="button" class="collapsible">+ Deploying to AWS Electric Beanstalk</button>
<div class="content" style="display: none;" markdown="1">

* [https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/single-container-docker-configuration.html](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/single-container-docker-configuration.html)

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
</div>
</div>

<div id="google">   
<button type="button" class="collapsible">+ Deploying to Google Compute Engine</button>
<div class="content" style="display: none;" markdown="1">

* [https://cloud.google.com/compute/docs/containers/deploying-containers](https://cloud.google.com/compute/docs/containers/deploying-containers)

</div>
</div>

<div id="azure">   
<button type="button" class="collapsible">+ Deploying to Azure App Service</button>
<div class="content" style="display: none;" markdown="1">

* [https://docs.microsoft.com/en-us/learn/modules/deploy-run-container-app-service/](https://docs.microsoft.com/en-us/learn/modules/deploy-run-container-app-service/)

</div>
</div>

<div id="ocean">   
<button type="button" class="collapsible">+ Deploying to Digital Ocean</button>
<div class="content" style="display: none;" markdown="1">

* [https://stackabuse.com/deploying-a-node-js-app-to-a-digitalocean-droplet-with-docker/](https://stackabuse.com/deploying-a-node-js-app-to-a-digitalocean-droplet-with-docker/)

Summarised:
1. Create SSH keypair: `$ ssh-keygen -t rsa -b 4096`
1. Copy the public key to Digital Ocean account (Security -> Add SSH Key)
1. Obtain a Droplet configured for Docker (may need to search in the Marketplace)
1. Choose plan, region, SSH key and create Droplet
1. Identify the IP address of the Droplet
1. SSH to the Droplet: `$ ssh -i [path/to/private/key] root@ip-address]`
1. Run the Docker image: `$ docker run -p [external-port]:[internal-port] [container-name]`
1. Access the web page at: `http://[ip-address]:[external-port]/`

</div>
</div>
<p style="page-break-before: always"/> 

<div id="dev">   
<button type="button" class="collapsible">+ Development Workflow</button>
<div class="content" style="display: none;" markdown="1">

To develop and test an existing image, do the following:

1. Make changes to local files for the service under development
2. In `docker-compose.yml`, for the service of interest, replace the `image [image name]` property with a `build .` property.
3. Restart the services: `docker-compose down -v; docker-compose up -d --build`
4. If you have problems, try deleting the existing image (`docker image rm [image-name]`) and restart the services again.

</div>
</div>

<div id="images">   
<button type="button" class="collapsible">+ Commonly Used Images</button>
<div class="content" style="display: none;" markdown="1">

* Ubuntu - simple but flexible base image (use apt-get to install dependencies)
   * Example: [https://github.com/prakhar1989/FoodTrucks](https://github.com/prakhar1989/FoodTrucks)
* Python - base image with python pre-installed
   * Example: [https://github.com/prakhar1989/docker-curriculum/tree/master/flask-app](https://github.com/prakhar1989/docker-curriculum/tree/master/flask-app)
* Busybox - lightweight command line tools
* Elasticsearch - open source search and analytics engine

</div>
</div>

<div id="refs">   
<button type="button" class="collapsible">+ Additional References</button>
<div class="content" style="display: none;" markdown="1">

* Official Docker Reference Documentation: [https://docs.docker.com/reference/](https://docs.docker.com/reference/)
* Official DockerFile Documentation: [https://docs.docker.com/engine/reference/builder/](https://docs.docker.com/engine/reference/builder/)
* Official Docker Compose Documentation: [https://docs.docker.com/compose/compose-file/](https://docs.docker.com/compose/compose-file/)
* Official Docker Networks Documentation: [https://docs.docker.com/network/](https://docs.docker.com/network/)
* Get Started with Docker: [https://www.docker.com/get-started](https://www.docker.com/get-started)
* Docker for Beginners: [https://docker-curriculum.com/](https://docker-curriculum.com/)

</div>
</div>

&nbsp;

&nbsp;

&nbsp;

------
**Move along; nothing to see here...**

<script type="text/javascript">

    const loadCSS = (filename) => { 

       const file = document.createElement("link");
       file.setAttribute("rel", "stylesheet");
       file.setAttribute("type", "text/css");
       file.setAttribute("href", filename);
       document.head.appendChild(file);
    };

    const loadJS = (filename) => { 

       const file = document.createElement("script");
       file.setAttribute("type", "text/javascript");
       file.setAttribute("src", filename);
       document.head.appendChild(file);
    };
   
    //just call a function to load your CSS
    //this path should be relative your HTML location
    loadCSS("../collapse.css");
    loadJS("../collapse.js");

</script>

