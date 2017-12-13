<HEAD>
  <META HTTP-EQUIV="Pragma" CONTENT="no-cache">
</HEAD>
  
## DockerHub
- A public repository of prebuilt images
<img src='images/dockerhub-repos.png' height='200' />
- Official images
> Docker, Inc. sponsors a dedicated team that is responsible for reviewing and publishing all Official Repositories content


!SLIDE
### Container Configuration
- Common usage: configure with environment variables.
- Occasionally create a derived image with minor startup/configuration functions.

!SLIDE
### Docker Volumes
- Similar to containers, but just used for holding information.
- Exist in a Docker namespace, so easily referenced by containers.
- Persist nicely accross reboots.
- Avoids O/S filesystem naming requirements.
- Can be easily accessed simply by running a lightweight image and mounting. Then access with "docker cp", etc.

!SLIDE
### CHORDS
Show CHORDS: Jump to portal.chordsrt.com.

!SLIDE
### CHORDS Structure
Sketch of the containers/images.

!SLIDE
### Orchestration
- docker-compose utility
- guided by a recipe: docker-compoe.yml.
