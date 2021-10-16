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