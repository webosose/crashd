# Copyright (c) 2013-2018 LG Electronics, Inc.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
# http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
#
# SPDX-License-Identifier: Apache-2.0

webos_add_compiler_flags(ALL -fPIC -Wall -funwind-tables -Wno-unused-result)
webos_add_linker_options(ALL -shared --no-undefined)

webos_configure_source_files(SOURCE_FILES libSegFault.c)
add_library(libSegFault SHARED ${SOURCE_FILES})

find_library(UNWIND NAMES unwind)
if(UNWIND STREQUAL "UNWIND-NOTFOUND")
	message(FATAL_ERROR "Failed to find UNWIND libraries. Please install.")
endif()

if(CMAKE_SYSTEM_PROCESSOR MATCHES "^arm")
	find_library(UNWIND_ARCH NAMES unwind-arm)
elseif(CMAKE_SYSTEM_PROCESSOR MATCHES "^aarch64")
	find_library(UNWIND_ARCH NAMES unwind-aarch64)
elseif(CMAKE_SYSTEM_PROCESSOR MATCHES "(i[3-6]86|^x86$)")
	find_library(UNWIND_ARCH NAMES unwind-x86)
elseif(CMAKE_SYSTEM_PROCESSOR MATCHES "(x86_64|amd64)")
	find_library(UNWIND_ARCH NAMES unwind-x86_64)
else()
	message(FATAL_ERROR "Unknown CPU '${CMAKE_SYSTEM_PROCESSOR}'")
endif()
if(UNWIND_ARCH STREQUAL "UNWIND_ARCH-NOTFOUND")
	message(FATAL_ERROR "Failed to find UNWIND_ARCH libraries. Please install.")
endif()

target_link_libraries(libSegFault ${UNWIND} ${UNWIND_ARCH})
webos_build_library(NAME libSegFault NOHEADERS)
