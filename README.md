# Introduction
Thanks to [https://github.com/overshard](https://github.com/overshard) for the java install and initial script setup.  I've modified the script to do a Minecraft Forge install so that you can edit from here and install the mods you like.

# docker-minecraft.

A nice and easy way to get a Minecraft server up and running with mods using docker. For
help on getting started with docker see the [official getting started guide][0].
For more information on Minecraft and check out it's [website][1].

# Download
Download these, and place them in a directory named `forge` so that they will get copied in.

`wget "http://files.minecraftforge.net/maven/net/minecraftforge/forge/1.10.2-12.18.2.2099/forge-1.10.2-12.18.2.2099-installer.jar"`

`wget "https://s3.amazonaws.com/Minecraft.Download/versions/1.10.2/minecraft_server.1.10.2.jar"`

## Building docker-minecraft

Running this will build you a docker image with the latest version of both
docker-minecraft and Minecraft itself.

    git clone https://github.com/cmiller82/docker-minecraft
    cd docker-minecraft
    sudo docker build -t cmiller/minecraft .

After modifying the scripts you may need to rebuild with --no-cache to get everything updated.

    sudo docker build --no-cache -t cmiller/minecraft .


## Running docker-minecraft

Running the first time will set your port to a static port of your choice so
that you can easily map a proxy to. If this is the only thing running on your
system you can map the port to 25565 and no proxy is needed. i.e.
`-p=25565:25565` . If you want to enable the query protocol you need
to add another -p=25565:25565/udp to forward the UDP protocol on the
same port as well.
Also be sure your mounted directory on your host machine is
already created before running `mkdir -p /mnt/minecraft`.

    sudo docker run -d=true -p=25565:25565 -v=/mnt/minecraft:/data cmiller/forge /start

For troubleshooting run:

    sudo docker run -it --entrypoint=/bin/bash -p25565:25565 -v=/mnt/minecraft:/data cmiller/forge

You will need to run the /start script by hand, and make changes to the source file as needed.

Add mods to the Docker file after the forge and minecraft .Jar's are added, and include any setup in the start file.

Once you are done, when you start/stop docker-minecraft you should use the container id
with the following commands. To get your container id, after you initial run
type `sudo docker ps` and it will show up on the left side followed by the
image name which is `cmiller/minecraft:latest`.

    sudo docker start <container_id>
    sudo docker stop <container_id>


### Notes on the run command

 + `-v` is the volume you are mounting `-v=host_dir:docker_dir`
 + `overshard/minecraft` is simply what I called my docker build of this image
 + `-d=true` allows this to run cleanly as a daemon, remove for debugging
 + `-p` is the port it connects to, `-p=host_port:docker_port`


[0]: http://www.docker.io/gettingstarted/
[1]: http://minecraft.net/
