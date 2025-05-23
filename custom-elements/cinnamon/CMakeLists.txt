# Copyright (c) Siddharth Jayashankar. All rights reserved.
cmake_minimum_required(VERSION 3.15)

set(CINNAMON_INSTALL_PREFIX ${CMAKE_INSTALL_PREFIX})

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -D__STDC_FORMAT_MACROS -DHAVE_CONFIG_H")

project(cinnamon LANGUAGES CXX)
set(CMAKE_CXX_STANDARD 17)

FILE(GLOB_RECURSE CinnamonSRCS src/*.cc )
FILE(GLOB_RECURSE CinnamonHDRS src/*.h )
add_library(cinnamon SHARED ${CinnamonSRCS} ${CinnamonHDRS})
add_dependencies(cinnamon sst-core)
target_include_directories(cinnamon PUBLIC ${SST_CORE_HOME}/include)
target_include_directories(cinnamon PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/src)


install(
     TARGETS cinnamon 
     RUNTIME DESTINATION lib
 )

install( CODE "message(STATUS \"registering Cinnamon ${CMAKE_CURRENT_SOURCE_DIR}\")")
install( CODE "execute_process(COMMAND ${SST_CORE_HOME}/bin/sst-register cinnamon cinnamon_LIBDIR=${CINNAMON_INSTALL_PREFIX}/lib)" )
install( CODE "execute_process(COMMAND ${SST_CORE_HOME}/bin/sst-register SST_ELEMENT_SOURCE cinnamon=${CMAKE_CURRENT_SOURCE_DIR}/src)" )
install( CODE "execute_process(COMMAND ${SST_CORE_HOME}/bin/sst-register SST_ELEMENT_TESTS cinnamon=${CMAKE_CURRENT_SOURCE_DIR}/tests)" )