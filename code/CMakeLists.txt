cmake_minimum_required(VERSION 3.5.0)
project(code VERSION 0.1.0 LANGUAGES C CXX)

# Set the C++ standard to use
set(CMAKE_CXX_STANDARD 17)

# Set the output directory for executables
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/out)

# Function to create an executable for each project
function(create_project_executable project_dir)
    file(GLOB_RECURSE src_files ${project_dir}/*.cpp ${project_dir}/*.h)
    get_filename_component(project_name ${project_dir} NAME)
    add_executable(${project_name} ${src_files})
    # Set the output directory for the current project's executable
    set_target_properties(${project_name} PROPERTIES
        RUNTIME_OUTPUT_DIRECTORY ${CMAKE_RUNTIME_OUTPUT_DIRECTORY}
    )
endfunction()

# Iterate through subdirectories and create an executable for each one with a main.cpp file
file(GLOB children RELATIVE ${CMAKE_SOURCE_DIR} ${CMAKE_SOURCE_DIR}/*)
foreach (child ${children})
    if (IS_DIRECTORY ${CMAKE_SOURCE_DIR}/${child})
        file(GLOB main_cpp_files ${CMAKE_SOURCE_DIR}/${child}/main.cpp)
        if (main_cpp_files)
            create_project_executable(${CMAKE_SOURCE_DIR}/${child})
        endif ()
    endif ()
endforeach ()