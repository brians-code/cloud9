I recently decided to re-engineer my work flow around a cloud-based IDE. The benefits of being able to pull up a consistent interface and feature-set no matter where I am are clear. I decided to set it up as a Docker image for even more redundant consistency. Even if my dev box and servers all exploded simultaneously, I could be back up and running in minutes. 

After a lot of research and experimentation, I decided to go with the excellent [Cloud9](https://github.com/c9/core) service. They offered the features I wanted, including integrated console and debugging, plus they make their code publicly available for open-source development. There are a few Cloud9 Docker images out there but they didn't quite fit into the workflow I had in mind, so I ended up rolling my own solution.

The phusion-cloud9 image can be started directly, but to get a full development environment going you need to be able to access it reliably. I use the Docker Compose files in the root directory of this repo, with the following command:

    CLOUD9=$cloud9 MYUID=$UID docker-compose -f nginx-proxy.yml -f cloud9.yml up -d

I then enter the following in my local host file:

    0.0.0.0 cloud9.dev

At that point, I can pull up any browser and go to cloud9.dev to find my full development stack waiting for me. You can find the rest of the files I use to manage my workflow in my [lavalamp repo](https://github.com/briansrepo/lava-lamp).


> Written with [StackEdit](https://stackedit.io/).