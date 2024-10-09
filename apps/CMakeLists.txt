set(LTQMDD_TARGET_NAME "ltqmddv1")
add_executable(${LTQMDD_TARGET_NAME} ${CMAKE_CURRENT_SOURCE_DIR}/main.cpp)
target_compile_features(${LTQMDD_TARGET_NAME} PUBLIC cxx_std_14)
set_target_properties(${LTQMDD_TARGET_NAME} PROPERTIES CXX_EXTENSIONS OFF)
target_link_libraries(${LTQMDD_TARGET_NAME} PUBLIC ${PROJECT_NAME})
if (COVERAGE)
	target_compile_options(${LTQMDD_TARGET_NAME} PUBLIC --coverage)
	target_link_libraries(${LTQMDD_TARGET_NAME} PUBLIC --coverage)
endif()
if(MSVC)
	target_compile_options(${LTQMDD_TARGET_NAME} PUBLIC "/W4" "$<$<CONFIG:RELEASE>:/O2 /GL>")
	target_link_options(${LTQMDD_TARGET_NAME} PUBLIC "$<$<CONFIG:Release>:/LTCG>")
else()
	target_compile_options(${LTQMDD_TARGET_NAME} PRIVATE -Wall $<$<CONFIG:DEBUG>:-g3 -Og -Wextra -Wpedantic -pedantic> $<$<CONFIG:RELEASE>:-O3 -mtune=native -march=native -g>)
endif()

# add_custom_command(TARGET ${LTQMDD_TARGET_NAME}
                #    POST_BUILD
                #    COMMAND ${CMAKE_COMMAND} -E create_symlink $<TARGET_FILE_DIR:${LTQMDD_TARGET_NAME}>/${LTQMDD_TARGET_NAME} ${CMAKE_BINARY_DIR}/${LTQMDD_TARGET_NAME}
                #    COMMAND ${CMAKE_COMMAND} -E copy_directory ${CMAKE_CURRENT_SOURCE_DIR}/circuits/ $<TARGET_FILE_DIR:${LTQMDD_TARGET_NAME}>/circuits
                #    COMMAND ${CMAKE_COMMAND} -E create_symlink $<TARGET_FILE_DIR:${LTQMDD_TARGET_NAME}>/circuits ${CMAKE_BINARY_DIR}/circuits
                #    COMMENT "Copying circuits and creating symlinks for ${LTQMDD_TARGET_NAME}"
                #    VERBATIM)



add_executable(${PROJECT_NAME}_app ${CMAKE_CURRENT_SOURCE_DIR}/app.cpp)
target_compile_features(${PROJECT_NAME}_app PUBLIC cxx_std_14)
set_target_properties(${PROJECT_NAME}_app PROPERTIES CXX_EXTENSIONS OFF)
target_link_libraries(${PROJECT_NAME}_app PRIVATE ${PROJECT_NAME})
if(MSVC)
	target_compile_options(${PROJECT_NAME}_app PUBLIC "/W4" "$<$<CONFIG:RELEASE>:/O2 /GL>")
	target_link_options(${PROJECT_NAME}_app PUBLIC "$<$<CONFIG:Release>:/LTCG>")
else()
	target_compile_options(${PROJECT_NAME}_app PUBLIC -Wall $<$<CONFIG:DEBUG>:-g3 -Og -Wextra -Wpedantic -pedantic> $<$<CONFIG:RELEASE>:-O3 -mtune=native -march=native -DNDEBUG -g0>)
endif()

add_custom_command(TARGET ${PROJECT_NAME}_app
                   POST_BUILD
                   COMMAND ${CMAKE_COMMAND} -E create_symlink ${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}_app ${CMAKE_BINARY_DIR}/${PROJECT_NAME}_app
                   COMMENT "Creating symlink for ${PROJECT_NAME}_app"
                   VERBATIM)

include(GNUInstallDirs)
install(TARGETS ${PROJECT_NAME}_app
        RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR})
set_target_properties(${PROJECT_NAME}_app PROPERTIES EXPORT_NAME ${PROJECT_NAME}_app)
