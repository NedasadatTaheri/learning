# Running gem5 on the Cluster <br />
Steps for running gem5 on Cluster:<br />
## Creating the gem5 Docker <br />
Install [Docker](https://docs.docker.com/engine/install/ubuntu/) and [Singularity](https://sylabs.io/guides/3.0/user-guide/installation.html) on local system to build the SIF file locally before copying it to the cluster. <br />
<br />
Retrieve Dockerfile with all gem5 dependencies from gem5 source at [gem5/util/dockerfiles/ubuntu-20.04_all-dependencies/Dockerfile](https://gem5.googlesource.com/public/gem5/+/refs/heads/stable/util/dockerfiles/ubuntu-20.04_all-dependencies/Dockerfile) <br />

Copy the Dockerfile to a new directory and build it. <br />
```
 sudo docker build ./ 
 ```

 Use "docker images" command to get image id:<br />
```
 docker images 
 ```

Save the docker image to a tar file:<br />
```
 sudo docker save id -o gem5.tar 
 ```

Use singularity to build a .sif container file based off of the archived docker image:
 ```
 sudo singularity build gem5.sif docker-archive://gem5.tar
 ```
Copy the .sif file to the cluster <br />
##### **The created sif file (gem5.sif) is already in "/work/alian" directory on the cluster.<br />
## Accessing the Cluster <br />
Logging into the cluster :
 ```
ssh {username}@login1.ittc.ku.edu
 ```
Request enteractive cluster node session with desired amount of cores/Memory/Time:
 ```
srun -p intel -N 1 -n 1 -c {Number of cores} --mem {Memory} -t {Number of days-00:00:00} --pty /bin/bash
```
Load singularity module :
  ```
module load Singularity/3.4.1 
```  
Run containerunder :
  ```
singularity run gem5.sif 
```  
Start tmux session for building process:
  ```
tmux
```  
Then checkout and [build gem5](https://www.gem5.org/documentation/general_docs/building).
    
    
    
    
    

