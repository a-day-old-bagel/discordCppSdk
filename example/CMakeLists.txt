set( CMAKE_FOLDER example )
set( TARGET_NAME ${TARGET_PREFIX}_${CMAKE_FOLDER} )

add_executable( ${TARGET_NAME} main.cpp )
target_link_libraries( ${TARGET_NAME} ${LIB_NAME} )
target_include_directories( ${TARGET_NAME} PUBLIC
  ${CMAKE_CURRENT_SOURCE_DIR}
  )

if ( FOR_WINDOWS )
  add_custom_command( TARGET ${TARGET_NAME} POST_BUILD
      COMMAND ${CMAKE_COMMAND} -E copy_if_different
      "${PROJECT_BINARY_DIR}/source/discord.dll"
      $<TARGET_FILE_DIR:${TARGET_NAME}> )
  add_custom_command( TARGET ${TARGET_NAME} POST_BUILD
      COMMAND ${CMAKE_COMMAND} -E copy_if_different
      "${PROJECT_BINARY_DIR}/source/discord_game_sdk.dll"
      $<TARGET_FILE_DIR:${TARGET_NAME}> )
else ( )
  add_custom_command( TARGET ${TARGET_NAME} POST_BUILD
      COMMAND ${CMAKE_COMMAND} -E copy_if_different
      "${PROJECT_BINARY_DIR}/source/libdiscord.so"
      $<TARGET_FILE_DIR:${TARGET_NAME}> )
  add_custom_command( TARGET ${TARGET_NAME} POST_BUILD
      COMMAND ${CMAKE_COMMAND} -E copy_if_different
      "${PROJECT_BINARY_DIR}/source/discord_game_sdk.so"
      $<TARGET_FILE_DIR:${TARGET_NAME}> )
endif ( )