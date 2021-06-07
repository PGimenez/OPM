## OpenModelica Docker image
Link to image: https://hub.docker.com/repository/docker/pjgim/opmgui

This image is based off the  [v1.17.0-gui](https://hub.docker.com/r/openmodelica/openmodelica/tags?page=1&ordering=last_updated) default OpenModelica installation. It's built to work with the [Dynawo](https://dynawo.github.io/) library using Python3. Its contents are
 
 - OpenModelica 1.17 and its libraries
 - Dynawo library
 - OMPython, numpy, matplotlib and other packages
 - Jupyter 
 - tmux, vim and other utilities

## Running on Mac

In order to run the OMEdit GUI, XQuartz is needed on the host machine. First ``open -a XQuartz``, and make sure that "Allow connections from network clients" is checked in the Security preferences. Then, execute the following:
```bash
ip=$(ifconfig en0 | grep inet | awk '$1=="inet" {print $2}')
export DISPLAY=:0
/opt/X11/bin/xhost + $ip
docker run -it --platform linux/amd64 --user om -e USER='om' -v /Users/<hostusername>/openmodelica:/home/om/openmodelica/ -e DISPLAY=$ip:0 p
jgim/opm /bin/bash
````

The first three lines start an X11 server that will accept incoming connections from the Docker container; make sure ``en0`` is the correct network interface. The options in the ``docker run`` command mean the wollowing:

- `` --platform linux/amd64`` tells Docker to emulate amd64 platform so that the image can run on M1 macs. Some things, such as the OpenModelica compiler, will still not work.
- ``--user om``start logged in as user om
- ``-e USER='om'``is required so that OMPython launches the OpenModelica compiler with the correct user
- ``-v /Users/<hostusername>/openmodelica:/home/om/openmodelica/`` mounts a folder from the host in the home directory of the om user
- ``-e DISPLAY=$ip:0`` tells Docker which display to use

