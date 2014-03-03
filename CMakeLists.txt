# Copyright (c) 2014 Flokart World, Inc.
# All rights reserved.

cmake_minimum_required (VERSION 2.8.12)
project (minizip)
find_package (CMakeSupports REQUIRED)

CMS_PROJECT_VERSION(1.1)
CMS_PROJECT_DESCRIPTION(MiniZip)
CMS_PROJECT_VAR_PREFIX(MINIZIP)

CMS_IMPORT_MODULE(ZLIB NAME ZLib PACKAGE zlib REQUIRED)

CMS_INSTALL_LIBRARY_PACKAGE()
CMS_INSTALL_MODULE(MiniZip)

CMS_BEGIN_LIBRARY_C(minizip)

set (PUBLIC_HEADERS ioapi.h mztools.h unzip.h zip.h)

if (WIN32)
  list (APPEND PUBLIC_HEADERS iowin32.h)
  CMS_ADD_SOURCE_FILES(iowin32.c)
endif ()

CMS_ADD_SOURCE_FILES(crypt.h ioapi.c mztools.c unzip.c zip.c)
CMS_ADD_LIBRARY_HEADERS(${PUBLIC_HEADERS})

if (MSVC)
  CMS_DISABLE_MSVC_WARNINGS(4100 4127 4131 4189 4244)
  CMS_SOURCE_FILES_COMPILE_DEFINITIONS(mztools.c DEFINITIONS
                                       "_CRT_SECURE_NO_WARNINGS")
endif ()

CMS_END_LIBRARY()
