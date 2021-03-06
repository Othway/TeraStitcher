# CmakeLists.txt in matlablib dir

# make sure the compiler can find include files of other modules.
include_directories (${TeraStitcher_SOURCE_DIR}/imagemanager)
include_directories (${TeraStitcher_SOURCE_DIR}/iomanager)
include_directories (${TeraStitcher_SOURCE_DIR}/common)

if(APPLE)

set(MATLAB_INCLUDE_DIR "/Applications/MATLAB_R2013a.app/extern/include" CACHE STRING "Include directory for mex")
set(MATLAB_LD_DIR "/Applications/MATLAB_R2013a.app/bin/maci64" CACHE STRING "Library directory for mex")

include_directories (${MATLAB_INCLUDE_DIR})

# add 3rd party modules to include path
link_directories (${MATLAB_LD_DIR})

# add a library called matlablib (matlablib.so under Linux, matlablib.dll under Windows) from the all .h and .cpp files
file(GLOB matlablib_headers *.h)
file(GLOB matlablib_sources *.cpp)
add_library(matlablib SHARED ${matlablib_headers} ${matlablib_sources})
set_target_properties(matlablib PROPERTIES SUFFIX ".mexmaci64")

target_link_libraries(matlablib mx )
target_link_libraries(matlablib mex )
target_link_libraries(matlablib mat )

elseif(UNIX)

    message (FATAL_ERROR "UNIX not supported yet for mex build")

elseif(WIN32)

set(MATLAB_INCLUDE_DIR "C:/Program Files/MATLAB/R2013a/extern/include" CACHE STRING "Include directory for mex")
set(MATLAB_LD_DIR "C:/Program Files/MATLAB/R2013a/extern/lib/win64/microsoft" CACHE STRING "Library directory for mex")

set( CMAKE_SHARED_MODULE_SUFFIX "mexw64" )
set( EXPORT_FLAGS  "/EXPORT:mexFunction" )
set( CMAKE_SHARED_LINKER_FLAGS  "${CMAKE_SHARED_LINKER_FLAGS} ${EXPORT_FLAGS}" )

include_directories (${MATLAB_INCLUDE_DIR})

# add 3rd party modules to include path
link_directories (${MATLAB_LD_DIR})

# add a library called matlablib (matlablib.so under Linux, matlablib.dll under Windows) from the all .h and .cpp files
file(GLOB matlablib_headers *.h)
file(GLOB matlablib_sources *.cpp)
add_library(matlablib SHARED ${matlablib_headers} ${matlablib_sources})
set_target_properties(matlablib PROPERTIES SUFFIX ".mexw64")

target_link_libraries(matlablib libmx )
target_link_libraries(matlablib libmex )
target_link_libraries(matlablib libmat )

else()

    message (FATAL_ERROR "Architecture not supported for mex build")

endif()

target_link_libraries(matlablib imagemanager )
target_link_libraries(matlablib iomanager )
target_link_libraries(matlablib volumemanager )
target_link_libraries(matlablib tinyxml )
target_link_libraries(matlablib stitcher )
target_link_libraries(matlablib crossmips )
target_link_libraries(matlablib common )