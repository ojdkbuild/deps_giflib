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

# static library
add_subdirectory ( ${CMAKE_CURRENT_LIST_DIR}/resources/giflib_416_cmake ${CMAKE_CURRENT_BINARY_DIR}/lookaside_giflib )

# shared_library
add_library ( ${PROJECT_NAME} SHARED ${CMAKE_CURRENT_LIST_DIR}/resources/${PROJECT_NAME}.def )
set_target_properties ( ${PROJECT_NAME} PROPERTIES LINKER_LANGUAGE C )
target_link_libraries ( ${PROJECT_NAME} lookaside_giflib )

# pkg-config
set ( ${PROJECT_NAME}_MODULE giflib )
set ( ${PROJECT_NAME}_BUNDLE_LIB_PATH ${CMAKE_LIBRARY_OUTPUT_DIRECTORY}/${CMAKE_SHARED_LIBRARY_PREFIX}${PROJECT_NAME}${CMAKE_SHARED_LIBRARY_SUFFIX} )
set ( ${PROJECT_NAME}_PC_CFLAGS "-I${CMAKE_CURRENT_LIST_DIR}/../../lookaside/giflib/lib" )
set ( ${PROJECT_NAME}_PC_LIBS "-L${CMAKE_LIBRARY_OUTPUT_DIRECTORY} -l${PROJECT_NAME}" )
configure_file ( ${CMAKE_CURRENT_LIST_DIR}/../../resources/cmake/pkg-config.in 
        ${CMAKE_LIBRARY_OUTPUT_DIRECTORY}/pkgconfig/giflib.pc )
