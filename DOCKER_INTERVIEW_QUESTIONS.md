# Docker interview questions and answers

These questions cover the container model, image builds, storage, networking, Compose, and production concerns. Explain what happens at build time and what happens when a container runs.

## Study checklist

- [ ] Build an image from a Dockerfile and inspect its layers.
- [ ] Run an app and database with Compose, then explain their network and storage.
- [ ] Diagnose a container that exits or cannot be reached from the host.

## Images and containers

Sources: [Docker containers](https://docs.docker.com/get-started/docker-concepts/the-basics/what-is-a-container/), [Docker images](https://docs.docker.com/get-started/docker-concepts/the-basics/what-is-an-image/), [running containers](https://docs.docker.com/engine/containers/run/).

### 1. What is a container?

A container is an isolated process with its own filesystem view, network settings, and resource limits. On a Linux host, containers share the host kernel rather than booting a separate guest kernel.

### 2. How does a container differ from a virtual machine?

A VM runs a guest operating system on virtualized hardware. A container isolates processes while sharing the host kernel. Containers often start faster and use fewer resources, but kernel sharing is a different isolation boundary.

### 3. What is an image, and how does it differ from a container?

An image is an immutable package of filesystem layers and configuration. A container is a running or stopped instance created from an image, with its own writable layer and runtime state.

### 4. What happens to files written inside a container?

Writes go to its writable layer unless a mount redirects them. Removing the container removes that layer. Put data that must survive container replacement in a volume or another persistent service.

### 5. What is a registry?

A registry stores and distributes images. A repository contains related image versions. Tags are human-readable references and can move; a digest identifies exact image content.

### 6. Why might you deploy an image by digest instead of tag?

A digest pins the exact artifact tested and avoids a mutable tag resolving to new content. You can still use tags for discoverability, but record the digest for reproducible promotion.

### 7. What do `docker run`, `docker start`, and `docker exec` do?

`run` creates and starts a new container. `start` restarts an existing stopped container. `exec` runs another process inside a running container, often for diagnosis.

### 8. Why does a container stop immediately after starting?

The main process exited or failed. Inspect `docker logs` and the exit code. A container's lifetime follows its main process; it does not stay alive merely because the image exists.

## Dockerfiles and builds

Sources: [Dockerfile reference](https://docs.docker.com/reference/dockerfile/), [build best practices](https://docs.docker.com/build/building/best-practices/), [multi-stage builds](https://docs.docker.com/build/building/multi-stage/), [build cache](https://docs.docker.com/build/cache/).

### 9. What does a Dockerfile describe?

Instructions for building an image, including a base image, copied files, commands, environment, and default process. The build creates layers that can be cached and reused.

### 10. How do `RUN`, `CMD`, and `ENTRYPOINT` differ?

`RUN` executes during image build and creates a layer. `CMD` supplies a default command or arguments at container start. `ENTRYPOINT` sets the executable that the container runs; a runtime command can override `CMD` more directly.

### 11. What is the difference between `COPY` and a bind mount?

`COPY` places files into an image at build time. A bind mount makes a host path visible in a container at runtime. Development often uses bind mounts for live edits; production images normally include the app artifact.

### 12. What is the build context?

It is the set of files the builder can access for instructions such as `COPY`. Use `.dockerignore` to exclude unnecessary files, secrets, and large directories from that context.

### 13. How does build caching work?

The builder reuses a cached result when an instruction and its relevant inputs have not changed. Copy dependency manifests and install dependencies before copying frequently changing application files to retain useful cache hits.

### 14. What is a multi-stage build?

A Dockerfile uses separate build and runtime stages. Copy only the compiled artifact and required runtime files into the final stage. This can reduce image size and remove build tools from production.

### 15. Why should a base image be chosen carefully?

It determines the runtime, operating system packages, compatibility, and security update path. Choose a trusted maintained image, keep it updated, and pin versions or digests when reproducibility matters.

### 16. Why run as a non-root user inside a container?

It limits what a compromised process can change in the container and through mounts. It does not replace host isolation, permissions, or other security controls.

### 17. Why should secrets not be passed through Dockerfile `ARG` or `ENV`?

Build arguments and image environment can appear in metadata, history, or layers. Use BuildKit secret mounts for build-time secrets and a runtime secret mechanism for deployed containers.

### 18. What is the difference between `EXPOSE` and publishing a port?

`EXPOSE` documents the port an image expects to listen on; it does not open the host port. `docker run -p host:container` publishes a container port on the host.

## Storage, networking, and Compose

Sources: [Docker storage](https://docs.docker.com/engine/storage/), [volumes](https://docs.docker.com/engine/storage/volumes/), [Compose overview](https://docs.docker.com/compose/), [Compose networking](https://docs.docker.com/compose/how-tos/networking/), [Compose secrets](https://docs.docker.com/compose/how-tos/use-secrets/).

### 19. How do volumes and bind mounts differ?

A Docker volume is managed by Docker and persists beyond a container. A bind mount maps a specific host path into a container. Volumes fit container-owned persistent data; bind mounts fit files the host and container both need to edit.

### 20. What happens when a mount covers files already in an image directory?

The mounted content hides those files while the mount is active. This often explains why an app built into an image appears to vanish after a development bind mount is added.

### 21. What does `localhost` mean inside a container?

It refers to that container's own network namespace. To reach another Compose service, use its service name and container port on the Compose network, not `localhost`.

### 22. How do Compose services discover one another?

Compose normally creates a project network. Services on it can use each other's service names as hostnames. Published host ports are for connections from outside that network.

### 23. What is Docker Compose for?

Compose describes a multi-container application in a YAML file: services, images or builds, networks, volumes, and configuration. It is useful for repeatable local development and some deployment setups.

### 24. Does `depends_on` mean a database is ready for queries?

Starting a database container is not the same as readiness. Use a health check and an appropriate dependency condition where supported, and make the app handle transient connection failures.

### 25. How should an app persist database data in Compose?

Mount a named volume at the database's data directory. Recreating the container then preserves the volume. Backups are still needed because persistence is not a backup strategy.

### 26. How should Compose secrets be supplied?

Define a secret and grant it only to services that need it. Compose mounts secrets as files for Linux containers. Avoid committing real secret values or baking them into an image.

### 27. Why is a container reachable from another container but not the host?

The service may have no published port, may listen only on `127.0.0.1` inside its container, or may use the wrong host-to-container port mapping. Inspect the app listener and `docker ps` port mapping.

## Operations and security

Sources: [Docker health checks](https://docs.docker.com/reference/dockerfile/#healthcheck), [Docker logs](https://docs.docker.com/reference/cli/docker/container/logs/), [Docker resource constraints](https://docs.docker.com/engine/containers/resource_constraints/), [Docker Scout](https://docs.docker.com/scout/).

### 28. What is a health check?

A command that reports whether the app inside a container is functioning. A running process can be unhealthy. Choose a check that covers a useful dependency or application behavior without causing heavy load.

### 29. How do you inspect a failing container?

Check `docker ps -a`, `docker logs`, and `docker inspect` for the exit code, command, mounts, environment, and health status. Reproduce with the same image and runtime settings rather than changing the image blindly.

### 30. Why set CPU and memory limits?

They prevent one container from using unlimited host resources and reveal resource needs. A process that exceeds memory limits may be killed, so monitor usage and size limits from evidence.

### 31. Why should images be scanned and rebuilt regularly?

Base images and dependencies gain security fixes. Scanning finds known issues in a particular image, while rebuilding and redeploying brings updated layers into use. Review whether a finding is reachable and how to remediate it.

### 32. Why is the Docker socket sensitive?

Access to the daemon socket can grant broad control over images, containers, mounts, and often the host. Do not mount it into an untrusted application container merely for convenience.

### 33. Why should an app handle termination signals?

Deployments stop containers before replacing them. A process should stop accepting new work, finish or release current work when possible, and exit within its shutdown window.

## Practical interview scenarios

### 34. A Node app works locally but fails in its container. What do you check?

Inspect logs and the image's working directory, copied files, dependency install, runtime command, environment, and listener address. Confirm the image contains its production build and the port is published correctly.

### 35. Your database data disappears after `docker compose down`. Why?

Check whether the database writes to its container layer or a named volume. Also check whether `down --volumes` removed the volume. Restore from backup if the volume was deleted.

### 36. A frontend container cannot reach `http://localhost:5432`. What is wrong?

Inside that container, `localhost` points back to the frontend container. Use the database service name and its container port for container-to-container traffic. Keep database credentials on the server side, not in browser code.

## Further reading

- [Docker concepts](https://docs.docker.com/get-started/docker-concepts/)
- [Dockerfile reference](https://docs.docker.com/reference/dockerfile/)
- [Compose documentation](https://docs.docker.com/compose/)
