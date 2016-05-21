# swatbotics/apriltags-cpp/CMakeLists.txt

cmake_minimum_required(VERSION 2.6)

project(apriltags_swatbotics)

set(CMAKE_CXX_FLAGS "-Wall -g")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wsign-compare -frounding-math")
set(CMAKE_CXX_FLAGS_DEBUG "-O")
set(CMAKE_CXX_FLAGS_RELEASE "-O2")

find_package(catkin REQUIRED COMPONENTS 
  cmake_modules  
)

catkin_package(
  DEPENDS OpenCV
  INCLUDE_DIRS include
  LIBRARIES ${PROJECT_NAME}
)

find_package( CGAL QUIET COMPONENTS  )
if (CGAL_FOUND)
  include( ${CGAL_USE_FILE} )
  add_definitions(-DHAVE_CGAL)
  find_package( Boost REQUIRED )
else()
  message("CGAL not found, can not use new quad detector")
endif()


find_package(OpenCV REQUIRED)
include_directories(
  include/${PROJECT_NAME}
  ${OpenCV_INCLUDE_DIRS}
  ${catkin_INCLUDE_DIRS}
)

FILE(GLOB_RECURSE apriltags_swatbotics_headers "include/*.h")

add_library(${PROJECT_NAME} 
  src/CameraUtil.cpp
  src/DebugImage.cpp
  src/Geometry.cpp 
  src/GrayModel.cpp
  src/MathUtil.cpp 
  src/Refine.cpp
  src/TagDetector.cpp 
  src/TagFamily.cpp
  src/TagFamilies.cpp
  src/UnionFindSimple.cpp
  ${apriltags_swatbotics_headers}
)

target_link_libraries(${PROJECT_NAME} 
  ${OpenCV_LIBRARIES}
  ${catkin_LIBRARIES}
)

