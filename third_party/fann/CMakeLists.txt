cmake_minimum_required (VERSION 2.8)

if (NOT DEFINED CMAKE_BUILD_TYPE)
    set (CMAKE_BUILD_TYPE Release CACHE STRING "Build type")
endif ()

project (FANN)

list (APPEND CMAKE_MODULE_PATH ${CMAKE_CURRENT_SOURCE_DIR}/cmake/Modules)

set (FANN_VERSION_MAJOR 2)
set (FANN_VERSION_MINOR 2)
set (FANN_VERSION_PATCH 0)
set (FANN_VERSION_STRING ${FANN_VERSION_MAJOR}.${FANN_VERSION_MINOR}.${FANN_VERSION_PATCH})

option(BUILD_SHARED_LIBS "build shared/static libs" OFF)

INCLUDE(DefineInstallationPaths)

configure_file (cmake/config.h.in ${CMAKE_CURRENT_SOURCE_DIR}/src/include/config.h)
include_directories (${CMAKE_CURRENT_SOURCE_DIR}/src/include/)

configure_file (cmake/fann.pc.cmake ${CMAKE_CURRENT_SOURCE_DIR}/fann.pc @ONLY)

ADD_SUBDIRECTORY( src ) 

################# cpack ################

SET(CPACK_PACKAGE_DESCRIPTION_SUMMARY "Fast Artificial Neural Network Library (FANN)")
SET(CPACK_PACKAGE_VENDOR "Steffen Nissen")
SET(CPACK_PACKAGE_DESCRIPTION_FILE "${CMAKE_CURRENT_SOURCE_DIR}/README.md")
SET(CPACK_RESOURCE_FILE_LICENSE "${CMAKE_CURRENT_SOURCE_DIR}/LICENSE.md")
SET(CPACK_PACKAGE_VERSION_MAJOR "${FANN_VERSION_MAJOR}")
SET(CPACK_PACKAGE_VERSION_MINOR "${FANN_VERSION_MINOR}")
SET(CPACK_PACKAGE_VERSION_PATCH "${FANN_VERSION_PATCH}")
SET(CPACK_GENERATOR "TGZ;ZIP")
SET(CPACK_SOURCE_GENERATOR "TGZ;ZIP")
SET(CPACK_DEBIAN_PACKAGE_MAINTAINER "Steffen Nissen") 
IF(WIN32 AND NOT UNIX)
  # There is a bug in NSI that does not handle full unix paths properly. Make
  # sure there is at least one set of four (4) backlasshes.
#  SET(CPACK_PACKAGE_ICON "${CMAKE_CURRENT_SOURCE_DIR}/Utilities/Release\\\\InstallIcon.bmp")
#  SET(CPACK_NSIS_INSTALLED_ICON_NAME "bin\\\\MyExecutable.exe")
#  SET(CPACK_NSIS_DISPLAY_NAME "${CPACK_PACKAGE_INSTALL_DIRECTORY} My Famous Project")
  SET(CPACK_NSIS_HELP_LINK "http:\\\\\\\\leenissen.dk/fann/")
  SET(CPACK_NSIS_URL_INFO_ABOUT "http:\\\\\\\\leenissen.dk/fann/")
  SET(CPACK_NSIS_CONTACT "steffen.fann@gmail.com")
  SET(CPACK_NSIS_MODIFY_PATH ON)
ELSE(WIN32 AND NOT UNIX)
#  SET(CPACK_STRIP_FILES "bin/MyExecutable")
#  SET(CPACK_SOURCE_STRIP_FILES "")
ENDIF(WIN32 AND NOT UNIX)
#SET(CPACK_PACKAGE_EXECUTABLES "MyExecutable" "My Executable")
INCLUDE(CPack)

################# config ################

set (FANN_USE_FILE ${CMAKE_CONFIG_DIR}/fann-use.cmake)
set (FANN_ROOT_DIR ${CMAKE_INSTALL_PREFIX})
set (FANN_INCLUDE_DIR ${CMAKE_INSTALL_PREFIX}/include)
set (FANN_INCLUDE_DIRS ${FANN_INCLUDE_DIR})
set (FANN_LIBRARY_DIRS ${CMAKE_INSTALL_PREFIX}/lib)
set (FANN_LIBRARY fann)
set (FANN_LIBRARIES ${FANN_LIBRARY})
if (UNIX)
  list (APPEND FANN_LIBRARIES m)
endif ()

if (CMAKE_VERSION VERSION_LESS 2.8.8)
  configure_file (cmake/fann-config.cmake.in ${CMAKE_CURRENT_SOURCE_DIR}/fann-config.cmake @ONLY)
endif ()
