# w251-hw11

## Setup
Monitor Jetson stats
```
# install the jetson-stats package
sudo -H pip3 install -U jetson-stats

# reboot to load the libraries
reboot

# run as root
sudo jtop
```

## Build the dockerfile
```
# If you haven't added your User to the docker group, do it now
sudo usermod -aG docker $USER

# reboot to make the previous step take effect
sudo docker build -t hw11 -f Dockerfile.agent .

# enable video sharing from the container
xhost +

# maximize performance on the device
sudo jetson_clocks --store
sudo jetson_clocks
time docker run -it --rm --net=host --runtime nvidia  -e DISPLAY=$DISPLAY -v /tmp/.X11-unix/:/tmp/.X11-unix:rw --privileged -v /data/videos:/tmp/videos hw11

# don't forget to turn off the fan on the device
sudo jetson_clocks --restore
```


