project(RakNetDLL)

add_library(RakNetDLL SHARED ${ALL_CPP_SRCS})

IF (MSVC)
	add_definitions("/D _RAKNET_DLL")
ENDIF (MSVC)

target_link_libraries (RakNetDLL ${RAKNET_LIBRARY_LIBS})

IF (UNIX)
	INSTALL(TARGETS RakNetDLL DESTINATION ${RakNet_SOURCE_DIR}/lib/DLL)
ENDIF (UNIX)