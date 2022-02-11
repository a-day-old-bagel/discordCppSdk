set( CMAKE_FOLDER source )
set( TARGET_NAME ${TARGET_PREFIX} )

add_library( dsdk_lib_import SHARED IMPORTED )
if( FOR_WINDOWS )
  configure_file(
    ${CMAKE_CURRENT_SOURCE_DIR}/discord_game_sdk.dll ${CMAKE_LIBRARY_OUTPUT_DIRECTORY}/discord_game_sdk.dll COPYONLY )
  configure_file(
    ${CMAKE_CURRENT_SOURCE_DIR}/discord_game_sdk.lib ${CMAKE_LIBRARY_OUTPUT_DIRECTORY}/discord_game_sdk.lib COPYONLY )
  set_target_properties( dsdk_lib_import PROPERTIES
    IMPORTED_LOCATION ${CMAKE_LIBRARY_OUTPUT_DIRECTORY}/discord_game_sdk.dll
    IMPORTED_IMPLIB ${CMAKE_LIBRARY_OUTPUT_DIRECTORY}/discord_game_sdk.lib )
else()
  configure_file(
    ${CMAKE_CURRENT_SOURCE_DIR}/discord_game_sdk.so ${CMAKE_LIBRARY_OUTPUT_DIRECTORY}/discord_game_sdk.so COPYONLY )
  set_target_properties(
    dsdk_lib_import PROPERTIES IMPORTED_LOCATION ${CMAKE_LIBRARY_OUTPUT_DIRECTORY}/discord_game_sdk.so )
endif()

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
target_include_directories( ${LIB_NAME} PUBLIC ${CMAKE_CURRENT_SOURCE_DIR} )
set_target_properties( ${LIB_NAME} PROPERTIES BUILD_WITH_INSTALL_RPATH FALSE LINK_FLAGS "-Wl,-rpath='$ORIGIN'" )
target_link_libraries( ${LIB_NAME} PRIVATE dsdk_lib_import )

if( WIN32 )
  include( GenerateExportHeader )
    set_target_properties( ${LIB_NAME} PROPERTIES
      WINDOWS_EXPORT_ALL_SYMBOLS ON
      )
elseif( FOR_WINDOWS )
  include( GenerateExportHeader )
  set_target_properties( ${LIB_NAME} PROPERTIES
    WINDOWS_EXPORT_ALL_SYMBOLS ON
    PREFIX ""
    SUFFIX ""
    )
endif()
