set( CMAKE_FOLDER example )
set( TARGET_NAME ${TARGET_PREFIX}_${CMAKE_FOLDER} )

add_executable( ${TARGET_NAME} main.cpp )
target_link_libraries( ${TARGET_NAME} ${TARGET_PREFIX} )
target_include_directories( ${TARGET_NAME} PUBLIC
  ${CMAKE_CURRENT_SOURCE_DIR}
  )

if ( WIN32 )
  add_custom_command(TARGET ${TARGET_NAME} POST_BUILD
      COMMAND ${CMAKE_COMMAND} -E copy_if_different
      "${PROJECT_BINARY_DIR}/source/discord.dll"
      $<TARGET_FILE_DIR:${TARGET_NAME}>)
  add_custom_command(TARGET ${TARGET_NAME} POST_BUILD
      COMMAND ${CMAKE_COMMAND} -E copy_if_different
      "${PROJECT_BINARY_DIR}/source/discord_game_sdk.dll"
      $<TARGET_FILE_DIR:${TARGET_NAME}>)
else ( WIN32 )
  add_custom_command(TARGET ${TARGET_NAME} POST_BUILD
      COMMAND ${CMAKE_COMMAND} -E copy_if_different
      "${PROJECT_BINARY_DIR}/source/discord.so"
      $<TARGET_FILE_DIR:${TARGET_NAME}>)
endif ( WIN32 )