SET (VTK_VMTK_RENDERING_SRCS
  vtkvmtkInteractorStyleTrackballCamera.cxx
  )

SET (VTK_VMTK_RENDERING_TARGET_LINK_LIBRARIES vtkvmtkRendering vtkCommon vtkFiltering vtkGraphics vtkHybrid vtkRendering vtkWidgets)

ADD_LIBRARY (vtkvmtkRendering ${VTK_VMTK_RENDERING_SRCS})
IF(VMTK_LIBRARY_PROPERTIES)
  SET_TARGET_PROPERTIES(vtkvmtkRendering PROPERTIES ${VMTK_LIBRARY_PROPERTIES})
ENDIF(VMTK_LIBRARY_PROPERTIES)
TARGET_LINK_LIBRARIES(vtkvmtkRendering ${VTK_VMTK_RENDERING_TARGET_LINK_LIBRARIES})

INSTALL(TARGETS vtkvmtkRendering
  LIBRARY DESTINATION ${VTK_VMTK_INSTALL_LIB_DIR} COMPONENT RuntimeLibraries
  ARCHIVE DESTINATION ${VTK_VMTK_INSTALL_LIB_DIR} COMPONENT Development
  RUNTIME DESTINATION ${VTK_VMTK_INSTALL_BIN_DIR} COMPONENT RuntimeExecutables
)

#FILE(GLOB files "${VTK_VMTK_RENDERING_SRCS}/*.h")
FILE(GLOB files "${VTK_VMTK_SOURCE_DIR}/Rendering/*.h")
INSTALL(FILES ${files}
  DESTINATION ${VTK_VMTK_INSTALL_INCLUDE_DIR}
  COMPONENT Development)

### START VMTK IN SLICER PATCH ###
IF (VTK_VMTK_IN_SLICER)
  SET(LIBS vtkvmtkRendering)

  FOREACH(LIB ${LIBS})
    slicer3_set_modules_output_path(${LIB})
  ENDFOREACH(LIB ${LIBS})
    
  slicer3_install_modules(${LIBS})
ENDIF (VTK_VMTK_IN_SLICER)
### END VMTK IN SLICER PATCH ###



IF (VTK_WRAP_PYTHON AND VTK_VMTK_WRAP_PYTHON)
  VTK_WRAP_PYTHON3(vtkvmtkRenderingPython VTK_VMTK_RENDERING_PYTHON_SRCS "${VTK_VMTK_RENDERING_SRCS}")
  ADD_LIBRARY(vtkvmtkRenderingPythonD ${VTK_VMTK_RENDERING_PYTHON_SRCS})
  IF(VMTK_LIBRARY_PROPERTIES)
    SET_TARGET_PROPERTIES(vtkvmtkRenderingPythonD PROPERTIES ${VMTK_LIBRARY_PROPERTIES})
  ENDIF(VMTK_LIBRARY_PROPERTIES)
  ADD_LIBRARY(vtkvmtkRenderingPython MODULE vtkvmtkRenderingPythonInit.cxx)
  TARGET_LINK_LIBRARIES(vtkvmtkRenderingPythonD vtkvmtkRendering vtkCommon vtkCommonPythonD vtkFiltering vtkFilteringPythonD vtkGraphics vtkGraphicsPythonD vtkRendering vtkRenderingPythonD vtkWidgets vtkWidgetsPythonD)
  TARGET_LINK_LIBRARIES (vtkvmtkRenderingPython vtkvmtkRenderingPythonD)
  IF(WIN32 AND NOT CYGWIN)
     SET_TARGET_PROPERTIES(vtkvmtkRenderingPython PROPERTIES SUFFIX ".pyd")
  ENDIF(WIN32 AND NOT CYGWIN)
  INSTALL(TARGETS vtkvmtkRenderingPythonD
    LIBRARY DESTINATION ${VTK_VMTK_INSTALL_LIB_DIR} COMPONENT RuntimeLibraries
    ARCHIVE DESTINATION ${VTK_VMTK_INSTALL_LIB_DIR} COMPONENT Development
    RUNTIME DESTINATION ${VTK_VMTK_INSTALL_BIN_DIR} COMPONENT RuntimeExecutables
    )
  INSTALL(TARGETS vtkvmtkRenderingPython
    LIBRARY DESTINATION ${VTK_VMTK_MODULE_INSTALL_LIB_DIR} COMPONENT RuntimeLibraries
  )
ENDIF (VTK_WRAP_PYTHON AND VTK_VMTK_WRAP_PYTHON)

IF (VTK_WRAP_TCL AND VTK_VMTK_WRAP_TCL)
  SET(VTK_WRAP_HINTS ${VTK_VMTK_SOURCE_DIR}/Wrapping/Tcl/hints)
  VTK_WRAP_TCL3(vtkvmtkRenderingTCL VTK_VMTK_RENDERING_TCL_SRCS "${VTK_VMTK_RENDERING_SRCS}" "")
  ADD_LIBRARY(vtkvmtkRenderingTCL ${VTK_VMTK_RENDERING_TCL_SRCS})
  IF(VMTK_LIBRARY_PROPERTIES)
    SET_TARGET_PROPERTIES(vtkvmtkRenderingTCL PROPERTIES ${VMTK_LIBRARY_PROPERTIES})
  ENDIF(VMTK_LIBRARY_PROPERTIES)
  TARGET_LINK_LIBRARIES(vtkvmtkRenderingTCL vtkvmtkRendering vtkCommon vtkCommonTCL vtkFiltering vtkFilteringTCL vtkGraphics vtkGraphicsTCL vtkRendering vtkRenderingTCL vtkWidgets vtkWidgetsTCL)

  INSTALL(TARGETS vtkvmtkRenderingTCL
    LIBRARY DESTINATION ${VTK_VMTK_INSTALL_LIB_DIR} COMPONENT RuntimeLibraries
    ARCHIVE DESTINATION ${VTK_VMTK_INSTALL_LIB_DIR} COMPONENT Development
    RUNTIME DESTINATION ${VTK_VMTK_INSTALL_BIN_DIR} COMPONENT RuntimeExecutables
    )

  ### START VMTK IN SLICER PATCH ###
  IF (VTK_VMTK_IN_SLICER)
    SET(LIBS vtkvmtkRenderingTCL)

    FOREACH(LIB ${LIBS})
      slicer3_set_modules_output_path(${LIB})
    ENDFOREACH(LIB ${LIBS})
    
    slicer3_install_modules(${LIBS})
  ENDIF (VTK_VMTK_IN_SLICER)
  ### END VMTK IN SLICER PATCH ###

ENDIF (VTK_WRAP_TCL AND VTK_VMTK_WRAP_TCL)