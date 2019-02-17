* [Dev vs Prod](#dev-vs-prod)
* [Dev](#dev)
    * [Build Docker Image](#build-docker-image)
    * [Create container](#cretate-container)
        * [Create Container With Build Option](#create-container-with-build-option)
    * [Run The Test](#run-the-test)
* [Prod](#prod)
    * [Build](#build)
    * [Create Container](#create-container)
* [Easy Create React App](https://github.com/harryosmar/react-docker/blob/master/react-create-app.md)

## Dev vs Prod

![CMD diff Dev vs Prod](https://github.com/harryosmar/react-docker/raw/master/src/images/cmd-diff-dev-vs-prod.jpg)
![Dev Server](https://github.com/harryosmar/react-docker/raw/master/src/images/dev-server.jpg)
![Prod Server](https://github.com/harryosmar/react-docker/raw/master/src/images/prod-server.jpg)

## Dev

- `Dockerfile` : [`Dockerfile.dev`](https://github.com/harryosmar/react-docker/blob/master/Dockerfile.dev)
- use [`docker-compose.yml`](https://github.com/harryosmar/react-docker/blob/master/docker-compose.yml)
- entrypoint command : `npm run start`
- use react `development-server`
- use docker `volumes` option to mapping :
    - `local dir` with the `container dir`
    - for folder `node_modules/` bookmark to use the dir inside container, instead of replacing it with the `reference`/`mapping` to local dir volume.
        ![With Bookmark Volume](https://github.com/harryosmar/react-docker/raw/master/src/images/docker-vol-with-bookmark.jpg)
        ![Without Bookmark Volume](https://github.com/harryosmar/react-docker/raw/master/src/images/docker-vol-without-bookmark.jpg)
- url : http://localhost:3000/

### Build Docker Image

Manually using `docker-cli` to build the docker image.

```
docker build -f Dockerfile.dev .
```

Using `docker-compose` to [Create Container With Build Option](#create-container-with-build-option)

#### Create Container

```
docker-compose up
```

##### Create container with build option

If there is a changes to `Dockerfile.dev` or new sevices added to `docker-composer.yml` file.

```
docker-compose up --build
```

### Run The Test

List the services :

```
docker ps -a
```
Then get the container id of the web-app. Then run script below. Replace `[CONTAINER-ID]` with the actual container id value.

```
docker exec -it [CONTAINER-ID] npm run test
```

## Prod

- `Dockerfile` : [`Dockerfile`](https://github.com/harryosmar/react-docker/blob/master/Dockerfile)
- do not use `docker-compose.yml`
- entrypoint command : `npm run build`
- use `nginx` server
- Has 2 phases :
    - `build` phase : install npm dependencies, then build the files for *production* usage inside
    - `run` phase : copy build files from `/app/build` folder in `build` phase, then create the web server/`nginx`
![2 phases](https://github.com/harryosmar/react-docker/raw/master/src/images/prod-dockerfile-phase.jpg)
- url : http://localhost:8080/

### Build

```
docker build .
```

### Create Container

```
docker run -p 8080:80 [CONTAINER-ID]
```