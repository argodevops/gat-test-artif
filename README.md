Contains files for testing

All will be needed on the environment.


To BUILD the image:

Required:
- Dockerfile
- start.sh
- .jar file

docker build -t gatling-test .

The above command is going to build a docker image, tag it as "gatling-test" and the . means using this location (i.e. this is where the Dockerfile is).


once image is built, can check it exists:
docker image ls

look for entry of gatling-test with image tag "latest"



docker-compose is a command which will look for a docker-compose.yml file containing instructions of what it is doing, and a .env file which contains environment variables it will have access to.

Prior to running docker-compose the docker-compose yml needs editing. Edit the image: line to have the built docker image and tag.
e.g. If your image is built as gatling-test and the tag was latest, then 
image: gatling-test:latest

At the bottom is a line saying
networks:
  nodejs-restapi:
    external: true

Delete / remote the external: true line. Otherwise will need to create a network named nodejs-restapi; removing the line means it's created automatically.

During the current experiment, the .env file does not need editing; it points to localhost (localhost of the docker container, so no conflicts/issues) and port 3000. There's no service being supplied so should be fine.

Run the command from the directory that docker-compose.yml file is in:
docker-compose up

This should start the container and then output some Gatling related information to the screen (i.e. users and counts, everything will fail). As the network nodejs-restapi isn't shared with any other containers, the tests will attempt against the containers own localhost address and fail everything.

Once done, it exits the container automatically. 
