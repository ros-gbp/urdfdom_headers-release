cmake_minimum_required( VERSION 2.8 FATAL_ERROR )
project (urdfdom_headers)

set(${PROJECT_NAME}_inc_dirs urdf_exception/include urdf_model/include urdf_model_state/include urdf_sensor/include urdf_world/include)
find_package(catkin REQUIRED)
catkin_package(
  INCLUDE_DIRS ${${PROJECT_NAME}_inc_dirs}
)
include_directories(${${PROJECT_NAME}_inc_dirs})

# split version in parts and pass to preprocessor
string(REPLACE "." ";" ${PROJECT_NAME}_VERSION_LIST "${${PROJECT_NAME}_VERSION}")
list(LENGTH ${PROJECT_NAME}_VERSION_LIST _count)
if(NOT _count EQUAL 3)
  message(FATAL_ERROR "${PROJECT_NAME} version '${${PROJECT_NAME}_VERSION}' does not match 'MAJOR.MINOR.PATCH' pattern")
endif()
list(GET ${PROJECT_NAME}_VERSION_LIST 0 URDF_MAJOR_VERSION)
list(GET ${PROJECT_NAME}_VERSION_LIST 1 URDF_MINOR_VERSION)
list(GET ${PROJECT_NAME}_VERSION_LIST 2 URDF_PATCH_VERSION)

#set (URDF_MAJOR_VERSION 0)
#set (URDF_MINOR_VERSION 2)
#set (URDF_PATCH_VERSION 0)

set (URDF_VERSION ${URDF_MAJOR_VERSION}.${URDF_MINOR_VERSION}.${URDF_PATCH_VERSION})

message (STATUS "${PROJECT_NAME} version ${URDF_VERSION}")

# This shouldn't be necessary, but there has been trouble
# with MSVC being set off, but MSVCXX ON.
if(MSVC OR MSVC90 OR MSVC10)
  set(MSVC ON)
endif (MSVC OR MSVC90 OR MSVC10)

add_subdirectory(urdf_sensor)
add_subdirectory(urdf_model)
add_subdirectory(urdf_model_state)
add_subdirectory(urdf_world)
add_subdirectory(urdf_exception)

set(PACKAGE_NAME ${PROJECT_NAME})
set(cmake_conf_file "${CMAKE_CURRENT_SOURCE_DIR}/cmake/${PROJECT_NAME}-config.cmake")
#configure_file("${cmake_conf_file}.in" "${cmake_conf_file}" @ONLY)
#install(FILES ${cmake_conf_file} DESTINATION share/${PROJECT_NAME}/cmake/ COMPONENT cmake)

# Make the package config file
if (NOT MSVC)
  set(PACKAGE_DESC "Unified Robot Description Format")
  set(pkg_conf_file "${CMAKE_CURRENT_SOURCE_DIR}/cmake/pkgconfig/urdfdom_headers.pc")
  #configure_file("${pkg_conf_file}.in" "${pkg_conf_file}" @ONLY)
  #install(FILES ${pkg_conf_file} DESTINATION lib/pkgconfig/ COMPONENT pkgconfig)
endif()

#message(STATUS "Configuration successful. Type make install to install urdfdom_headers.")
