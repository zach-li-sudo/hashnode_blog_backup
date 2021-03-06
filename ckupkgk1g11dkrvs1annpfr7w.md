## Docker Container - A Beginner's Guide 2

### Continued! 

4. Play with Containers

   ##### Exit from a Container without stopping it:

   Using `exit` to escape from a container will turn it down when you get back to the shell of your host machine. To exit without stopping the container, press `ctrl`+`p`, then `ctrl` + `q`, you can exit from the container while it is still running in behind.

   ```shell
   $ docker ps
   CONTAINER ID   IMAGE     COMMAND   CREATED          STATUS          PORTS     NAMES
   6f8913b2d51d   ubuntu    "bash"    10 minutes ago   Up 10 minutes             ubt00
   $ docker attach 6f
   root@6f8913b2d51d:/home# ls
   README.md  file.txt  pyTest.py
   root@6f8913b2d51d:/home# read escape sequence
   ```

    Now we check the current running containers:

   ```bash
   $ docker ps
   CONTAINER ID   IMAGE     COMMAND   CREATED          STATUS          PORTS     NAMES
   6f8913b2d51d   ubuntu    "bash"    10 minutes ago   Up 10 minutes             ubt00
   ```

   You can see that our container is still running in backstage.

   ##### Check container's logs

   We use `docker logs` to check the container's log info.

   ```bash
   $ docker logs --help
   
   Usage:  docker logs [OPTIONS] CONTAINER
   
   Fetch the logs of a container
   
   Options:
         --details        Show extra details provided to logs
     -f, --follow         Follow log output
         --since string   Show logs since timestamp (e.g. 2013-01-02T13:23:37Z) or relative (e.g. 42m for 42 minutes)
     -n, --tail string    Number of lines to show from the end of the logs (default "all")
     -t, --timestamps     Show timestamps
         --until string   Show logs before a timestamp (e.g. 2013-01-02T13:23:37Z) or relative (e.g. 42m for 42 minutes)
   ```

   To check the output of an Ubuntu container, we first run a container and execute some bash commands in it.

   ```bash
   $ docker run -d ubuntu /bin/sh -c "while true; do echo container_logs;sleep 1;done"
   c1773d0908d63b0cf3c6569886b7bc38c248d2ff7b64b585705f35672518ab00
   ```

   Now the container will output the string "container_logs" every 1 second. Let's access the latest 5 log messages. `--tail` will show from the end of the logs and `-t` allows timestamps on each log message.

   ```bash
   $ docker logs -f -t --tail 5 c1
   2021-10-16T08:40:02.220289500Z container_logs
   2021-10-16T08:40:03.222847800Z container_logs
   2021-10-16T08:40:04.229942500Z container_logs
   2021-10-16T08:40:05.237922300Z container_logs
   2021-10-16T08:40:06.246496900Z container_logs
   
   ```

   ##### Copy file from container to host machine

   Copying files from container to host machine is more or less the same with the [`scp` command](https://linuxize.com/post/how-to-use-scp-command-to-securely-transfer-files/) in Linux shell:

   ```bash
   $ docker cp container_id:PATH host_machine_PATH
   ```

   To do this, we first access a container and create a file in its root path :

   ```bash
   $ docker ps
   CONTAINER ID   IMAGE     COMMAND   CREATED       STATUS       PORTS     NAMES
   0d389aacce6e   ubuntu    "bash"    6 hours ago   Up 6 hours             confident_chebyshev
   $ docker attach 0d
   root@0d389aacce6e:~# touch new_file_in_container.txt
   root@0d389aacce6e:~# ls
   new_file_in_container.txt
   ```

   And then, write some in the `.txt` file:

   ```bash
   root@0d389aacce6e:~# echo "this file is created in container 0d" > new_file_in_container.txt
   ```

   Exit from the container without stopping it: press `ctrl`+`p`, then `ctrl` + `q`. Now we are in the shell of our host machine:

   ```bash
   $ docker cp 0d:root/new_file_in_container.txt ~
   $ echo new_file_in_container.txt
   new_file_in_container.txt
   ```

   File copying success!

5. Commit a Container
   `docker commit`

6. User Docker Volume
   When you want to persist the data generated by Docker containers, Docker Volumes are the preferred choice. These volumes are in your host filesystem (Linux users) or a virtual machine (Docker Desktop for MacOS/Windows users), which confuses many beginners, especially those Docker Desktop users (we talk about this later). Here are some volume commands.

   ```shell
   $ docker volume create my-docker-vol
   my-docker-vol
   $ docker volume ls
   DRIVER    VOLUME NAME
   local     my-docker-vol
   $ docker volume inspect my-docker-vol
   [
       {
           "CreatedAt": "2021-10-17T13:45:08Z",
           "Driver": "local",
           "Labels": {},
           "Mountpoint": "/var/lib/docker/volumes/my-docker-vol/_data",
           "Name": "my-docker-vol",
           "Options": {},
           "Scope": "local"
       }
   ]
   ```

   Remove a volume:

   ```bash
   $ docker volume rm my-docker-vol
   my-docker-vol
   ```

   An important implementation of Docker Volumes is to share data between container and host, or share between containers.

   - Share data between container and host:
     Create a container with a new volume:

     ```bash
     $ docker run -idt --name CONTAINER_NAME -v VOLUME_NAME:path_shared_in_container IMAGE_NAME:TAG
     ```

     Here we create a new ubuntu container `ubt-shared` with a new volume `new_volume` and share the path `\tmp` (by default) . 

     ```bash
     $ docker run -idt --name ubt-shared -v new_volume:/tmp ubuntu:latest
     0e9c3e252f231a54e1b243d7894873830143097078de235493353e9b2abe6e09
     ```

      Now we check the running containers and volumes:

     ```bash
     $ docker ps
     CONTAINER ID   IMAGE           COMMAND   CREATED          STATUS          PORTS     NAMES
     0e9c3e252f23   ubuntu:latest   "bash"    4 minutes ago    Up 4 minutes              ubt-shared
     $ docker volume ls
     DRIVER    VOLUME NAME
     local     new_volume
     ```

     Both the new container and its volume are created. Next we create some files in the shared folder in container:

     ```bash
     $ docker attach 0e
     root@0e9c3e252f23:/# ls
     bin  boot  dev  etc  home  lib  lib32  lib64  libx32  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
     root@0e9c3e252f23:/# cd tmp
     root@0e9c3e252f23:/tmp# touch shared.txt
     root@0e9c3e252f23:/tmp# echo 'shared file' > shared.txt
     root@0e9c3e252f23:/tmp# cat shared.txt
     shared file
     ```

     Then we exit from the container and check the shared file on the host machine, by default the path on host is `/var/lib/docker/volumes/VOLUME_NAME/_data`. You will need root password to access this directory:

     ```bash
     $ sudo cd var/lib/docker/volumes/new_volume/_data
     $ ls
     shared.txt
     $ cat shared.txt
     shared file
     ```

     However, for Docker Desktop (Windows/MacOS) users, the path `/var/lib/docker/volumes` does not exist, because the `docker` service are typically doing one of these two things:

     (1) Run Linux containers in a full Linux VM (what Docker typically does today)

     (2) Run Linux containers with Hyper-V isolation.

     So the shared files will not appear in the default path. Please refer to these links for details:

     1. https://gist.github.com/onlyphantom/0bffc5dcc25a756e247cb526c01072c0
     2. https://newbedev.com/locating-data-volumes-in-docker-desktop-windows
     3. https://stackoverflow.com/questions/38532483/where-is-var-lib-docker-on-mac-os-x

     

   - Share data between containers:
     Create a container `containerA` from the ubuntu image with new volume named `sharedVol` and the shared path is `/shared_dir`:

     ```shell
     $ docker run -idt --name=containerA -v sharedVol:/shared_dir ubuntu:latest
     ```

     Then create a file in the source path in the container and add some text in it:

     ```bash
     ~ docker attach 84
     root@840a03481e79:/# ls
     bin  boot  dev  etc  home  lib  lib32  lib64  libx32  media  mnt  opt  proc  root  run  sbin  shared_dir  srv  sys  tmp  usr  var
     root@840a03481e79:/# cd shared_dir/
     root@840a03481e79:/shared_dir# echo "shared container file A, also show up in other container mounted with the same Docker volume" > shared.txt
     root@840a03481e79:/shared_dir# ls
     shared.txt
     root@840a03481e79:/shared_dir# cat shared.txt
     shared container file A, also show up in other container mounted with the same Docker volume
     root@840a03481e79:/shared_dir# exit
     ```

     Next, create another container B mounted on the volume `sharedVol`:

     ```shell
     $ docker run -ti --name=containerB --volumes-from sharedVol ubuntu
     root@435fbe79c3c4:/# cat /shared_dir/shared.txt
     shared container file A, also show up in other container mounted with the same Docker volume
     ```

     We can also create some files in container B, then check whether it is shared in container A:

     ```bash
     root@435fbe79c3c4:/# cd shared_dir/
     root@435fbe79c3c4:/shared_dir# ls
     shared.txt
     root@435fbe79c3c4:/shared_dir# echo "file created in containerB also shows in containerA" > shared_fromB.txt
     root@435fbe79c3c4:/shared_dir# exit
     exit
     $ docker container restart containerA
     $ docker attach containerA
     root@840a03481e79:/# cat shared_dir/shared_fromB.txt
     file created in containerB also shows in containerA
     root@840a03481e79:/#
     ```