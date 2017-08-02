# docker-forgetproxy

A transparent socks proxy for Docker.

If you are running Docker behind a corporate http proxy, you probably know how annoying it is
having to configure the proxy in every container.

You can run this container instead to just forget it and let docker run as if you had a direct connection
to the outside world.

This container is based on the [klabs / forgetproxy](https://hub.docker.com/r/klabs/forgetproxy/) one.
Proxy authentication support has been added, and pass proxy directly instead of through environment variables.


## Disclaimer

**/!\ BEWARE! THIS CONTAINER MODIFIES THE IPTABLES RULES FOR THE docker0 INTERFACE ON THE HOST MACHINE!!!**
**PLEASE READ THE EXPLANATIONS BELOW AND MAKE SURE TO EXIT IT CLEANLY OR TO RUN THE STOP COMMAND TO RESET YOUR RULES**


## How it works

All network connections coming out of the docker0 interface will automatically be proxified.


## How to use it

### start

    docker run -ti --net=host --privileged jtdoepke/forgetproxy proxy_addr proxy_port [username] [password]

It is recommended to let the container run in the foreground as it is configured to intercept the CTRL+C and clean
the iptables rules on exit.

### stop

If you have your container running, just press CTRL+C.
If you need to manually clean the iptables rules that are set by the container, you can run the following command
until you get an error telling you the rules do no exist.

    docker run --net=host --privileged jtdoepke/forgetproxy stop
