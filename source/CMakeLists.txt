set( CMAKE_FOLDER source )
set( TARGET_NAME ${TARGET_PREFIX} )

add_library( ${TARGET_NAME}_game_sdk MODULE IMPORTED)
if ( WIN32 )
  configure_file( ${CMAKE_CURRENT_SOURCE_DIR}/discord_game_sdk.lib ${CMAKE_CURRENT_BINARY_DIR} COPYONLY )
  configure_file( ${CMAKE_CURRENT_SOURCE_DIR}/discord_game_sdk.dll ${CMAKE_CURRENT_BINARY_DIR} COPYONLY )
  set_target_properties( ${TARGET_NAME}_game_sdk PROPERTIES
      IMPORTED_LOCATION ${CMAKE_CURRENT_BINARY_DIR}/discord_game_sdk.lib
      )
  include( GenerateExportHeader )
else ( WIN32 )
  configure_file( ${CMAKE_CURRENT_SOURCE_DIR}/discord_game_sdk.so ${CMAKE_CURRENT_BINARY_DIR} COPYONLY )
  set_target_properties( ${TARGET_NAME}_game_sdk PROPERTIES
      IMPORTED_LOCATION ${CMAKE_CURRENT_BINARY_DIR}/discord_game_sdk.so
      )
endif ( WIN32 )
extern( link ${TARGET_NAME}_game_sdk )
extern( include ${CMAKE_CURRENT_SOURCE_DIR} )
extern( finish )

add_library( ${TARGET_NAME} SHARED
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
target_link_libraries( ${TARGET_NAME}
  ${TARGET_NAME}_game_sdk
  )
target_include_directories( ${TARGET_NAME} PUBLIC
  ${CMAKE_CURRENT_SOURCE_DIR}
  )
set_target_properties( ${TARGET_NAME} PROPERTIES
  WINDOWS_EXPORT_ALL_SYMBOLS ON
  )