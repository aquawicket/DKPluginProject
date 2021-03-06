### DKPluginProject CMakeLists.txt FILE ###
CMAKE_MINIMUM_REQUIRED(VERSION 3.4)
INCLUDE(${CMAKE_SOURCE_DIR}/../DKCMake/FUNCTIONS.cmake)
DKSET(RELEASE ON)
DKSET(REBUILDALL ON)
INCLUDE(${CMAKE_SOURCE_DIR}/../DKCMake/OPTIONS.cmake)


##### CREATE 3rdParty #####
DKSET(ZLIB ${CMAKE_SOURCE_DIR}/3rdParty/zlib-1.2.11)
IF(NOT EXISTS "${ZLIB}")
	DKSET(CURRENT_DIR ${DIGITALKNOB}/Download)
	DKDOWNLOAD(http://digitalknob.com/Download/Libs/zlib1211.zip)
	DKEXTRACT(${DIGITALKNOB}/Download/zlib1211.zip ${CMAKE_SOURCE_DIR}/3rdParty)
ENDIF()
IF(NOT EXISTS "${ZLIB}/win32/Release")
	DKSETPATH(${ZLIB}/${OS})
	DKSET(QUEUE_BUILD ON)
	WIN32_COMMAND(${CMAKE_COMMAND} -G "Visual Studio 14 2015" "-DCMAKE_C_FLAGS=/DTEST /DWIN32 /D_WINDOWS /W3 /nologo" "-DCMAKE_C_FLAGS_DEBUG=/MTd /Od /Ob0 /Zi /RTC1 /DDEBUG /D_DEBUG" "-DCMAKE_C_FLAGS_RELEASE=/MT /O2 /Ob2 /DNDEBUG" ${ZLIB})
	WIN32_VS_DEBUG(zlib-1.2.11 zlib.sln zlib)
	WIN32_VS_RELEASE(zlib-1.2.11 zlib.sln zlib)
ENDIF()



##### CREATE Plugin1 #####
PROJECT(Plugin1)
FILE(GLOB Plugin1_SRC
	${CMAKE_SOURCE_DIR}/Plugin1/*.h
	${CMAKE_SOURCE_DIR}/Plugin1/*.h
	${CMAKE_SOURCE_DIR}/Plugin1/*.c
	${CMAKE_SOURCE_DIR}/Plugin1/*.cpp)
ADD_LIBRARY(Plugin1 STATIC ${Plugin1_SRC})
SET_TARGET_PROPERTIES(Plugin1 PROPERTIES LINKER_LANGUAGE CXX)


##### CREATE Plugin2 #####
PROJECT(Plugin2)
FILE(GLOB Plugin2_SRC 
	${CMAKE_SOURCE_DIR}/Plugin2/*.h
	${CMAKE_SOURCE_DIR}/Plugin2/*.c
	${CMAKE_SOURCE_DIR}/Plugin2/*.cpp)
ADD_LIBRARY(Plugin2 STATIC ${Plugin2_SRC})
SET_TARGET_PROPERTIES(Plugin2 PROPERTIES LINKER_LANGUAGE CXX)


#####  CREATE App #####
PROJECT(App)
FILE(GLOB App_SRC 
	${CMAKE_SOURCE_DIR}/App/*.h
	${CMAKE_SOURCE_DIR}/App/*.c
	${CMAKE_SOURCE_DIR}/App/*.cpp)

LIST(APPEND DEBUG_LIBS "")
LIST(APPEND RELEASE_LIBS "")
IF(WIN32)
	LIST(APPEND DEBUG_LINK_FLAGS /SUBSYSTEM:CONSOLE)
	LIST(APPEND RELEASE_LINK_FLAGS /SUBSYSTEM:CONSOLE)
	LIST(APPEND WIN_LIBS 
		kernel32.lib
		user32.lib
		gdi32.lib
		winspool.lib
		shell32.lib
		ole32.lib
		oleaut32.lib
		uuid.lib
		comdlg32.lib
		advapi32.lib
		odbc32.lib
		odbccp32.lib
		opengl32.lib
		DbgHelp.lib
	)
ENDIF()
STRING(REPLACE ";" " " DEBUG_FLAGS "${DEBUG_LINK_FLAGS}")
STRING(REPLACE ";" " " RELEASE_FLAGS "${RELEASE_LINK_FLAGS}")

ADD_EXECUTABLE(App WIN32 ${App_SRC})
TARGET_LINK_LIBRARIES(App ${DEBUG_LIBS} ${RELEASE_LIBS} ${WIN_LIBS})
IF(WIN32)
	SET_TARGET_PROPERTIES(App PROPERTIES LINK_FLAGS_DEBUG ${DEBUG_FLAGS} LINK_FLAGS_RELEASE ${RELEASE_FLAGS})
ENDIF()
SET_TARGET_PROPERTIES(App PROPERTIES LINKER_LANGUAGE CXX)