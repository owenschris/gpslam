cmake_minimum_required(VERSION 2.6)
enable_testing()
project(gpslam CXX C)

# version indicator
set(GPSLAM_VERSION_MAJOR 0)
set(GPSLAM_VERSION_MINOR 1)
set(GPSLAM_VERSION_PATCH 0)
set(GPSLAM_VERSION_STRING "${GPSLAM_VERSION_MAJOR}.${GPSLAM_VERSION_MINOR}.${GPSLAM_VERSION_PATCH}")


# Find dependent libraries
set(GTSAM_DIR "/mnt/DATA1/asdr_sbir/tisdk/build/arago-tmp-external-linaro-toolchain/work/armv7ahf-neon-linux-gnueabi/astronav/1.0-r0-r0/build/libgtsam/src/libgtsam-install-RelWithDebInfo/lib/cmake/GTSAM")
set(GTSAMCMakeTools_DIR "/mnt/DATA1/asdr_sbir/tisdk/build/arago-tmp-external-linaro-toolchain/work/armv7ahf-neon-linux-gnueabi/astronav/1.0-r0-r0/build/libgtsam/src/libgtsam-install-RelWithDebInfo/lib/cmake/GTSAMCMakeTools" CACHE INTERNAL "")

# Find GTSAM components
find_package(GTSAM REQUIRED) # Uses installed package
include_directories(${GTSAM_INCLUDE_DIR})
set(GTSAM_LIBS gtsam)

# for unittest scripts
set(CMAKE_MODULE_PATH "${CMAKE_MODULE_PATH}" "${GTSAM_DIR}/../GTSAMCMakeTools")

find_package(GTSAMCMakeTools)
include(GtsamMakeConfigFile)
include(GtsamBuildTypes)
include(GtsamTesting)

# options
option(GPSLAM_BUILD_MATLAB_TOOLBOX "whether build matlab toolbox" OFF)

# Boost - same requirement as gtsam
find_package(Boost 1.46 REQUIRED)
find_package(Boost COMPONENTS filesystem REQUIRED)
find_package(Boost COMPONENTS system REQUIRED)
find_package(Boost COMPONENTS thread REQUIRED)
find_package(Boost COMPONENTS serialization REQUIRED)

include_directories(${Boost_INCLUDE_DIR})


# include current source folder, at the very beginning
include_directories(BEFORE ${CMAKE_CURRENT_SOURCE_DIR})

# Process source subdirs
add_subdirectory(gpslam)


# Install config and export files
GtsamMakeConfigFile(gpslam)
export(TARGETS ${gpslam_EXPORTED_TARGETS} FILE gpslam-exports.cmake)

