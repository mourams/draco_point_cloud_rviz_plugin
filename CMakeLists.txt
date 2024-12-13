cmake_minimum_required(VERSION 2.8.3)
project(draco_point_cloud_rviz_plugin)

list(APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_LIST_DIR}/cmake")

find_package(catkin REQUIRED 
  COMPONENTS
  roscpp
  rviz
  sensor_msgs
  roslib
  draco_point_cloud_transport
)

find_package(PCL REQUIRED)
find_package(Draco REQUIRED)

catkin_package(
  INCLUDE_DIRS include
  LIBRARIES ${PROJECT_NAME}
  CATKIN_DEPENDS rviz draco_point_cloud_transport sensor_msgs
)

set(CMAKE_AUTOMOC ON)
find_package(Qt5 REQUIRED COMPONENTS Core Widgets OpenGL)
add_definitions( ${QT_DEFINITIONS} )

###########
## Build ##
###########

include_directories(
  include
  ${CMAKE_BINARY_DIR}
  ${catkin_INCLUDE_DIRS}
  ${PCL_INCLUDE_DIRS}
  ${draco_INCLUDE_DIR}
)

set(HEADER_FILES
    include/${PROJECT_NAME}/compressed_cloud_display.h
    include/${PROJECT_NAME}/DracotoPC2.h
    include/${PROJECT_NAME}/point_cloud_common.h
    include/${PROJECT_NAME}/point_cloud.h
    include/${PROJECT_NAME}/conversion_utilities.h
)

set(SRC_FILES
    src/compressed_cloud_display.cpp
    src/DracotoPC2.cpp
    src/point_cloud_common.cpp
    src/point_cloud.cpp
    src/conversion_utilities.cpp
)

add_library(${PROJECT_NAME} ${SRC_FILES} ${HEADER_FILES})

add_dependencies(${PROJECT_NAME} ${${PROJECT_NAME}_EXPORTED_TARGETS} ${catkin_EXPORTED_TARGETS})

target_link_libraries(${PROJECT_NAME}
  ${catkin_LIBRARIES}
  ${PCL_LIBRARIES}
  ${draco_LIBRARIES}
)

#############
## Install ##
#############

install(TARGETS ${PROJECT_NAME}
        ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
        LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
        RUNTIME DESTINATION ${CATKIN_GLOBAL_BIN_DESTINATION}
)

install(
  DIRECTORY include/${PROJECT_NAME}/
  DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
)
