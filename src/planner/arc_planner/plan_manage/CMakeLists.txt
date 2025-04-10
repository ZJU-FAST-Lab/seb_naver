cmake_minimum_required(VERSION 2.8.3)
project(plan_manage)

set(CMAKE_VERBOSE_MAKEFILE "true")
set(CMAKE_BUILD_TYPE "Release")
set(CMAKE_CXX_FLAGS "-std=c++14 -g ${TORCH_CXX_FLAGS}")
set(CMAKE_CXX_FLAGS_RELEASE "-O3 -w -fPIC")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -O3 -w" )
find_package(Eigen3 REQUIRED)

find_package(PCL REQUIRED)
find_package(catkin REQUIRED COMPONENTS
    roscpp
    visualization_msgs
    std_msgs
    tools
    path_search
    message_generation
    cv_bridge
    decomp_ros_utils
    mpc_controller
    se2_grid_core
    se2_grid_ros
    se2_grid_msgs
    sensor_msgs
    geometry_msgs

    # roscpp
    # visualization_msgs
    # std_msgs
    # sensor_msgs
    # geometry_msgs
    # message_generation
    # cv_bridge
    # decomp_ros_utils
    # swarm_bridge
    # se2_grid_core
    # se2_grid_ros
    # se2_grid_msgs
)

include_directories(
    include
    ${catkin_INCLUDE_DIRS}
    ${EIGEN3_INCLUDE_DIRS}
    ${PCL_INCLUDE_DIRS}
)


#if this catkin packge's header is used by other packages, use catkin_package to
#declare the include directories of this package.
catkin_package(
    INCLUDE_DIRS include 
)
#singua:plan_manage  plan_manage_benchmark
add_library(plan_manage
    src/plan_manage.cpp
)
target_link_libraries(plan_manage 
    kino_astar
    astar
    ${catkin_LIBRARIES}
)
add_executable(plan_node
    src/plan_node.cpp
    src/replan_fsm.cpp
)
target_link_libraries(plan_node 
    plan_manage
    ${catkin_LIBRARIES}
    ${PCL_LIBRARIES}
)