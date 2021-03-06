#
#   Copyright 2013 Pixar
#
#   Licensed under the Apache License, Version 2.0 (the "Apache License")
#   with the following modification; you may not use this file except in
#   compliance with the Apache License and the following modification to it:
#   Section 6. Trademarks. is deleted and replaced with:
#
#   6. Trademarks. This License does not grant permission to use the trade
#      names, trademarks, service marks, or product names of the Licensor
#      and its affiliates, except as required to comply with Section 4(c) of
#      the License and to reproduce the content of the NOTICE file.
#
#   You may obtain a copy of the Apache License at
#
#       http://www.apache.org/licenses/LICENSE-2.0
#
#   Unless required by applicable law or agreed to in writing, software
#   distributed under the Apache License with the above modification is
#   distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
#   KIND, either express or implied. See the Apache License for the specific
#   language governing permissions and limitations under the Apache License.
#

# *** dxViewer ***

set(SHADER_FILES
     shader.hlsl
)

set(PLATFORM_LIBRARIES
    "${OSD_LINK_TARGET}"
    "${DXSDK_LIBRARIES}"
    "${PTEX_LIBRARY}"
)

include_directories(
    "${OPENSUBDIV_INCLUDE_DIR}"
    "${DXSDK_INCLUDE_DIR}"
    "${PTEX_INCLUDE_DIR}"
)

if (OPENCL_FOUND)
    include_directories("${OPENCL_INCLUDE_DIRS}")
endif()

# XXX: This is unfortunate, we should probably refactor examples_common_obj
# to separate out the GL from the DX dependencies so that we don't end up
# in the situation where we have to link in GLFW for dxPtexViewer.
if (GLFW_FOUND)
    list(APPEND PLATFORM_LIBRARIES
        "${GLFW_LIBRARIES}"
    )
endif()

set(SOURCE_FILES
    dxPtexViewer.cpp
)

_stringify("${SHADER_FILES}" INC_FILES)

include_directories("${CMAKE_CURRENT_BINARY_DIR}")

_add_possibly_cuda_executable(dxPtexViewer WIN32
    "${SOURCE_FILES}"
    "${INC_FILES}"
    $<TARGET_OBJECTS:regression_common_obj>
    $<TARGET_OBJECTS:regression_far_utils_obj>
    $<TARGET_OBJECTS:examples_common_obj>
)

target_link_libraries(dxPtexViewer
    ${PLATFORM_LIBRARIES}
)

install(TARGETS dxPtexViewer DESTINATION "${CMAKE_BINDIR_BASE}")
