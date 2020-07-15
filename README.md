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
 - Consider mounting a `.bashrc` file into the home of the container so your commands history will be saved.
 - Any host port can be used instead 10001, but container port must be 22. You can't use the same host port in different containers.


### Build and Publish

devbase:
```
docker build -t krautzera/devbase:X.Y.Z -t krautzera/devbase:latest -f Dockerfile.devbase .
docker login
docker push krautzera/devbase:X.Y.X
docker push krautzera/devbase:latest 
```
