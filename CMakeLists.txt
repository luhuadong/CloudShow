cmake_minimum_required(VERSION 3.5)

project(cloudshow)

#========================
# Driver core
#========================
add_subdirectory(core)
find_package(ag_driver REQUIRED)
include_directories(${ag_driver_INCLUDE_DIRS})

message(=============================================================)
message("-- Ready to compile cloudshow")
message(=============================================================)

include_directories(${DRIVER_INCLUDE_DIRS})

if (CMAKE_BUILD_TYPE STREQUAL "")
  set(CMAKE_BUILD_TYPE Release)
endif()

if(WIN32)
  cmake_policy(SET CMP0074 NEW)
  set(OPENNI_ROOT "C:\\Program Files\\OpenNI2")
  set(OPENNI_LIBRARY "${OPENNI_ROOT}\\Lib\\OpenNI2.lib")
  set(OPENNI_INCLUDE_DIRS "${OPENNI_ROOT}\\Include\\")
  file(COPY ${OPENNI_ROOT}\\Redist\\OpenNI2.dll DESTINATION ${PROJECT_BINARY_DIR}\\Release)
  file(COPY ${OPENNI_ROOT}\\Redist\\OpenNI2.dll DESTINATION ${PROJECT_BINARY_DIR}\\Debug)
endif(WIN32)

find_package(PCL COMPONENTS common visualization io QUIET REQUIRED)
add_definitions(${PCL_DEFINITIONS})
include_directories(${PCL_INCLUDE_DIRS})
link_directories(${PCL_LIBRARY_DIRS})

if(PCL_FOUND)

#========================
# Build Setup
#========================
add_executable(cloudshow
               cloud_show.cpp)

target_link_libraries(cloudshow
                    ${EXTERNAL_LIBS}    
                    ${PCL_LIBRARIES}
                    ${ag_driver_LIBRARIES})

else()

message("PCL Not found! Can not compile cloudshow!")

endif()

install(TARGETS cloudshow
        RUNTIME DESTINATION /usr/bin)
