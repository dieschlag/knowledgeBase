Docker is a container runtime, which creates and executes a container based on a image. An image is a read-only template with instructions to create the container. It also contains all the files necessary to run the container.

The Docker Engine is a daemon running on the machine that searches for an image on the machine (when calling docker run) and if it doesn't find it, looks up in a registry to pull the image from there and create the container.

![[Pasted image 20250601153429.png]]

On OS that do not provide native support for containers, it is required to install Docker Desktop

## Command to run

`docker run [options] image-name [command] [arg]`

where:
- options: list of options
- image-name: name of the image to use to run the container, only mandatory argument
- command: command to be executed in the container
- arg: list of arguments taken by the command executed in the container

## Name format

The format for an image is the following:

`name-of-the-registry/name-of-the-user-or-organisation/name-ofimage/tag-or-version`

with as an example:

`registry.redhat.io/rhel8/mysql-80:1-69`

## Image

An image is a read-only template with instructions to create a Docker image. it consists in a collection of files and folders necessasry to run an application.

These files/folders are organized in a layer system. All these layers contain specific files and are read-only. The first layer is the base layer.

![[Pasted image 20250601163932.png]]

These layers are independent of the image and as such are stored independently inside an image registry. The image is stored as a manifest, a file containing instructions on how to run the container.

Similarly, when pulling an image, the layers are pulled independently. Docker then reassembles these different layers.

This division in layers allows the download of an image across several servers and can be shared once pulled by different images. The integrity of an image can be done as well by checking the integrity of the layers themselves. Each layer has a SHA256 digest of its content, used to compare with the digest calculated by the Docker Engine on the layer that has been pulled.

## Dockerfile

A docker file is an efficient way to build an image. It is a simple text containing simple commands to guide the build process.

Exemple: 

If an application is run using `python main.py sentence`, we need to execute this program:
- a pyhton interpreter
- all the dependencies
- the source code
- instruction on how to run the code

To create the associated docker file we can rely on other existing images:

FROM pyhton:3.7-slim
RUN mkdir -p /app
WORKDIR /app
COPY ./main.py ./requirements.txt .
RUN pip install -r requirements.txt
ENTRYPOINT \["python", "main.py]
CMD \["A default sentence"]

Explanation of the commands:
- FROM: specifies base image
- RUN: runs any valid Linux command
- WORKDIR: sets working directory in the image, all subsequent commands will be made relative to this folder
- ENTRYPOINT: specifies command to execute
- CMD: specifies default argument to pass to the containarised application

To build the image, you can do the following:

`docker build -t sentiment-analysis:latest` with -t allowing to specify the image name

Then we can run it: `docker run sentiment-analysis "Docker is an excellent tool"`

Here if no extra argument is given, the python application will execute using "A default sentence" as a parameter.

Each line will correspond to a layer:

![[Pasted image 20250601165020.png]]

Each layer can only read elements from lower layers. A build cache is also maintained by Docker, to avoid creating the layers from scratch if not needed. For exemple, if the code in main.py is changed, since it only affects the layer 3, layers 2 and 1 will be left untouched.

That's why the order in which we write a Dockerfile is important to beneficiate from this cache. 

In our case, it would have been better to first copy the requirements.txt and install the dependencies, then copy main.py.

Best practices:

- choose the appropriate image accoring to what the project does (ex: chosing a slim version)
- keep the number of layers small, RUN commands for example can be merged
- use the build cache as we did above
- keep the image size small by only adding into it what is necessary

## Volumes

The container contains in top of the read-only layers a writable layer. Is a file must be open, it is first copied inside the writable layer. This operation is called copy-on-write.

If the docker is destroyed, so are destroyed the files of the writable layer. To persist data, we need to use volumes. They are a greate tool as well to share data between different containers.

A volume lives outside of the image file system and corresponds to a directory in the host computer. 

## Networks

Networks allow the exchange of data when there is no need to persist it. A network consists of three elements:

- the sandbox: contains the network configuration
- the endpoint: equivalent to a network interface card of a computer
- network: provides the functions to connect 2+ endpoints

![[Pasted image 20250601175016.png]]

Each implementation of a network is called a driver. The default driver is the bridge, which is based on a linux bridge, behaving like a network switch. 

The other possible drivers are host, which lets the container connect to the host network, and the null driver, that we can use when we want to isolate a container from any network connection.

To create a network (here called buckingham): `docker network create buckingham`.

Any created network has its own IP adress assigned by the Docker engine, chosen in the private range 172.17.17.0.0/16 - 172.31.0.0/16.

If a container receives the IP address 172.17.17.0.0/16, then all containers attached to that network will receive an address within that range.

Exemple of two containers linked to a network (`buckingham` again):

```shell
docker run --rm -it --name c1 --network buckingham alpine
docker run --rm -it --name c2 --network buckingham alpine
```

The bridge `docker0` acts as a gateway for the network. It routes the traffic between the containers and the host network interface. By default only the egress traffic is allowed (can reach the Internet but not the other way around). To make our container available to the Internet, we need to do port mapping.

The port mapping is done by using `-p` when running the image, like such:

```shell
docker run --name c1 --network buckingham -p 8080:7070 my-netcat-image
```

Here we map the port 7070 of our container to the port 8080 of our host.

It looks like that:

![[Pasted image 20250601180022.png]]

In order to connect to our netcat server, a client needs to type `nc 192.168.1.8 8080` where 192.168.1.8 is the address associated with the network `eth0` and 8080 is the port associated with the service. It does not use the IP adress or the port of the container.
## Automation

The management and the deployment of containers can be automated using tools like Kubernetes.

An example of deployment can be found [[Deployment example of an app on Kubernetes|here]]. It requires notions on [[Docker Compose]].