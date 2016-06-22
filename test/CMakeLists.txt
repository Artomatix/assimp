cmake_minimum_required( VERSION 3.0 )

include( CTest )
enable_testing()

INCLUDE_DIRECTORIES(
	../contrib/gtest/include
	../contrib/gtest/
    ${Assimp_SOURCE_DIR}/include
    ${Assimp_SOURCE_DIR}/code
)

# Add the temporary output directories to the library path to make sure the
# Assimp library can be found, even if it is not installed system-wide yet.
LINK_DIRECTORIES( ${Assimp_BINARY_DIR} ${AssetImporter_BINARY_DIR}/lib )

SOURCE_GROUP( unit FILES
  unit/CCompilerTest.c
)

SET( TEST_SRCS
  unit/UTLogStream.h
  unit/AbstractImportExportBase.cpp
  unit/TestIOSystem.h
  unit/TestModelFactory.h
  unit/ut3DImportExport.cpp
  unit/ut3DSImportExport.cpp
  unit/utACImportExport.cpp
  unit/utAMFImportExport.cpp
  unit/utASEImportExport.cpp  
  unit/utAnim.cpp
  unit/AssimpAPITest.cpp
  unit/utB3DImportExport.cpp
  unit/utBatchLoader.cpp
  unit/utBlenderIntermediate.cpp
  unit/utBlendImportAreaLight.cpp
  unit/utBlenderImportExport.cpp
  unit/utBlendImportMaterials.cpp
  unit/utBVHImportExport.cpp
  unit/utColladaExportCamera.cpp
  unit/utColladaExportLight.cpp
  unit/utColladaImportExport.cpp
  unit/utCSMImportExport.cpp
  unit/utDefaultIOStream.cpp
  unit/utDXFImporterExporter.cpp
  unit/utFastAtof.cpp
  unit/utFBXImporterExporter.cpp
  unit/utFindDegenerates.cpp
  unit/utFindInvalidData.cpp
  unit/utFixInfacingNormals.cpp
  unit/utGenNormals.cpp
  unit/utglTFImportExport.cpp
  unit/utHMPImportExport.cpp
  unit/utIFCImportExport.cpp
  unit/utImporter.cpp
  unit/utImproveCacheLocality.cpp
  unit/utIOSystem.cpp
  unit/utIOStreamBuffer.cpp
  unit/utIssues.cpp
  unit/utJoinVertices.cpp
  unit/utLimitBoneWeights.cpp
  unit/utLWSImportExport.cpp
  unit/utMaterialSystem.cpp
  unit/utMatrix3x3.cpp
  unit/utMatrix4x4.cpp
  unit/utMetadata.cpp
  unit/SceneDiffer.h
  unit/SceneDiffer.cpp
  unit/utSIBImporter.cpp
  unit/utObjImportExport.cpp
  unit/utObjTools.cpp
  unit/utOpenGEXImportExport.cpp
  unit/utPretransformVertices.cpp
  unit/utPLYImportExport.cpp
  unit/utPMXImporter.cpp
  unit/utRemoveComments.cpp
  unit/utRemoveComponent.cpp
  unit/utRemoveRedundantMaterials.cpp
  unit/utRemoveVCProcess.cpp
  unit/utScenePreprocessor.cpp
  unit/utSharedPPData.cpp
  unit/utStringUtils.cpp
  unit/utSMDImportExport.cpp
  unit/utSortByPType.cpp
  unit/utSplitLargeMeshes.cpp
  unit/utTargetAnimation.cpp
  unit/utTextureTransform.cpp
  unit/utTriangulate.cpp
  unit/utTypes.cpp
  unit/utVertexTriangleAdjacency.cpp
  unit/utVersion.cpp
  unit/utVector3.cpp
  unit/utXImporterExporter.cpp
  unit/utD3MFImportExport.cpp
  unit/utProfiler.cpp
)

SOURCE_GROUP( tests FILES  ${TEST_SRCS} )

add_executable( unit
    ../contrib/gtest/src/gtest-all.cc
    unit/CCompilerTest.c
    unit/Main.cpp
    ../code/Version.cpp
    ${TEST_SRCS}
)

add_definitions(-DASSIMP_TEST_MODELS_DIR="${CMAKE_CURRENT_LIST_DIR}/models")
    
SET_PROPERTY( TARGET assimp PROPERTY DEBUG_POSTFIX ${CMAKE_DEBUG_POSTFIX} )

IF( WIN32 )
    SET( platform_libs )
ELSE( WIN32 )
    SET( platform_libs pthread )
ENDIF( WIN32 )

IF(MSVC)
		add_definitions(-D_CRT_SECURE_NO_WARNINGS)
ENDIF(MSVC)

target_link_libraries( unit assimp ${platform_libs} )

add_subdirectory(headercheck)

add_test( unittests unit )

