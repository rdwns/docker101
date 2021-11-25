### Docker 101 Training Session

This repository containes the reference code for building and run a simple docker imageof a python based web application. If you're following along from the session you can refer the following steps incase you miss out on anything:


### Step 1: Create a Dockerfile

The first step is to create a Dockerfile for whatever application it is that you're trying to containerize.

Roughly whenever you create a DockerFile you have to specify the following:

- **copy** all app files into the docker container
- **install** dependencies like pip requirements
- **open up a port**, in the container that can be accessed from the outside
- **instruct**, the container how to start our app

In a more complex application, we might need to do things like setting environment variables or setting credentials for a database or running a database seed to populate the database, and so on.

### Step 2: Build Docker image

Now that we're done with building the Dockerfile, the next step is to turn the Dockerfile into an actual image:

We use the **`docker build`** command to do so. To build an image, you can do 

```docker
docker build . -t <name-of-image>
```

This command tells Docker to look in the current directory for a file called `Dockerfile` and to follow the instructions inside to build an image. After doing so, tag the image so we can easily find it later. Docker image tags let you version your images as well. Say you wanted to build a v1 for your image, you would do `docker build . -t <name-of-image>:v1`. If you have a different name for your Dockerfile, you can also refer to it using the `-f` flag like so: `docker build . -t <name-of-image> -f <name-of-dockerfile>`

To get a list of all the images you've built, you can do **`docker image ls`**

The command will give you an output that looks something like the following.


### Step 3: Run image as a container

Now that you’ve built an image, you can just run it by doing 

```docker
docker run <name-of-container>:<version>.
```

More often than not, you can just use the latest version of the image, `docker run <name-of-container>:latest`. However, certain applications (like servers) need to listen to specific ports. By default, Docker doesn’t allow containers to use ports on your local machine, but you can allow this by specifying ports using the `-p` flag, `docker run -p <host-port>:<container-port> <name-of-container>`. If your container listens on port 3000, but you want it to appear as port 5000 on your local machine, it would look like `docker run -p 5000:3000 <name-of-container>`

If you want to run your container in the background in a detached manner, you can just add the `-d` flag.

> How do I figure out what containers are currently running?
>

You can get a list of currently running containers by doing `docker ps`, which will give you each container running along with details about its Container ID, what image it was created from, when it was created, as well as which ports are open.

<aside>
ℹ️ To allow for easier management of containers use the `--name containerName` flag, Then you don't have to use the container ID hash to stop or kill the containers
</aside>


> How do I run a command inside a container?
>

You can use the command `docker exec -it <container name> /bin/bash` to get a bash shell in the container, allowing you to run commands from within the container as if it was a full-fledged machine. If you know specifically what command you want to execute, you can use `docker exec -it <container name> <command>` to execute whatever command you specify in the container.

### Step 4: Push the image to Container Registry

To push any local image to Container Registry using Docker you need to first tag it with the registry name and then push the image!

To tag any image we use the command:

```docker
docker tag image:version username/image:version
```

For Dockerhub the convention is usually like `docker push username/image_name` for other private container registries it's generally `docker push <container_registry_url>/imagename`

The steps are the same, however, the commands are different for different cloud providers.

### Step 5: Pull the image into a separate system

After the image has been pushed to a container registry, the same can be pulled from any system that has Docker installed and has access to the container registry. The containerized application will run the same way as it did on the system the image was built in.

To pull the images from a registry we use:

```docker
docker pull <registry_name>/<image_name>
```