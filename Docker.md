# Docker

Descargar e instalar docker para Windows, la versión actual es 2.0.0.3.
- https://hub.docker.com/editions/community/docker-ce-desktop-windows

Desde **PowerShell** ejecutar docker.run para inicializarlo.

### Instalar un Container

##### Instalación de rabbitMQ
Con estos comandos bajamos el contenedor, se inicializa con las configuraciones default y se lista para ver que se este ejecutando:
```docker
docker pull rabbitmq
docker run -d --hostname my-rabbit --name some-rabbit -p 5672:5672 -p 15672:15672 rabbitmq:3
docker container ps -a
```

Al reiniciar el sistema se debe levantar nuevamente los servicios con
```docker
docker start id-container
```

#### Docker file for rabbitmq
The Docker file you need is the following:

```
FROM rabbitmq

# Define environment variables.
ENV RABBITMQ_USER user
ENV RABBITMQ_PASSWORD user

ADD init.sh /init.sh
EXPOSE 15672

# Define default command
CMD ["/init.sh"]
```

And the init.sh:

```
#!/bin/sh

# Create Rabbitmq user
( sleep 5 ; \
rabbitmqctl add_user $RABBITMQ_USER $RABBITMQ_PASSWORD 2>/dev/null ; \
rabbitmqctl set_user_tags $RABBITMQ_USER administrator ; \
rabbitmqctl set_permissions -p / $RABBITMQ_USER  ".*" ".*" ".*" ; \
echo "*** User '$RABBITMQ_USER' with password '$RABBITMQ_PASSWORD' completed. ***" ; \
echo "*** Log in the WebUI at port 15672 (example: http:/localhost:15672) ***") &

# $@ is used to pass arguments to the rabbitmq-server command.
# For example if you use it like this: docker run -d rabbitmq arg1 arg2,
# it will be as you run in the container rabbitmq-server arg1 arg2
rabbitmq-server $@
```
This script also initialize and expose the RabbitMQ webadmin at port 15672.


### Docker on Azure
> - crear VM o usar una existente
> - si se cre una nueva hacerlo en un grupo de recursos llamado container
> - conectarse a la VM
> - descargar docker for windows e instalar
> - reiniciar la VM y volver a conectarse
> - abrir power shell e iniciar uso de docker