# Based on Apollo11.0


To make it compatible with Apollo 11.0, I have modified the original repository: https://github.com/guardstrikelab/carla_apollo_bridge.
Additionally, the environment configuration has also been changed.

You can configure it by following the steps below:

Prerequisite: A running Apollo Docker container must already be deployed

clone the repository in the path apollo/modules outside the container


1.Environment Variable Configuration

Enter the Apollo container and modify /home/$USER/.bashrc.


Append the following to the end of the file:

```
export PYTHONPATH=$PYTHONPATH:/apollo/modules/carla_apollo_bridge/carla_bridge/carla_api/carla-0.9.15-py3.7-linux-x86_64.egg
export PYTHONPATH=$PYTHONPATH:/apollo/bazel-bin
export PYTHONPATH=$PYTHONPATH:/apollo/modules/carla_apollo_bridge
export PYTHONPATH=$PYTHONPATH:/apollo/cyber/python
```


2.Install the dependencies required for carla_bridge

cd carla_apollo_bridge/carla_bridge


cp -r map/. /apollo/modules/map/data


pip3 install -r requirements.txt


3.Download CARLA_0.9.15.tar.gz
https://github.com/carla-simulator/carla/releases


Running Instructions:

1.Inside the container, compile data modules

Modify channel name where use the "/apollo/canbus/chassis" to "/apollo/canbus/carla/chassis"
```
part 1
modules/control/control_component/control_component.cc
chassis_reader_config.channel_name = "/apollo/canbus/carla/chassis";

part 2
modules/external_command/command_processor/action_command_processor/conf/config.pb.txt
command_status_name: "/apollo/canbus/carla/chassis"

part 3
modules/planning/planning_component/conf/planning_config.pb.txt
chassis_topic: "/apollo/canbus/carla/chassis"
```

Start the three Apollo modules: planning, control and dreamview_plus.

2.Outside the container, launch CarlaUE4 with the specified port:

./CarlaUE4.sh -carla-port=2000

3.Inside the container, start the carla_bridge:

python carla_apollo_bridge/carla_bridge/main.py



![carla_apollo10 0_bridge](https://github.com/user-attachments/assets/1a922718-8bb7-4ddc-8738-7867506493ba)



