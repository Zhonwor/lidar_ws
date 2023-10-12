cmake_minimum_required(VERSION 3.0.2)
project(fusion_pointclouds)

add_compile_options(-std=c++14)

find_package(catkin REQUIRED COMPONENTS
  dynamic_reconfigure
  pcl_ros
  roscpp
  rospy
  sensor_msgs
  std_msgs
  pcl_conversions
)

find_package(PCL  REQUIRED)

# set(PCL_INCLUDE_DIRS /usr/include/pcl-1.10)



generate_dynamic_reconfigure_options( 
    config/set_params_tf.cfg
    config/set_params_bounds.cfg
  )


catkin_package(

  CATKIN_DEPENDS dynamic_reconfigure pcl_ros roscpp rospy sensor_msgs std_msgs
#  DEPENDS system_lib
)


include_directories(
  include
  ${catkin_INCLUDE_DIRS}
  ${PCL_INCLUDE_DIRS}
)
link_directories(${PCL_LIBRARY_DIRS})
add_definitions(${PCL_DEFINITIONS})

add_library(load_params

src/load_params.cpp
 ) 

add_executable(set_params_tf src/set_params_tf.cpp)
target_link_libraries( set_params_tf ${catkin_LIBRARIES} ${PCL_LIBRARIES})

add_executable(set_params_bounds src/set_params_bounds.cpp)
target_link_libraries( set_params_bounds ${catkin_LIBRARIES} ${PCL_LIBRARIES})

add_executable(fusion_pointclouds src/fusion_pointclouds.cpp)
add_dependencies(fusion_pointclouds ${${PROJECT_NAME}_EXPORTED_TARGETS} ${catkin_EXPORTED_TARGETS})
target_link_libraries(fusion_pointclouds  ${catkin_LIBRARIES} ${PCL_LIBRARIES} ${YAMLCPP_LIBRARIES} load_params)

