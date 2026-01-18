set(CMAKE_EXPORT_COMPILE_COMMANDS ON)
cmake_minimum_required(VERSION 3.18)
project(hpc_project LANGUAGES C CXX CUDA)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CUDA_STANDARD 17)

find_package(MPI REQUIRED)
find_package(OpenMP)

add_executable(hpc_app
    src/main.cpp
)

target_link_libraries(hpc_app MPI::MPI_CXX)

if(OpenMP_CXX_FOUND)
    target_link_libraries(hpc_app OpenMP::OpenMP_CXX)
endif()

set_target_properties(hpc_app PROPERTIES
    CUDA_SEPARABLE_COMPILATION ON
)

target_compile_options(hpc_app PRIVATE -O3)
