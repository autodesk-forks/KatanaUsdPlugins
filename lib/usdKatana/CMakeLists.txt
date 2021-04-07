# Modifications Copyright 2020 Autodesk, Inc.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
set(PXR_PACKAGE usdKatana)

set(vtKatana_LIBRARY "")
if (NOT ${KATANA_API_VERSION} VERSION_LESS "3.0.0")
    set(vtKatana_LIBRARY "vtKatana")
endif()

pxr_shared_library(${PXR_PACKAGE}
    LIBRARIES
        ar
        arch
        cameraUtil
        gf
        kind
        pcp
        plug
        tf
        trace
        vt
        sdf
        usd
        usdHydra
        usdImagingGL
        usdSkel
        usdGeom
        usdRi
        usdUI
        usdUtils
        usdLux
        usdShade
        hio
        work
        katanaPluginApi
        ${vtKatana_LIBRARY}
        Boost::system
        Boost::regex
        Boost::filesystem

    PUBLIC_CLASSES
        attrMap
        baseMaterialHelpers
        blindDataObject
        cache
        debugCodes
        locks
        tokens
        lookAPI
        utils

        usdInArgs
        usdInPrivateData
        usdInPluginRegistry

        readBasisCurves
        readBlindData
        readCamera
        readConstraintTarget
        readGprim
        readGeomSubset
        readLight
        readLightFilter
        readMaterial
        readMesh
        readModel
        readNurbsPatch
        readPointInstancer
        readPoints
        readPrim
        readXformable

        bootstrap

    PUBLIC_HEADERS
        api.h

    PYMODULE_CPPFILES
        wrapBlindDataObject.cpp
        wrapCache.cpp
        wrapLookAPI.cpp
        module.cpp

    PYMODULE_FILES
        __init__.py

    RESOURCE_FILES
        plugInfo.json
        generatedSchema.usda
        schema.usda:usdKatana/schema.usda
)

# Hack to work around https://github.com/PixarAnimationStudios/USD/issues/738
if(CMAKE_SYSTEM_NAME MATCHES Windows)
set_source_files_properties(cache.cpp PROPERTIES COMPILE_FLAGS "/Od")
endif()
