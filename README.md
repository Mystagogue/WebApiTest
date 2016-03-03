# WebApiTest

A Visual Studio newly created Web API project, with only two changes made - to target a demonstration of ASP.NET Core running in a Ubuntu Docker container.  There are two samples on the web that are also clean, though each has a small problem. 

This [12/8/2015 post of "ASP.NET 5 and Docker"](http://luukmoret.github.io/2015/12/08/asp-net-5-with-docker/) is excellent, but requires "yo" and what I suggest is an improper adjustment to the project.json file.  Likewise, this ["Ten Step" 11/26/2015 post](http://dotnetliberty.com/index.php/2015/11/26/asp-net-5-on-aws-ec2-container-service-in-10-steps/) is excellent, but has a DockerFile that generates errors.  Both projects are using an out-dated version of .NET Core (one has beta8-coreclr, the other has rc1-final-coreclr).

The code sample in this repository resolves these issues, and uses the rc1-update1-coreclr edition of ASP.NET Core, which is still the latest as of 3/2/2016. 

##DockerHub

The DockerHub image generated from this sample can be found here:

https://hub.docker.com/r/barias/webapitest/

##Usage

On a Windows machine with Docker and VirtualBox installed, run the image as follows:

    docker run -d -p 4000:5000 [--name <friendlyName>] barias/webapitest
    
The above command indicates that port 4000 of your VirtualBox docker-machine will be forwarded to port 5000 of the barias/webapitest Docker container.  Finally, before trying to use curl or a browser to reach the running docker container, it is necessary to configure the docker-machine so that a port of your choosing (we'll use 6000 as an example) from your local machine is forwarded to port 4000 of the docker-machine.  To do this, open the Oracle VM VirtualBox Manager.  Select the "default" vm (this is created by Docker) and select Settings->Network.  On Adapter 1 (for NAT), select "Port Forwarding" and add a new rule.  You may give whatever name you like.

The rule will forward 127.0.0.1:6000 to port 4000.

Select OK.

Now in a browser or with curl, enter this:

    http://localhost:6000/api/values

The (very uninteresting) response will be:

    ["value1","value2"]

This means that ASP.NET Web API is working, and running on Linux.  Now that *is* interesting!

Note: Chrome has a security mechanism that will disallow using the address `http://localhost:6000` because it does not allow any port specification besides port 80.  So use the Edge browser, Firefox, or just try curl at a command prompt.
