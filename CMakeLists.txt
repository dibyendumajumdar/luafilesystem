cmake_minimum_required(VERSION 3.0)
project(luafilesystem)

option ( USE_LUA53 "Controls whether the library will be for Lua 5.3 or Ravi, default is OFF" OFF)

SET(CMAKE_MODULE_PATH
  "${CMAKE_CURRENT_SOURCE_DIR}/cmake"
  "${CMAKE_MODULE_PATH}")

include(${CMAKE_INSTALL_PREFIX}/cmake/lua-config.cmake)

message(STATUS "LUA_INCLUDE_DIR: ${LUA_INCLUDE_DIR}")
message(STATUS "LUA_LIBRARIES  : ${LUA_LIBRARIES}")

message(STATUS "LUA_INCDIR     : ${LUA_INCDIR}")
message(STATUS "LUA_LIBDIR     : ${LUA_LIBDIR}")
message(STATUS "LUA_BINDIR     : ${LUA_BINDIR}")
message(STATUS "LUALIB         : ${LUALIB}")
message(STATUS "LIBDIR (LUA_CPATH)  : ${LIBDIR}")
message(STATUS "LUADIR (LUA_PATH)   : ${LUADIR}")

INCLUDE_DIRECTORIES("${CMAKE_CURRENT_BINARY_DIR}")
INCLUDE_DIRECTORIES("${LUA_INCDIR}")
LINK_DIRECTORIES("${LUA_LIBDIR}")

IF(MSVC)
  ADD_DEFINITIONS(-D_CRT_SECURE_NO_DEPRECATE=1)
ENDIF(MSVC)

add_library(lfs SHARED 
	src/lfs.h 
	src/lfs.c)
set_target_properties(lfs PROPERTIES PREFIX "")
target_link_libraries(lfs ${LUALIB})

INSTALL(TARGETS lfs
  LIBRARY DESTINATION ${LIBDIR}
  RUNTIME DESTINATION ${LUA_BINDIR})
