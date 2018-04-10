# Copyright 2016, akashche at redhat.com
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

cmake_minimum_required ( VERSION 2.8.12 )

# project
include ( ${CMAKE_CURRENT_LIST_DIR}/../../resources/cmake/ojdkbuild_common.cmake )
project ( ojdkbuild_giflib C CXX )
set ( ${PROJECT_NAME}_OJDKBUILD_VERSION 4.1.6-1 )

configure_file ( ${OJDKBUILD_DIR}/resources/buildres/deplib.rc
        ${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}.rc )

# static library
ojdkbuild_add_external ( ${OJDKBUILD_DIR}/external/giflib )

# shared library
add_library ( ${PROJECT_NAME} SHARED
        ${CMAKE_CURRENT_LIST_DIR}/resources/${PROJECT_NAME}.def
        ${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}.rc )
set_target_properties ( ${PROJECT_NAME} PROPERTIES LINKER_LANGUAGE C )
target_link_libraries ( ${PROJECT_NAME} gif )
set_property ( TARGET ${PROJECT_NAME} APPEND_STRING PROPERTY LINK_FLAGS "/manifest:no" )

# pkg-config
set ( ${PROJECT_NAME}_MODULE ojdkbuild_giflib )
set ( ${PROJECT_NAME}_BUNDLE_LIB_PATH ${CMAKE_LIBRARY_OUTPUT_DIRECTORY}/${CMAKE_SHARED_LIBRARY_PREFIX}${PROJECT_NAME}${CMAKE_SHARED_LIBRARY_SUFFIX} )
set ( ${PROJECT_NAME}_PC_CFLAGS "-I${gif_LOOKASIDE_DIR}/lib" )
set ( ${PROJECT_NAME}_PC_LIBS "-L${CMAKE_LIBRARY_OUTPUT_DIRECTORY} -l${PROJECT_NAME}" )
configure_file ( ${CMAKE_CURRENT_LIST_DIR}/../../resources/cmake/pkg-config.in 
        ${CMAKE_LIBRARY_OUTPUT_DIRECTORY}/pkgconfig/ojdkbuild_giflib.pc )
