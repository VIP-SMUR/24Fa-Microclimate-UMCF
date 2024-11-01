### UMCF - OpenFOAM errors

#### Introduction

This documentation contains the different warning and errors obtained during the OpenFOAM simulation process. The errors are mostly related to geometry complexity and the overall number of cells/faces/points. During the testing, we found that the errors can be avoided by lowering the refinement levels in the **snappyHexMeshDict.veg** and **snappyHexMeshDict.air**. The standard refinement levels for this configuration are the following:

```
  refinementSurfaces
  {
    buildings
    {
      level    (3 5);
      patchInfo    { type wall; };
    }
    trees
    {
      level    (3 5);
      patchInfo    { type wall; };
    }
  }
  refinementRegions
  {
    refinementBox
    {
      mode    inside;
      levels    ((1E15 2));
    }
    buildings
    {
      mode    distance;
      levels    ((1.0 4)(2.0 3));
    }
    trees
    {
      mode    distance;
      levels    ((0.5 4)(1.0 3));
    }
```

Lowering the levels of refinement of the minimum and maximum by 1 would reduce drastically the overall number of cells in the case domain. Additionally, the initial mesh geometry can be simplified or decreased in number to prepare and run the simulation successfully. In our study for error testing and limitations we found that with the vegetation geometry in the case study below, 63 instances of trees worked, but 64 failed:

![Captura de pantalla 2024-11-01 001421](https://github.com/user-attachments/assets/d8bcc354-b8dc-4f77-ba2f-b4fb7e68dce7)

![Captura de pantalla 2024-11-01 001438](https://github.com/user-attachments/assets/fc6cb3b7-31c0-4361-b78a-2503fcd44198)

![Captura de pantalla 2024-11-01 001449](https://github.com/user-attachments/assets/d1752bf9-8316-40c0-bb7a-09aa348980ac)

![Captura de pantalla 2024-11-01 001458](https://github.com/user-attachments/assets/94da7077-fd31-4466-8a54-7825d8bbb2f0)

![Captura de pantalla 2024-11-01 001507](https://github.com/user-attachments/assets/1643b4c6-5eb4-4911-81f2-f868ef230b49)


- ##### **subMap**
```
	FOAM FATAL ERROR:
	cannot find file "C:/*case folder*/*case name*/constant/vegetation/subMap" 
```

![subMap](https://github.com/user-attachments/assets/739e64af-9620-46a0-b6c3-3aadb45837f4)


**Initial observation:** when dealing with a geometry with high number of cells, the solver request a subMap file/subdirectory. This is not required when dealing with a domain with less number of cells.

**Workaround:** Remove geometry from the meshing process, or reduce the refinement levels. Need to test if there is a correlation with divqrsw error.

- ##### **divqrsw**
```
	FOAM FATAL ERROR:
	cannot fin file "C:/*case folder*/*case name*/constant/air/divqrsw"
```

![divqrsw](https://github.com/user-attachments/assets/59aadd1d-495b-41b2-a89d-58818bc1c792)


**Initial observation:** when dealing with a geometry with high number of cells, the solver request a subMap file/subdirectory. This is not required when dealing with a domain with less number of cells.

**Workaround:** Remove geometry from the meshing process, or reduce the refinement levels. Need to test if there is a correlation with subMap error.

- ##### **No base point produces a valid tet decomposition**
```
	FOAM Warning:
	From function Foam::triFace:: Foam:_tetIndices::faceTriIs(const Foam::polyMesh&) const
	in file meshes/polyMesh/polyMeshTetDecomposition/tetIndices.H at line 76
	No base point for face 1158192, 7(363833 149882 2230303 2176367 1006506 1461031 106203), produces a valid tet decomposition.
```

![divqrsw previous log](https://github.com/user-attachments/assets/d6b83241-c76e-426b-80f0-7b042dd8be5d)


**Initial observation:** warning leading to the divqrsw error. 

**Workaround: Workaround:** Remove geometry from the meshing process, or reduce the refinement levels. Need to test if there is a correlation with subMap error.

- ##### **sigSegv**
```
	Backtrace:
		ZN10StackTraceC1Ev [0x626c1855+0x25]
			module: C:\Program Files\blueCFD-Core-2020\ThirdParty-8\platforms\mingw_w64GccDPInt32\lib\libstack_trace.dll
		ZN4Foam5error10printStackERNS_7OstreamE [0x6c30ae5a+0x23a]
			module: C:\Program Files\blueCFD-Core-2020\OpenFOAM-8\platforms\mingw_w64GccDPInt32\lib\libOpenFOAM.dll
		ZN4Foam7sigSegv14sigSegvHandlerEi [0x6c30bb0b+0x33]
			module: C:\Program Files\blueCFD-Core-2020\OpenFOAM-8\platforms\mingw_w64GccDPInt32\lib\libOpenFOAM.dll
		(No symbol) [0x4087e2]
			module: C:\Program Files\blueCFD-Core-2020\ofuser-of8\platforms\mingw_w64GccDPInt32Opt\bin\urbanMicroclimateFoam.exe
		0_C_specific_handler [0x7ffcb5bc2b48+0x98]
			module: C:\WINDOWS\System32\msvcrt.dll
		_chkstk [0x7ffcbc55517f+0x12f]
			module: C:\WINDOWS\SYSTEM32\ntdll.dll
		RtlFindCharInUnicodeString [0x7ffcbc4ee856+0xa96]
			module: C:\WINDOWS\SYSTEM32\ntdll.dll
		KiUserExceptionDispatcher [0x7ffcbc5541e8+0x2e]
			module: C:\WINDOWS\SYSTEM32\ntdll.dll
		ZN4Foam15mappedPatchBase9facePointsERKNS_9polyMeshEiN5l17cellDecompositionE [0x6aa8d97a+0x16a]
			module: C:\Program Files\blueCFD-Core-2020\OpenFOAM-8\platforms\mingw_w64GccDPInt32Opt\lib\libmeshTools.dll
		ZNK4Foam15mappedPatchBase10facePointsERKNS_9polyPatchE [0x6aa8fed3+0xa3]
			module: C:\Program Files\blueCFD-Core-2020\OpenFOAM-8\platforms\mingw_w64GccDPInt32Opt\lib\libmeshTools.dll		
		
```
![Captura de pantalla 2024-10-14 185130](https://github.com/user-attachments/assets/fbad4b8a-cfa5-41b1-ad13-f756535fd5a2)

**Initial observation:** when dealing with a geometry with high number of cells, the solver request a subMap file/subdirectory. This is not required when dealing with a domain with less number of cells. This error is generated if the simulation reached the final step in the process, apparently due to lack of memory.

**Workaround:** Remove geometry from the meshing process, or reduce the refinement levels. Need to test if there is a correlation with subMap error.

