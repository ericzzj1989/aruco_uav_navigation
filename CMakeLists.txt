cmake_minimum_required(VERSION 2.8.3)
project(aruco_navigation)

add_definitions(-std=c++11)

find_package(catkin REQUIRED COMPONENTS
  std_msgs
  roscpp
  geometry_msgs
  aruco_eye_msgs
  message_filters
  message_generation
)

# find_package(Boost REQUIRED COMPONENTS system)

find_package(OpenCV REQUIRED)
find_package(cmake_modules REQUIRED)
# catkin_python_setup()

add_message_files(
  FILES
  Navdata_aruco.msg
)

generate_messages(
  DEPENDENCIES
  std_msgs
  geometry_msgs
)

catkin_package(
   INCLUDE_DIRS include
   CATKIN_DEPENDS roscpp geometry_msgs message_runtime std_msgs aruco_eye_msgs
   DEPENDS Eigen OpenCV
#  DEPENDS system_lib
)


include_directories(
  include
  #${Eigen_INCLUDE_DIRS}
  ${catkin_INCLUDE_DIRS}
  ${OpenCV_INCLUDE_DIRS}
)


add_executable(get_nav_data_node src/get_pos_uav_node.cpp src/localization.cpp )

add_dependencies(get_nav_data_node ${${PROJECT_NAME}_EXPORTED_TARGETS} ${catkin_EXPORTED_TARGETS})

# Specify libraries to link a library or executable target against

target_link_libraries(get_nav_data_node ${catkin_LIBRARIES})

