Every container has its own isolated file system. To change a file inside, either the image has to be rebuilt, or you must copy the file in manually everytime.

A **volume mount** tells docker to take a folder in the host machine and make it appear as a folder within the container. In the compose file, this would appear as:
```
volumes:
 - /host/path:/container/path:ro
```
Where the host's folder path is mapped to the container's folder path. The ":ro" at the end is optional and signifies that the folder is read only for the container.
