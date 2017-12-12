### DockerHub
- A repository of prebuilt images.
- Common usage: configure with environment variables.
- Occaisonally create a derived image with minor startup/configuration functions.

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
