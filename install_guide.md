# Install R, RStudioServer, and system libraries on AWS machines with Ubuntu Server OS

There are several ways how software can be installed on AWS machines. In this guide, we will **not** install all software directly on the machine but use Docker as an additional tool to make use of available _recipes_ (Dockerfiles) how to install recent R and RStudioServer on Linux.
In this guide, we will assume that the machine has already been launched. The setup requires the following steps:

1. Login to the machine via SSH

2. Install Docker on the machine

3. Run an RStudioServer Docker container

4. Install system libraries in the container that are needed by R packages

5. Install R packages


## 1. Login to the machine via SSH

You can connect to the ubuntu machine on AWS via SSH e.g. from the command line with `ssh -i /path/to/your/key.pem ubuntu@x.x.x.x`, where x.x.x.x is the public IP address of the machine. If you use Windows or MacOS on your computer, other software such as PuTTY can be used.


## 2. Install Docker on the machine

You can follow the instructions on `https://docs.docker.com/engine/install/ubuntu`.



## 3. Run an RStudioServer Docker container

We can use available _recipes_ from the https://github.com/rocker-org project:

```
sudo docker run -d -p 8787:8787 -e PASSWORD=yourpasswordhere --restart="unless-stopped" rocker/rstudio
```

Running this command may take while because a lot of software is downloaded. Please notice that after successful creation of the container, this will give you the container ID, which we will need in the next step.


## 4. Install system libraries in the container that are needed by R packages

To install software _within_ the container, we must first run:

`docker exec -it CONTAINER_ID /bin/bash`

This gives us root access in the container and we can install some system libraries hat are needed by r-spatial packages by running:

```
apt update
apt install libproj-dev libgdal-dev gdal-bin libnetcdf-dev libudunits2-dev
```

## 5. Install R packages

Open your browser and access Rstudio: http://x.x.x.x:8787. If this does **not** give a a login screen but a _URL not found_ message, you most likely need to add a security rule for making port 8787 accessible in the AWS machine settings. 

If the login screen shows up, congratulations, you can login using `rstudio` as username and the password given before.
After successflul login, you can install packages as on your local machine, e.g. with:

```
install.packages("sf","gdalcubes","rstac")
```



