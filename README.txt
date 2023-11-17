Use this link to access the Google Drive to download the packages: 
https://drive.google.com/drive/folders/15CczYhMxquQuJYYAJIEPyaXP0t6Tzw13?usp=sharing

# The Folder /Docker should contain  
1- Dockerfile  
2- ros_entrypoint.sh
3- Readme.txt 

# Build the Docker from the folder /Docker using:

sudo docker build -t ros-noetic . 

## Debuging 
    Error1:
        IF faced with : 
        	ERROR: failed to solve: failed to fetch anonymous token: unexpected status: 401 Unauthorized 
        From: 
        	ERROR [internal] load metadata for nvcr.io/nvidia/pytorch:20.12-py3 
        Try:  
        	sudo docker pull nvcr.io/nvidia/pytorch:20.12-py3
        Then Build: 
        	sudo docker build -t ros-noetic . 
        	
#  running the docker image 
	sudo docker run -it --rm --net=host --runtime nvidia -e DISPLAY=$DISPLAY -v /tmp/.X11-unix/:/tmp/X11-unix -v ~/{PATH-TO_SUBMISSION}/docker_submission/docker_ros_ws:/home/ros_ws ros-noetic /bin/bash 
	(example, go to the Dockerfile directory and run:
	sudo docker run -it --rm --net=host --runtime nvidia -e DISPLAY=$DISPLAY -v /tmp/.X11-unix/:/tmp/X11-unix -v ./docker_ros_ws:/home/ros_ws ros-noetic /bin/bash  )
	
# Building the workspace : 
	cd ros_ws/
	catkin_make -DPYTHON_EXECUTABLE=/usr/bin/python3 -DPYTHON_INCLUDE_DIR=/usr/include/python3.7m
	source devel/setup.bash
	
# run the code: 
	'''you can follow the Readme.txt inside the package tii_detector_final/src for details about the running'''
	Run the Python code :  (dont required to build the catkin workspace) 
		navigate inside the tii_detector_final_package {package tii_detector_final}/src/ 
		python YoloV8_python.py (add -h for help or check the Readme.txt inside the tii_detector_final/src)
		
	Run the Ros Node:  (requires the build of catkin workspace) 
		Use byobu (already installed) to open multiple terminals: byobu ( F2: new terminal and F3/F4: backword/forward) 
		Start roscore : roscore 
		(note to do the next step you need to add a ROSBAG to the image , this can be done by coping the bag inside the 
		/docker_submission/docker_ros_ws then run the image. inside the docker, the bag should be found in /home/ros_ws)  
		Start rosbag : rosbag play <ROSBAG_NAME>
		Start Node:rosrun tii_detector_final YoloV8_node.py (add -h for help or check the Readme.txt inside the tii_detector_final/src)
		
	Output: 
		Both methods  output a json file inside tii_detector_final/src/output/output.json
		both method have the option to publish the result as image
			Node: publishes a topic 
			python: display a windows using openCV 
		
