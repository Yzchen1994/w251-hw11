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

## Questions
### What parameters did you change? 
Original
```
        self.density_first_layer = 16
        self.density_second_layer = 8
        self.num_epochs = 1
        self.batch_size = 64
        self.epsilon_min = 0.01

        # epsilon will randomly choose the next action as either
        # a random action, or the highest scoring predicted action
        self.epsilon = 1.0
        self.epsilon_decay = 0.995
        self.gamma = 0.99

        # Learning rate
        self.lr = 0.001
```
Result:  Average Reward:  -153.01253982849627 epsilon:  0.49571413690105054

Attempt 1
```
        self.density_first_layer = 64
        self.density_second_layer = 32
        self.num_epochs = 1
        self.batch_size = 64
        self.epsilon_min = 0.01

        # epsilon will randomly choose the next action as either
        # a random action, or the highest scoring predicted action
        self.epsilon = 1.0
        self.epsilon_decay = 0.995
        self.gamma = 0.99

        # Learning rate
        self.lr = 0.001
```
Result: Average Reward:  -170.45798842507023 epsilon:  0.47147873742168567

Attempt 2
```
        self.density_first_layer = 64
        self.density_second_layer = 32
        self.num_epochs = 1
        self.batch_size = 128
        self.epsilon_min = 0.01

        # epsilon will randomly choose the next action as either
        # a random action, or the highest scoring predicted action
        self.epsilon = 1.0
        self.epsilon_decay = 0.995
        self.gamma = 0.99

        # Learning rate
        self.lr = 0.001
```
Result

### What values did you try?
### Did you try any other changes that made things better or worse?
Attempt 1 make it worse. 

### Did they improve or degrade the model? Did you have a test run with 100% of the scores above 200?
### Based on what you observed, what conclusions can you draw about the different parameters and their values? 
### What is the purpose of the epsilon value?
It's the % of time that model tries random value. At the beginning, we want epsilon to be high so that you take big leaps and learn things.

### Describe "Q-Learning".
Q learning is kind of model-free reinforcement learning algorithm that tells an agent what action to take under certain circumstances. It's a model-free algorithm.


