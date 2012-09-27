cmake_minimum_required(VERSION 2.8)
include(ExternalProject)

# The version number.
set(PROJECT_VER_MAJOR 0)
set(PROJECT_VER_MINOR 0)
set(PROJECT_VER_PATCH 2)
set(PROJECT_VER "${PROJECT_VER_MAJOR}.${PROJECT_VER_MINOR}.${PROJECT_VER_PATCH}")
set(PROJECT_APIVER "${PROJECT_VER_MAJOR}.${PROJECT_VER_MINOR}")
set(PROJECT_DESCRIPTION "Asterisk silk codec support")

set(SILK_VERSION "1.0.9")
set(SILK_DOWNLOAD_URL "http://172.17.3.1/SILK_SDK_SRC_v${SILK_VERSION}.zip")

#set path for additional modules
SET(CMAKE_MODULE_PATH "${CMAKE_SOURCE_DIR}/CMakeModules")

# Find the host operating system and architecture
# include (FindOS)
find_package(asterisk)

project(codec_silk)

#require silk library
ExternalProject_Add(
   silk
   URL ${SILK_DOWNLOAD_URL}
   PREFIX "silk"
   DOWNLOAD_DIR ${CMAKE_CURRENT_BINARY_DIR}/silk
   PATCH_COMMAND ${CMAKE_COMMAND} -E copy "${CMAKE_CURRENT_SOURCE_DIR}/silk/CMakeLists.txt" <SOURCE_DIR>/CMakeLists.txt
   UPDATE_COMMAND ""
   INSTALL_COMMAND ""
)




# include_directories (${CMAKE_CURRENT_SOURCE_DIR}/include)

# Add library project
set(CMAKE_CXX_FLAGS "-g -Wall")
set(LIBRARYNAME codec_silk)

# tel cmake that external project silk created a library, import this as libsilk
add_library(libsilk UNKNOWN IMPORTED) 
set_property(TARGET libsilk PROPERTY IMPORTED_LOCATION ${CMAKE_BINARY_DIR}/silk/src/silk-build/libsilk.a ) 


#set include path for codec_silk
include_directories(${CMAKE_CURRENT_SOURCE_DIR} ${CMAKE_CURRENT_BINARY_DIR}/silk/src/silk/SILK_SDK_SRC_FLP_v${SILK_VERSION}/interface)

LIST(APPEND SRC codec_silk.c)
add_library(${LIBRARYNAME} SHARED ${SRC})

set_target_properties(${LIBRARYNAME} PROPERTIES VERSION ${PROJECT_VER} SOVERSION ${PROJECT_APIVER})
set_target_properties(${LIBRARYNAME} PROPERTIES OUTPUT_NAME codec_silk)
set_target_properties(${LIBRARYNAME} PROPERTIES PREFIX "")


add_dependencies(${LIBRARYNAME} silk)
target_link_libraries(codec_silk libsilk)

if(UNIX AND NOT APPLE)
  SET_TARGET_PROPERTIES (${LIBRARYNAME} PROPERTIES COMPILE_FLAGS "-fPIC")
endif()

set(LIBRARY_OUTPUT_PATH ${PROJECT_BINARY_DIR}/modules)
install(TARGETS codec_silk LIBRARY DESTINATION ${ASTERISK_LIBRARY_DIR})
