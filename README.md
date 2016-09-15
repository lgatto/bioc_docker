Information about available containers, installation and modification can be found on the [Bioconductor Docker Page](http://bioconductor.org/help/docker/).

### General Docker Usage

#### List which docker machines are available locally
    docker images
#### List running containers
    docker ps
#### List all containers
    docker ps -a
#### Get container IP address
    docker inspect --format '{{ .NetworkSettings.IPAddress }}' <name>
#### Keep a container running at startup
    docker run -itd <name>
#### Shutdown container
    docker stop <name>
#### Resume container
    docker start <name>

#### Delete container
    docker rm <name>
#### Shell into a running container with either of the following:
    docker exec -it <name> /bin/bash
    docker attach <name>

#### Building and modifying the Bioconductor docker images

The BioC Dockerfiles are not directly edited. Instead, 
for each biocView, there is a common `Dockerfile.in`,
from which two output files for `release` and `devel` files are generated by running 
the `rake` command. All the creation is controlled by the `Rakefile`, 
which will also take care if any of the dependencies (i.e. the *.in files) have changed.

E.g. the `Dockerfile` for the BioC development branch for Proteomics packages 
is created from `src/proteomics/Dockerfile.in` and placed into 
`out/devel_proteomics/Dockerfile`. 

If you want to build and push an image to dockerhub, copy the `auth.yml.template` 
file over to `auth.yml`, and add your dockerhub credentials. The pushing is done 
via the `docker push` command, so for this to succeed you'll have to `docker login` 
first. Then all you need is:

````
rake devel_core 
````

