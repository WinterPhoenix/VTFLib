set(TARGET vtfcmd)

set(SOURCES
	vtfcmd.cpp)
set(RESOURCES
	VTFCmd.rc)

include_directories(../thirdparty/include)

if(UNIX)
	add_executable(${TARGET} ${SOURCES})
	target_link_directories(${TARGET} PUBLIC ../thirdparty/lib/x64)
else()
	add_executable(${TARGET} WIN32 ${SOURCES} ${RESOURCES})
	target_link_directories(${TARGET} PUBLIC ../thirdparty/lib)
endif()

target_precompile_headers(${TARGET} PRIVATE "$<$<COMPILE_LANGUAGE:CXX>:${CMAKE_CURRENT_SOURCE_DIR}/stdafx.h>")

#add_dependencies(${TARGET} vtflib)
target_compile_definitions(${TARGET} PRIVATE -D VTFLIB_EXPORTS)
target_link_libraries(${TARGET} vtflib)
