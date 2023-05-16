# Micorservces Architecture

## What is a Microservice (MS)

![Alt text](MS%20diagram%202.png)

It is an architectural style that structures as a collection of services which are:
* Independently deployable
* Loosely coupled
* Organized around business capabilities
* Owned by a small team

They enable a business to deliver larger, complex applications rapdily and more frequently. Each service performs a different task  and they communicate to each other through API's.

## Benefits of MS
* Faster deployment as you can work on indvidiual parts
* More frequent delivery
* Reliable
* More uptime as if one service goes down it should not affect the others
* Sustainable
* All these make a business more competitive

## Difference between 2/Ntier architecture and microservices architecture
* Ntier is usually deployed and updated altogether whereas microservcies are independant of each other and loosely coupled
* If one part fails in microservices then the other microservcies should still be able to run as they are independant. Whereas if the key tier in Ntier fails then they will all fail

## What is Docker?

![Alt text](docker%20diagram.svg)

Docker is an open platform designed to help developers build, deploy and run applications. It delivers software in packages called containers and it containes everything the software needs to run such as libraries and code.

## What is containerisation?
* This is a software process that bundles an application's code with all the files and libraries that it needs to be able to run. 
* The software can be deployed as a single package without needing to worry about the dependancies of each OS

## Virtualisation vs containerisation
* Virtualisation is fully isolated OS and VM instance whereas containerisation isolated the host OS machine and containers from each other. So potentially greater security for containerisation.
* Virtualisation can host multiple operating systems but containerisation runs all contianers on one OS
* Virtualisation uses a virtual hard disk for each VM for storage but containerisation uses the local hard disk
* Containerisation does not require as much CPU, memory and storage

## How to install an image
1. Open a Git Bash terminal
1. Use ```docker run``` hello-world to install the hello world image
2. Use ```docker images``` to see all the images that are installed

## How to run an image
1. Use ```docker run -d -p 80:80 nginx``` to install nginx image
-d is the dahsboard
-p is the port
2. Go to a browser and enter ```localhost``` to see the nginx page
* ```docker ps``` to show all the containers running
* ```docker stop 'name or ID of the image'``` to stop the image
* ```docker start 'name or ID of the image'``` to start the image
* ```docker rm 'name or ID of the image'``` to terminate the image
* ```alias docker="winpty docker"``` to create an alias for docker


## How to change Nginx HTML
1. Firstly use ```docker ps``` to find the contaienr ID for Nginx
2. Run ```docker exec -it 'container ID' sh``` to go to container location
3. We can now use linux commands to naviagte to the nginx location
4. Run these four commands to get to the HTML file location 
* ```cd /usr```
* ```cd share```
* ```cd nginx```
* ```cd html```
5. You can use ```pwd``` to make sure you are in the correct location after each command
6. After all four you should be at ```/usr/share/nginx/html```
7. Use ```apt update -y```
8. Then use ```apt install sudo``` to install sudo
9. Run ```sudo apt install nano``` to install nano
10. Then use ```sudo nano index.html``` to go into the html file
11. We can then change what the nginx page shows. Under the header we can change it to ```Welcome to the dream team of DevOps!``` and then when we go to our browser and run local host we will see the change.
12. This is what you should now see

![Alt text](Nginx.png)