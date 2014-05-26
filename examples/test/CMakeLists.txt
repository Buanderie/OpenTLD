set(EXECUTABLE_OUTPUT_PATH ${PROJECT_BINARY_DIR}/bin)

include_directories( ${PROJECT_SOURCE_DIR}/include )

# Find OpenCV
find_package(OpenCV 2)
if( OpenCV_FOUND )
        include_directories (${OpenCV_INCLUDE_DIRS})
        set( DEPS "${DEPS};${OpenCV_LIBS};cvblob" )
endif()

set( DEPS "${DEPS};opentld" )

add_executable( opentld_test main.cpp )

target_link_libraries(opentld_test ${DEPS})
