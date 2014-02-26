# Copyright (c) 2014 Flokart World, Inc.
# All rights reserved.

cmake_minimum_required (VERSION 2.8.12)
project (minizip)
find_package (CMakeSupportPrologue REQUIRED)

if (NOT ZLIB_FOUND)
  find_package (ZLib REQUIRED)
endif ()

set (CURRENT_TARGET minizip)
set (PUBLIC_HEADERS ioapi.h mztools.h unzip.h zip.h)
set (INTERNAL_FILES crypt.h ioapi.c mztools.c unzip.c zip.c)

if (WIN32)
  list (APPEND PUBLIC_HEADERS iowin32.h)
  list (APPEND INTERNAL_FILES iowin32.c)
endif ()

include_directories (${ZLIB_INCLUDE_DIRS})

add_library (${CURRENT_TARGET} ${INTERNAL_FILES} ${PUBLIC_HEADERS})
set_target_properties (${CURRENT_TARGET} PROPERTIES
                       OUTPUT_NAME_DEBUG "minizipd")

if (MSVC)
  target_compile_options(${CURRENT_TARGET} PUBLIC
      $<$<CONFIG:MinSizeRel>:/Os>)
  target_compile_options(${CURRENT_TARGET} PUBLIC
      $<$<OR:$<CONFIG:Release>,$<CONFIG:RelWithDebInfo>>:/Oi /Ot>)
  target_compile_options(${CURRENT_TARGET} PUBLIC
      $<$<OR:$<CONFIG:MinSizeRel>,$<CONFIG:Release>>:/Oy>)
  target_compile_options(${CURRENT_TARGET} PUBLIC
      $<$<NOT:$<CONFIG:Debug>>:/GL /GS->)
  target_compile_options(${CURRENT_TARGET} PUBLIC /fp:fast)

  set_source_files_properties (FILES mztools.c PROPERTIES
                               COMPILE_DEFINITIONS "_CRT_SECURE_NO_WARNINGS")
endif ()

CMS_INSTALL_MODULE(MiniZip)
CMS_INSTALL_PACKAGE(minizip)

install (FILES ${PUBLIC_HEADERS} DESTINATION include)
install (TARGETS minizip DESTINATION lib)

set (MINIZIP_INCLUDE_DIR "${CMAKE_CURRENT_SOURCE_DIR}"
     CACHE PATH "" FORCE)
set (MINIZIP_INCLUDE_DIRS ${ZLIB_INCLUDE_DIRS} ${MINIZIP_INCLUDE_DIR}
     CACHE INTERNAL "" FORCE)
mark_as_advanced (MINIZIP_INCLUDE_DIR)
