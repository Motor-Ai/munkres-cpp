cmake_minimum_required(VERSION 3.5)
project(munkres-cpp VERSION 3.0.0)

add_library(munkres-cpp INTERFACE)
add_library(lp::munkres-cpp ALIAS munkres-cpp)

target_compile_features(munkres-cpp
    INTERFACE 
        cxx_std_11
)

target_include_directories(munkres-cpp
    INTERFACE 
        $<BUILD_INTERFACE:${PROJECT_SOURCE_DIR}/src>
        $<INSTALL_INTERFACE:include>
)

install(DIRECTORY ${PROJECT_SOURCE_DIR}/src/
    DESTINATION include
    FILE_PERMISSIONS OWNER_READ OWNER_WRITE GROUP_READ WORLD_READ
)

install(TARGETS munkres-cpp
    EXPORT munkres-cpp-targets
    ARCHIVE DESTINATION lib
    LIBRARY DESTINATION lib
    RUNTIME DESTINATION bin
    INCLUDES DESTINATION include
)

# Generate the config file that includes the exports
include(CMakePackageConfigHelpers)

# Fix the version file name to match the project
write_basic_package_version_file(
    "${CMAKE_CURRENT_BINARY_DIR}/munkres-cppConfigVersion.cmake"
    VERSION ${PROJECT_VERSION}
    COMPATIBILITY SameMajorVersion
)

# Create the main config file using configure_package_config_file instead of configure_file
configure_package_config_file(
    "${CMAKE_CURRENT_SOURCE_DIR}/cmake/MunkresConfig.cmake.in"
    "${CMAKE_CURRENT_BINARY_DIR}/munkres-cppConfig.cmake"
    INSTALL_DESTINATION lib/cmake/munkres-cpp
)

# Export targets for build tree
export(EXPORT munkres-cpp-targets
    FILE "${CMAKE_CURRENT_BINARY_DIR}/munkres-cpp-targets.cmake"
    NAMESPACE munkres_cpp::
)

# Install the config files
install(EXPORT munkres-cpp-targets
    FILE munkres-cpp-targets.cmake
    NAMESPACE munkres_cpp::
    DESTINATION lib/cmake/munkres-cpp
)

install(
    FILES
        "${CMAKE_CURRENT_BINARY_DIR}/munkres-cppConfig.cmake"
        "${CMAKE_CURRENT_BINARY_DIR}/munkres-cppConfigVersion.cmake"
    DESTINATION
        lib/cmake/munkres-cpp
)

add_subdirectory(examples)
