# OpenFOAM_Overset_Case_Contra_Props
 Example case for OpenFOAM overset meshing with dynamic meshes. This case has two contra rotating 60mm propellers that are inline with one another. Further information about the case is located in the ReadMe.txt file.

This case uses overset meshing and pimpleFoam to simulate two contra rotating 60mm propellers similar to the ones seen on small brushed quad copters.
Each propeller is spinning at 3100 rad/s and are in line with eachother relative to the flow velocity.

#[Geometry]:
The geometry for the propellers are in each subfolder PropellerMesh_CCW and CW, and the origninal STL's are located in the baseGeometry folder.
Within each mesh subfolder is a rotateAircraft and translate file, these are used for preprocessing the STL files. Currently the STL's located
in the constant folder of each mesh subfolder has already been transformed with these two files. 

If the STL's in the constant folder get modified
replace them with the base STL's in the baseGeometry folder and run ./rotateAircraft then ./translate

#[Running]:
To run the case you can execute ./Allclean then ./Allrun

./Allrun will fail at setFields if zoneID is not copied from 0.base by ./Allrun. ./Allrun will copy zoneID from 0.base into 0, but this can fail if 
it is blocked by permissions/read/write access

./Allrun will also fail at decomposePar if cellLevel and pointLevel are not deleted from the 0 folder after the merging of the meshes. ./Allrun will attempt
to delete the files from the 0 folder, but could also be blocked by permissions.

If the mpirun command fails, check the logs for setFields and decomposePar and verify that they ran properlly. If they did not then rerun each application after
remedying the issue, and mpirun should execute properlly afterwards.


#[Notes]: The mesh for this case is not optimal for resolving smaller LES features in the wall layer or in the near field around the propeller geometry, if the users
hardware allows it, the mesh should be refined in the region around the propellers and just downstream of the propeller in a 70mm diameter cylinder. 

Ideally the region arodun the propellers should be split another 2 times to reach a cell size of 0.52mm or half the thickness of the propeller blade, or smaller.
Currently the cell size at the overset interface is 2.08mm which does not allow for good resolution of blade tip vorticies from the propellers.

If mesh settings are adjusted, care should be taken to match cell sizes at the interface between the background mesh and the overset mesh regions.

Also, at 3100rad/s the local velocity at the propeller blade tips reaches ~130m/s (0.37Mach) which warrents the use of a compressible solver, rather than
overPimpleDyMFoam.
