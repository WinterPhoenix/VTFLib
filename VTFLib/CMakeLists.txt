set(SOURCES
	Error.cpp
	FileReader.cpp
	FileWriter.cpp
	Float16.cpp
	MemoryReader.cpp
	MemoryWriter.cpp
	Proc.cpp
	ProcReader.cpp
	ProcWriter.cpp
	VMTFile.cpp
	VMTGroupNode.cpp
	VMTIntegerNode.cpp
	VMTNode.cpp
	VMTSingleNode.cpp
	VMTStringNode.cpp
	VMTValueNode.cpp
	VMTWrapper.cpp
	VTFFile.cpp
	VTFLib.cpp
	VTFMathlib.cpp
	VTFWrapper.cpp

	# Miniz sources
	../thirdparty/miniz/miniz.c)

set(RESOURCES
	VTFLib.rc)

include_directories(../thirdparty/include ../thirdparty/miniz)

if (VTFLIB_SHARED)
	set(TARGET vtflib)

	add_library(${TARGET} SHARED ${SOURCES} ${RESOURCES})

	# Link against compressonator libs & set runtime lib
	if (UNIX)
		target_link_directories(${TARGET} PUBLIC ../thirdparty/lib/x64)
		target_link_libraries(${TARGET} PUBLIC CMP_Compressonator pthread)
	else()
		target_link_directories(${TARGET} PUBLIC ../thirdparty/lib)
		target_link_libraries(${TARGET} PUBLIC "Compressonator_MT$<$<CONFIG:Debug>:d>")
	endif()

	target_compile_definitions(${TARGET} PRIVATE VTFLIB_EXPORTS)
	target_precompile_headers(${TARGET} PRIVATE "$<$<COMPILE_LANGUAGE:CXX>:${CMAKE_CURRENT_SOURCE_DIR}/stdafx.h>")
	target_include_directories(${TARGET} PUBLIC lib)
endif()

if (VTFLIB_STATIC)
	set(TARGET vtflib_static)

	add_library(${TARGET} STATIC ${SOURCES} ${RESOURCES})
	target_compile_definitions(${TARGET} PUBLIC VTFLIB_STATIC)

	# Link against compressonator libs & set runtime lib
	if (UNIX)
		target_link_directories(${TARGET} PUBLIC ../thirdparty/lib/x64)
		target_link_libraries(${TARGET} PUBLIC CMP_Compressonator pthread)
	else()
		target_link_directories(${TARGET} PUBLIC ../thirdparty/lib)
		target_link_libraries(${TARGET} PUBLIC "Compressonator_MT$<$<CONFIG:Debug>:d>")
	endif()

	target_compile_definitions(${TARGET} PRIVATE VTFLIB_EXPORTS)
	target_precompile_headers(${TARGET} PRIVATE "$<$<COMPILE_LANGUAGE:CXX>:${CMAKE_CURRENT_SOURCE_DIR}/stdafx.h>")
	target_include_directories(${TARGET} PUBLIC lib)
endif()
