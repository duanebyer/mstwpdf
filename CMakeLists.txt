cmake_minimum_required(VERSION 3.1...3.15)
if(${CMAKE_VERSION} VERSION_LESS 3.12)
	cmake_policy(VERSION ${CMAKE_MAJOR_VERSION}.${CMAKE_MINOR_VERSION})
endif()

project(
	MSTWPDF	
	VERSION 0.0.1
	DESCRIPTION "Library for calculation of parton distribution functions."
	HOMEPAGE_URL "https://mstwpdf.hepforge.org/"
	LANGUAGES CXX Fortran)

# MSTWPDF library.
add_library(
	mstwpdf
	mstwpdf.cc
	alphaS.f
	mstwpdf.h)
add_library(MSTWPDF::mstwpdf ALIAS mstwpdf)
target_include_directories(mstwpdf PUBLIC ".")
target_compile_features(mstwpdf PUBLIC cxx_std_11)
set_target_properties(mstwpdf PROPERTIES CXX_EXTENSIONS OFF)
target_compile_options(
	mstwpdf
	PRIVATE
	$<$<OR:$<CXX_COMPILER_ID:AppleClang>,$<CXX_COMPILER_ID:Clang>,$<CXX_COMPILER_ID:GNU>>:-Wall -Wextra -pedantic>
	$<$<CXX_COMPILER_ID:MSVC>:/W4>)

# Example program.
add_executable(example example.cc)
target_link_libraries(example PRIVATE mstwpdf)
target_compile_features(example PRIVATE cxx_std_11)
set_target_properties(example PROPERTIES CXX_EXTENSIONS OFF)
target_compile_options(
	example
	PRIVATE
	$<$<OR:$<CXX_COMPILER_ID:AppleClang>,$<CXX_COMPILER_ID:Clang>,$<CXX_COMPILER_ID:GNU>>:-Wall -Wextra -pedantic>
	$<$<CXX_COMPILER_ID:MSVC>:/W4>)

# Installation commands.
include(GNUInstallDirs)
set_target_properties(
	mstwpdf PROPERTIES
	INSTALL_RPATH "${CMAKE_INSTALL_PREFIX}/${CMAKE_INSTALL_LIBDIR}"
	INSTALL_RPATH_USE_LINK_PATH TRUE)
install(
	FILES "${MSTWPDF_SOURCE_DIR}/mstwpdf.h"
	DESTINATION "${CMAKE_INSTALL_INCLUDEDIR}/mstwpdf")
install(
	TARGETS mstwpdf
	LIBRARY DESTINATION "${CMAKE_INSTALL_LIBDIR}"
	ARCHIVE DESTINATION "${CMAKE_INSTALL_LIBDIR}"
	INCLUDES DESTINATION "${CMAKE_INSTALL_INCLUDEDIR}")

