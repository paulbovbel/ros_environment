cmake_minimum_required(VERSION 2.8.3)
project(ros_environment)
find_package(catkin REQUIRED)

catkin_package()

set(ROS_VERSION "1")
set(ROS_DISTRO "melodic")

# allow overriding the Python version
if(DEFINED ENV{ROS_PYTHON_VERSION})
  set(ROS_PYTHON_VERSION "$ENV{ROS_PYTHON_VERSION}")
else()
  set(ROS_PYTHON_VERSION "2")
endif()

# allow overriding the distro name
if(NOT $ENV{ROS_DISTRO_OVERRIDE} STREQUAL "")
  set(ROS_DISTRO "$ENV{ROS_DISTRO_OVERRIDE}")
endif()

set(
  hooks
  "1.ros_distro"
  "1.ros_etc_dir"
  "1.ros_package_path"
  "1.ros_version"
  "1.ros_python_version"
)
if(CMAKE_HOST_UNIX)
  set(shell "sh")
else()
  set(shell "bat")
endif()
foreach(hook ${hooks})
  catkin_add_env_hooks("${hook}" SHELLS ${shell} DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/env-hooks)
endforeach()
