cmake_minimum_required(VERSION 2.4.6)
project(orb_slam)

## Find catkin macros and libraries
## if COMPONENTS list like find_package(catkin REQUIRED COMPONENTS xyz)
## is used, also find other catkin packages
find_package(catkin REQUIRED COMPONENTS
  roscpp
  roslib
  tf
  sensor_msgs
  image_transport
  cv_bridge
)

SET(CMAKE_BUILD_TYPE Release)

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS}  -Wall  -O3 -march=native ")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall  -O3 -march=native")

set(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/bin)

LIST(APPEND CMAKE_MODULE_PATH ${CMAKE_CURRENT_SOURCE_DIR}/cmake_modules)

find_package(OpenCV REQUIRED)
find_package(Eigen3 REQUIRED)
find_package(Cholmod REQUIRED)

###################################
## catkin specific configuration ##
###################################
## The catkin_package macro generates cmake config files for your package
## Declare things to be passed to dependent projects
## INCLUDE_DIRS: uncomment this if you package contains header files
## LIBRARIES: libraries you create in this project that dependent projects also need
## CATKIN_DEPENDS: catkin_packages dependent projects also need
## DEPENDS: system dependencies of this project that dependent projects also need
catkin_package()

include_directories(
include
${PROJECT_SOURCE_DIR}
${EIGEN3_INCLUDE_DIR}
${CHOLMOD_INCLUDE_DIR}
${catkin_INCLUDE_DIRS}
)

add_executable(${PROJECT_NAME}
src/main.cc
src/Tracking.cc
src/LocalMapping.cc
src/LoopClosing.cc 
src/ORBextractor.cc
src/ORBmatcher.cc
src/FramePublisher.cc
src/Converter.cc
src/MapPoint.cc
src/KeyFrame.cc
src/Map.cc
src/MapPublisher.cc
src/Optimizer.cc
src/PnPsolver.cc
src/Frame.cc
src/KeyFrameDatabase.cc
src/Sim3Solver.cc
src/Initializer.cc
)

find_package(Boost REQUIRED COMPONENTS system thread)
include_directories(${Boost_INCLUDE_DIRS})
target_link_libraries(${PROJECT_NAME} ${Boost_LIBRARIES} ${catkin_LIBRARIES})

target_link_libraries(${PROJECT_NAME}
${OpenCV_LIBS}
${EIGEN3_LIBS}
cholmod
${PROJECT_SOURCE_DIR}/Thirdparty/DBoW2/lib/libDBoW2.so
${PROJECT_SOURCE_DIR}/Thirdparty/g2o/lib/libg2o_core.so
${PROJECT_SOURCE_DIR}/Thirdparty/g2o/lib/libg2o_solver_cholmod.so
${PROJECT_SOURCE_DIR}/Thirdparty/g2o/lib/libg2o_solver_dense.so
${PROJECT_SOURCE_DIR}/Thirdparty/g2o/lib/libg2o_stuff.so
${PROJECT_SOURCE_DIR}/Thirdparty/g2o/lib/libg2o_types_sba.so
${PROJECT_SOURCE_DIR}/Thirdparty/g2o/lib/libg2o_types_sim3.so
${PROJECT_SOURCE_DIR}/Thirdparty/g2o/lib/libg2o_types_slam3d.so
)

