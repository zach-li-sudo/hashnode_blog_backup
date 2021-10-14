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

   You can see that our container is still running in the backend.

   