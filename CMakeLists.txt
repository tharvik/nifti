#File Generated by Hans J. Johnson
#Please contact hans-johnson@uiowa.edu for making enhancments/corrections
project(NIFTI)
cmake_minimum_required(VERSION 2.6)
if(COMMAND cmake_policy)
    cmake_policy(SET CMP0003 NEW)
endif(COMMAND cmake_policy)

subdirs(znzlib niftilib)
add_definitions(-DHAVE_ZLIB)

# add option to build shared libraries. The default is OFF to maintain the
# current build behavior
option( BUILD_SHARED_LIBS "Toggle building shared libraries")

# Michael Hanke 2007-04-25 Note:
# 'Utilities' is merged into the include install path to be able to easily
# install into any other path if necessary. For the same reason lib, bin and
# include dir for standalone builds are only set if not yet defined.

###  USE AS PART OF ITK
if("${ITK_VERSION_MAJOR}" GREATER 0.0)
#   message(STATUS "---------------------ITK ZLIB  ${ITK_ZLIB_LIBRARIES}")
  add_definitions(-DHAVE_ZLIB)
  add_definitions(-DITKZLIB)
  set(PACKAGE_PREFIX "ITK")
  set(NIFTI_LIBRARY_PROPERTIES ${ITK_LIBRARY_PROPERTIES})
  set(NIFTI_INSTALL_BIN_DIR "${ITK_INSTALL_BIN_DIR_CM24}")
  set(NIFTI_INSTALL_LIB_DIR "${ITK_INSTALL_LIB_DIR_CM24}")

  if(ITK_USE_FLAT_DIRECTORY_INSTALL)
    set(NIFTI_INSTALL_INCLUDE_DIR "${ITK_INSTALL_INCLUDE_DIR_CM24}")
  else(ITK_USE_FLAT_DIRECTORY_INSTALL)
    set(NIFTI_INSTALL_INCLUDE_DIR "${ITK_INSTALL_INCLUDE_DIR_CM24}/Utilities")
  endif(ITK_USE_FLAT_DIRECTORY_INSTALL)

  set(NIFTI_ZLIB_LIBRARIES ${ITK_ZLIB_LIBRARIES})
  set(NIFTI_INSTALL_NO_DEVELOPMENT ${ITK_INSTALL_NO_DEVELOPMENT})
  set(NIFTI_INSTALL_NO_LIBRARIES ${ITK_INSTALL_NO_LIBRARIES})

else("${ITK_VERSION_MAJOR}" GREATER 0.0)
  set(CPACK_PACKAGE_VERSION_MAJOR "2")
  set(CPACK_PACKAGE_VERSION_MINOR "0")
  set(CPACK_PACKAGE_VERSION_PATCH "0")
###  USE AS STAND ALONE PACKAGE
   find_package(ZLIB)
#  message(STATUS "---------------------SYSTEM ZLIB ${ZLIB_LIBRARY}")
  set(PACKAGE_PREFIX "")
  # Set default shared library version
  # This library version will be applied to all libraries in the package
  # unless it is not explicitely for a certain lib.
  set(NIFTI_SHAREDLIB_VERSION ${CPACK_PACKAGE_VERSION_MAJOR}.${CPACK_PACKAGE_VERSION_MINOR}.${CPACK_PACKAGE_VERSION_PATCH} )
  string( REGEX MATCH "^[0-9]+" NIFTI_SHAREDLIB_SOVERSION ${NIFTI_SHAREDLIB_VERSION})
  set(NIFTI_LIBRARY_PROPERTIES VERSION ${NIFTI_SHAREDLIB_VERSION} SOVERSION ${NIFTI_SHAREDLIB_SOVERSION})

  set(NIFTI_INSTALL_BIN_DIR "${CMAKE_INSTALL_PREFIX}/bin")
  set(NIFTI_INSTALL_LIB_DIR "${CMAKE_INSTALL_PREFIX}/lib")
  set(NIFTI_INSTALL_INCLUDE_DIR "${CMAKE_INSTALL_PREFIX}/include/nifti")
  set(NIFTI_ZLIB_LIBRARIES ${ZLIB_LIBRARIES})
  set(NIFTI_INSTALL_NO_DEVELOPMENT 0)
  set(NIFTI_INSTALL_NO_LIBRARIES 0)

  #Needs an if clause and more work before testing can take place.
  subdirs(Testing)
  subdirs(nifticdf utils)

  option(USE_FSL_CODE "If OFF, The copyright of this code is questionable for inclusion with nifti." OFF)
  mark_as_advanced(USE_FSL_CODE)

  # the order of subdirs is important! fsliolob has to preceed examples
  # as otherwise FSLIOLIB_SOURCE_DIR is undefined and hence the examples
  # will fail to compile
  if(USE_FSL_CODE)
    subdirs(fsliolib)
    subdirs(examples)
  endif(USE_FSL_CODE)
  enable_testing()
  include(CTest)

  ####################################
  ### Define information necessary for packaging with CPACK (http://www.cmake.org/Wiki/CMake:Packaging_With_CPack)
