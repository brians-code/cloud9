
##Cloud9 docker image that provides a stable full-featured cloud-based IDE##

I recently decided to re-engineer my work flow around a cloud-based IDE. I wanted the benefits of being able to pull up a consistent interface and feature-set no matter what machine I'm on. I decided to set it up as a Docker image for easy deployment and cross-platform consistency. If my dev box and servers all exploded simultaneously, I could potentially be back up and coding in minutes. 

After a lot of research and experimentation, I decided to go with the excellent [Cloud9](https://github.com/c9/core) service. They were the only online IDE I found that offered the features I wanted, including integrated console and debugging, along with a publicly accessible repo. There are a few Cloud9 Docker images out there but they didn't quite fit into the workflow I had in mind, so I ended up rolling my own solution. I based it on [Phusion's stripped down version of Ubuntu](https://github.com/phusion/baseimage-docker). I run Cloud9 in the foreground as a child of the main [runit](https://en.wikipedia.org/wiki/Runit) process. You can see a great analysis of why that matters in [Phusion's README](https://github.com/phusion/baseimage-docker/blob/master/README.md).

For domain name resolution I use [jwilder's automated nginx reverse proxy](https://github.com/jwilder/nginx-proxy). It searches for a Docker container with a VIRTUAL_HOST environment variable that matches the domain name of the request. If it finds one, it forwards the request to it. Easy peasy.

Getting Started
-----

Start by adding this line to your local host file:

    0.0.0.0 cloud9.dev
    
Then clone this repo and enter the directory you cloned it into:

    git clone https://github.com/briansrepo/dockerfiles && cd dockerfiles

Now launch the Docker container:

    DIRECTORY=[directory] MYUID=$UID docker-compose -f nginx-proxy.yml -f cloud9.yml up -d

The [directory] variable should be set to the directory containing the code you want to work on. For example, I might enter "DIRECTORY=/home/brian/projects/acme.com". The MYUID=$UID sets the internal ownership of the docker file system to the system ID you are currently logged in with. This ensures the files you create and edit in the mounted directory will be owned by you.

At that point, I can pull up a browser and go to http://cloud9.dev to find my full development stack waiting for me. You can also run this container on a remote server and access it by putting your server's ip in your host file, such as "123.45.67.89 cloud9.dev". You can find the rest of the files I use to manage my LAMP workflow in my [lavalamp repo](https://github.com/briansrepo/lava-lamp).


> Written with [StackEdit](https://stackedit.io/).
