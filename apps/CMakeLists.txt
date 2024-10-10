set(LTQMDD_TARGET_NAME "ltqmddv1")
add_executable(${LTQMDD_TARGET_NAME}-mix ${CMAKE_CURRENT_SOURCE_DIR}/main-mix.cpp)
target_compile_features(${LTQMDD_TARGET_NAME}-mix PUBLIC cxx_std_14)
set_target_properties(${LTQMDD_TARGET_NAME}-mix PROPERTIES CXX_EXTENSIONS OFF)
target_link_libraries(${LTQMDD_TARGET_NAME}-mix PUBLIC ${PROJECT_NAME})
if (COVERAGE)
	target_compile_options(${LTQMDD_TARGET_NAME}-mix PUBLIC --coverage)
	target_link_libraries(${LTQMDD_TARGET_NAME}-mix PUBLIC --coverage)
endif()
if(MSVC)
	target_compile_options(${LTQMDD_TARGET_NAME}-mix PUBLIC "/W4" "$<$<CONFIG:RELEASE>:/O2 /GL>")
	target_link_options(${LTQMDD_TARGET_NAME}-mix PUBLIC "$<$<CONFIG:Release>:/LTCG>")
else()
	target_compile_options(${LTQMDD_TARGET_NAME}-mix PRIVATE 
	-Wall $<$<CONFIG:DEBUG>:-g3 -Og -Wextra -Wpedantic -pedantic> 
	$<$<CONFIG:RELEASE>:-O3 -mtune=native -march=native -g>)
endif()

add_executable(${LTQMDD_TARGET_NAME}-original-sifting ${CMAKE_CURRENT_SOURCE_DIR}/main-original.cpp)
target_compile_features(${LTQMDD_TARGET_NAME}-original-sifting PUBLIC cxx_std_14)
set_target_properties(${LTQMDD_TARGET_NAME}-original-sifting PROPERTIES CXX_EXTENSIONS OFF)
target_link_libraries(${LTQMDD_TARGET_NAME}-original-sifting PUBLIC ${PROJECT_NAME})
if (COVERAGE)
	target_compile_options(${LTQMDD_TARGET_NAME}-original-sifting PUBLIC --coverage)
	target_link_libraries(${LTQMDD_TARGET_NAME}-original-sifting PUBLIC --coverage)
endif()
if(MSVC)
	target_compile_options(${LTQMDD_TARGET_NAME}-original-sifting PUBLIC "/W4" "$<$<CONFIG:RELEASE>:/O2 /GL>")
	target_link_options(${LTQMDD_TARGET_NAME}-original-sifting PUBLIC "$<$<CONFIG:Release>:/LTCG>")
else()
	target_compile_options(${LTQMDD_TARGET_NAME}-original-sifting PRIVATE 
	-Wall $<$<CONFIG:DEBUG>:-g3 -Og -Wextra -Wpedantic -pedantic> 
	$<$<CONFIG:RELEASE>:-O3 -mtune=native -march=native -g>)
endif()


add_executable(${LTQMDD_TARGET_NAME}-lower ${CMAKE_CURRENT_SOURCE_DIR}/main-lower.cpp)
target_compile_features(${LTQMDD_TARGET_NAME}-lower PUBLIC cxx_std_14)
set_target_properties(${LTQMDD_TARGET_NAME}-lower PROPERTIES CXX_EXTENSIONS OFF)
target_link_libraries(${LTQMDD_TARGET_NAME}-lower PUBLIC ${PROJECT_NAME})
if (COVERAGE)
	target_compile_options(${LTQMDD_TARGET_NAME}-lower PUBLIC --coverage)
	target_link_libraries(${LTQMDD_TARGET_NAME}-lower PUBLIC --coverage)
endif()
if(MSVC)
	target_compile_options(${LTQMDD_TARGET_NAME}-lower PUBLIC "/W4" "$<$<CONFIG:RELEASE>:/O2 /GL>")
	target_link_options(${LTQMDD_TARGET_NAME}-lower PUBLIC "$<$<CONFIG:Release>:/LTCG>")
else()
	target_compile_options(${LTQMDD_TARGET_NAME}-lower PRIVATE 
	-Wall $<$<CONFIG:DEBUG>:-g3 -Og -Wextra -Wpedantic -pedantic> 
	$<$<CONFIG:RELEASE>:-O3 -mtune=native -march=native -g>)
endif()

add_executable(${LTQMDD_TARGET_NAME}-upper ${CMAKE_CURRENT_SOURCE_DIR}/main-upper.cpp)
target_compile_features(${LTQMDD_TARGET_NAME}-upper PUBLIC cxx_std_14)
set_target_properties(${LTQMDD_TARGET_NAME}-upper PROPERTIES CXX_EXTENSIONS OFF)
target_link_libraries(${LTQMDD_TARGET_NAME}-upper PUBLIC ${PROJECT_NAME})
if (COVERAGE)
	target_compile_options(${LTQMDD_TARGET_NAME}-upper PUBLIC --coverage)
	target_link_libraries(${LTQMDD_TARGET_NAME}-upper PUBLIC --coverage)
endif()
if(MSVC)
	target_compile_options(${LTQMDD_TARGET_NAME}-upper PUBLIC "/W4" "$<$<CONFIG:RELEASE>:/O2 /GL>")
	target_link_options(${LTQMDD_TARGET_NAME}-upper PUBLIC "$<$<CONFIG:Release>:/LTCG>")
else()
	target_compile_options(${LTQMDD_TARGET_NAME}-upper PRIVATE 
	-Wall $<$<CONFIG:DEBUG>:-g3 -Og -Wextra -Wpedantic -pedantic> 
	$<$<CONFIG:RELEASE>:-O3 -mtune=native -march=native -g>)
endif()


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
