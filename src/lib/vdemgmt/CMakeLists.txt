# Include directories
include_directories(${CMAKE_SOURCE_DIR}/include)

# Source files
set(SOURCES
  libvdemgmt.c
  ${CMAKE_SOURCE_DIR}/src/common/canonicalize.c
    ${CMAKE_SOURCE_DIR}/src/common/cmdparse.c
)

# Library
add_library(vdemgmt STATIC ${SOURCES})

# Profiling and coverage options
if(ENABLE_PROFILE)
  target_compile_options(vdemgmt PRIVATE -pg --coverage)
  target_link_libraries(vdemgmt PRIVATE -pg --coverage)
endif()

# Set version information
set_target_properties(vdemgmt PROPERTIES
  VERSION 0.0.1
  SOVERSION 0
  LINK_FLAGS "-export-dynamic"
)

# Install pkgconfig file
configure_file(vdemgmt.pc.in ${CMAKE_CURRENT_BINARY_DIR}/vdemgmt.pc @ONLY)
install(FILES ${CMAKE_CURRENT_BINARY_DIR}/vdemgmt.pc DESTINATION ${CMAKE_INSTALL_LIBDIR}/pkgconfig)