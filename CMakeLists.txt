# ----------------------------------------------------------------------------
# Root CMake file for nanoflann
#
#  2011-2016 - Jose Luis Blanco 
# ----------------------------------------------------------------------------
cmake_minimum_required(VERSION 3.0)

# Dependencies:
# * examples : Eigen
# * tests : gtest
include("cmake/HunterGate.cmake")
HunterGate(
    URL "https://github.com/ruslo/hunter/archive/v0.19.0.tar.gz"
    SHA1 "e9d11aee66607cfcfbec5e3811d4804135b5bfac"
)

PROJECT(nanoflann VERSION 1.2.3) # see NANOFLANN_VERSION in include/nanoflann.hpp

add_library(nanoflann INTERFACE)

set(nanoflann_inc_dir "${CMAKE_CURRENT_LIST_DIR}/include")
target_include_directories(nanoflann
  INTERFACE
  "$<BUILD_INTERFACE:${nanoflann_inc_dir}>" # i.e., submodule use
  )

# Examples
OPTION(NANOFLANN_BUILD_EXAMPLES "Build examples" ON)
IF(NANOFLANN_BUILD_EXAMPLES)
  add_subdirectory(examples)
ENDIF()

# Tests
OPTION(NANOFLANN_BUILD_TESTS "Build unit tests" ON)
IF(NANOFLANN_BUILD_TESTS)
  add_subdirectory(tests)
ENDIF()



### Install ###
set(generated_dir "${CMAKE_CURRENT_BINARY_DIR}/generated")

set(config_install_dir "lib/cmake/${PROJECT_NAME}")
set(include_install_dir "include")

set(version_config "${generated_dir}/${PROJECT_NAME}ConfigVersion.cmake")
set(project_config "${generated_dir}/${PROJECT_NAME}Config.cmake")
set(targets_export_name "${PROJECT_NAME}Targets")
set(namespace "${PROJECT_NAME}::")

include(CMakePackageConfigHelpers)
write_basic_package_version_file(
  "${version_config}" COMPATIBILITY SameMajorVersion
  )

# Note: use 'targets_export_name'
configure_package_config_file(
  "${PROJECT_SOURCE_DIR}/cmake/Config.cmake.in"
  "${project_config}"
  INSTALL_DESTINATION "${config_install_dir}"
  )

install(
  TARGETS ${PROJECT_NAME}
  EXPORT "${targets_export_name}"
  LIBRARY DESTINATION "lib"
  ARCHIVE DESTINATION "lib"
  RUNTIME DESTINATION "bin"
  INCLUDES DESTINATION "${include_install_dir}"
  )

install(
  FILES
  include/nanoflann.hpp
  DESTINATION "${include_install_dir}/"
  )

install(
  FILES "${project_config}" "${version_config}"
  DESTINATION "${config_install_dir}"
  )

install(
  EXPORT "${targets_export_name}"
  NAMESPACE "${namespace}"
  DESTINATION "${config_install_dir}"
  )
