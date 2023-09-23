# Copyright 2023 alex@staticlibs.net
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

cmake_minimum_required ( VERSION 3.0 )

project ( pg_hint_plan C )

if ( NOT DEFINED ENV{PGWIN_INSTALL_DIR} )
    message( FATAL_ERROR "Environment variable 'PGWIN_INSTALL_DIR' (pointing to the postgresql_modified_for_babelfish installation directory) must be specified" )
endif ()

add_library ( ${PROJECT_NAME} SHARED 
    pg_hint_plan.c
    normalize_query.h
    ${PROJECT_NAME}.def )

target_include_directories ( ${PROJECT_NAME} PRIVATE
    ${CMAKE_CURRENT_LIST_DIR} )

target_include_directories ( ${PROJECT_NAME} PRIVATE
    $ENV{PGWIN_INSTALL_DIR}/include/server/port/win32_msvc 
    $ENV{PGWIN_INSTALL_DIR}/include/server/port/win32
    $ENV{PGWIN_INSTALL_DIR}/include/server
    $ENV{PGWIN_INSTALL_DIR}/include )

target_compile_options ( ${PROJECT_NAME} PRIVATE
    /wd4244
    /wd4267 
    )

target_compile_definitions ( ${PROJECT_NAME} PRIVATE
    _CRT_SECURE_NO_WARNINGS )
 
target_link_libraries ( ${PROJECT_NAME} PRIVATE
    $ENV{PGWIN_INSTALL_DIR}/lib/postgres.lib )

install ( TARGETS ${PROJECT_NAME} RUNTIME
    DESTINATION $ENV{PGWIN_INSTALL_DIR}/lib )

install( FILES
    $<TARGET_PDB_FILE:${PROJECT_NAME}>
    DESTINATION $ENV{PGWIN_INSTALL_DIR}/symbols )

install ( FILES
    ${PROJECT_NAME}--1.3.0.sql
    ${PROJECT_NAME}--1.3.0--1.3.1.sql
    ${PROJECT_NAME}--1.3.1--1.3.2.sql
    ${PROJECT_NAME}--1.3.2--1.3.3.sql
    ${PROJECT_NAME}--1.3.3--1.3.4.sql
    ${PROJECT_NAME}--1.3.5--1.3.6.sql
    ${PROJECT_NAME}--1.3.4--1.3.5.sql
    ${PROJECT_NAME}--1.3.6--1.3.7.sql
    ${PROJECT_NAME}--1.3.7--1.3.8.sql
    ${PROJECT_NAME}--1.3.8--1.3.9.sql
    ${PROJECT_NAME}--1.3.9--1.4.sql
    ${PROJECT_NAME}--1.4--1.4.1.sql
    ${PROJECT_NAME}--1.4.1--1.4.2.sql
    ${PROJECT_NAME}--1.4.2--1.5.sql
    ${PROJECT_NAME}--1.5--1.5.1.sql
    ${PROJECT_NAME}.control
    DESTINATION $ENV{PGWIN_INSTALL_DIR}/share/extension )
