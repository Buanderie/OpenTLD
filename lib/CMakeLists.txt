#------------------------------------------------------------------------------
#build type
if(NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE Release CACHE STRING
        "Choose the type of build, options are: None Debug Release RelWithDebInfo MinSizeRel."
        FORCE)
endif(NOT CMAKE_BUILD_TYPE)

option(WITH_OPENMP "Use OpenMP." OFF)

if(WITH_OPENMP)
	find_package(OpenMP REQUIRED)
	set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} ${OpenMP_C_FLAGS}")
	set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${OpenMP_CXX_FLAGS}")
	set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} ${OpenMP_EXE_LINKER_FLAGS}")
endif(WITH_OPENMP)

if(WIN32)
	add_definitions(-DLIBCONFIGXX_STATIC -DLIBCONFIG_STATIC) #Needed when linking libconfig statically
endif(WIN32)

if(APPLE)
	add_definitions(-DHAVE_XLOCALE_H)
endif(APPLE)

if(NOT MSVC)
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -fPIC")
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fPIC")
endif(NOT MSVC)

set(EXECUTABLE_OUTPUT_PATH ${PROJECT_BINARY_DIR}/bin)
set(LIBRARY_OUTPUT_PATH ${PROJECT_BINARY_DIR}/lib)

# Find OpenCV
find_package(OpenCV 2)
if( OpenCV_FOUND )
        include_directories (${OpenCV_INCLUDE_DIRS})
        set( DEPS "${DEPS};${OpenCV_LIBS};" )
endif()

#CVBLOB
set(cvblob_dir "/usr/local" CACHE PATH "")
find_library(cvblob_lib cvblob PATHS ${cvblob_dir}/lib NO_DEFAULT_PATH)
include_directories(${cvblob_dir}/include)
set( DEPS "${DEPS};${cvblob_lib}")

include_directories(
    ${PROJECT_SOURCE_DIR}/include/imacq
    ${PROJECT_SOURCE_DIR}/include/mftracker
    ${PROJECT_SOURCE_DIR}/include/tld
    ${PROJECT_SOURCE_DIR}/lib/cvblobs/
)

add_library(
    opentld
    SHARED
    imacq/ImAcq.cpp
    mftracker/BB.cpp
    mftracker/BBPredict.cpp
    mftracker/FBTrack.cpp
    mftracker/Lk.cpp
    mftracker/Median.cpp
    tld/Clustering.cpp
    tld/DetectionResult.cpp
    tld/DetectorCascade.cpp
    tld/EnsembleClassifier.cpp
    tld/ForegroundDetector.cpp
    tld/MedianFlowTracker.cpp
    tld/NNClassifier.cpp
    tld/TLD.cpp
    tld/TLDUtil.cpp
    tld/VarianceFilter.cpp
    cvblobs/blob.cpp
    cvblobs/BlobContour.cpp
    cvblobs/BlobOperators.cpp
    cvblobs/BlobProperties.cpp
    cvblobs/BlobResult.cpp
    cvblobs/ComponentLabeling.cpp
)

target_link_libraries(opentld ${DEPS})

