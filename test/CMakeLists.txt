# 包含 CUDA GTest glog
find_package(GTest REQUIRED)
include(../cmake/FindGlog.cmake)

set(link_ext_lib ${GLOG_LIBRARIES} GTest::gtest)
# 在../test目录中查找所有的.c 和.cpp文件，将这些文件的完整路径存储到 DIR_TEST变量中
aux_source_directory(../test DIR_TEST)

add_executable(test ${DIR_TEST})

set(CMAKE_CUDA_FLAGS "${CMAKE_CUDA_FLAGS} -g -G")
target_link_libraries(test ${link_ext_lib})
target_include_directories(test PUBLIC ${glog_INCLUDE_DIR})
target_include_directories(test PUBLIC ${GTest_INCLUDE_DIR})

target_include_directories(test PUBLIC ../include)

set_target_properties(test PROPERTIES WORKING_DIRECTORY ${CMAKE_SOURCE_DIR})


