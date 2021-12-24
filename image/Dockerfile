FROM ros:melodic-ros-core-bionic
#Now, works on Melodic only

LABEL maintainer="Waipot Ngamsaad <waipotn@hotmail.com>"

SHELL ["/bin/bash", "-c"]

ENV DEBIAN_FRONTEND noninteractive

RUN  apt-key adv --keyserver 'hkp://keyserver.ubuntu.com:80' --recv-key C1CF6E31E6BADE8868B172B4F42ED6FBAB17C654

RUN sed -i -e 's/http:\/\/archive/mirror:\/\/mirrors/' -e 's/http:\/\/security/mirror:\/\/mirrors/' -e 's/\/ubuntu\//\/mirrors.txt/' /etc/apt/sources.list

RUN apt-get update && apt-get upgrade -y 
RUN apt-get install -y \ 
	ros-${ROS_DISTRO}-tf2 \
    ros-${ROS_DISTRO}-tf2-ros \
	ros-${ROS_DISTRO}-tf \
	build-essential \
    git \
	python-serial \
	python-rosdep \
	&& apt-get autoremove -y \
	&& apt-get clean \
	&& rm -rf /var/lib/apt/lists/*

RUN rosdep init && rosdep update

RUN mkdir -p /catkin_ws/src && \
    cd /catkin_ws/src && \
	git clone https://github.com/LFZ1994/sensor_imu.git && \
	cd /catkin_ws && \
	source /opt/ros/$ROS_DISTRO/setup.bash && \
	catkin_make && \
	#catkin_make install -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=/opt/ros/$ROS_DISTRO -DCATKIN_ENABLE_TESTING=0 && \
	#rm -rf /catkin_ws && \
	echo "DONE!"

RUN apt-get purge -y \
	git \
	&& apt-get autoremove -y \
	&& apt-get clean \
	&& rm -rf /var/lib/apt/lists/*

COPY ./sensor_imu.launch /

#CMD ["roslaunch", "--wait", "/sensor_imu.launch"]

#CMD source /catkin_ws/devel/setup.bash && roslaunch sensor_imu sensor_imu.launch

CMD source /catkin_ws/devel/setup.bash && roslaunch /sensor_imu.launch
