if(MSVC)
    add_definitions(-DFCAppFem -DHAVE_ACOSH -DHAVE_ASINH -DHAVE_ATANH)
else(MSVC)
    add_definitions(-DHAVE_LIMITS_H -DHAVE_CONFIG_H)
endif(MSVC)


if(BUILD_FEM_NETGEN)
    add_definitions(-DFCWithNetgen ${NETGEN_DEFINITIONS})
endif(BUILD_FEM_NETGEN)


include_directories(
    ${CMAKE_BINARY_DIR}
    ${CMAKE_SOURCE_DIR}/src
    ${CMAKE_BINARY_DIR}/src
    ${CMAKE_CURRENT_BINARY_DIR}
    ${Boost_INCLUDE_DIRS}
    ${OCC_INCLUDE_DIR}
    ${PYTHON_INCLUDE_DIRS}
    ${ZLIB_INCLUDE_DIR}
    ${XercesC_INCLUDE_DIRS}
    ${SMESH_INCLUDE_DIR}
    ${NETGEN_INCLUDE_DIRS}
    ${VTK_INCLUDE_DIRS}
)


link_directories(${OCC_LIBRARY_DIR})


set(Fem_LIBS
    Part
    FreeCADApp
    StdMeshers
    SMESH
    SMDS
    SMESHDS
)


if(BUILD_FEM_NETGEN)
    list(APPEND Fem_LIBS
        NETGENPlugin
    )
endif(BUILD_FEM_NETGEN)


generate_from_xml(FemMeshPy)
generate_from_xml(FemPostPipelinePy)


SET(Python_SRCS
    FemMeshPy.xml
    FemMeshPyImp.cpp
    HypothesisPy.cpp
    HypothesisPy.h
)


if(BUILD_FEM_VTK)
    SET(Python_SRCS
        ${Python_SRCS}
        FemPostPipelinePy.xml
        FemPostPipelinePyImp.cpp
    )
endif(BUILD_FEM_VTK)
SOURCE_GROUP("Python" FILES ${Python_SRCS})


if(BUILD_FEM_VTK)
    SET(FemPost_SRCS
        PropertyPostDataObject.h
        PropertyPostDataObject.cpp
        FemPostObject.h
        FemPostObject.cpp
        FemPostPipeline.h
        FemPostPipeline.cpp
        FemPostFilter.h
        FemPostFilter.cpp
        FemPostFunction.h
        FemPostFunction.cpp
        FemVTKTools.h
        FemVTKTools.cpp
    )
    SOURCE_GROUP("PostObjects" FILES ${FemPost_SRCS})
endif(BUILD_FEM_VTK)


SET(Mod_SRCS
    AppFem.cpp
    AppFemPy.cpp
    FemTools.cpp
    FemTools.h
    PreCompiled.cpp
    PreCompiled.h
)
SOURCE_GROUP("Module" FILES ${Mod_SRCS})


SET(FemScripts_SRCS
    Init.py
    InitGui.py
    FemConstraint.py
    FemInputWriter.py
    FemToolsCcx.py
    FemUtils.py
    ObjectsFem.py
    TestFem.py
)
#SOURCE_GROUP("Scripts" FILES ${FemScripts_SRCS})


SET(FemObjectsScripts_SRCS
    PyObjects/__init__.py
    PyObjects/_FemConstraintBodyHeatSource.py
    PyObjects/_FemConstraintElectrostaticPotential.py
    PyObjects/_FemConstraintFlowVelocity.py
    PyObjects/_FemConstraintInitialFlowVelocity.py
    PyObjects/_FemConstraintSelfWeight.py
    PyObjects/_FemElementFluid1D.py
    PyObjects/_FemElementGeometry1D.py
    PyObjects/_FemElementGeometry2D.py
    PyObjects/_FemMaterial.py
    PyObjects/_FemMaterialMechanicalNonlinear.py
    PyObjects/_FemMeshBoundaryLayer.py
    PyObjects/_FemMeshGmsh.py
    PyObjects/_FemMeshGroup.py
    PyObjects/_FemMeshRegion.py
    PyObjects/_FemMeshResult.py
    PyObjects/_FemResultMechanical.py
    PyObjects/_FemSolverCalculix.py
)


SET(FemCommand_SRCS
    femcommand/__init__.py
    femcommand/commands.py
    femcommand/manager.py
)


SET(FemInterfaces_SRCS
    feminterface/__init__.py
    feminterface/convert2TetGen.py
    feminterface/importCcxDatResults.py
    feminterface/importCcxFrdResults.py
    feminterface/importFenicsMesh.py
    feminterface/importInpMesh.py
    feminterface/importToolsFem.py
    feminterface/importVTKResults.py
    feminterface/importZ88Mesh.py
    feminterface/importZ88O2Results.py
    feminterface/readFenicsXDMF.py
    feminterface/readFenicsXML.py
    feminterface/writeFenicsXDMF.py
    feminterface/writeFenicsXML.py
)


SET(FemMesh_SRCS
    femmesh/__init__.py
    femmesh/femmesh2mesh.py
    femmesh/gmshtools.py
    femmesh/meshtools.py
)


