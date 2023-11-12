# Docker

## What is Docker?

Virtualization software which makes development and deploying applications much easier than before using docker.
Docker actually packages application with all the necessary dependencies, configuration, system tools and also runtime and env configuration. So application and it's running environment are both packet in single docker package which was easily share and distributed.

<br/>

#### Why is this a big deal?

and 

#### How it work before Docker?

### What problems Docker solves?

#### Development process before containers?

So if we have team of developers working on some application, they would install all the services that application depends (like databases, cecheing systems, some messaging tools) directly on their operating system.
So if we developing some javascript application, we maybe using some dependencies like postgresql database, redis for caching mechanizm and ect...
so very developer should install all the services, then configure and run them on their local development environment, and depended which operating system they are using, the installation process will be different, becouse for example it's different to install postgresql into macOS and on windows.  
Another thing is that when we trying to install some services on local operating system, it usually follows multiple steps of installation, and then configuration of the service, so with multiple commands with which to install configure and set up the service, the chances of something going wrong and error is actually happening was high.
And depending how complex is the application, this process can be pretty tedious, because if our app is huge and for example it have 10 dependencies and services, we need to do this all installation 10 times for each service. 

So now let's see how containers solve some of this problems.

With containers at first we don't need to install dependencies directly ton our operating system, because with Docker we have that service packaged in one isolated environment, and also it packaged with all configurations and dependencies. So we can start that service as a Docker container using a single Docker command which fetches the container package from internet and starts it on your computer, and the docker command will be same for all operation systems.
So if our app have 10 dependencies, we just need to run 10 docker commands for each container, and it will be it. 
So Docker standardizes process of running any service on any local dev environment so we can focus and work on development instead of trying install and configure services on your machine. + with Docker we can have different versions of the same application running in our local environment without having any conflict, which is very difficult to do if you are  installing that same application with different versions directly on your OS. 

#### Deployment process before containers?

Team would produce an application artifact and also set of instructions how to install and configure the application service in the server, also in we will some database service, for which we also need to write set of rules how to configure and install on the server, how to run it so that application could connect and use it.
So the development team should provide that artifact or package  to the operations team, and they will need to install and configure them into server, but as metioned they can be very varios problems with installing and setup process, we also can have conflict with dependency verisons where two services are depending on the same library for example but with different versions and when that happends it's going to make the setup process way more difficult and complex, also there a lot of things can go wrong, because instructions passing by textual guide, and the operations team will need to comunicate with development team to understand, whats going on and what they do wrong, becouse somewhere they can be some cases that developer just forget to mention that in the guide and that is the reasoun of failure the installation and configuration process.

With Docker this process was simplified, because now developers create the application package that doesn't only include the code itself but also all dependencies and configurations for running application or any other service, so beside to write some textual guide how to install dependencies and configure them, now they just packaged all dependencies and configurations into the artifact, and since it already encapsulated in one environment, the operations team don't need to configure any on this staff directly into server, and only need to run Docker command thats gets the containers package, which created by developers and run it on the server, and also withe same way operation teams will run any services that application needs, also as a docker contaioners,only thing that need to do operations team before, it's setup docker runtime on the server before they will be able to run contaioners, but thats just one time effort 

### Virtual machine vs Docker
