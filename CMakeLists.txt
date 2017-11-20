cmake_minimum_required(VERSION 2.8.3)
project(sbpl_utils)

# Set the build type.  Options are:
#  Coverage       : w/ debug symbols, w/o optimization, w/ code-coverage
#  Debug          : w/ debug symbols, w/o optimization
#  Release        : w/o debug symbols, w/ optimization
#  RelWithDebInfo : w/ debug symbols, w/ optimization
#  MinSizeRel     : w/o debug symbols, w/ optimization, stripped binaries
set(ROS_BUILD_TYPE Release)
set(CMAKE_CXX_FLAGS "-std=c++11")

find_package(Boost REQUIRED COMPONENTS graph)
find_package(OpenCV REQUIRED)
find_package(catkin REQUIRED)
find_package(sbpl REQUIRED)

catkin_package(
    DEPENDS OpenCV
    CATKIN_DEPENDS
    INCLUDE_DIRS include ${SBPL_INCLUDE_DIRS}
    LIBRARIES ${PROJECT_NAME} ${SBPL_LIBRARIES}
)

include_directories(
  ${Boost_INCLUDE_DIRS}
  ${OpenCV_INCLUDE_DIRS}
  ${SBPL_INCLUDE_DIRS}
  include)

# common commands for building c++ executables and libraries
add_library(
  ${PROJECT_NAME}
  src/hash_manager/hash_manager.cpp
  src/environments/boost_graph_environment.cpp
  src/visualization/grid_visualizer.cpp)
target_link_libraries(${PROJECT_NAME} ${OpenCV_LIBRARIES} ${SBPL_LIBRARIES})

catkin_add_gtest(hash_manager_test tests/hash_manager_test.cpp examples/hashable_states.cpp)
target_link_libraries(hash_manager_test ${PROJECT_NAME})

# catkin_add_gtest(boost_environment_test tests/boost_environment_test.cpp)
# target_link_libraries(boost_environment_test ${PROJECT_NAME})

add_executable(boost_environment_test tests/boost_environment_test.cpp)
target_link_libraries(boost_environment_test ${PROJECT_NAME})
