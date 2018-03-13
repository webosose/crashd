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

webos_add_compiler_flags(ALL -Wall -funwind-tables -Wno-unused-result)
webos_add_linker_options(ALL --no-undefined)

include(FindPkgConfig)

# Check that we have glib
pkg_check_modules(GLIB2 REQUIRED glib-2.0)
include_directories(${GLIB2_INCLUDE_DIRS})
webos_add_compiler_flags(ALL ${GLIB2_CFLAGS_OTHER})

# Check that we have librdx
pkg_check_modules(LIBRDX REQUIRED rdx)
include_directories(${LIBRDX_INCLUDE_DIRS})
webos_add_compiler_flags(ALL ${LIBRDX_CFLAGS_OTHER})

# Check that we have PmLogLib
pkg_check_modules(PMLOGLIB REQUIRED PmLogLib)
include_directories(${PMLOGLIB_INCLUDE_DIRS})
webos_add_compiler_flags(ALL ${PMLOGLIB_CFLAGS_OTHER})

webos_configure_source_files(SOURCE_FILES crashd.c)

add_executable(crashd ${SOURCE_FILES})
target_link_libraries(crashd ${LIBRDX_LDFLAGS} ${PMLOGLIB_LDFLAGS} ${GLIB2_LDFLAGS})

webos_build_daemon()
