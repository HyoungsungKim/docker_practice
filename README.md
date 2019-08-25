# Let's study docker

- Pull container image

```
docker image pull <container_name>
```

example)

```
docker image pull ubuntu:latest
```

- Pull all of the images from repository

```
docker image pull -a <Repo_diretory>
```

- Show docker images

```
docker image ls
```

- Run docker container

```
docker container run -it <container_name>
```

example)

```
docker container run -it ubunto:latest /bin/bash
```

`-it` means switch terminal to container command terminal

`/bin/bash` means we tell to container that container run with `/bin/bash`

- Show running docker containers
  - If i exit docker using `ctrl + PQ` then containers are still running
  - I can see which container is running with this command

```
docker container ls
```

- Attach running containers

```
docker container exec
```

- Stop the container and kill it 

```
docker container stop <container_name>
docker container rm <container_name>
```

- Build a docker container

When there is `Dockerfile`  in directory,

```
docker image build -t <file_name>:version DockerFileDir
```

example) 

```
docker image build -t test:latest .
```

***Don't forget `.` It means current location of `Dockerfile`***

- Filtering the output of docker Image ls

example)

```
docker image ls --filter dangling=true
docker image ls --filter=reference="*.latest"
docker image ls --format "{{.Size}}"
docker image ls ==format "{{.Repository}}: {{.Tag}}: {{.Size}}"
```

- Searching Docker Hub from the CLI

```
docker search <NAME>
```

example)

```
docker search unbuntu
docker search ubuntu --filter "is-official=true"
```

