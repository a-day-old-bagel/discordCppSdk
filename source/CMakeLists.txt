set( CMAKE_FOLDER source )
set( TARGET_NAME ${TARGET_PREFIX} )

set( SDK_LIB_DIR ${CMAKE_CURRENT_BINARY_DIR} )
add_library( ${TARGET_NAME}_game_sdk_lib SHARED IMPORTED GLOBAL )
if ( FOR_WINDOWS )
  configure_file( ${CMAKE_CURRENT_SOURCE_DIR}/discord_game_sdk.lib
    ${SDK_LIB_DIR}/discord_game_sdk.lib COPYONLY
    )
  configure_file( ${CMAKE_CURRENT_SOURCE_DIR}/discord_game_sdk.dll
    ${SDK_LIB_DIR}/discord_game_sdk.dll COPYONLY
    )
  set_target_properties( ${TARGET_NAME}_game_sdk_lib PROPERTIES
      IMPORTED_LOCATION ${SDK_LIB_DIR}/discord_game_sdk.lib
      )
  include( GenerateExportHeader )
else ( )
  configure_file( ${CMAKE_CURRENT_SOURCE_DIR}/discord_game_sdk.so
    ${SDK_LIB_DIR}/discord_game_sdk.so COPYONLY
    )
  set_target_properties( ${TARGET_NAME}_game_sdk_lib PROPERTIES
      IMPORTED_LOCATION ${SDK_LIB_DIR}/discord_game_sdk.so
      )
endif ( )
extern( link ${TARGET_NAME}_game_sdk_lib )
extern( include ${CMAKE_CURRENT_SOURCE_DIR} )
extern( finish )

add_library( ${LIB_NAME} SHARED
  achievement_manager.cpp achievement_manager.h
  activity_manager.cpp activity_manager.h
  application_manager.cpp application_manager.h
  core.cpp core.h
  discord.h
  event.h
  ffi.h
  image_manager.cpp image_manager.h
  lobby_manager.cpp lobby_manager.h
  network_manager.cpp network_manager.h
  overlay_manager.cpp overlay_manager.h
  relationship_manager.cpp relationship_manager.h
  storage_manager.cpp storage_manager.h
  store_manager.cpp store_manager.h
  types.cpp types.h
  user_manager.cpp user_manager.h
  voice_manager.cpp voice_manager.h
  )
target_link_libraries( ${LIB_NAME} PRIVATE
  ${TARGET_NAME}_interface
  )
target_include_directories( ${LIB_NAME} PUBLIC
  ${CMAKE_CURRENT_SOURCE_DIR}
  )
if( FOR_WINDOWS )
  set_target_properties( ${LIB_NAME} PROPERTIES
    WINDOWS_EXPORT_ALL_SYMBOLS ON
    PREFIX ""
    SUFFIX ""
    #LINK_FLAGS "-Wl,--add-stdcall-alias"
    #POSITION_INDEPENDENT_CODE 0 # this is to avoid MinGW warning;
    )
endif()