SET(FemResult_SRCS
    femresult/__init__.py
    femresult/resulttools.py
)


SET(FemSolver_SRCS
    femsolver/__init__.py
    femsolver/equationbase.py
    femsolver/report.py
    femsolver/reportdialog.py
    femsolver/run.py
    femsolver/settings.py
    femsolver/signal.py
    femsolver/solverbase.py
    femsolver/task.py
)


SET(FemSolverElmer_SRCS
    femsolver/elmer/__init__.py
    femsolver/elmer/sifio.py
    femsolver/elmer/solver.py
    femsolver/elmer/tasks.py
    femsolver/elmer/writer.py
)


SET(FemSolverElmerEquations_SRCS
    femsolver/elmer/equations/__init__.py
    femsolver/elmer/equations/electrostatic.py
    femsolver/elmer/equations/elasticity.py
    femsolver/elmer/equations/equation.py
    femsolver/elmer/equations/flow.py
    femsolver/elmer/equations/fluxsolver.py
    femsolver/elmer/equations/heat.py
    femsolver/elmer/equations/linear.py
    femsolver/elmer/equations/nonlinear.py
)


SET(FemSolverCalculix_SRCS
    femsolver/calculix/__init__.py
    femsolver/calculix/solver.py
    femsolver/calculix/tasks.py
    femsolver/calculix/writer.py
)


SET(FemSolverZ88_SRCS
    femsolver/z88/__init__.py
    femsolver/z88/solver.py
    femsolver/z88/tasks.py
    femsolver/z88/writer.py
)


SET(FemGuiScripts_SRCS
    PyGui/__init__.py
    PyGui/_TaskPanelFemElementFluid1D.py
    PyGui/_TaskPanelFemElementGeometry1D.py
    PyGui/_TaskPanelFemElementGeometry2D.py
    PyGui/_TaskPanelFemMaterial.py
    PyGui/_TaskPanelFemMeshBoundaryLayer.py
    PyGui/_TaskPanelFemMeshGmsh.py
    PyGui/_TaskPanelFemMeshGroup.py
    PyGui/_TaskPanelFemMeshRegion.py
    PyGui/_TaskPanelFemResultShow.py
    PyGui/_TaskPanelFemSolverCalculix.py
    PyGui/_TaskPanelFemSolverControl.py
    PyGui/_ViewProviderFemConstraintBodyHeatSource.py
    PyGui/_ViewProviderFemConstraintElectrostaticPotential.py
    PyGui/_ViewProviderFemConstraintFlowVelocity.py
    PyGui/_ViewProviderFemConstraintInitialFlowVelocity.py
    PyGui/_ViewProviderFemConstraintSelfWeight.py
    PyGui/_ViewProviderFemElementFluid1D.py
    PyGui/_ViewProviderFemElementGeometry1D.py
    PyGui/_ViewProviderFemElementGeometry2D.py
    PyGui/_ViewProviderFemMaterial.py
    PyGui/_ViewProviderFemMaterialMechanicalNonlinear.py
    PyGui/_ViewProviderFemMeshBoundaryLayer.py
    PyGui/_ViewProviderFemMeshGmsh.py
    PyGui/_ViewProviderFemMeshGroup.py
    PyGui/_ViewProviderFemMeshRegion.py
    PyGui/_ViewProviderFemMeshResult.py
    PyGui/_ViewProviderFemResultMechanical.py
    PyGui/_ViewProviderFemSolverCalculix.py
    PyGui/FemSelectionObserver.py
    PyGui/FemSelectionWidgets.py
    PyGui/TaskPanelFemElectrostaticPotential.ui
    PyGui/TaskPanelFemElementFluid1D.ui
    PyGui/TaskPanelFemElementGeometry1D.ui
    PyGui/TaskPanelFemElementGeometry2D.ui
    PyGui/TaskPanelFemFlowVelocity.ui
    PyGui/TaskPanelFemInitialFlowVelocity.ui
    PyGui/TaskPanelFemMaterial.ui
    PyGui/TaskPanelFemMeshBoundaryLayer.ui
    PyGui/TaskPanelFemMeshGmsh.ui
    PyGui/TaskPanelFemMeshGroup.ui
    PyGui/TaskPanelFemMeshGroupXDMFExport.ui
    PyGui/TaskPanelFemMeshRegion.ui
    PyGui/TaskPanelFemResultShow.ui
    PyGui/TaskPanelFemSolverCalculix.ui
)


SET(FemTests_SRCS
    femtest/__init__.py
    femtest/testfemcommon.py
)


