
set(D4D_ROOT "${CMAKE_CURRENT_SOURCE_DIR}/../../../2015 sapphire_ws/D4DWand") 
include("${D4D_ROOT}/1stParty/Cmake/D4DCommonConfig.cmake")

project (yaml-cpp-pm VERSION 0.3.0)
include(CheckCXXCompilerFlag)


###
### Project options
###
## Project stuff
set(YAML_CPP_BUILD_TOOLS  OFF)
set(YAML_CPP_BUILD_CONTRIB  OFF)

## Build options
# --> General
# see http://www.cmake.org/cmake/help/cmake2.6docs.html#variable:BUILD_SHARED_LIBS
#     http://www.cmake.org/cmake/help/cmake2.6docs.html#command:add_library
set(BUILD_SHARED_LIBS OFF)

# --> Apple
option(APPLE_UNIVERSAL_BIN "Apple: Build universal binary" OFF)


###
### Sources, headers, directories and libs
###
set(header_directory "include/yaml-cpp-pm/")

file(GLOB sources "src/[a-zA-Z]*.cpp")
file(GLOB public_headers "include/yaml-cpp-pm/[a-zA-Z]*.h")
file(GLOB private_headers "src/[a-zA-Z]*.h")

add_definitions(-DYAML_CPP_NO_CONTRIB)


###
### General compilation settings
###
if(BUILD_SHARED_LIBS)
	set(LABEL_SUFFIX "shared")
else()
	set(LABEL_SUFFIX "static")
endif()

if(APPLE)
	if(APPLE_UNIVERSAL_BIN)
		set(CMAKE_OSX_ARCHITECTURES ppc;i386)
	endif()
endif()





###
### Library
###
add_library(${PROJECT_NAME}
	${sources}
	${public_headers}
	${private_headers}
	${contrib_sources}
	${contrib_public_headers}
	${contrib_private_headers}
)

target_include_directories(${PROJECT_NAME}
				PRIVATE
			${CMAKE_CURRENT_SOURCE_DIR}/include  
			   ${CMAKE_CURRENT_SOURCE_DIR}/src)


install(TARGETS ${PROJECT_NAME} EXPORT ${PROJECT_NAME}Targets
    LIBRARY DESTINATION lib/\@CMAKE_INSTALL_CONFIG_NAME@ COMPONENT Devel
    ARCHIVE DESTINATION lib/\@CMAKE_INSTALL_CONFIG_NAME@ COMPONENT Devel
    RUNTIME DESTINATION bin/\@CMAKE_INSTALL_CONFIG_NAME@ COMPONENT Devel
    PUBLIC_HEADER DESTINATION include/${PROJECT_NAME} COMPONENT Devel #since it is a static lib, it is only installed as a develepment component
    INCLUDES DESTINATION include/${PROJECT_NAME}
)

#install(TARGETS ${PROJECT_NAME}  EXPORT  ${PROJECT_NAME}Targets 
#		ARCHIVE DESTINATION ${INSTALL_LIB_DIR}
#		INCLUDES DESTINATION include/${PROJECT_NAME})

include(D4DCreateProjectConfig)
D4D_create_project_config()




