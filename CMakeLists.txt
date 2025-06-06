cmake_minimum_required(VERSION 3.5)
project(predicates)

if(NOT CMAKE_VERSION VERSION_LESS "3.3")
	# https://cmake.org/cmake/help/latest/policy/CMP0063.html
	cmake_policy(SET CMP0063 NEW)
else()
	message(WARNING "CMake version is < 3.3, visibility properties for static libraries will be ignored.")
endif()

add_library(${PROJECT_NAME} predicates.c predicates.h)

include(GNUInstallDirs)
target_include_directories(${PROJECT_NAME} PUBLIC
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>
    $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>
)

# Generate position independent code
set_target_properties(${PROJECT_NAME} PROPERTIES POSITION_INDEPENDENT_CODE ON)

# Export interface symbols and hide internal implementation
include(GenerateExportHeader)
generate_export_header(${PROJECT_NAME})
target_include_directories(${PROJECT_NAME} PUBLIC 
	$<BUILD_INTERFACE:${PROJECT_BINARY_DIR}>
    $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>
)
set_target_properties(${PROJECT_NAME} PROPERTIES CXX_VISIBILITY_PRESET hidden)
