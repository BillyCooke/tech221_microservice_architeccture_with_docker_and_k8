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

## Difference between 2/Ntier architecture and microservcies architecture
* Ntier is usually deployed and updated altogether whereas microservcies are independant of each other and loosely coupled
*

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