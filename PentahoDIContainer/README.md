# PentahoDIContainer Example Container DIRECTIONS
 
 *in dir:   /dk/AnalyticContainers directory

 *build :                   ``` sudo docker build  -f PentahoDIContainer/Dockerfile -t ac_pentahodi_container . ```
 *(or forced re-build):     ``` sudo docker build  -f PentahoDIContainer/Dockerfile --no-cache -t ac_pentahodi_container . ```

 * create data container:    ``` sudo docker run -d -v /dk/AnalyticContainer/PentahoDIContainer/docker-share --name="data_ac_pentahodi_container" ac_pentahodi_container:latest ```
 * add config file:          ``` sudo docker cp  /dk/AnalyticContainer/PentahoDIContainer/docker-share/config.json data_ac_pentahodi_container:/dk/AnalyticContainer/PentahoDIContainer/docker-share/config.json ```
 * run:                      
 ``` 
 sudo docker run -e CONTAINER_INPUT_CONFIG_FILE_PATH=" /dk/AnalyticContainer/PentahoDIContainer/docker-share" -e CONTAINER_INPUT_CONFIG_FILE_NAME="config.json" -e CONTAINER_OUTPUT_PROGRESS_FILE="progress.json" -e CONTAINER_OUTPUT_LOG_FILE="ac_logger.log" -e INSIDE_CONTAINER_FILE_MOUNT="/dk/AnalyticContainer/PentahoDIContainer" -e INSIDE_CONTAINER_FILE_DIRECTORY="/dk/AnalyticContainer/PentahoDIContainer/docker-share" --volumes-from data_ac_pentahodi_container ac_pentahodi_container:latest
 ```
 
 * copy data back:           ``` sudo docker cp data_ac_pentahodi_container:/dk/AnalyticContainer/PentahoDIContainer/docker-share/progress.json /dk/AnalyticContainer/PentahoDIContainer/docker-share/progress.json ```
                             ``` sudo docker cp data_ac_pentahodi_container:/dk/AnalyticContainer/PentahoDIContainer/docker-share/ac_logger.log /dk/AnalyticContainer/PentahoDIContainer/docker-share/ac_logger.log ```

 * see in data container:    ``` sudo ls -al `docker inspect --format='{{(index .Mounts 0).Source}}' data_ac_pentahodi_container`  ```

## cleanup containers
```
         docker rm $(docker ps -a -q)
         docker rm $(docker ps -a | grep ac_pentahodi_container) 
         docker rm $(docker ps -a | grep data_ac_pentahodi_container) 
```
## cleanup images (you may have to cleanup contianers first)
```         
         docker rmi $(docker images | grep ac_pentahodi_container) 
```
## cleanup stranded volumes:  https://github.com/chadoe/docker-cleanup-volumes
```
         docker run -v /var/run/docker.sock:/var/run/docker.sock -v /var/lib/docker:/var/lib/docker --rm martin/docker-cleanup-volumes
         look in 'sudo ls -al /var/lib/docker/volumes/'
```
## other commands (FYI)
 * run in bash:     ``` sudo docker run -e AC_FILE_MOUNT="/mnt/docker-share" -it ac_pentahodi_container:latest /bin/bash ```
 
## push to docker hub
 1. login            ``` sudo docker login ```
 2. list images      ``` docker images ```
 3. tag              ``` docker tag <image ID> datakitchen/ac_pentahodi_container:latest ```
 4. push             ``` docker push datakitchen/ac_pentahodi_container:latest ```
 

