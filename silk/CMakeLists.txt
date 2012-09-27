cmake_minimum_required(VERSION 2.8)

######################################################################################
# Build for main library
######################################################################################

set(LIBRARYNAME silk)
set(CMAKE_C_FLAGS "-Wall")


aux_source_directory(SILK_SDK_SRC_FLP_v${SILK_VERSION}/src silk_SRC)
include_directories(${CMAKE_CURRENT_SOURCE_DIR} SILK_SDK_SRC_FLP_v${SILK_VERSION}/interface)


LIST(APPEND SRC ${silk_SRC})

add_library(silk ${SRC})
set_source_files_properties(silk PROPERTIES LANGUAGE C)

# set_target_properties(silk PROPERTIES VERSION ${SILK_VERSION})

IF(UNIX AND NOT APPLE)
  SET_TARGET_PROPERTIES (silk PROPERTIES COMPILE_FLAGS "-fPIC")
ENDIF()
