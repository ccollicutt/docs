<img src="../images/docker-logo.png" alt="Vagrant" style="width: 100px;"/>

# Install Open Baton using Docker

This tutorial will guide towards the installation of a minimal Open Baton environment on a single Docker container, comprising the following components:

* The NFVO implemented in java using the [spring.io][spring] framework. For more details about the NFVO architecture, you can refer to the next sections
* [RabbitMQ][reference-to-rabbit-site] as messaging system
* Test VIM Driver for being able to execute the [hello world][dummy-NSR] tutorial without needing an OpenStack instance
* OpenStack VIM Driver for deploying VNFs on OpenStack
* Generic VNFM for the instantiation of VNFs part of the Open Baton ecosystem

In case you would like to run each component in an individual docker container, you can make use of the docker compose files available on the [bootstrap repository][docker-compose] 

## Requirements

You need to have [Docker] installed, please refer to the official docker documentation to setup it on your environment. 

## User guide

To have a running standalone Open Baton Docker container type the following commands:

```bash
docker pull openbaton/standalone
docker run --name openbaton -d -p 8080:8080 -p 5672:5672 -p 15672:15672 -p 8443:8443 -e RABBITMQ_BROKERIP=<RabbitMQ IP> -e RABBITMQ_ADMIN_PSWD=<RabbitMQ admin password> openbaton/standalone
```

***VERY IMPORTANT NOTE*** - You should put as input for the RABBITMQ_BROKERIP the RabbitMQ IP making sure that this IP can be
  reached by external components (VMs, or host where will run other VNFMs) otherwise you will have runtime issues.
  In particular, you should select the external IP of your host on top of which the docker container is running

***NOTE*** - the RABBITMQ_ADMIN_PSWD is the password which will be assigned to the "admin" RabbitMQ user. In case you do not set the RABBITMQ_ADMIN_PSWD variable while running the container, the "openbaton" default value will be used as password.

***NOTE*** - With the command above you will run the latest Open Baton version. You can see all the standalone Open Baton Docker images available from [this][reference-to-op-repo-on-public-docker-hub] list (the RABBITMQ_ADMIN_PSWD environment variable is supported only from version 5.0.0).

After running the container you should see as output an alphanumeric string (which represents the full ID of the Open Baton container running) similar to the following:

```bash
cfc4a7fb23d02c47e25b447d30f6fe7c0464355a16ee1b02d84657f6fba88e07
```

After few minutes the Open Baton NFVO should be started, then you can open a browser and go on [localhost:8080]. To log in, the default credentials for the administrator user are:

```
user: admin
password: openbaton
```

### Troubleshooting


To verify that the container is running you can type the following command:

```bash
docker ps
```

which output should be similar to the following:

```bash
CONTAINER ID        IMAGE                        COMMAND                  CREATED             STATUS                   PORTS                                                                                              NAMES
cfc4a7fb23d0        openbaton/standalone:latest  "/usr/bin/supervisord"   49 seconds ago      Up 49 seconds            0.0.0.0:5672->5672/tcp, 0.0.0.0:8080->8080/tcp, 0.0.0.0:8443->8443/tcp, 0.0.0.0:15672->15672/tcp   openbaton
```

The standalone container uses the standard debian packages for installing the open baton components, thus logs are available under /var/log/openbaton. To connect to the running container containing Open Baton you can type the following command:

```bash
docker exec -ti openbaton bash
```

To stop and delete the running Open Baton container you can type respectively the following commands:

```bash
docker stop openbaton
docker rm openbaton
```

[docker]: https://www.docker.com/
[docker-compose]: https://github.com/openbaton/bootstrap/tree/develop/distributions/docker/compose
[spring]: https://spring.io
[localhost:8080]:http://localhost:8080/
[use-openbaton]:use.md
[dummy-NSR]:dummy-NSR.md
[reference-to-rabbit-site]:https://www.rabbitmq.com/
[reference-to-op-repo-on-public-docker-hub]:https://hub.docker.com/r/openbaton/standalone/tags/

<!---
Script for open external links in a new tab
-->
<script type="text/javascript" charset="utf-8">
      // Creating custom :external selector
      $.expr[':'].external = function(obj){
          return !obj.href.match(/^mailto\:/)
                  && (obj.hostname != location.hostname);
      };
      $(function(){
        $('a:external').addClass('external');
        $(".external").attr('target','_blank');
      })
</script>
