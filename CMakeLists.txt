cmake_minimum_required(VERSION 3.28)
project(MiniMaya)

# Set CMake Language options
set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_FLAGS_DEBUG "-g")

# Set some user defined variables for better compilation
set(TARGET_SOURCE_PATH ${CMAKE_CURRENT_SOURCE_DIR}/src)
set(TARGET_DEPENDENCY_PATH ${CMAKE_CURRENT_SOURCE_DIR}/dependencies)
set(TARGET_INCLUDE_PATH ${CMAKE_CURRENT_SOURCE_DIR}/include)
set(TARGET_UI_FORMS_PATH ${CMAKE_CURRENT_SOURCE_DIR}/forms)
set(TARGET_SHADER_PROGRAM_PATH ${CMAKE_CURRENT_SOURCE_DIR}/forms)

# Set some CMake Prepend paths
set(CMAKE_PREFIX_PATH "D:/libs/qt/6.2.2/mingw_64") # @TODO change this path accordingly to your project

# Find required dependencies and set package related stuff by CMake
find_package(OpenGL REQUIRED)
find_package(
        Qt6 6.2.2 COMPONENTS Core Widgets Gui OpenGL OpenGLWidgets
        REQUIRED
)


if(NOT Qt6_FOUND)
    message(FATAL_ERROR "QT6 either not installed, or its path is unknown to CMake")
endif()

# Automatically run moc and uic on sources and headers
set(CMAKE_AUTOMOC ON) # automatically
set(CMAKE_AUTORCC ON) # automatically parse the qrc file
set(CMAKE_AUTOUIC ON) # automatically parse the UI form files for QT windows
# below is the search path for the UI files
# this is subject to change for different projects
set (CMAKE_AUTOUIC_SEARCH_PATHS ${TARGET_UI_FORMS_PATH}) # Set the search path for the UI files


# add executables and link libraries, parse the source
file(GLOB_RECURSE TARGET_SOURCES
        ${TARGET_SOURCE_PATH}/*.cc
        ${TARGET_SOURCE_PATH}/*.cpp
        ${TARGET_SOURCE_PATH}/*.c
)

# Statically linking std libraries
if(WIN32)
    set(CMAKE_EXE_LINKER_FLAGS "-static")
endif()

add_executable(MiniMaya ${TARGET_SOURCES})

target_include_directories(MiniMaya PRIVATE
        ${TARGET_SOURCE_PATH}
        ${TARGET_INCLUDE_PATH}
)
target_link_libraries(MiniMaya PRIVATE
        Qt6::Widgets
        Qt6::Gui
        Qt6::OpenGLWidgets
        Qt6::Core
        OpenGL::GL
        OpenGL::GLU
)

