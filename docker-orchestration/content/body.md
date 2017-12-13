## Containers as Services
----
<img src='images/chords-services.png' height='500' />

!SLIDE
## DockerHub (Free as in beer)
----
A public repository of prebuilt images

<img src='images/dockerhub-repos.png' height='150' />

And your own

<img src='images/dockerhub-ncareol.png' height='150' />
!SLIDE
## Container Configuration
----
- Common usage: configure with environment variables.
- Occasionally create a derived image with minor startup/configuration functions.
!SLIDE
## Docker Volumes
----
- Similar to containers, but just used for strage.
- Exist in a Docker namespace; easily mounted by containers.
- Persistant across reboots.
- Avoids O/S filesystem naming requirements.
- Can be accessed simply by running a lightweight container.
!SLIDE
## CHORDS
----
Show CHORDS: Jump to portal.chordsrt.com.
!SLIDE
## Orchestration
----
```sh
docker-compose up -d
```

docker-compose.yml
```yml
version: '2'
services:
  # CHORDS Rails application: nginx + rails + CHORDS rails code 
  app:
    container_name: chords_app
    image: ncareol/chords:${DOCKER_TAG}
  # Rails mysql database
  mysql:
    container_name: chords_mysql
    image: mysql:5.7
  # InfluxData influxdb time-series database
  influxdb:
    container_name: chords_influxdb
    image: influxdb:1.2
  # InfluxData kapacitor time series monitoring
  kapacitor:
    container_name: chords_kapacitor
    image: ncareol/kapacitor
  # Grafana graphing server. It draws data from influxdb.
  grafana:
    container_name: chords_grafana
    image: ncareol/grafana
volumes:
  # CHORDS Rails meta-data
  mysql-data: 
  # CHORDS time-series data
  influxdb-data:
  # kapacitor data
  kapacitor-data:
  # Grafana configuration data
  grafana-data:
  #shared_tmp volume
  shared-tmp:
  # log directories
  rails-log:
  nginx-log:
  mysql-log:
  influxdb-log:
  kapacitor-log:
  grafana-log:
  ```

