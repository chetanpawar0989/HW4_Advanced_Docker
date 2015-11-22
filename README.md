# Homework 4 - Advanced Docker

In this homework assignment, you'll get to practice several common architectural patterns for dealing with multiple docker containers.

1) **File IO**: You want to create a container for a legacy application. You succeed, but you need access to a file that the legacy app creates.

* Create a container that runs a command that outputs to a file.
	
	I am creating an image called 'legacyimage' from ubuntu:14.04 image, which will echo "This is legacy application" to op.txt. I will be building this image using Dockerfile.
	Dockerfile contents are as follows:
	```
	from ubuntu:14.04
	RUN apt-get 		update		
	RUN apt-get install -y socat
	RUN echo "This is legacy application" > op.txt
	CMD socat -d -d TCP-LISTEN:9001,fork SYSTEM:'cat op.txt'
	```
	This image is built by command: `docker build -t legacyimage .`

* Use socat to map file access to read file container and expose over port 9001 (hint can use SYSTEM + cat).
	
	This built image is run in container named 'legacyApp', which will read the contents of op.txt created while building an image and put it on port 9001 using following command.

	`docker run -it --rm --name legacyappcontainer legacyimage`

* Use a linked container that access that file over network. The linked container can just use a command such as curl to access data from other container.
	
	New linker container is created using docker run command and it is linked to legacyappcontainer by following command.

	`docker run -it --rm --name linkercontainer --link legacyappcontainer ubuntu:14.04 /bin/bash`

	This will open up a bash in newly created linkercontainer. We can see the linking with following command in docker host.

	`docker inspect -f "{{ .HostConfig.Links }}" linkercontainer`

	Going back to linkercontainer shell, first I need to install curl. Then I will curl to linked legacyApp container and port 9001.

	`curl legacyappcontainer:9001`


2) **Ambassador pattern**: Implement the remote ambassador pattern to encapsulate access to a redis container by a container on a different host.

* Use Docker Compose to configure containers.
* Use two different VMs to isolate the docker hosts. VMs can be from vagrant, DO, etc.
	vagrant init dbit/ubuntu-docker-fig; vagrant up
* The client should just be performing a simple "set/get" request.
* In total, there should be 4 containers....

3) **Docker Deploy**: Extend the deployment workshop to run a docker deployment process.

* A commit will build a new docker image.
* Push to local registery.
* Deploy the dockerized [simple node.js App](https://github.com/CSC-DevOps/App) to blue or green slice.
* Add appropriate hook commands to pull from registery, stop, and restart containers.


docker run -d -p 5000:5000 --restart=always --name registry registry:2
git commit -am 'some commit'
export ROOT=home/chetanpawar0989/DevOps/HW4_Advanced_Docker/Docker_Deploy/deploy
git remote add blue "file://$ROOT/blue.git"
git remote add green "file://$ROOT/green.git"
git push blue master		check on port localhost:8080
git push green master		check on port localhost:9090



### Evaluation

* File IO (20%)
* Ambassador pattern (40%)
* Docker Deploy (40%)

### Submission

[Submit a README.md](https://docs.google.com/a/ncsu.edu/forms/d/1oioay5bF5Le7PpuH1VAzxHCSNsOdkTvEqfrymHI1wjk/viewform?usp=send_form#start=invite) with a screencast (or .gif) for each component. Include code/scripts/configuration files in repo.

Assignment is due, Monday, November 23rd midnight