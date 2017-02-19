# docker-with-r
Some notes on using docker with R

Here are the links from which I culled the information in these notes:

https://hub.docker.com/_/r-base/

https://github.com/rocker-org

http://ropenscilabs.github.io/r-docker-tutorial/


First install docker:

    sudo apt-get update
    sudo apt-get install docker.io
    
Now install a Rocker image:

    sudo docker pull rocker/tidyverse
    
Or for the most up-to-date compilers, etc
    
    sudo docker pull r-base
    
Chek to make sure that you have the images:
    
    sudo docker images
    
Start an interactive bash terminal on your image:

    sudo docker run -ti --rm rocker/tidyverse bash

or

    sudo docker run -ti --rm r-base bash
    
If you go the `r-base` route, you'll need to install a number of other components by hand (no sudo needed since you're root!)
        
        apt-get update
        apt-get install libcurl4-openssl-dev libssl-dev
        apt-get install git
        apt-get install vim

Now install whatever packages you need in R (first invoke R in the terminal)

        install.packages("RcppNumerical")
        install.packages("devtools")
        library(devtools)
        install_github("fditraglia/forcedMigration")
        
Make whatever other changes and additions you want (e.g. edit `.vimrc` or `.bashrc`) and then we'll save the image for reuse later so we don't need to carry out these steps! Launch a new terminal window (a fresh ssh session if you're connecting to a remote machine) that's *not* within the docker bash session while leaving the window with the docker bash session open. Then do the following:

        sudo docker ps
        
to find the container id (e.g. `f7bd5d97470c`) and then commit it:

        sudo docker commit -m "description here" <container id goes here>  name_goes_here
        
Now check that it worked:

        sudo docker images
        
If your image is listed you're all set, so go ahead and `exit` from the docker container bash session.

## How can I use this?
Once you have a docker image set up the way you want it, you can launch it and do work on it. For example:
        
        sudo docker run -ti --rm my-image-name-here bash
        
to start up an interactive bash session. There may be instances when you want to be able to connect to this session from multiple places at once. Say for example, you ran the above on your work machine and started a long-running computation. When at home, you wanted to check the results by using `ssh` to connect to your work machine. Here's how you could subsequently connect to the running docker container:

        sudo docker exec -i -t container_name_here /bin/bash
        
where the `container_name_here` refers not to the name of the *image* but of the *container*. This will have an auto-generated name that is typically something silly and memorable such as `loving_heisenberg`. There are some more details here:

http://askubuntu.com/questions/505506/how-to-get-bash-or-ssh-into-a-running-container-in-background-mode#507009
