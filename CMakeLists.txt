cmake_minimum_required(VERSION 2.8)

# The version number.
set (PROJECT_VER_MAJOR 0)
set (PROJECT_VER_MINOR 0)
set (PROJECT_VER_PATCH 2)
set (PROJECT_VER "${PROJECT_VER_MAJOR}.${PROJECT_VER_MINOR}.${PROJECT_VER_PATCH}")
set (PROJECT_APIVER "${PROJECT_VER_MAJOR}.${PROJECT_VER_MINOR}")
set (PROJECT_DESCRIPTION "Asterisk silk codec support")

#set path for additional modules
SET(CMAKE_MODULE_PATH "${CMAKE_SOURCE_DIR}/CMakeModules")

# Find the host operating system and architecture
# include (FindOS)
find_package(asterisk)

project(codec_silk)

set(MY_URL "http://developer.skype.com/silk/SILK_SDK_SRC_v1.0.9.zip")
set(MY_DOWNLOAD_PATH "SILK_SDK_SRC_v1.0.9.zip")
set(MY_EXTRACTED_FILE "silk")

# if (NOT EXISTS "${MY_DOWNLOAD_PATH}")
#     file(DOWNLOAD "${MY_URL}" "${MY_DOWNLOAD_PATH}")
# endif()
# 
# add_custom_command(
#     OUTPUT "${MY_EXTRACTED_FILE}"
#     COMMAND unzip -u SILK_SDK_SRC_v1.0.9.zip
#     DEPENDS "${MY_DOWNLOAD_PATH}")
    
    
# Add library project
add_subdirectory (silk)


set(CMAKE_CXX_FLAGS "-g -Wall")


# include_directories (${CMAKE_CURRENT_SOURCE_DIR}/include)

# Add library project
set(LIBRARYNAME codec_silk)
add_dependencies(codec_silk silk)
set(CMAKE_LIBRARY_PATH ${CMAKE_LIBRARY_PATH} silk)
include_directories(${CMAKE_CURRENT_SOURCE_DIR} silk/interface)




LIST(APPEND SRC codec_silk.c)
add_library (codec_silk SHARED ${SRC})

set_target_properties (codec_silk PROPERTIES VERSION ${PROJECT_VER} SOVERSION ${PROJECT_APIVER})
set_target_properties (codec_silk PROPERTIES OUTPUT_NAME codec_silk)
set_target_properties(codec_silk PROPERTIES PREFIX "")

target_link_libraries(${LIBRARYNAME} silk)
# add_executable(codec_silk codec_silk.c)
set(LIBRARY_OUTPUT_PATH ${PROJECT_BINARY_DIR}/modules)

IF(UNIX AND NOT APPLE)
  SET_TARGET_PROPERTIES (codec_silk PROPERTIES COMPILE_FLAGS "-fPIC")
ENDIF()

INSTALL(TARGETS codec_silk LIBRARY DESTINATION /usr/lib/asterisk/modules)

