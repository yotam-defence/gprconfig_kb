cmake_minimum_required(VERSION 3.28)

project(gprconfig_kb VERSION 0.1.0
  DESCRIPTION "GPRconfig compiler knowledge base (XML data)"
  LANGUAGES NONE)

include(GNUInstallDirs)
include(CMakePackageConfigHelpers)

set(GPRCONFIG_KB_INSTALL_DIR "${CMAKE_INSTALL_DATADIR}/gprconfig")

# Build-tree interface target: consumers read the property GPRCONFIG_KB_DB_DIR.
add_library(gprconfig_kb_db INTERFACE)
add_library(gprconfig_kb::db ALIAS gprconfig_kb_db)
set_target_properties(gprconfig_kb_db PROPERTIES
  EXPORT_NAME db
  INTERFACE_GPRCONFIG_KB_DB_DIR "${PROJECT_SOURCE_DIR}/db")
set(gprconfig_kb_DB_DIR "${PROJECT_SOURCE_DIR}/db")

install(DIRECTORY db/ DESTINATION ${GPRCONFIG_KB_INSTALL_DIR}
  FILES_MATCHING PATTERN "*.xml")

write_basic_package_version_file(
  "${PROJECT_BINARY_DIR}/gprconfig_kbConfigVersion.cmake"
  VERSION ${PROJECT_VERSION} COMPATIBILITY AnyNewerVersion ARCH_INDEPENDENT)
configure_package_config_file(cmake/gprconfig_kbConfig.cmake.in
  "${PROJECT_BINARY_DIR}/gprconfig_kbConfig.cmake"
  INSTALL_DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/gprconfig_kb
  PATH_VARS GPRCONFIG_KB_INSTALL_DIR)
install(FILES
  "${PROJECT_BINARY_DIR}/gprconfig_kbConfig.cmake"
  "${PROJECT_BINARY_DIR}/gprconfig_kbConfigVersion.cmake"
  DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/gprconfig_kb)
# The validation script is shipped so consumers can re-run it.
install(FILES cmake/ValidateKB.cmake
  DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/gprconfig_kb)

include(CTest)
if(BUILD_TESTING)
  add_test(NAME kb_structure
    COMMAND ${CMAKE_COMMAND} "-DKB_DB_DIR=${PROJECT_SOURCE_DIR}/db"
            -P "${PROJECT_SOURCE_DIR}/cmake/ValidateKB.cmake")
endif()
