# ROS2 Foxy Installation Guide on Ubuntu 20.04

## System Requirements

Foxy Fitzroy is supported on Ubuntu Linux Focal Fossa (20.04) 64-bit x86 and 64-bit ARM.

## Add The ROS 2 Apt Repository

You will need to add the ROS 2 apt repositories to your system. To do so, first authorize our GPG key with apt like this:

```sh
sudo apt update && sudo apt install curl gnupg2 lsb-release
sudo curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key  -o /usr/share/keyrings/ros-archive-keyring.gpg
```

And then add the repository to your sources list:

```sh
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] http://packages.ros.org/ros2/ubuntu $(source /etc/os-release && echo $UBUNTU_CODENAME) main" | sudo tee /etc/apt/sources.list.d/ros2.list > /dev/null
```

## Downloading ROS 2

Go the releases page (https://github.com/ros2/ros2/releases)

Download the latest package for Ubuntu; let’s assume that it ends up at `~/Downloads/ros2-foxy-20220208-linux-focal-amd64.tar.bz2`

Note: there may be more than one binary download option which might cause the file name to differ.

Unpack it:

```sh
mkdir -p ~/ros2_foxy
cd ~/ros2_foxy
tar xf ~/Downloads/ros2-foxy-20220208-linux-focal-amd64.tar.bz2
```

## Installing And Initializing ROS Dependencies

```sh
sudo apt update
sudo apt install -y python3-rosdep
sudo rosdep init
rosdep update
```

## Installing The Missing Dependencies

Set your rosdistro according to the release you downloaded.

```sh
rosdep install --from-paths ~/ros2_foxy/ros2-linux/share --ignore-src -y --skip-keys "cyclonedds fastcdr fastrtps rti-connext-dds-5.3.1 urdfdom_headers"
```

Note: If you’re using a distribution that is based on Ubuntu (like Linux Mint) but does not identify itself as such, you’ll get an error message like Unsupported OS [mint]. In this case append --os=ubuntu:focal to the above command.
Optional:
If you want to use the ROS 1<->2 bridge, then you must also install ROS 1.
Follow the normal install instructions: (http://wiki.ros.org/noetic/Installation/Ubuntu)

The script will fail to run `sudo -H apt-get install -y libopencv-dev`  

Run this for find the specific dependency error.

```
sudo -H apt-get install -y libopencv-dev
```

There will be an error message mentioning a dependency issue with libopencv-dev. Install that dependency with the following command:

```sh
sudo aptitude install libopencv-dev
```

2 options will appear. If both suggest not installing, reply NO by entering `n`.

Then 1 option will appear. Reply YES by entering `y`.
Reply YES to the next step also.

Now, this will run smoothly.

```
sudo -H apt-get install -y libopencv-dev
```

## Installing The Python3 Libraries

```sh
sudo apt install -y libpython3-dev python3-pip
pip3 install -U argcomplete
```

## Install Additional DDS Implementations (Optional)

If you would like to use another DDS or RTPS vendor besides the default, eProsima’s Fast RTPS, you can find instructions here.

## Environment Setup

## Sourcing The Setup Script

Set up your environment by sourcing the following file.

```sh
. ~/ros2_foxy/ros2-linux/setup.bash
```

## Resolve The DDS Middleware Error

```sh
sudo apt-get install ros-foxy-rmw-connext.cpp
. ~/ros2_foxy/ros2-linux/setup.bash
```

## Add The Environment Variable To The Bashrc File

```sh
nano ~/.bashrc
```

Add the following line at the bottom of the file
```sh
source ~/ros2_foxy/ros2-linux/setup.bash
```

## Exit


## Try Some Examples

In one terminal, source the setup file and then run a C++ talker:

```sh
. ~/ros2_foxy/ros2-linux/setup.bash
ros2 run demo_nodes_cpp talker
```

In another terminal source the setup file and then run a Python listener:

```sh
. ~/ros2_foxy/ros2-linux/setup.bash
ros2 run demo_nodes_py listener
```

You should see the talker saying that it’s Publishing messages and the listener saying I heard those messages. This verifies both the C++ and Python APIs are working properly. Hooray!

### Last Update: 2nd May, 2022

### Reference: https://docs.ros.org/en/foxy/Installation/Ubuntu-Install-Debians.html

