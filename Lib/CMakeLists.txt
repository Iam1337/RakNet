FILE(GLOB ALL_CPP_SRCS 
	${RakNet_SOURCE_DIR}/Source/*.cpp
	${RakNet_SOURCE_DIR}/Source/*.h)

if (MSVC)
	add_definitions("/D WIN32 /D _CRT_NONSTDC_NO_DEPRECATE /D _CRT_SECURE_NO_DEPRECATE /GS- /GR-")
endif (MSVC)

# Create Static Link Library project.
if( RAKNET_STATIC_COMPILE )
	add_library(${RAKNET_STATIC_NAME} STATIC ${ALL_CPP_SRCS})
	target_link_libraries (${RAKNET_STATIC_NAME} ${RAKNET_LIBRARY_LIBS})
	target_compile_definitions(${RAKNET_STATIC_NAME} PUBLIC "_RAKNET_LIB")

	if (MSVC)
		set_target_properties(${RAKNET_STATIC_NAME} PROPERTIES STATIC_LIBRARY_FLAGS "/NODEFAULTLIB:\"LIBCD.lib LIBCMTD.lib MSVCRT.lib\"" )
	endif (MSVC)
endif()

# Create Dynamic Link Library project.
if( RAKNET_DLL_COMPILE )
	add_library(${RAKNET_DLL_NAME} SHARED ${ALL_CPP_SRCS})
	target_link_libraries (${RAKNET_DLL_NAME} ${RAKNET_LIBRARY_LIBS})
	target_compile_definitions(${RAKNET_DLL_NAME} PUBLIC "_RAKNET_DLL")

	#if (MSVC)
	#	target_compile_definitions(${RAKNET_DLL_NAME} PUBLIC "_RAKNET_DLL")
	#else(MSVC)
	#	target_compile_definitions(${RAKNET_DLL_NAME} PUBLIC "-D_RAKNET_DLL")
	#endif (MSVC)
endif()