# docker-intro

A container uses an image of a preconfigured operating system optimized for a specific task. When a Docker image is launched, it exists in a container.

## List Docker Containers

The basic format for using docker is:

```bash
docker command [options]
```

To list all running Docker containers, enter the following into a terminal window:

```bash
docker ps
```

- To list all containers, both running and stopped, add `–a`
- To list containers by their ID use `–aq`
- To list the total file size of each container, use `–s`

## Start Docker Container

The main command to launch or start a single or multiple stopped Docker containers is `docker start`:

```bash
docker start [options] container_id
```

To create a new container from an image and start it, use `docker run`

```bash
docker run [options] image [command] [argument]
```

A container may be running, but you may not be able to interact with it. To start the container in interactive mode, use the –i and –t options

## Stop Docker Container

Use the docker stop command to stop a container:

```bash
docker stop [option] container_id
```

To stop all running containers, enter the following:

docker stop \$(docker ps –a –q)

## docker system prune
