# Include directories
include_directories(${CMAKE_SOURCE_DIR}/include)

# Add subdirectories
add_subdirectory(vdemgmt)

# Enable profiling if requested
if(ENABLE_PROFILE)
  set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -pg --coverage")
  set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} -pg --coverage")
endif()

# Define libraries
add_library(vdesnmp STATIC libvdesnmp.c)
add_library(vdeplug STATIC libvdeplug.c)
add_library(vdehist STATIC libvdehist.c)

# Link libraries
target_link_libraries(vdesnmp vdecommon vdemgmt)
target_link_libraries(vdeplug vdecommon)
target_link_libraries(vdehist vdecommon)

# Set version numbers and export symbols
set_target_properties(vdesnmp PROPERTIES VERSION 0.0.1 SOVERSION 0)
set_target_properties(vdeplug PROPERTIES VERSION 3.0.1 SOVERSION 3)
set_target_properties(vdehist PROPERTIES VERSION 0.0.1 SOVERSION 0)

# Install pkgconfig files
# Configure pkg-config files
configure_file(vdesnmp.pc.in ${CMAKE_BINARY_DIR}/vdesnmp.pc @ONLY)
configure_file(vdeplug.pc.in ${CMAKE_BINARY_DIR}/vdeplug.pc @ONLY)
configure_file(vdehist.pc.in ${CMAKE_BINARY_DIR}/vdehist.pc @ONLY)

install(FILES ${CMAKE_CURRENT_BINARY_DIR}/vdesnmp.pc ${CMAKE_CURRENT_BINARY_DIR}/vdeplug.pc ${CMAKE_CURRENT_BINARY_DIR}/vdehist.pc
        DESTINATION ${CMAKE_INSTALL_LIBDIR}/pkgconfig)