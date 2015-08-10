# Copyright (c) 2014 Flokart World, Inc.
# All rights reserved.

cmake_minimum_required (VERSION 3.0.0)
project (minizip)
find_package (CMakeSupports 0.0.4 REQUIRED)

CMS_BEGIN(Module MiniZip)
  CMS_SET_PROPERTY(Version 0.1)
  CMS_SET_PROPERTY(Compatibility AnyNewerVersion)

  CMS_BEGIN(LibraryC minizip)
    CMS_SET_PROPERTY(Version 1.1)

    CMS_IMPORT_PACKAGE(ZLIB PREFIX ZLIB REQUIRED)

    CMS_ADD_PUBLIC_HEADERS(ioapi.h mztools.h unzip.h zip.h)
    CMS_ADD_SOURCE_FILES(crypt.h ioapi.c mztools.c unzip.c zip.c)

    if (WIN32)
      CMS_ADD_PUBLIC_HEADERS(iowin32.h)
      CMS_ADD_SOURCE_FILES(iowin32.c)
    elseif (APPLE AND DARWIN_MAJOR_VERSION GREATER 9)
      CMS_ADD_DEFINITIONS(PUBLIC USE_FILE32API)
    endif ()

    if (MSVC)
      CMS_DISABLE_MSVC_WARNINGS(4100 4127 4131 4189 4244)
      CMS_APPEND_TO_SOURCE_FILE_PROPERTY(mztools.c
                                         CompileDefinitions
                                         _CRT_SECURE_NO_WARNINGS)
    elseif (CMAKE_CXX_COMPILER_ID MATCHES "Clang")
      foreach (_src unzip.c zip.c)
        CMS_APPEND_TO_SOURCE_FILE_PROPERTY("${_src}" CompileOptions
                                           -Wno-parentheses-equality)
      endforeach ()
    endif ()
  CMS_END()
CMS_END()
