# docker-for-sta426
This repo contains the `Dockerfile` recipe for building images that can run [ARMOR](https://github.com/csoneson/ARMOR).

# To build the image (just FYI; students don't need to do this)

```
# building the image
git clone https://github.com/sta426hs2024/docker-for-sta426.git
cd docker-for-sta426
docker build -t markrobinsonuzh/sta426hs2024:latest .

# if successful:
docker push markrobinsonuzh/sta426hs2024:latest
```

# Install docker

One needs to install Docker; instructions are available for [Mac](https://docs.docker.com/desktop/install/mac-install/), [Linux](https://docs.docker.com/desktop/install/linux-install/) and [Windows](https://docs.docker.com/desktop/install/windows-install/).  


# Most important commands

```
# download image locally
docker pull markrobinsonuzh/sta426hs2024:latest

# start bash shell for testing from command line
docker run -it markrobinsonuzh/sta426hs2024:latest /bin/bash
#docker run --platform=linux/amd64 -it markrobinsonuzh/sta426hs2024:latest /bin/bash

# list local images
docker image ls

# run container with Rstudio; goto http://localhost:8886/ in browser (username: rstudio, password: as below)
# note the mapping of a local dir to that within container
docker run -v /Users/mark/projects/sta426_scratch:/home/rstudio/work --restart unless-stopped \
       --cpus 2 --memory 16GB -e PASSWORD=abc -p 8886:8787 markrobinsonuzh/sta426hs2024:latest

# list running containers (same as `docker ps` ?)
docker container ls

# login to already running container
docker exec -it <container name> /bin/bash
```

# Test that `ARMOR` is working

Goto [http://localhost:8886/](http://localhost:8886/) and type in username (`rstudio`) and password (as defined in the command above); RStudio Server should open. Go to Terminal and type:

```
cd /home/rstudio/work
git clone https://github.com/csoneson/ARMOR.git
cd ARMOR
snakemake --cores 1
```

# Some modifications to make ..

After cloning the `ARMOR` repository, we are not using the `snakemake --use-conda` part. Instead we need to make the following modificaitons to the `config.yaml`:

```
useCondaR: False
Rbin: /usr/local/bin/R
```

.. and in the `.Renviron` file, you'll need to set:

```
R_LIBS_USER="/usr/local/lib/R/site-library"
```



