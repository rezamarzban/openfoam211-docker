# OpenFoam 2.1.1 in docker
###### Please note that this repo reuses publicly available scripts and the following instruction created by OpenCFD. Scripts and the instruction were modified according to my needs and to run OpenFoam 2.1.1.

### This instruction assumes that you have a working docker on your system.

To installing docker on `Ubuntu 22.0.4`:

```
sudo apt update
sudo apt install apt-transport-https ca-certificates curl software-properties-common -y
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt update
apt-cache policy docker-ce
sudo apt install docker-ce -y
sudo systemctl status docker
```

For other OS see guide in https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-22-04 and choose your OS from drop-down menu.

## How-to
###### Note that you may need to mark the install and start scripts as executable.

#### 1. Install 

Run the installOpenFOAM211 script,
This will download graphical `OpenFoam 2.1.1` docker image from hub.docker.io and setup a container. Otherwise, If you want to install minimal `OpenFoam 2.1.1` run:

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

#### 2. Run

To run graphical `OpenFoam 2.1.1` docker container run the "startOpenFOAM211" script. Otherwise, If you want to run minimal `OpenFoam 2.1.1` run:

```
docker start openfoam211-min -i
```

Then, run this command everytime in the minimal `OpenFoam 2.1.1` docker running container:
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
