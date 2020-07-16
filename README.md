# Docker Base
Repository to store base Dockerfiles.


## Development Dockers


### Usage

Development dockers are intended to be used instead deployed ones to test code being developed before building an image.

For that matter, simply creater a docker file using `krautzera/devbase` image and install needed depencies.

Once a development image is ready, create a `docker-compose-dev.yml` file in your project and add the following snippet to it, assuming we want to develop on `my-service`:
```
version: "3"

services:

  my-service:
    image: krautzera/devbase
    ports:
      - "10001:22"
    volumes:
      - "/root/projects:/root/projects"
```
And then restart my-service with `docker-compose -f docker-compose.yml -f docker-compose-dev.yml up -d --force-recreate my-service`.

Once that is done, you can enter your container via `ssh -A -p 10001 root@127.0.0.1`.

Then you will be inside a contianer running over the same docker-compose rules of deployment, but instead you can run your code being developed!

##### Notes

 - It assumes that the project to be edited is inside `/root/projects` folder in your host.
 - Any host port can be used instead 10001, but container port must be 22. You can't use the same host port in different containers.
 - Replace `krautzera/devbase` in image with the desired one with the service language, like: `krautzera/godev`
 - Consider mounting a `.bashrc` file into the home of the container so your commands history will be saved.
 - When entering the container via ssh, environment variables won't take effect, you'll have to re-define them on the mounted .bashrc (on bash exec they will work just fine).
 - Consider mounting as well your .gitconfig file to create commits with correct author.
 - Development containers password is `senhaboa`. If you don't want to type it every connection, add your's VM pub key to authorized_keys on the contianer.
 - Useful command to link VM's files to home mount point on containers are `ln <ORIGIN> <DESTINATION>`

### Build and Publish

#### Base Image for Development Containers
```
docker build -t krautzera/devbase:X.Y.Z -t krautzera/devbase:latest -f Dockerfile.devbase .
docker login
docker push krautzera/devbase:X.Y.X
docker push krautzera/devbase:latest 
```

#### Golang Development Container
```
docker build --build-arg version=X.Y.Z -t krautzera/godev:X.Y.Z -t krautzera/godev:latest -f Dockerfile.godev .
docker login
docker push krautzera/godev:X.Y.Z
docker push krautzera/godev:latest
```

##### Notes
 - Version used in godev image is based on the golang version (argument version refers to golang version to be installed).
 - If connecting via ssh, remember to add a .bashrc file setting `GOPATH` and `PATH` environment variables.


#### Node Development Container
```
docker build --build-arg version=X.Y.Z -t krautzera/nodedev:X.Y.Z -t krautzera/nodedev:latest -f Dockerfile.nodedev .
docker login
docker push krautzera/nodedev:X.Y.Z
docker push krautzera/nodedev:latest
```

##### Notes
 - Version used in nodedev image is based on the nodejs version (argument version refers to nodejs version to be installed).
