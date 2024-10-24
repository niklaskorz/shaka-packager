# Copyright 2022 Google LLC. All rights reserved.
#
# Use of this source code is governed by a BSD-style
# license that can be found in the LICENSE file or at
# https://developers.google.com/open-source/licenses/bsd

# Root-level CMake build file.

# Minimum CMake version.  This must be in the root level CMakeLists.txt.
cmake_minimum_required(VERSION 3.24)

set(CMAKE_CXX_STANDARD 17)

# These policy settings should be included before the project definition.
include("packager/policies.cmake")

# Project name.  May not contain spaces.  Versioning is managed elsewhere.
project(shaka-packager VERSION "")

# Whether to build a shared libpackager library.  By default, don't.
option(BUILD_SHARED_LIBS "Build libpackager as a shared library" OFF)

# Whether to attempt a static linking of the command line front-end.
# Only supported on Linux and with musl.  This will also cause us to link
# against mimalloc to replace the standard allocator in musl, which is slow.
option(FULLY_STATIC "Attempt fully static linking of all CLI apps" OFF)

# Enable CMake's test infrastructure.
enable_testing()

option(SKIP_INTEGRATION_TESTS "Skip the packager integration tests" OFF)

find_package(absl REQUIRED)
find_package(MbedTLS REQUIRED)
find_package(GTest REQUIRED)
find_package(CURL REQUIRED)
find_package(LibXml2 REQUIRED)
find_package(PNG REQUIRED)
find_package(nlohmann_json REQUIRED)
find_package(Protobuf CONFIG REQUIRED)
find_package(webm REQUIRED)

# Alias to same names as vendored dependencies
add_library(mbedtls ALIAS MbedTLS::mbedtls)
add_library(gmock ALIAS GTest::gmock)
add_library(gtest ALIAS GTest::gtest)
add_library(gtest_main ALIAS GTest::gtest_main)
add_library(libcurl ALIAS CURL::libcurl)
add_library(LibXml2 ALIAS LibXml2::LibXml2)
add_library(png_static ALIAS PNG::PNG) # not static but the expected library name
add_library(libprotobuf ALIAS protobuf::libprotobuf)
add_executable(protoc ALIAS protobuf::protoc)
add_library(webm ALIAS webm::webm)

# Subdirectories with their own CMakeLists.txt
add_subdirectory(packager)
add_subdirectory(link-test)
