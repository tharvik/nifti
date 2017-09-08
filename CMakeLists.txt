project(NIFTI)

cmake_minimum_required(VERSION 3.2)

set(VERSION_MAJOR 2)
set(VERSION_MINOR 0)
set(VERSION_PATCH 0)
set(VERSION ${VERSION_MAJOR}.${VERSION_MINOR}.${VERSION_PATCH})


option(USE_ZLIB "add zlib support" YES)
if(USE_ZLIB)
	find_package(ZLIB REQUIRED)
endif(USE_ZLIB)

set(INSTALL_BIN_DIR "${CMAKE_INSTALL_PREFIX}/bin"
	CACHE PATH "install path for binaries")
set(INSTALL_LIB_DIR "${CMAKE_INSTALL_PREFIX}/lib"
	CACHE PATH "install path for libraries")
set(INSTALL_INC_DIR "${CMAKE_INSTALL_PREFIX}/include/nifti"
	CACHE PATH "install path for include files")
set(INSTALL_CMAKE_DIR "${CMAKE_INSTALL_PREFIX}/lib/cmake/NIFTI"
	CACHE PATH "install path for cmake config files")


enable_testing()

add_subdirectory(znzlib)
add_subdirectory(niftilib)
add_subdirectory(nifti2)
add_subdirectory(Testing)
add_subdirectory(nifticdf)
add_subdirectory(utils)



# --
option(USE_FSL_CODE "If OFF, The copyright of this code is questionable for inclusion with nifti." OFF)
mark_as_advanced(USE_FSL_CODE)

# the order of subdirs is important! fsliolob has to preceed examples
# as otherwise FSLIOLIB_SOURCE_DIR is undefined and hence the examples
# will fail to compile
if(USE_FSL_CODE)
	subdirs(fsliolib)
	subdirs(examples)
endif(USE_FSL_CODE)
