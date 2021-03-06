cmake_minimum_required(VERSION 2.8.3)
project(rotors_control)

add_definitions(-std=c++11)

find_package(catkin REQUIRED COMPONENTS
  geometry_msgs
  mav_msgs
  roscpp
  sensor_msgs
  cmake_modules
  glog_catkin
)

find_package(Eigen REQUIRED)

catkin_package(
  INCLUDE_DIRS include ${Eigen_INCLUDE_DIRS}
  LIBRARIES lee_position_controller roll_pitch_yawrate_thrust_controller
  CATKIN_DEPENDS geometry_msgs mav_msgs roscpp sensor_msgs
  DEPENDS Eigen glog_catkin
)

include_directories(
  include
  ${catkin_INCLUDE_DIRS}
  ${Eigen_INCLUDE_DIRS}
)

add_library(lee_position_controller
  src/library/lee_position_controller.cpp
)

add_library(roll_pitch_yawrate_thrust_controller
  src/library/roll_pitch_yawrate_thrust_controller.cpp
)

target_link_libraries(lee_position_controller ${catkin_LIBRARIES})
add_dependencies(lee_position_controller ${catkin_EXPORTED_TARGETS})

target_link_libraries(roll_pitch_yawrate_thrust_controller ${catkin_LIBRARIES})
add_dependencies(roll_pitch_yawrate_thrust_controller ${catkin_EXPORTED_TARGETS})

add_executable(lee_position_controller_node src/nodes/lee_position_controller_node.cpp)
add_dependencies(lee_position_controller_node ${catkin_EXPORTED_TARGETS})
target_link_libraries(lee_position_controller_node
  lee_position_controller ${catkin_LIBRARIES})

add_executable(roll_pitch_yawrate_thrust_controller_node
  src/nodes/roll_pitch_yawrate_thrust_controller_node.cpp)
add_dependencies(roll_pitch_yawrate_thrust_controller_node ${catkin_EXPORTED_TARGETS})
target_link_libraries(roll_pitch_yawrate_thrust_controller_node
  roll_pitch_yawrate_thrust_controller ${catkin_LIBRARIES})
