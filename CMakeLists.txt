set(ZIP FALSE CACHE BOOL "Zip most resources (useful for stand-alone builds, deprecated for linux packages and git builds)")

function(create_zip output_file input_files working_dir)
	add_custom_command(
		COMMAND ${CMAKE_COMMAND} -E tar "cf" "${output_file}" --format=zip -- ${input_files}
		WORKING_DIRECTORY "${working_dir}"
		OUTPUT  "${output_file}"
		DEPENDS ${input_files}
		COMMENT "Zipping to ${output_file}"
	)
endfunction()


file(GLOB ALL_FILES_DIRS "${CMAKE_CURRENT_SOURCE_DIR}/*")

foreach(ITEM ${ALL_FILES_DIRS})
   IF( IS_DIRECTORY "${ITEM}" )
      LIST( APPEND ALL_DIRS "${ITEM}" )
   ELSE()
      LIST( APPEND ALL_FILES "${ITEM}" )
   ENDIF()
endforeach()

if(ZIP)
	install(FILES "boot.lua" DESTINATION ${DATADIR} COMPONENT DATA) # Install boot.lua outside of zip, as this boots the program
	create_zip("${CMAKE_CURRENT_BINARY_DIR}/native_data.zip" "${ZIP_FILES}" "${CMAKE_CURRENT_SOURCE_DIR}")
	install(FILES "${CMAKE_CURRENT_BINARY_DIR}/native_data.zip" DESTINATION ${DATADIR} RENAME "data000.zip" COMPONENT DATA) # Install zip
	add_custom_target("cubosphere-zip" ALL DEPENDS "${CMAKE_CURRENT_BINARY_DIR}/native_data.zip")
else()
	install(FILES ${ALL_FILES} DESTINATION ${DATADIR} COMPONENT DATA) # Install unpacked files
	install(DIRECTORY ${ALL_DIRS} DESTINATION ${DATADIR} COMPONENT DATA) # Install unpacked dirs
endif()

add_subdirectory(desktop)
