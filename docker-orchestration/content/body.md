## Orchestration in Action
----
The NCAR Wx Stations

<a href="http://portal.chordsrt.com" target="_blank"> 
<img src='images/chords-mosaic.png' height='400' /> 
</a>

!SLIDE
## DockerHub (Free as in beer)
----
A public repository of prebuilt images

<img src='images/dockerhub-repos.png' height='150' />

And your own

<img src='images/dockerhub-ncareol.png' height='150' />
!SLIDE
## Containers as Services
----
<img src='images/chords-services.png' height='500' />

!SLIDE
## Image Customization
----
Dockerfile

```yml
FROM grafana/grafana:4.6.2 
MAINTAINER martinc@ucar.edu

# Update
RUN apt-get update 

# Specify plugins via environment
ENV GF_INSTALL_PLUGINS=\
grafana-clock-panel,\
grafana-worldmap-panel,\
grafana-piechart-panel

# Normal start
ENTRYPOINT ["/run.sh"]
```

```sh
docker build -t ncareol/grafana:4.6.2 .
docker push ncareol/grafana:4.6.2
```

!SLIDE
## Custom Startup
----
```yml
FROM kapacitor:1.3 
MAINTAINER martinc@ucar.edu
# Update
RUN apt-get update 
# Add custom startup
WORKDIR /
COPY ./kapacitor_start.sh kapacitor_start.sh
# Start kapacitor
CMD ["/bin/bash", "-f", "kapacitor_start.sh"]
```

kapacitor_start.sh
```sh
#!/bin/bash
if [ $KAPACITOR_ENABLED == "true" ]; then
  echo "*** Kapacitor enabled; starting"
  kapacitord
else
  echo "Kapaciptor was not enabled (via KAPACITOR_ENABLED); not starting"
fi
```

!SLIDE
## Docker Volumes
----
| |
|-|
|Similar to containers, but just used for storage.|
|Exist in a Docker namespace; easily mounted by containers.|
|Persistant across reboots.|
|Avoids O/S filesystem naming requirements.|
|Can be accessed simply by running a lightweight container.|
!SLIDE

## Orchestration
----
docker-compose.yml

```yml
version: '2'
services:
  app:
    container_name: chords_app
    image: ncareol/chords:0.9.5
    volumes:
      - mysql-data:/var/lib/mysql
    ports:
        - "80:80"
    links:
      - mysql
  mysql:
    container_name: chords_mysql
    image: mysql:5.7
    volumes:
      - mysql-data:/var/lib/mysql
volumes:
  mysql-data: 
```

!SLIDE
## Starting
----
```sh
> docker-compose -p chords up -d
Creating chords_mysql ... 
Creating chords_influxdb ... 
Creating chords_mysql
Creating chords_influxdb ... done
Creating chords_grafana ... 
Creating chords_app ... 
Creating chords_grafana
Creating chords_app ... done
Creating chords_kapacitor ... 
Creating chords_grafana ... done
```
```sh
> docker ps
CONTAINER ID  IMAGE                        COMMAND                  CREATED             STATUS              PORTS                                            NAMES
8e63c13d7893  ncareol/kapacitor            "/entrypoint.sh /bin.."   38 seconds ago      Up About a minute   0.0.0.0:9092->9092/tcp                           chords_kapacitor
36a025ef989c  ncareol/chords:development   "bundle exec bash -c.."   38 seconds ago      Up About a minute   0.0.0.0:80->80/tcp                               chords_app
8233efd1b575  ncareol/grafana              "/run.sh"                 38 seconds ago      Up About a minute   0.0.0.0:3000->3000/tcp                           chords_grafana
c087ca5c9aaa  influxdb:1.2                 "/entrypoint.sh infl.."   39 seconds ago      Up About a minute   0.0.0.0:8083->8083/tcp, 0.0.0.0:8086->8086/tcp   chords_influxdb
b3a0fd15cc40  mysql:5.7                    "docker-entrypoint.s.."   40 seconds ago      Up About a minute   3306/tcp                                         chords_mysql
```

!SLIDE
## Stopping
----

```sh
> docker-compose -p chords down
Stopping chords_kapacitor ... done
Stopping chords_app       ... done
Stopping chords_grafana   ... done
Stopping chords_influxdb  ... done
Stopping chords_mysql     ... done
Removing chords_kapacitor ... done
Removing chords_app       ... done
Removing chords_grafana   ... done
Removing chords_influxdb  ... done
Removing chords_mysql     ... done
Removing network chords_default
```
!SLIDE
## Volume Persistance
----
```
> docker volume ls
DRIVER              VOLUME NAME
local               chords_grafana-data
local               chords_grafana-log
local               chords_influxdb-data
local               chords_influxdb-log
local               chords_kapacitor-data
local               chords_kapacitor-log
local               chords_mysql-data
local               chords_mysql-log
local               chords_nginx-log
local               chords_rails-log
local               chords_shared-tmp
local               chordsportal_grafana-data
local               chordsportal_grafana-log
local               chordsportal_influxdb-data
local               chordsportal_influxdb-log
local               chordsportal_kapacitor-data
local               chordsportal_kapacitor-log
local               chordsportal_mysql-data
local               chordsportal_mysql-log
local               chordsportal_nginx-log
local               chordsportal_rails-log
local               chordsportal_shared-tmp
```
!SLIDE
## Conclusions
|Using|Provides|
|-|-|
|*docker-compose.yml*|Interacting services, defined via a rational syntax|
|`docker-compose`|Start/stop/restart the system|
|*volumes*|Persistance and naming|
|*environment*|System customization|

On MaoOS/Linux/Windows. Cloud/local/Raspberry Pi.

```sh
docker-compose pull
docker-compose up -d
```
