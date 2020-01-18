
# Set required version and policy version
cmake_minimum_required( VERSION 3.9.2 FATAL_ERROR )
cmake_policy( VERSION 3.9.2...3.13 )

# Prohibit screwing up the source tree
set( CMAKE_DISABLE_IN_SOURCE_BUILD ON )
set( CMAKE_DISABLE_SOURCE_CHANGES ON )

# Use C++17 everywhere
set( CMAKE_CXX_STANDARD 17 )
set( CMAKE_CXX_STANDARD_REQUIRED ON )

# Name the project and set a prefix for naming internal targets and variables
project( discord VERSION 0.01.01.01 LANGUAGES CXX C )
set( TARGET_PREFIX ${PROJECT_NAME} )

# Use target folders to organize target list
set_property( GLOBAL PROPERTY USE_FOLDERS ON )

# Introduce functions to help link to external libs
include( helpers.cmake )

# Build shared libs
add_subdirectory( source )

# Optionally build the example(s)
option( DISCORD_BUILD_EXAMPLES "Build Examples"	ON )
if ( DISCORD_BUILD_EXAMPLES )
  add_subdirectory( example )
endif()
