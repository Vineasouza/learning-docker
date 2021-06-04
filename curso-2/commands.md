# **Commands**

### The pull command fetches a image from the Docker registry and saves it to our system

- `docker pull <image>`

### Shows you all containers that are currently running.

- `docker ps`

### Shows you all containers that are stopped and running

- `docker ps -a`

### Running the run command with the -it flags attaches us to an interactive tty in the container.

- `docker run -it <container> <comand>`

### This command deletes all containers that have a status of `exited`. In case you're wondering, the `-q` flag, only returns the numeric IDs and `-f` filters output based on conditions provided. One last thing that'll be useful is the `--rm` flag that can be passed to docker run which automatically deletes the container once it's exited from. For one off docker runs, `--rm` flag is very useful.

- `docker rm $(docker ps -a -q -f status=exited)`

### This command also deletes all containers that have a status of exited

- `docker container prune`

### Deletes all images that you no longer need by running

- `docker rmi`

</br>

> ## **Terminology**
>
> - _**Images**_ - The blueprints of our application which form the basis of containers. We use the command `docker pull` command to download a specific image.
> - _**Containers**_ - Created from Docker images and run the actual application
> - _**Docker Daemon**_ - The background service running on the host that manages building, running and distributing Docker containers. The daemon is the process that runs in the operating system which clients talk to.
> - _**Docker Client**_ - The command line tool that allows the user to interact with the daemon.
> - _**Docker Hub**_ - A registry of Docker images. You can think of the registry as a directory of all available Docker images. If required, one can host their own Docker registries and can use them for pulling images.

</br>

# Deploying _**web applications**_ with Docker!

</br>

### The image that we are going to use is a single-page website that I've already created for the purpose of this demo and hosted on the registry - `prakhar1989/static-site`. We can download and run the image directly in one go using docker run. As noted above, the `--rm` flag automatically removes the container when it exits. If all goes well, you should see a `Nginx is running...` message in your terminal.

- `docker run --rm prakhar1989/static-site`

### Expose a port to run the container in detached mode. The command, `-d` will detach our terminal, `-P` will publish all exposed ports to random ports and finally `--name` corresponds to a name we want to give.

- `docker run -d -P --name static-site prakhar1989/static-site`

### See the ports that are running a specific containter

- `docker port [CONTAINER]`
- Now you can open `localhost:[PORT /80]`

### You can also specify a custom port to which the client will forward connections to the container.

- `docker run -p 8888:80 prakhar1989/static-site`

</br>

# Creating my own Docker Image!

</br>

### To see the list of images that are available locally, use

- `docker images`
- The `TAG` refers to a particular snapshot of the image and the `IMAGE ID` is the corresponding unique identifier for that image

</br>

> ### For simplicity, you can think of an image akin to a **git repository** - images can be committed with changes and have multiple versions. If you don't provide a specific version number, the client defaults to latest

</br>

### To get a new Docker image you can either get it from a registry (such as the Docker Hub) or create your own. There are tens of thousands of images available on Docker Hub. You can also search for images directly from the command line using `docker search`.

### An important distinction to be aware of when it comes to images is the difference between base and child images.

- _**Base images**_ - are images that have no parent image, usually images with an OS like ubuntu, busybox or debian.

- _**Child images**_ - are images that build on base images and add additional functionality.

### Then there are official and user images, which can be both base and child images.

- _**Official images**_ - are images that are officially maintained and supported by the folks at Docker. These are typically one word long. In the list of images above, the `python`, `ubuntu`, `busybox` and `hello-world` images are official images.

- _**User images**_ - are images created and shared by users like you and me. They build on base images and add additional functionality. Typically, these are formatted as `user/image-name`.

</br>

## Our First Image

### Our goal in this section will be to create an image that sandboxes a simple Flask application. For this, run the command

```
git clone https://github.com/prakhar1989/docker-curriculum.git
cd docker-curriculum/flask-app
```

### Since our application is written in Python, the base image we're going to use will be `Python 3`.

</br>

> ## Dockerfile
>
> ### Is a simple text file that contains a list of commands that the Docker client calls while creating an image. It's a simple way to automate the image creation process. The best part is that the commands you write in a Dockerfile are almost identical to their equivalent Linux commands. This means you don't really have to learn new syntax to create your own dockerfiles.

</br>

### To start, create a new blank file in our favorite text-editor and save it in the same folder as the flask app by the name of `Dockerfile`.

### We start with specifying our base image. Use the `FROM` keyword to do that

```
FROM python:3
```

### The next step usually is to write the commands of copying the files and installing the dependencies. First, we set a working directory and then copy all the files for our app.

```
# set a directory for the app
WORKDIR /usr/src/app

# copy all the files to the container
COPY . .
```

### Now, that we have the files, we can install the dependencies.

```
# install dependencies
RUN pip install --no-cache-dir -r requirements.txt
```

### The next thing we need to specify is the port number that needs to be exposed. Since our flask app is running on port `5000`, that's what we'll indicate

```
EXPOSE 5000
```

### The last step is to write the command for running the application, which is simply - `python ./app.py`. We use the CMD command to tell the container which command it should run when it is started. With that, our Dockerfile is now ready

```
CMD ["python", "./app.py"]
```

### Now that we have our `Dockerfile`, we can build our image. The `docker build` command does the heavy-lifting of creating a Docker image from a `Dockerfile`.

- `docker build -t [yourusername]/catnip <dir>`

### If everything went well, your image should be ready! Run `docker images` and see if your image shows. The last step in this section is to run the image and see if it actually works (replacing my username with yours).

- `docker run -p 8888:5000 yourusername/catnip`

### The command we just ran used port 5000 for the server inside the container and exposed this externally on port 8888. Head over to the URL with port 8888, where your app should be live.

- `http://localhost:8888/`

> ### 🎉 Congratulations! 🎉 You have successfully created your first docker image.

</br>

# Docker on AWS 🕒
