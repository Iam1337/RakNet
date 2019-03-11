cmake_minimum_required(VERSION 3.14)

project(RakNet)
set(CMAKE_CXX_STANDARD 11)

#idk
if( NOT APPLE )
# check 64 bit
	if( CMAKE_SIZEOF_VOID_P MATCHES "4" )
		set( HAVE_64_BIT 0 )
	else(  CMAKE_SIZEOF_VOID_P MATCHES "4")
		set( HAVE_64_BIT 1 )
	endif( CMAKE_SIZEOF_VOID_P MATCHES "4")
endif( NOT APPLE )

#idk
if (WIN32 AND NOT UNIX)
	set (PROGRAMFILESX86 $ENV{PROGRAMFILES})
	string(REPLACE "\\" "/" PROGRAMFILESX86 ${PROGRAMFILESX86})
endif(WIN32 AND NOT UNIX)

# Sock library.
if (WIN32 AND NOT UNIX)
	set(RAKNET_LIBRARY_LIBS ws2_32.lib)
else (WIN32 AND NOT UNIX)
	set(RAKNET_LIBRARY_LIBS pthread)
endif (WIN32 AND NOT UNIX)

# Debug postfix.
set(CMAKE_DEBUG_POSTFIX "_d")

# Options
# option( RAKNET_ENABLE_SAMPLES 	"Generate RakNet sample projects if true." 		TRUE )

# DLL OPTIONS
option( RAKNET_DLL_COMPILE		"Generate the Dynamic Link Library project."		TRUE )
set( RAKNET_DLL_NAME "RakNetDLL" CACHE STRING "Dynamic Link Library project name.")
#set( RAKNET_DLL_NAME "RakNetDLL")

# STATIC OPTIONS
option( RAKNET_STATIC_COMPILE	"Generate the Static Link Library project."			TRUE )
set( RAKNET_STATIC_NAME "RakNetLibStatic" CACHE STRING "Static Link Library project name.")
#set( RAKNET_STATIC_NAME	"RakNetLibStatic")

#option( RAKNET_GENERATE_INCLUDE_ONLY_DIR "Setup a include/RakNet/ directory in which all the headers are copied." TRUE )

set( RAKNETHEADERFILES ${RakNet_SOURCE_DIR}/Source ) #This name doesn't follow CMake conventions but for retro compatibility I'll let it there.

#if( RAKNET_GENERATE_INCLUDE_ONLY_DIR )
#	set( RAKNET_INCLUDE_ONLY_DIR ${RakNet_SOURCE_DIR}/include ) # this will be visible by client code
#	set( RAKNET_NAMED_INCLUDE_ONLY_DIR ${RAKNET_INCLUDE_ONLY_DIR}/RakNet )
#	message( STATUS "Setting up the ${RAKNET_NAMED_INCLUDE_ONLY_DIR} directory..." ) # Now setup the include/RakNet/*.h files.
#	file( MAKE_DIRECTORY ${RAKNET_NAMED_INCLUDE_ONLY_DIR} )
#	file( COPY ${RAKNETHEADERFILES}/ DESTINATION ${RAKNET_NAMED_INCLUDE_ONLY_DIR} FILES_MATCHING PATTERN "*.h"  )
#	message( STATUS "DONE: Setting up the ${RAKNET_NAMED_INCLUDE_ONLY_DIR} directory." )
#endif()

set( RAKNET_INCLUDE_DIRS ${RAKNETHEADERFILES} ${RAKNET_INCLUDE_ONLY_DIR} ) # Visible from outside

include(./CmakeIncludes/CmakeMacros.txt)

if (MSVC)
	FIXLINKOPTIONS()
	FIXCOMPILEOPTIONS()
endif (MSVC)

include_directories(SYSTEM include/RakNet DependentExtensions)
add_subdirectory(lib)


set(RAKNET_COMMON_LIBS ${RAKNET_STATIC_NAME})

#disabled
#if( RAKNET_ENABLE_SAMPLES )
#	add_subdirectory(Samples)
#endif()