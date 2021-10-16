## Docker Container - A Beginner's Guide 1

## Docker Container - A Beginner's Guide 1

### All you need to read this article ☑️

1. [Basic Linux Commands](https://hackr.io/blog/basic-linux-commands) 
2. [Linux Command Piping](https://www.geeksforgeeks.org/piping-in-unix-or-linux/)
3. A computer with Internet connection 🌎

### What are Docker Engine and Containers 🐳?

> A container is a standard unit of software that packages up code and all its dependencies so the application runs quickly and reliably from one computing environment to another.
>
> -- from <cite>Docker Official Webpage</cite>

<img src="https://www.docker.com/sites/default/files/d8/styles/large/public/2018-11/container-what-is-container.png?itok=vle7kjDj" alt="img" style="zoom:50%;" />

>Source: Docker Official Website

For example, in traditional ways of web application deployment, the frontend server, the backend server, SQL database, the media files server etc are all hosted on one physical machine or a virtual machine (VM). However, in a containerized way, we split different parts of the web application into Docker containers, which are managed by Docker Engine. 



### Why to choose?

The crucial advantage of Containerization is agility, which basically refers to getting things done and keeping your services up-to-date faster and easier to maintain. 

1. Light-weighted

   Compared with VMs of server-layer [virtualization](https://en.wikipedia.org/wiki/Hardware_virtualization), Containers is virtualization of Operating Systems (OS). You can think of VMs as independent holiday cottages🏠 with bedrooms, swimming pools, kitchen and other sort of utilities, and consider Containers as rooms 🛏 in a Motel. 

   (Not a rigorous comparison, but I hope it can help you better understand😝)

   As VM has it own complete OS, it usually comes in Gigabytes and takes minutes to start, just like the OS in your laptop! But Containers are much lighter and they are often in Megabytes and take only a few seconds to run. 

![Screen Shot 2021-10-12 at 10.07.51 AM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1634395585360/TfGKfFXdo.png)

> Docker containers on my MacBook

2. Better Maintenance

   Docker images are easy to be archived into different versions. This is extremely benefit to developers especially when a problem happens on the current image, because it only takes a few steps to roll back to the previous versions. 

3. Other benefits

   Containers become so popular in recent years, because they also enjoy many other advantages. Please see [this article](https://hentsu.com/docker-containers-top-7-benefits/) in your want to know more 😀



### Get our hands dirty!

1. Install Docker Engine on your machine 

   - For Linux users (ubuntu, Debian, CentOS, Fedora etc), please [install Docker Engine](https://docs.docker.com/engine/). Here we use Debian as an example, for more details please check the [official manual](https://docs.docker.com/engine/install/debian/)📕.

     ##### Update and upgrade `apt-get`

     ```bash
     $ sudo apt-get update
     $ sudo apt-get upgrade
     ```

     ##### Set up the repository

     ```bash
     $  sudo apt-get install \
         apt-transport-https \
         ca-certificates \
         curl \
         gnupg \
         lsb-release
     ```

     ##### Add Docker's official GPG key:

     ```shell
     $ curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
     ```

     For beginners, stable repository is recommended. Here we add **nightly** or **test** repository:

     ```bash
     $  echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/debian \
       $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
     ```

     ##### Install Docker Engine:

     Now we are ready to install Docker Engine simply via `apt-get`.

     ```shell
     $ sudo apt-get install docker-ce docker-ce-cli containerd.io
     ```

     Just take a tea break 🍵 while the package manager is getting thing ready for you.

     ##### Verify installation:

     To verify your installation, just run the `hello-world` image use `docker` command.

     ```bash
     $ sudo docker run hello-world
     Hello from Docker!
     This message shows that your installation appears to be working correctly.
     ```

     Congrats 🎉 You can now see the hello message from Docker 🐳!

     

   - For MacOS/Windows users, please [install Docker Desktop](https://docs.docker.com/desktop/).

     ##### Download the graphical installation package

     Docker Desktop provides both a graphical interface and a command line interface (CLI) `docker`. Choose the right link for your operation systems from [Docker downloads](https://docs.docker.com/desktop/). Then open the Docker Desktop, voilà! 🍻

     <img src="https://docs.docker.com/desktop/mac/images/docker-tutorial-mac.png" alt="docker-tutorial-mac" style="zoom:33%;" />

     You can try the Quick Start Guide and play with it! 

2. Start your Docker Engine.

   - For Linux users (Ubuntu as an example here), start Docker Service

     ```shell
     $ sudo service docker start
     ```

     or

     ```shell
     $ sudo systemctl start docker	
     ```

     Then add ubuntu to group docker and run `hello-world` image.

     ```bash
     $ sudo usermod -a -G docker ubuntu
     $ sudo docker run hello-world
     ```

     To see if docker works well, use `docker info`

     ```bash
     $ docker info
     ```

   - For Docker Desktop users, just double click the App icon to start the Docker Engine. Then use `docker info` to check the service status

     ```bash
     $ docker info
     Client:
      Context:    default
      Debug Mode: false
      Plugins:
       buildx: Build with BuildKit (Docker Inc., v0.6.1-docker)
       compose: Docker Compose (Docker Inc., v2.0.0-rc.3)
       scan: Docker Scan (Docker Inc., v0.8.0)
     Server:
      Containers: 11
       Running: 1
      ...
      Live Restore Enabled: false
     ```

     Now you just start the Docker Engine. Next we will play with the docker images and containers.

3. Manage your Docker Images

   ##### Get help from Docker

   First we introduce the `docker --help` command, which is the most helpful function for you to check the usage. For both Linux CLI users and Docker Desktop users, we now play everything in the command line/terminal/PowerShell.

   ```bash
   $ docker --help
   Usage:  docker [OPTIONS] COMMAND
   A self-sufficient runtime for containers
   Options:
   ...
   ```

   Check what we can do and how to do with images:

   ```shell
   $ docker image --help
   Usage:  docker image COMMAND
   
   Manage images
   
   Commands:
     build       Build an image from a Dockerfile
     history     Show the history of an image
     import      Import the contents from a tarball to create a filesystem image
     inspect     Display detailed information on one or more images
     load        Load an image from a tar archive or STDIN
     ls          List images
     prune       Remove unused images
     pull        Pull an image or a repository from a registry
     push        Push an image or a repository to a registry
     rm          Remove one or more images
     save        Save one or more images to a tar archive (streamed to STDOUT by default)
     tag         Create a tag TARGET_IMAGE that refers to SOURCE_IMAGE
   
   Run 'docker image COMMAND --help' for more information on a command.
   ```

   Other helpful commands like `docker version` are omitted here.

   ##### Pull images

   There are a large amount of container images, like ubuntu, mySQL and postgres from the official DockerHub. To get a copy of these images on your host machine, simply use `docker pull [image_name]` command. Let's say we wanna get a Python docker image, it is quite easy to do so

   ```bash
   $ docker pull python
   Using default tag: latest
   latest: Pulling from library/python
   df5590a8898b: Downloading  16.27MB/54.93MB
   705bb4cb554e: Download complete
   519df5fceacd: Downloading  10.59MB/10.87MB
   ccc287cbeddc: Downloading  524.6kB/54.57MB
   e3f8e6af58ed: Waiting
   aebed27b2d86: Waiting
   54c32182bdcc: Waiting
   cc8b7caedab1: Waiting
   462c3718af1d: Waiting
   ```

   Wait a few minutes you will see in your command line window:

   ```bash
   Using default tag: latest
   latest: Pulling from library/python
   df5590a8898b: Pull complete
   705bb4cb554e: Pull complete
   519df5fceacd: Pull complete
   ccc287cbeddc: Pull complete
   e3f8e6af58ed: Pull complete
   aebed27b2d86: Pull complete
   54c32182bdcc: Pull complete
   cc8b7caedab1: Pull complete
   462c3718af1d: Pull complete
   Digest: sha256:5ca194a80ddff913ea49c8154f38da66a41d2b73028c5cf7e46bc3c1d6fda572
   Status: Downloaded newer image for python:latest
   docker.io/library/python:latest
   ```

   Pull complete! To pull a selected version of image, just add its tag like this 

   ```bash
   $ docker pull image_name:version
   ```

    Let check what images we have right now on our host machine (includes images I pulled before 🤓):

   ```shell
   $ docker images
   REPOSITORY               TAG       IMAGE ID       CREATED        SIZE
   ubuntu_upd               latest    306f0077378b   18 hours ago   105MB
   python                   latest    618fff2bfc18   6 days ago     915MB
   ubuntu                   latest    597ce1600cf4   11 days ago    72.8MB
   ubuntu                   18.04     5a214d77f5d7   11 days ago    63.1MB
   mysql                    latest    2fe463762680   13 days ago    514MB
   alpine/git               latest    37ca3b12dde9   2 weeks ago    25.2MB
   hello-world              latest    feb5d9fea6a5   2 weeks ago    13.3kB
   docker/getting-started   latest    083d7564d904   4 months ago   28MB
   ```

   ##### Remove images

   Remove images is very simple: `docker image rm image_id`. For example, to remove the first `ubuntu_upd` image from the list.

   ```bash
   $ docker image rm 30
   Untagged: ubuntu_upd:latest
   Deleted: sha256:306f0077378b0d92a1a6faf049c8663aeff5e1ca07227cb1ad5123f0b373d299
   Deleted: sha256:2e7e0a83f339fe65ed52e381970b4681aaa24e598bd41ec99515c6b8f871149c
   
   $ docker images
   REPOSITORY               TAG       IMAGE ID       CREATED        SIZE
   python                   latest    618fff2bfc18   10 days ago    915MB
   ubuntu                   latest    597ce1600cf4   2 weeks ago    72.8MB
   ubuntu                   18.04     5a214d77f5d7   2 weeks ago    63.1MB
   mysql                    latest    2fe463762680   2 weeks ago    514MB
   alpine/git               latest    37ca3b12dde9   2 weeks ago    25.2MB
   hello-world              latest    feb5d9fea6a5   3 weeks ago    13.3kB
   docker/getting-started   latest    083d7564d904   4 months ago   28MB
   brandoz/cc_flask_demo    latest    f5590286aad8   2 years ago    109MB
   ```

   Another option is to use `docker rmi`

   ```bash
   $ docker rmi -f 2f
   Untagged: mysql:latest
   Untagged: mysql@sha256:4fcf5df6c46c80db19675a5c067e737c1bc8b0e78e94e816a778ae2c6577213d
   Deleted: sha256:2fe4637626805dc6df98d3dc17fa9b5035802dcbd3832ead172e3145cd7c07c2
   ```

   The `mysql` image is removed from your machine. Great! Next we run containers from these images.

   

4. Play with Containers

   ##### Run and manage Containers

   Use `docker run` command to run Containers. The usage is as follow:

   ```bash
   $ docker run --help
   
   Usage:  docker run [OPTIONS] IMAGE [COMMAND] [ARG...]
   
   #OPTIONS
   --name="name" #container name
   -d  #run as Daemon
   -it #allocate a tty for interactive processes (like a shell)
   -P #map container port randomly to host machine's port
   ```

   For example, we run a container from the ubuntu image:

   ```bash
   $ docker run -idt ubuntu:latest
   57aef4b00df55bc36dbfe6ca44af5a3ea1f5b81cd15570a68e0849d2bf8fe663
   ```

   The code string is the ID of the container. To check the containers running in the system, use `docker ps` command.

   ```bash
   $ docker ps
   CONTAINER ID   IMAGE           COMMAND   CREATED         STATUS         PORTS     NAMES
   57aef4b00df5   ubuntu:latest   "bash"    2 minutes ago   Up 2 minutes             compassionate_chaum
   ```

   Or we can check the container running history (both current and past running containers) by adding a `-a` option.

   ```bash
   $ docker ps -a
   61a40e68d83e   hello-world                     "/hello"                 2 minutes ago       Exited (0) About a minute ago                                               great_dubinsky
   86cf5af6c35c   alpine/git                      "git --help"             19 hours ago        Exited (0) 19 hours ago                                                     sleepy_chaplygin
   dac7cd46ad2c   docker/getting-started          "/docker-entrypoint.…"   19 hours ago        Exited (255) 19 hours ago       80/tcp                                      flamboyant_archimedes
   0ad030b20a91   mysql                           "docker-entrypoint.s…"   19 hours ago        Exited (1) 19 hours ago                                                     peaceful_mcnulty
   06c3d968f3bd   docker/getting-started:latest   "/docker-entrypoint.…"   5 days ago          Exited (255) 19 hours ago       80/tcp
   ```

   Or list a number of (10 for example) recent created containers

   ```bash
   $ docker ps -n=10
   ```

   List all history containers ID

   ```bash
   $ docker ps -aq
   ```

   To manipulate the status of the containers, use the following commands:

   ```bash
   $ docker start container_id 
   $ docker restart container_id 
   $ docker stop container_id 
   $ docker kill container_id
   $ docker rm container_id
   ```
   
   It is noted that you don't need to enter the entire ID string, but just the first two digits to identify which container you want to manipulate.
   
   ```bash
   $ docker start 57
   ```
   
   For the ubuntu container we just created with ID `57aef4b00df55bc36dbfe...e663`.
   
   
   
   ##### Check Container resource information
   
   - `docker top ID/Name`
   
     ```bash
     $ docker top 57
     UID                 PID                 PPID                C                   STIME               TTY                 TIME                CMD
     root                2088                2061                0                   06:23               ?                   00:00:00            bash
     ```
   
   - `docker inspect ID/Name`
   
   - `docker stats`
   
   ##### Access the running containers
   
   If we want the access the running containers, for example, to see what's going on in our ubuntu container, use `docker attach` or `docker exec`. Then we can interact with the container, e.g. update `apt` from shell.
   
   ```bash
   $ docker attach 57
   root@57aef4b00df5:/# ls
   bin  boot  dev  etc  home  lib  lib32  lib64  libx32  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
   root@57aef4b00df5:/# apt-get update
   Get:1 http://security.ubuntu.com/ubuntu focal-security InRelease [114 kB]
   Get:2 http://archive.ubuntu.com/ubuntu focal InRelease [265 kB]
   ...
   Get:17 http://archive.ubuntu.com/ubuntu focal-backports/main amd64 Packages [2668 B]
   Get:18 http://archive.ubuntu.com/ubuntu focal-backports/universe amd64 Packages [6310 B]
   Fetched 19.3 MB in 22s (878 kB/s)
   Reading package lists... Done
   ```
   
   To exit from the container shell, just enter `exit`. Note that this will stop your container! Another option is to use `docker exec -it ID [bash/shell]`:
   
   ```bash
   $ docker run -idt ubuntu
   eb2a4e3f167216ee0f037a81953ac53c341778ce43b93bb083b2784b8e3c1aeb
   $ docker exec -it eb bash
   root@eb2a4e3f1672:/#
   ```
   
   