### The last section is concerned with installing the binaries and making distributions.
include(InstallRequiredSystemLibraries)

set(CPACK_PACKAGE_NAME "NIFTI")

set(CPACK_SOURCE_PACKAGE_FILE_NAME "${CPACK_PACKAGE_NAME}-dev")
set(CPACK_PACKAGE_DESCRIPTION_SUMMARY "Niftilib is a set of i/o libraries for reading and writing files in the nifti-1 data format. nifti-1 is a binary file format for storing medical image data, e.g. magnetic resonance image (MRI) and functional MRI (fMRI) brain images.")
set(CPACK_PACKAGE_DESCRIPTION_FILE "${NIFTI_SOURCE_DIR}/README")
#set(CPACK_RESOURCE_FILE_LICENSE "${NIFTI_SOURCE_DIR}/Copyright.txt")
set(CPACK_RESOURCE_FILE_LICENSE "${NIFTI_SOURCE_DIR}/README")

set(CPACK_PACKAGE_DEFAULT_LOCATION "/opt/${CPACK_PACKAGE_NAME}")
#set(CPACK_PACKAGE_INSTALL_DIRECTORY "NIFTI ${CPACK_PACKAGE_VERSION_MAJOR}.${CPACK_PACKAGE_VERSION_MINOR}")
set(CPACK_PACKAGE_INSTALL_DIRECTORY "${NIFTI}")
set(CPACK_PACKAGING_INSTALL_PREFIX "/")
set(CPACK_SET_DESTDIR ON)

set(CPACK_PACKAGE_VENDOR "NIFTI DFWG")

set(CPACK_SOURCE_GENERATOR "TGZ;TZ")
#set(CPACK_SOURCE_IGNORE_FILES "/CVS/;/\\.svn/;\\.swp$;\\.#;/#;\\.*~")
set(CPACK_SOURCE_IGNORE_FILES "")
set(CPACK_PACKAGE_EXECUTABLES "NIFTI";"A program for registering medical
images with mutual information.")

if(WIN32 AND NOT UNIX)
  set(CPACK_PACKAGE_INSTALL_REGISTRY_KEY
"${CPACK_PACKAGE_NAME}-${CPACK_PACKAGE_VERSION_MAJOR}.${CPACK_PACKAGE_VERSION_MINOR}.${CPACK_PACKAGE_VERSION_PATCH}")
  # There is a bug in NSI that does not handle full unix paths properly. Make
  # sure there is at least one set of four (4) backlasshes.
  set(CPACK_PACKAGE_ICON
"${CMake_SOURCE_DIR}/Utilities/Release\\\\InstallIcon.bmp")
  set(CPACK_NSIS_INSTALLED_ICON_NAME "bin\\\\MyExecutable.exe")
  set(CPACK_NSIS_DISPLAY_NAME "${CPACK_PACKAGE_INSTALL_DIRECTORY} My Famous
Project")
  set(CPACK_NSIS_HELP_LINK "http:\\\\\\\\www.nitrc.org")
  set(CPACK_NSIS_URL_INFO_ABOUT "http:\\\\\\\\www.nitrc.org")
  set(CPACK_NSIS_CONTACT "xyz@domain.edu")
  set(CPACK_NSIS_MODIFY_PATH ON)
else(WIN32 AND NOT UNIX)
  set(CPACK_STRIP_FILES OFF)
  set(CPACK_SOURCE_STRIP_FILES OFF)
endif(WIN32 AND NOT UNIX)

include(CPack)


endif("${ITK_VERSION_MAJOR}" GREATER 0.0)



if(NOT DEFINED NIFTI_INSTALL_LIB_DIR)
  message(FATAL_ERROR "Must set NIFTI_INSTALL_LIB_DIR.")
endif(NOT DEFINED NIFTI_INSTALL_LIB_DIR)
if(NOT DEFINED NIFTI_INSTALL_BIN_DIR)
  message(FATAL_ERROR "Must set NIFTI_INSTALL_BIN_DIR.")
endif(NOT DEFINED NIFTI_INSTALL_BIN_DIR)
if(NOT DEFINED NIFTI_INSTALL_INCLUDE_DIR)
  message(FATAL_ERROR "Must set NIFTI_INSTALL_INCLUDE_DIR.")
endif(NOT DEFINED NIFTI_INSTALL_INCLUDE_DIR)






