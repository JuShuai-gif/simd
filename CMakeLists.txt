cmake_minimum_required(VERSION 3.16)

set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

# 设置CUDA编译器路径
# set(CUDA_TOOLKIT_ROOT_DIR "/usr/local/cuda-12.4")
# set(CMAKE_CUDA_COMPILER "/usr/local/cuda-12.4/bin/nvcc")

# 设置可执行文件的路径
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_SOURCE_DIR}/bin)

# 设置项目编译的语言类型
project(cmake_templae CXX CUDA)

# 包含CUDA glog
include(cmake/cuda.cmake)
include(cmake/FindGlog.cmake)

# 设置编译器版本
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CUDA_STANDARD_REQUIRED ON)
set(CMAKE_CUDA_STANDARD 14)

# 单元测试
find_package(GTest REQUIRED)

# 包含源文件
aux_source_directory(${CMAKE_SOURCE_DIR}/src DIR_BASE)

# 设置静态链接库的位置
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${PROJECT_SOURCE_DIR}/lib)

# 生成链接库
add_library(llama SHARED ${DIR_BASE})
target_link_libraries(llama sentencepiece  ${GLOG_LIBRARIES} gtest gtest_main pthread)

target_include_directories(llama PUBLIC ${glog_INCLUDE_DIR})
target_include_directories(llama PUBLIC ${PROJECT_SOURCE_DIR}/include)

set_target_properties(llama PROPERTIES CUDA_SEPARABLE_COMPILATION ON)

add_subdirectory(test)