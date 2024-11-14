# OpenFoam 2.1.1 in docker
###### Please note that this repo reuses publicly available scripts and the following instruction created by OpenCFD. Scripts and the instruction were modified according to my needs and to run OpenFoam 2.1.1.

### This instruction assumes that you have a working docker on your system.

## How-to
###### Note that you may need to mark the install and start scripts as executable.

#### 1. Run the installOpenFOAM211 script.
This will download graphical `OpenFoam 2.1.1` docker image from hub.docker.io and setup a container.

If you want to install minimum `OpenFoam 2.1.1` run:

```
username="$USER"
user="$(id -u)"
home="${1:-$HOME}"

imageName="rm314159/openfoam211-min:latest"
containerName="openfoam211-min"   

# List container name :
echo "*******************************************************"
echo "Following Docker containers are present on your system:"
echo "*******************************************************"
docker ps -a 


# Create docker container for OpenFOAM operation   
echo "*******************************************************"
echo ""
echo "Creating Docker OpenFOAM container ${containerName}"

docker run  -it -d --name ${containerName} --entrypoint /bin/bash --user=${user}   \
    -e USER=${username}                                     \
    --workdir="${home}"                                     \
    --volume="${home}:${home}"                              \
    --volume="/etc/group:/etc/group:ro"                     \
    --volume="/etc/passwd:/etc/passwd:ro"                   \
    --volume="/etc/shadow:/etc/shadow:ro"                   \
    --volume="/etc/sudoers:/etc/sudoers:ro"                 \
    --volume="/etc/sudoers.d:/etc/sudoers.d:ro"             \
    ${imageName}
```

#### 2. To run OpenFoam run the "startOpenFOAM211" script.

If you want to run minimum `OpenFoam 2.1.1` run:

```
docker start openfoam211-min -i
```

Then, run in the docker environment:
```
source /opt/OpenFOAM/OpenFOAM-2.1.x/etc/bashrc
```

#### 3. "Getting started" instruction from OpenFOAM website.
Use it to check if everything is working well.

##### Getting Started
Create a project directory within the $HOME/OpenFOAM directory named <USER>-2.1.1 (e.g. chris-2.1.1 for user chris and OpenFOAM version 2.1.1) and create a directory named run within it, e.g. by typing:

```bash
mkdir -p $FOAM_RUN
```
Copy across the backward facing step example, generate the mesh with blockMesh and run the steady flow, incompressible solver simpleFoam

```bash
cd $FOAM_RUN
cp -r $FOAM_TUTORIALS/incompressible/simpleFoam/pitzDaily .
cd pitzDaily
blockMesh
simpleFoam
paraFoam
```
