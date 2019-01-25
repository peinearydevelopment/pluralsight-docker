docker pull {image-name}
docker run {image-name}
    - docker run -p {host-port(80)}:{container-port-to-forward-to(80)} {image-name}
    - docker run -v {volume-container-path(/var/www)} -p {host-port(80)}:{container-port-to-forward-to(80)} {image-name}
        - docker inspect {container-name}
            - in the information it spits out, there is a Mounts property which shows the mounted folder that docker automagically created for you
    - docker run -v {volume-host-path($(pwd))}:{volume-container-path(/var/www)} -p {host-port(80)}:{container-port-to-forward-to(80)} {image-name}
        - with creating volumes, the data is stored on the host, so even if you delete the container, the data is still persisted
    - `-d` run in dameon mode --> won't show in console, will run behind the scenes
docker start 
docker stop
docker rmi {hash/partialhash}
docker rm {hash/partialhash}
    - docker rm -v {hash/partialhash}
        - this will remove the container and remove the volume/virtual mount as well
docker run -p 8080:3000 -v $(pwd):/var/www -w "/var/www" node npm start
    - this can be run from within the folder that contains the code(for instance the express app folder)
    - `-w` allows you to specify the directory to use when running commands, in this case the `npm start` command to start the express server
    - $(pwd) doesn't seem to work on windows. refer to the example below instead
    - `--net:{network_name} --name {image_name}`
docker build -t PD0.0.1/node .
    - `-t` stands for tag, allows us to define custom names for the images we are building
    - `-f` for filename if not the default `Dockerfile`
docker exec {container_id/name} {command}
    - allows us to execute a command in an already running container
docker network create --driver bridge isolated_network
    - creates a custom network, using a bridge network with a name of isolated_network


express-app
    npm install express express-generator
    node_modules/.bin/express --hbs
    docker pull node
    docker run -p 8080:3000 -v c:\code\z\pluralsight-docker\express-app:/var/www -w "/var/www" node npm start

asp-net-app
    npm install yo generator-aspnet
    node_modules/.bin/yo aspnet
    docker pull microsoft/aspnetcore-build
    docker run -i -t -p 8080:5000 -v c:\code\z\pluralsight-docker\asp-net-app\AspNetApp:/app -w "/app" microsoft/aspnetcore-build /bin/bash
    dotnet restore
    dotnet run

DOCKER FILE
    FROM (base_image)
    MAINTAINER (name_of_maintainer)
    RUN (commands_to_run)
    COPY (copy_things_into_the_container)
    ENTRYPOINT (main_entry_point_for_container)
    WORKDIR (working_directory_for_command_execution)
    EXPOSE (port)
    ENV (environment_variables)
    VOLUME

express-app-dockerfile
    docker build -t PD0.0.1/node .
    docker run -d -p 8080:3000 PD0.0.1/node

asp-net-app-dockerfil
    docker build -t PD0.0.1/aspnetcore-build .
    docker run -d -p 8080:5000PD0.0.1/aspnetcore-build