SET(FemTestsCcx_SRCS
    femtest/testfiles/__init__.py
    femtest/testfiles/ccx/__init__.py
    femtest/testfiles/ccx/cube_mesh.py
    femtest/testfiles/ccx/cube_frequency.inp
    femtest/testfiles/ccx/cube_frequency.dat
    femtest/testfiles/ccx/cube_frequency.frd
    femtest/testfiles/ccx/cube_frequency_expected_values
    femtest/testfiles/ccx/cube_static.inp
    femtest/testfiles/ccx/cube_static.dat
    femtest/testfiles/ccx/cube_static.frd
    femtest/testfiles/ccx/cube_static_expected_values
    femtest/testfiles/ccx/cube.fcstd
    femtest/testfiles/ccx/spine_mesh.py
    femtest/testfiles/ccx/spine_thermomech.inp
    femtest/testfiles/ccx/spine_thermomech.dat
    femtest/testfiles/ccx/spine_thermomech.frd
    femtest/testfiles/ccx/spine_thermomech_expected_values
    femtest/testfiles/ccx/spine_thermomech.fcstd
    femtest/testfiles/ccx/Flow1D_mesh.py
    femtest/testfiles/ccx/Flow1D_thermomech.inp
    femtest/testfiles/ccx/Flow1D_thermomech.dat
    femtest/testfiles/ccx/Flow1D_thermomech.frd
    femtest/testfiles/ccx/Flow1D_thermomech_expected_values
    femtest/testfiles/ccx/Flow1D_thermomech_inout_nodes.txt
    femtest/testfiles/ccx/Flow1D_thermomech.fcstd
)


SET(FemTestsElmer_SRCS
    femtest/testfiles/elmer/__init__.py
    femtest/testfiles/elmer/case.sif
    femtest/testfiles/elmer/group_mesh.geo
    femtest/testfiles/elmer/ELMERSOLVER_STARTINFO
)


SET(FemBase_SRCS
    FemMeshObject.cpp
    FemMeshObject.h
    FemMeshShapeObject.cpp
    FemMeshShapeObject.h
    FemMeshShapeNetgenObject.cpp
    FemMeshShapeNetgenObject.h
    FemAnalysis.cpp
    FemAnalysis.h
    FemMesh.cpp
    FemMesh.h
    FemResultObject.cpp
    FemResultObject.h
    FemSolverObject.cpp
    FemSolverObject.h
    FemConstraint.cpp
    FemConstraint.h
    FemMeshProperty.cpp
    FemMeshProperty.h
    )
SOURCE_GROUP("Base types" FILES ${FemBase_SRCS})


SET(FemSet_SRCS
    FemSetObject.cpp
    FemSetObject.h
    FemSetNodesObject.cpp
    FemSetNodesObject.h
    FemSetElementsObject.cpp
    FemSetElementsObject.h
    FemSetFacesObject.cpp
    FemSetFacesObject.h
    FemSetGeometryObject.cpp
    FemSetGeometryObject.h
    )
SOURCE_GROUP("Set objects" FILES ${FemSet_SRCS})


SET(FemConstraints_SRCS
    FemConstraintBearing.h
    FemConstraintBearing.cpp
    FemConstraintFixed.cpp
    FemConstraintFixed.h
    FemConstraintForce.cpp
    FemConstraintForce.h
    FemConstraintFluidBoundary.cpp
    FemConstraintFluidBoundary.h
    FemConstraintPressure.cpp
    FemConstraintPressure.h
    FemConstraintGear.cpp
    FemConstraintGear.h
    FemConstraintPulley.cpp
    FemConstraintPulley.h
    FemConstraintDisplacement.h
    FemConstraintDisplacement.cpp
    FemConstraintTemperature.h
    FemConstraintTemperature.cpp
    FemConstraintHeatflux.h
    FemConstraintHeatflux.cpp
    FemConstraintInitialTemperature.h
    FemConstraintInitialTemperature.cpp
    FemConstraintPlaneRotation.cpp
    FemConstraintPlaneRotation.h
    FemConstraintContact.cpp
    FemConstraintContact.h
    FemConstraintTransform.cpp
    FemConstraintTransform.h
    )
SOURCE_GROUP("Constraints" FILES ${FemConstraints_SRCS})




SET(Fem_SRCS
    ${FemBase_SRCS}
    ${FemConstraints_SRCS}
    ${FemPost_SRCS}
    ${FemSet_SRCS}
    ${Mod_SRCS}
    ${Python_SRCS}
)


add_library(Fem SHARED ${Fem_SRCS})
target_link_libraries(Fem ${Fem_LIBS} ${VTK_LIBRARIES})


fc_target_copy_resource(Fem
    ${CMAKE_SOURCE_DIR}/src/Mod/Fem
    ${CMAKE_BINARY_DIR}/Mod/Fem
    Init.py
    ${FemCommand_SRCS}
    ${FemGuiScripts_SRCS}
    ${FemInterfaces_SRCS}
    ${FemMesh_SRCS}
    ${FemObjectsScripts_SRCS}
    ${FemScripts_SRCS}
    ${FemResult_SRCS}
    ${FemSolver_SRCS}
    ${FemSolverCalculix_SRCS}
    ${FemSolverElmer_SRCS}
    ${FemSolverElmerEquations_SRCS}
    ${FemSolverZ88_SRCS}
    ${FemTests_SRCS}
    ${FemTestsCcx_SRCS}
    ${FemTestsElmer_SRCS}
)


SET_BIN_DIR(Fem Fem /Mod/Fem)
SET_PYTHON_PREFIX_SUFFIX(Fem)


INSTALL(TARGETS Fem DESTINATION ${CMAKE_INSTALL_LIBDIR})
