cmake_minimum_required(VERSION 2.8)
project(vinbero-ifaces C)
set(CMAKE_BUILD_TYPE RelWithDebInfo)
enable_testing()

if(CMAKE_VERSION VERSION_LESS "3.1")
  if(CMAKE_C_COMPILER_ID STREQUAL "GNU")
    set (CMAKE_C_FLAGS "-std=gnu11 ${CMAKE_C_FLAGS}")
  endif()
else()
    set(CMAKE_C_STANDARD 11)
endif()

execute_process(COMMAND sh -c "git branch -a | grep remotes/origin/master | wc -l" ERROR_QUIET OUTPUT_VARIABLE master_branch_exists)
if(master_branch_exists EQUAL 1)
  message("** Branch origin/master exists, enable automatic AUTHORS generation")
  execute_process(COMMAND git shortlog -nse --no-merges origin/master OUTPUT_FILE "${CMAKE_SOURCE_DIR}/AUTHORS")
  add_custom_target(authors ALL git shortlog -nse --no-merges origin/master > "${CMAKE_SOURCE_DIR}/AUTHORS")
endif()

add_subdirectory(src)
#enable_testing()
#add_subdirectory(test)
