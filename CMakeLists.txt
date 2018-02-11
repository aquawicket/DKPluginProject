### DKPluginProject CMakeLists.txt FILE ###
CMAKE_MINIMUM_REQUIRED(VERSION 3.4)

##### CREATE 3rdParty1 #####
##TODO


##### CREATE 3rdParty2 #####
##TODO


##### CREATE Plugin1 #####
PROJECT(Plugin1)
FILE(GLOB Plugin1_SRC 
	C:/digitalknob/DKPluginProject/Plugin1/*.h
	C:/digitalknob/DKPluginProject/Plugin1/*.c
	C:/digitalknob/DKPluginProject/Plugin1/*.cpp)
ADD_LIBRARY(Plugin1 STATIC ${Plugin1_SRC})
SET_TARGET_PROPERTIES(DK PROPERTIES LINKER_LANGUAGE C11)

##### CREATE Plugin2 #####
##TODO


#####  CREATE App #####
PROJECT(App)
FILE(GLOB App_SRC 
	C:/digitalknob/DKPluginProject/App/*.h
	C:/digitalknob/DKPluginProject/App/*.c
	C:/digitalknob/DKPluginProject/App/*.cpp)
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
LIST(APPEND DEBUG_LIBS "")
LIST(APPEND RELEASE_LIBS "")
LIST(APPEND DEBUG_LINK_FLAGS /SUBSYSTEM:CONSOLE)
STRING(REPLACE ";" " " DEBUG_FLAGS "${DEBUG_LINK_FLAGS}")
LIST(APPEND RELEASE_LINK_FLAGS /SUBSYSTEM:CONSOLE)
STRING(REPLACE ";" " " RELEASE_FLAGS "${RELEASE_LINK_FLAGS}")
ADD_EXECUTABLE(App WIN32 ${App_SRC})
TARGET_LINK_LIBRARIES(App ${DEBUG_LIBS} ${RELEASE_LIBS} ${WIN_LIBS})
SET_TARGET_PROPERTIES(App PROPERTIES LINK_FLAGS_DEBUG ${DEBUG_FLAGS} LINK_FLAGS_RELEASE ${RELEASE_FLAGS})
SET_TARGET_PROPERTIES(App PROPERTIES LINKER_LANGUAGE C11)