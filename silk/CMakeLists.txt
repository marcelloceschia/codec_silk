set(SILK_VERSION "1.0.9")
set(SILK_URL "https://developer.skype.com/silk/SILK_SDK_SRC_v${SILK_VERSION}.zip")
set(SILK_DOWNLOAD_PATH "${CMAKE_CURRENT_LIST_DIR}/SILK_SDK_SRC_v${SILK_VERSION}.zip")



if(NOT EXISTS "${SILK_DOWNLOAD_PATH}")
    file(DOWNLOAD "${SILK_URL}" "${SILK_DOWNLOAD_PATH}" SHOW_PROGRESS)
    else()
    message("file ${SILK_DOWNLOAD_PATH} exists")
endif()
if (NOT EXISTS "${CMAKE_CURRENT_LIST_DIR}/src")
    execute_process(COMMAND "unzip" "-u" "-q" "${SILK_DOWNLOAD_PATH}")
    execute_process(COMMAND "cp" "-r" "${CMAKE_CURRENT_LIST_DIR}/SILK_SDK_SRC_FLP_v${SILK_VERSION}/src" "${CMAKE_CURRENT_LIST_DIR}/src")
    execute_process(COMMAND "cp" "-r" "${CMAKE_CURRENT_LIST_DIR}/SILK_SDK_SRC_FLP_v${SILK_VERSION}/interface" "${CMAKE_CURRENT_LIST_DIR}/interface")
    execute_process(COMMAND "rm" "-r" "${CMAKE_CURRENT_LIST_DIR}/SILK_SDK_SRC_*_v${SILK_VERSION}/")
endif()


# ADD_CUSTOM_TARGET(download depend)
# add_custom_command(
#     PRE_BUILD
#     TARGET download
#     COMMAND "unzip" ARGS "-u" "-q" "${SILK_DOWNLOAD_PATH}"
#     DEPENDS "${SILK_DOWNLOAD_PATH}"
# )
# add_custom_command(
#     PRE_BUILD
#     TARGET download
#     PRE_BUILD
#     COMMAND "cp" ARGS "-r" "SILK_SDK_SRC_FLP_v${SILK_VERSION}/src" "src"
#     DEPENDS "SILK_SDK_SRC_FLP_v${SILK_VERSION}"
# )
# 
# add_custom_command(
#     PRE_BUILD
#     TARGET download
#     PRE_BUILD
#     COMMAND "cp" ARGS "-r" "SILK_SDK_SRC_FLP_v${SILK_VERSION}/interface" "interface"
#     DEPENDS "SILK_SDK_SRC_FLP_v${SILK_VERSION}"
# )
# 
# add_custom_command(
#     PRE_BUILD
#     TARGET download
#     COMMAND "rm" ARGS "-r" "SILK_SDK_SRC_*_v${SILK_VERSION}/"
# )

######################################################################################
# Build library
######################################################################################

include_directories(${CMAKE_CURRENT_SOURCE_DIR} "interface")

# find_package(Threads REQUIRED)

set(LIBRARYNAME silk)
set(CMAKE_C_FLAGS "-Wall")


aux_source_directory(src silk_SRC)

LIST(APPEND SRC ${silk_SRC})

add_library(silk ${SRC})


IF(UNIX AND NOT APPLE)
  SET_TARGET_PROPERTIES (silk PROPERTIES COMPILE_FLAGS "-fPIC")
ENDIF()
set_target_properties (silk PROPERTIES VERSION ${SILK_VERSION})
set_target_properties(silk PROPERTIES LINKER_LANGUAGE C)
# set_target_properties (silk PROPERTIES OUTPUT_NAME silk)
# set_target_properties(silk PROPERTIES PREFIX "")

# target_link_libraries(${LIBRARYNAME})
# set(LIBRARY_OUTPUT_PATH ${PROJECT_BINARY_DIR}/modules)


