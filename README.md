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
    
Chek to make sure that you have the image:
    
    sudo docker images
    
Start an interactive bash terminal on your image:

    sudo docker run -ti --rm rocker/tidyverse bash
