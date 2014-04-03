# Copyright (c) 2014 Flokart World, Inc.
# All rights reserved.

cmake_minimum_required (VERSION 2.8.12)
project (minizip)
find_package (CMakeSupports 0.0.3 REQUIRED)

CMS_PROJECT_VERSION(1.1)
CMS_PROJECT_DESCRIPTION(MiniZip)
CMS_PROJECT_VAR_PREFIX(MINIZIP)

CMS_IMPORT_MODULE(ZLIB NAME ZLib REQUIRED)

CMS_BEGIN_LIBRARY_C(minizip)
  set (PUBLIC_HEADERS ioapi.h mztools.h unzip.h zip.h)

  if (WIN32)
    list (APPEND PUBLIC_HEADERS iowin32.h)
    CMS_ADD_SOURCE_FILES(iowin32.c)
  elseif (APPLE AND DARWIN_MAJOR_VERSION GREATER 9)
    CMS_ADD_DEFINITIONS(USE_FILE32API)
  endif ()

  CMS_ADD_SOURCE_FILES(crypt.h ioapi.c mztools.c unzip.c zip.c)
  CMS_LIBRARY_HEADERS(${PUBLIC_HEADERS})

  if (MSVC)
    CMS_DISABLE_MSVC_WARNINGS(4100 4127 4131 4189 4244)
    CMS_SOURCE_FILES_COMPILE_DEFINITIONS(mztools.c DEFINITIONS
                                         "_CRT_SECURE_NO_WARNINGS")
  endif ()
CMS_END_LIBRARY()

CMS_INSTALL_PACKAGE(MiniZip)
