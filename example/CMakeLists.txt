set( CMAKE_FOLDER example )
set( TARGET_NAME ${TARGET_PREFIX}_${CMAKE_FOLDER} )

add_executable( ${TARGET_NAME} main.cpp )
target_link_libraries( ${TARGET_NAME} ${TARGET_PREFIX} )
target_include_directories( ${TARGET_NAME} PUBLIC
  ${CMAKE_CURRENT_SOURCE_DIR}
  )
