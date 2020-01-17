
project( ${TARGET_PREFIX}_extern LANGUAGES CXX C )
set( TARGET_PREFIX ${PROJECT_NAME} )
set( CMAKE_FOLDER extern )

# helpful message
message( STATUS "To remove a submodule for real, see https://stackoverflow.com/a/36593218" )

# add disired compiler flags to extern libraries but not to our own sources
enable_cxx_compiler_flag_if_supported( "-w" )

# Use the extern function to easily combine external dependencies into a single interface.
# See helpers.cmake for the function definition and instructions.

add_library(${TARGET_PREFIX}_sdk SHARED IMPORTED)
if ( WIN32 )
  set( DISCORD_SDK_SHARED_LIB ${CMAKE_CURRENT_SOURCE_DIR}/discord_game_sdk.dll PARENT_SCOPE )
  set_property(TARGET ${TARGET_PREFIX}_sdk PROPERTY IMPORTED_LOCATION
    ${CMAKE_BINARY_DIR}/source/discord_game_sdk.dll)
else ( WIN32 )
  set( DISCORD_SDK_SHARED_LIB ${CMAKE_CURRENT_SOURCE_DIR}/discord_game_sdk.so PARENT_SCOPE )
  set_property(TARGET ${TARGET_PREFIX}_sdk PROPERTY IMPORTED_LOCATION
    ${CMAKE_BINARY_DIR}/source/discord_game_sdk.so)
endif ( WIN32 )
extern( link ${TARGET_PREFIX}_sdk )

extern( include ${CMAKE_CURRENT_SOURCE_DIR} ) 

# Add other linker flags or compiler definitions that don't fit anywhere else
if ( MSVC )
  extern( define _ENABLE_EXTENDED_ALIGNED_STORAGE )
  extern( define _CRT_SECURE_NO_WARNINGS )
endif ( MSVC )

# Create an external dependency ui that will appear as ${TARGET_PREFIX}_extern_interface outside of this scope.
extern( finish )
