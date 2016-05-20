# swatbotics/apriltags-cpp/CMakeLists.txt

cmake_minimum_required(VERSION 2.6)

project(apriltags_swatbotics)

find_package(catkin REQUIRED COMPONENTS 
  cmake_modules  
  )

catkin_package(
  DEPENDS OpenCV
  INCLUDE_DIRS}
  LIBRARIES ${PROJECT_NAME}
)

set(CMAKE_C_FLAGS "-Wall -g")
set(CMAKE_CXX_FLAGS "-Wall -g")

if (APPLE OR UNIX)
  set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -Wsign-compare -frounding-math")
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wsign-compare -frounding-math")
endif()

set(CMAKE_C_FLAGS_DEBUG "-O")
set(CMAKE_CXX_FLAGS_DEBUG "-O")

set(CMAKE_C_FLAGS_RELEASE "-O2")
set(CMAKE_CXX_FLAGS_RELEASE "-O2")


set(OPENGL_LIBRARY ${OPENGL_LIBRARY} ${GLU_LIBRARY})
include(FindPkgConfig)

pkg_check_modules(CAIRO cairo)

if (${CAIRO_FOUND})
  add_definitions(-DMZ_HAVE_CAIRO)
endif ()

find_package( CGAL QUIET COMPONENTS  )
if (CGAL_FOUND)
  include( ${CGAL_USE_FILE} )
  add_definitions(-DHAVE_CGAL)
  find_package( Boost REQUIRED )
else()
  message("CGAL not found, can not use new quad detector")
endif()

find_library(OPENGL_LIBRARY GL)
find_library(GLU_LIBRARY GLU)
find_library(GLUT_LIBRARY glut)

find_package(OpenCV REQUIRED)
include_directories(
  ${OpenCV_INCLUDE_DIRS}
)

add_library(${PROJECT_NAME} SHARED
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
)
target_link_libraries(${PROJECT_NAME} ${OpenCV_LIBRARIES})

