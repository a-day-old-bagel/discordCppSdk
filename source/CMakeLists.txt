set( CMAKE_FOLDER source )
set( TARGET_NAME ${TARGET_PREFIX} )

# Set DISTRIBUTABLE_INSTALL_DIR in some parent scope
set( DISCORD_INSTALL_DIR ${DISTRIBUTABLE_INSTALL_DIR} )
if( WIN32 )
  find_library( DISCORD_SDK_LIB discord_game_sdk HINTS ${CMAKE_CURRENT_SOURCE_DIR} )
  list( GET DISCORD_SDK_LIB 0 LIBFILE ) # take first lib
  get_filename_component( LIBFILE_DIR ${LIBFILE} DIRECTORY ) # and get the directory from its path
  install( FILES ${LIBFILE_DIR}/discord_game_sdk.dll DESTINATION ./${DISCORD_INSTALL_DIR} COMPONENT DIST )
elseif( FOR_WINDOWS )
  add_library( dsdk_lib_import SHARED IMPORTED )
  set_target_properties( dsdk_lib_import PROPERTIES IMPORTED_LOCATION ${CMAKE_CURRENT_SOURCE_DIR}/discord_game_sdk.dll.lib )
  install( FILES ${CMAKE_CURRENT_SOURCE_DIR}/discord_game_sdk.dll DESTINATION ./${DISCORD_INSTALL_DIR} COMPONENT DIST )
else()
  configure_file( ${CMAKE_CURRENT_SOURCE_DIR}/discord_game_sdk.so ${CMAKE_CURRENT_BINARY_DIR} COPYONLY )
  add_library( dsdk_lib_import SHARED IMPORTED )
  set_target_properties( dsdk_lib_import PROPERTIES IMPORTED_LOCATION ${CMAKE_CURRENT_BINARY_DIR}/discord_game_sdk.so )
  install( FILES ${CMAKE_CURRENT_BINARY_DIR}/discord_game_sdk.so DESTINATION ./${DISCORD_INSTALL_DIR} COMPONENT DIST )
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
target_include_directories( ${LIB_NAME} PUBLIC
  ${CMAKE_CURRENT_SOURCE_DIR}
  )
if( WIN32 )
  target_link_libraries( ${LIB_NAME} PRIVATE
    ${DISCORD_SDK_LIB}
    )
else()
  target_link_libraries( ${LIB_NAME} PRIVATE
    dsdk_lib_import
    )
endif()

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
