
.. _label-develop:

==============
For developers
==============

In Dyssol, you can develop and debug new steady-state or dynamic **units** as well as your own external modules (**solvers**). 

The installation package of Dyssol contains all necessary components for the development of such modules. It is provided with a pre-configured solution for IDE Microsoft Visual Studio 2015 (or its `Community edition <https://go.microsoft.com/fwlink/?LinkId=615448&clcid=0x409>`_).

The development of modules for Dyssol can be done in three following steps:

	1.	Copy template project with necessary header files and libraries from Dyssol installation path to desired folder and configure project. 
	
	2.	Copy template of the necessary unit (dynamic or steady-state) or solver, rename it and add this module to the previously copied template project.
	
	3.	Reimplement all necessary functions. In the case of dynamic units the internal DAE/NL solver can be used to solve DAE/NL systems automatically. For detailed information about implementation of units and solvers, please refer to :ref:`label-unitDev` and :ref:`label-solverDev`.

|

.. _label-VCconfig:

Configuration of Visual Studio project template
===============================================

1.	Open directory where Dyssol has been installed (usually ``C:\Program Files (x86)\Dyssol``) and copy folder ``VCProject`` to the desired location on your hard drive (further ``<PathToSolution>``).

2.	Open the copied folder ``VCProject`` and run file ``Dyssol.sln`` to open solution in Microsoft Visual Studio, which should be previously installed. 

3.	Select startup project: 

	Select project *ModelsAPI* in *solution explorer*, then choose *Project → Set as StartUp Project*.

4.	Select paths to executable files: 

	- Select project ModelsAPI in solution explorer, then choose *Project → Properties → Configuration Properties → Debugging*
	
	- Set combo box Configuration in the top of the window to position Debug, and provide the property Command with the path to debug version of executable, which is located at 
	
		``<PathToSolution>\VCProject\ExecutableDebug\Dyssol.exe``
	
	- Set combo box *Configuration* in the top of the window to position *Release*, and provide the property *Command* with the path to release version of executable, which is located in the directory where Dyssol has been installed: 
	
		``C:\Program Files (x86)\Dyssol\Dyssol.exe``

5.	Set combo box *Configuration* in the top of the window to position *Debug*. Press F7 (or *Build → Build project* in program menu) to build core project and wait until the solution is built.

6.	Press :kbd:`F5` (or *Debug → Run debug* in program menu) to run program in debug mode. New window of Dyssol should now be opened.

7.	Close Dyssol window.

Visual Studio solution is now ready to create and debug your own modules. 


|

.. _label-unitDev:

Unit development
================

You must do the following in order to develop your new solver (plese refer to :ref:`label-VCconfig`):

	1.	Install Microsoft Visual Studio 2015 (Community). 
	
	2.	Configure template project ``VCProject``.

There are 4 different pre-defined templates of units available:

	1.	``SteadyState``: performs steady-state calculation; current state of such unit does not depend on the previous state, but only on the input parameters.
	
	2.	``SteadyStateWithNLSolver``: steady-state unit with connected internal solver of non-linear equations.
	
	3.	``Dynamic``: performs dynamic calculation; current state of this unit depends not only on the input parameters as well as on the previous state of the unit.
	
	4.	``DynamicWithDAESolver``: dynamic unit with connected internal solver of differential-algebraic equations.

Please also refer to :ref:`label-baseUnit` for detailed informaiton on functions applied in unit development.

|

Add new unit to the template project
------------------------------------

1.	Copy the desired template of the unit from ``<PathToSolution>\VCProject\UnitsTemplates`` to the folder ``Units`` in solution (``<PathToSolution>\VCProject\Units``).

2.	Rename template’s folder according to the name of your new unit (further ``<MyUnitFolder>``). The name can be chosen freely.

3.	Rename project files in template’s folder (``*.vcxproj``, ``*.vcxproj.filters``) according to the name of the new unit.

4.	Run the solution file (``<PathToSolution>\Dyssol.sln``) to open it in Visual Studio.

5.	Add project with your new unit to the solution. To do this, select in Visual Studio *File → Add → Existing Project* and specify path to the project file (``<PathToSolution>\VCProject\Units\<MyUnitFolder>\<*.vcxproj>``).

6.	Rename added project in Visual Studio according to the name of your unit. 

Now you can implement functionality of your new unit. To build your solution press :kbd:`F7`, to run it in debug mode press :kbd:`F5`. Files with new units will be placed to ``<PathToSolution>\VCProject\Debug``.

As debug versions of compiled and built units contain a lot of additional information, which is used by Visual Studio to perform debugging, their calculation efficiency can be dramatically low. Thus, for the simulation purposes, units should be built in *Release* mode.

|

Configure Dyssol to work with implemented units
-----------------------------------------------

1.	Build your units in *Release* mode. To do this, open your solution in Visual Studio (run file ``<PathToSolution>\VCProject.sln``), switch *Solution configuration* combo box from the toolbox of Visual Studio from *Debug* to *Release* and build the project (press :kbd:`F7` or choose *Build → Build project* in program menu).

2.	Configure Dyssol by adding the path to new units: run Dyssol, choose *Tools → Models Manager* and add path to your models (``<PathToSolution>\VCProject\Release``).

Now, all newly developed units will be available in Dyssol.

In general, usual configuration of Models Manager should include following path for units:

	-	``<InstallationPath>\Units``: list of standard units;

	-	``<PathToSolution>\VCProject\UnitsDebugLibs``: debug versions of standard units;

	-	``<PathToSolution>\VCProject\Debug``: debug versions of developed units;

	-	``<PathToSolution>\VCProject\Release``: release versions of developed units.

|

Development of steady-state units
---------------------------------

.. code-block:: cpp

	Unit::Unit() 
	
**Constructor** of the unit: called only once when unit is added to the flowsheet. In this function a set of parameters should be specified:

1.	Basic info:

	-	``m_sUnitName``: Name of the unit that will be displayed in Dyssol.
	
	-	``m_sAuthorName``: Unit’s author
	
	-	``m_sUniqueID``: Unique identificator of the unit. Simulation environment distinguishes different units with the help of this identificator. 
	
	You must ensure that ID of your unit is unique. This ID can be created manually or using *GUID-generator* of Visual Studio (*Tools → GUID Genarator*).
	
2.	Specify ports: add new, rename or delete existing.
	
3.	If unit has some additionally parameters, than specify them here.
	
4.	Additional internal material streams can be defined here.
	
5.	All other operations, which should take place only once during the unit’s creation.
	
|
	
.. code-block:: cpp

	Unit::~Unit()
	
**Destructor** of the unit: called only once when unit is removed from the flowsheet. Here all memory which has been previously allocated in the constructor should be freed.

|

.. code-block:: cpp

	Unit::Initialize(Time)
	
Unit‘s **initialization**. This function is called only once at the start of the simulation. Starting from this point, information about defined compounds, phases, distributions, etc. are available for the unit. Here you can create state variables and initialize some additionaly objects (e.g. additional material streams, state variables or plots).

|

.. code-block:: cpp

	Unit::Simulate(Time) 
	
**Steady-state calculation** for a specified time point. This function is called iteratively for all time points for which this unit should be calculated. All main calculations should be implemented here.

|

.. code-block:: cpp

	Unit::Finalize()
	
Unit‘s **finalization**. This function is called only once at the end of the simulation. Here one can perform closing and cleaning operations to prepare for the next possible simulation run. Implementation of this function is not obligatory and can be skipped.

|

Development of steady-state units with internal non-linear solver
-----------------------------------------------------------------

You can solve nonlinear equation systems automatically in Dyssol system. In this case, the unit should contain one or several additional objects of ``NLModel`` class. This class is used to describe non-linear systems and can be automatically solved with ``NLSolver`` class. 

|

.. code-block:: cpp
   
	Unit::Unit()
	
**Constructor** of the unit: called only once when unit is added to the flowsheet. In this function a set of parameters should be specified:

1.	Basic info:

	-	``m_sUnitName``: Name of the unit that will be displayed in Dyssol.

	-	``m_sAuthorName``: Unit’s author

	-	``m_sUniqueID``: Unique identificator of the unit. Simulation environment distinguishes different units with the help of this identificator. You must ensure that ID of your unit is unique. This ID can be created manually or using *GUID-generator* of Visual Studio (*Tools → GUID Genarator*).

2.	Specify ports: add new, rename or delete existing.

3.	If unit has some additionally parameters, than specify them here.

4.	Additional material streams can be defined here.

5.	All other operations, which should take place only once during the unit’s creation.

|

.. code-block:: cpp

	Unit::~Unit()
	
**Destructor** of the unit: called only once when unit is removed from the flowsheet. Here all memory which has been previously allocated in the constructor should be freed.

|

.. code-block:: cpp

	Unit::Initialize(Time)
	
Unit‘s **initialization**. This function is called only once at the start of the simulation. Starting from this point, information about defined compounds, phases, distributions, etc. are available for the unit. Here you can create state variables and initialize some additionaly objects (for example holdups, material streams, state variables or plots).

In this function, variables of all ``NLModels`` should be specified by using function ``NLModel::AddNLVariable()``; connection between ``NLModel`` and ``NLSolver`` classes should be created by calling function ``NLSolver::SetModel()``.

|

.. code-block:: cpp

	Unit::Simulate(Time)
	
**Steady-state calculation** for a specified time point. This function is called iteratively for all time points for which this unit should be calculated. All main calculations should be implemented here. Calculation of the defined NL-system can be run here by calling function ``NLSolver::Calculate()``.

|

.. code-block:: cpp
	
	Unit::SaveState()
	
For flowsheets containing **recycled streams**, ``SaveState()`` function is called when the convergence on the current time interval is reached, this also ensures the return to the previous state of the unit if convergence fails during the calculation. Here all internal time-dependent variables which weren’t added to the unit by using ``AddStateVariable()`` and ``AddMaterialStream()`` functions should be manually saved. Implementation of this function is not obligatory and can be skipped.

|

.. code-block:: cpp

	Unit::LoadState()
	
**Load last state** of the unit which has been saved with ``SaveState()`` function. Implementation of this function is not obligatory and can be skipped.

|

.. code-block:: cpp

	Unit::Finalize()

Unit‘s **finalization**. This function is called only once at the end of the simulation. Here one can perform closing and cleaning operations to prepare for the next possible simulation run. Implementation of this function is not obligatory and can be skipped.

|

.. code-block:: cpp

	NLModel::ResultsHandler()

**Handling of results**, which are returned from ``NLSolver`` on each time point. Called by solver every time when the solution in a new time point is ready. 

|

.. code-block:: cpp

	NLModel::CalculateFunctions()
	
Here the non-linear system should be **specified**. This function will be called by solver automatically.

|

Development of dynamic units
----------------------------

.. code-block:: cpp

	Unit::Unit() 
	
**Constructor** of the unit: called only once when unit is added to the flowsheet. In this function a set of parameters should be specified:

1.	Basic info:

	-	``m_sUnitName``: Name of the unit that will be displayed in Dyssol.

	-	``m_sAuthorName``: Unit’s author

	-	``m_sUniqueID``: Unique identificator of the unit. Simulation environment distinguishes different units with the help of this identificator. You must ensure that ID of your unit is unique. This ID can be created manually or using *GUID-generator* of Visual Studio (*Tools → GUID Genarator*).

2.	Specify ports: add new, rename or delete existing.

3.	If unit has some additionally parameters, than specify them here.

4.	Internal holdups and additional material streams can be defined here.

5.	All other operations, which should take place only once during the unit’s creation.

|

.. code-block:: cpp

	Unit::~Unit()
	
**Destructor** of the unit: called only once when unit is removed from the flowsheet. Here all memory which has been previously allocated in the constructor should be freed.

|

.. code-block:: cpp

	Unit::Initialize(Time)
	
Unit‘s **initialization**. This function is called only once at the start of the simulation. Starting from this point, information about defined compounds, phases, distributions, etc. are available for the unit. Here you can create state variables and initialize some additionaly objects (e.g. holdups, material streams or state variables).

|


.. code-block:: cpp

	Unit::Simulate(Tstart, Tend)
	
**Dynamic calculation** of the unit on a specified time interval. All logic of the unit’s model must be implemented here.

|

.. code-block:: cpp

	Unit::SaveState()
	
For flowsheets containing **recycled streams**, ``SaveState()`` function is called when the convergence on the current time interval is reached, this also ensures the return to the previous state of the unit if convergence fails during the calculation. Here all internal time-dependent variables which weren’t added to the unit by using ``AddStateVariable()``, ``AddMaterialStream()`` or ``AddHoldup()`` functions should be manually saved. Implementation of this function is not obligatory and can be skipped.

|

.. code-block:: cpp

	Unit::LoadState()

**Load last state** of the unit which has been saved with the SaveState() function. Implementation of this function is not obligatory and can be skipped.

|

.. code-block:: cpp

	Unit::Finalize()

Unit‘s **finalization**. This function is called only once at the end of the simulation. Here one can perform closing and cleaning operations to prepare for the next possible simulation run. Implementation of this function is not obligatory and can be skipped.

|

Development of dynamic units with internal DAE solver
-----------------------------------------------------

You can solve systems of differential-algebraic equations (DAE) automatically in Dyssol system. In this case, the unit should contain one or several additional objects of ``DAEModel`` class. This class is used to describe DAE systems and can be automatically solved with ``DAESolver`` class.  

|

.. code-block:: cpp
  
	Unit::Unit()

**Constructor** of the unit: called only once when unit is added to the flowsheet. In this function a set of parameters should be specified:

1.	Basic info:

	-	``m_sUnitName``: Name of the unit that will be displayed in Dyssol.
	
	-	``m_sAuthorName``: Unit’s author.
	
	-	``m_sUniqueID``: Unique identificator of the unit. Simulation environment distinguishes different units with the help of this identificator. You must ensure that ID of your unit is unique. This ID can be created manually or using *GUID-generator* of Visual Studio (*Tools → GUID Genarator*).
	
2.	Specify ports: add new, rename or delete existing.

3.	If unit has some additionally parameters, than specify them here.

4.	Internal holdups and additional material streams can be defined here.

5.	All other operations, which should take place only once during the unit’s creation.

|

.. code-block:: cpp

	Unit::~Unit()

**Destructor** of the unit: called only once when unit is removed from the flowsheet. Here all memory which has been previously allocated in the constructor should be freed.

|

.. code-block:: cpp

	Unit::Initialize(Time)

Unit‘s **initialization**. This function is called only once at the start of the simulation. Starting from this point, information about defined compounds, phases, distributions, etc. are available for the unit. Here you can create state variables and initialize some additionaly objects (e.g. holdups, material streams or state variables).

In this function, variables of all DAEModels should be specified by using function ``DAEModel::AddDAEVariable()``; connection between ``DAEModel`` and ``DAESolver`` classes should be created by calling function ``DAESolver::SetModel()``.

|

.. code-block:: cpp

	Unit::Simulate(Tstart, Tend)
	
**Dynamic calculation** for a specified time interval. Is called for each time window on simulation interval. Calculation of the defined DAE-system can be run here by calling function ``DAESolver::Calculate()``.

|

.. code-block:: cpp

	Unit::SaveState()
	
For flowsheets containing **recycled streams**, ``SaveState()`` function is called when the convergence on the current time interval is reached, this also ensures the return to the previous state of the unit if convergence fails during the calculation. Here all internal time-dependent variables which weren’t added to the unit by using ``AddStateVariable()``, ``AddMaterialStream()`` or ``AddHoldup()`` functions should be manually saved. Implementation of this function is not obligatory and can be skipped.

|

.. code-block:: cpp

	Unit::LoadState()

**Load last state** of the unit which has been saved with SaveState() function. Implementation of this function is not obligatory and can be skipped.

|

.. code-block:: cpp

	Unit::Finalize()
	
Unit‘s **finalization**. This function is called only once at the end of the simulation. Here one can perform closing and cleaning operations to prepare for the next possible simulation run. Implementation of this function is not obligatory and can be skipped.

|

.. code-block:: cpp

	DAEModel::ResultsHandler()
	
**Handling of results**, which are returned from ``DAESolver`` on each time point. Called by solver every time when the solution in a new time point is ready. 

|

.. code-block:: cpp

	DAEModel::CalculateResiduals()
	
Here the DAE system should be **specified in implicit form**. This function will be called by solver automatically.

|

Configure unit to work with MATLAB
----------------------------------

You can use MATLAB Engine API in Dyssol during the development of solvers. It requires an installed 32-bit version of MATLAB. For API description refer to ... and `C Matrix API <http://de.mathworks.com/help/matlab/cc-mx-matrix-library.html>`_.

To enable interaction with MATLAB configure template project with your unit, do as follows:

1.	Add a new environment variable in Windows with the path to the MATLAB installation directory: 

	*Computer → Properties → Advanced system settings → Environment variables → System variables → New*
	
	Variable Name: ``MATLAB_PATH``.
	
	Variable value: path to installed 32-bit version of MATLAB (e.g. ``C:\Program Files (x86)\MATLAB\R2014b``). It may require restarting the Visual Studio or computer to apply changes.
	
2.	Provide the main project of template solution with path to MATLAB libraries: 

	Select project ``ModelsAPI`` in *solution explorer*, then choose *Project → Properties → Configuration Properties → Environment*, set combo box *Configuration* in the top of the window to position *All Configurations* and provide the *Environment* field with parameter ``PATH=$(MATLAB_PATH)\bin\win32``.
	
3.	Provide unit’s project with the path to MATLAB libraries: 

	Select project with your unit in *solution explorer*, then choose *Project → Properties → Configuration Properties → Environment*, set combo box *Configuration* in the top of the window to position *All Configurations* and provide the *Environment* field with parameter ``PATH=$(MATLAB_PATH)\bin\win32``.
	
4.	Add MATLAB libraries to the unit’s project: 

	Select project with your unit in solution explorer, then choose *Project → Properties → Configuration Properties → Linker → Input → Additional Dependencies*, set combo box *Configuration* in the top of the window to position *All Configurations* and add following four libraries at the beginning of the input field: ``libmx.lib``, ``libmat.lib``, ``libeng.lib``, ``libmex.lib``.
	
5.	Insert MATLAB’s header in ``Unit.h``: add the line ``#include "engine.h"`` to the include section at the top of your ``Unit.h`` file.


|

.. _label-solverDev:

Solver development
==================

You must do the following in order to develop your new solver (plese refer to :ref:`label-VCconfig`):

	1.	Install Microsoft Visual Studio 2015 (Community). 
	
	2.	Configure template project ``VCProject``.

After builiding your own new solvers, the functionality of them can be applied in all units by adding them as unit parameters ((link to "baseUnit")). 

In the current version of Dyssol, only one type of solver is available: :ref:`label-agg-solvers`. 

Basically, all solvers have a set of constant functions and parameters, which are available in each new solver ``link to "external solver"``. and a set of specific ones, which depend on the solver’s type. New types of solvers can be added upon request and will include a set of parameters and functions that are needed to solve a specific problem.

You can implement several solvers of one type (e.g. with different models) and then choose a specific one to use it in unit by user interface, ``section ‘Unit parameters’``.

|

Add new solver to the template project
--------------------------------------

1.	Copy the desired template of the unit from ``<PathToSolution>\VCProject\SolversTemplates`` to the folder ``Solvers`` in solution (``<PathToSolution>\VCProject\Solvers``).

2.	Rename template’s folder according to the name of your new solver (further ``<MySolverFolder>``). The name can be chosen freely.

3.	Rename project files in template’s folder (``*.vcxproj``, ``*.vcxproj.filters``) according to the name of the new solver.

4.	Run the solution file (``<PathToSolution>\Dyssol.sln``) to open it in Visual Studio.

5.	Add project with your new solver to the solution. To do this, select in Visual Studio *File → Add → Existing Project* and specify path to the project file: ``<PathToSolution>\VCProject\Solvers\<MySolverFolder>\<*.vcxproj>``.

6.	Rename added project in Visual Studio according to the name of your solver. 

Now you can implement functionality of your new solver. The list of available functions depends on type of selected solver. 

To build your solution press :kbd:`F7`, to run it in debug mode press :kbd:`F5`. Files with new solvers will be placed to ``<PathToSolution>\VCProject\Debug``.

As debug versions of compiled and built solvers contain a lot of additional information, which is used by Visual Studio to perform debugging, their calculation efficiency can be dramatically low. Thus, for the simulation purposes, solvers should be built in *Release* mode.

|

Configure Dyssol to work with implemented solvers
-------------------------------------------------

1.	Build your solvers in *Release* mode. To do this, open your solution in Visual Studio (run file ``<PathToSolution>\VCProject.sln``), switch *Solution* configuration combo box from the toolbox of Visual Studio from *Debug* to *Release* and build the project (press F7 or choose *Build → Build project* in program menu).

2.	Configure Dyssol by adding the path to new solvers: run Dyssol, choose *Tools → Options → Model manager* and add path to your solvers (``<PathToSolution>\VCProject\Release``).

Now all new developed units will be available in Dyssol.

In general, usual configuration of *Model manager* should include following path for solvers:

	-	``<InstallationPath>\Solvers``: list of standard solvers;
	
	-	``<PathToSolution>\VCProject\SolversDebugLibs``: debug versions of standard solvers;
	
	-	``<PathToSolution>\VCProject\Debug``: debug versions of developed solvers;
	
	-	``<PathToSolution>\VCProject\Release``: release versions of developed solvers.

|

Development of agglomeration solver
-----------------------------------

Please refer to the background information :ref:`label-agg` and :ref:`label-agg-solvers` when necessary.

|

.. code-block:: cpp

	Solver::Solver() 

**Constructor** of the solver: called only once when solver is added to the unit. In this function, a set of parameters should be specified:

1.	Basic info:

	-	``m_solverName``: Name of the solver that will be displayed in Dyssol.

	-	``m_authorName``: Solver’s author.

	-	``m_solverUniqueKey``: Unique identificator of the solver. Simulation environment distinguishes different solvers with the help of this identificator. You must ensure that ID of your solver is unique. This ID can be created manually or using *GUID-generator* of Visual Studio (*Tools → GUID Genarator*).

2.	All operations, which should take place only once during the solver’s creation.

|

.. code-block:: cpp

	Solver::~Solver()

**Destructor** of the solver: called only once when solver is removed from the unit. Here all memory which has been previously allocated in the constructor should be freed.

|

.. code-block:: cpp

	Solver::Initialize(vector<double> grid, double betta0, EKernels kernel, size_t rank, vector<double> params)

Solver‘s **initialization**. This function is called only once for each simulation during the initialization of unit. All operations, which should take place only once after the solver’s creation should be implemented here. Implementation of this function is not obligatory and can be skipped.

|

.. code-block:: cpp

	Solver::Calculate(vector<double> N, vector<double> BRate, vector<double> DRate) 

**Calculation** of birth and death rates depending on particle size distribution. All logic of the solver must be implemented here.

|

.. code-block:: cpp

	Solver::Finalize()

Solver‘s **finalization**. This function is called only once for each simulation during the finalization of unti. Here one can perform closing and cleaning operations to prepare for the next possible simulation run. Implementation of this function is not obligatory and can be skipped.

|

Configure solver to work with MATLAB
------------------------------------

You can use MATLAB Engine API in Dyssol during the development of solvers. It requires an installed 32-bit version of MATLAB. For API description refer to ... and `C Matrix API <http://de.mathworks.com/help/matlab/cc-mx-matrix-library.html>`_.


To enable interaction with MATLAB configure template project with your solver, do as follows:

	1.	Add a new environment variable in Windows with the path to the MATLAB installation directory: 
	
		*Computer → Properties → Advanced system settings → Environment variables → System variables → New*
		
		Variable Name: ``MATLAB_PATH``.
		
		Variable value: path to installed 32-bit version of MATLAB (e.g. ``C:\Program Files (x86)\MATLAB\R2014b``). It may require restarting the Visual Studio or computer to apply changes.
	
	2.	Provide the main project of template solution with path to MATLAB libraries: 
		
		Select project ``ModelsAPI`` in *solution explorer*, then choose *Project → Properties → Configuration Properties → Environment*, set combo box *Configuration* in the top of the window to position *All Configurations* and provide the *Environment* field with parameter ``PATH=$(MATLAB_PATH)\bin\win32``.

	3.	Provide solver’s project with the path to MATLAB libraries: 
		
		Select project with your solver in *solution explorer*, then choose *Project → Properties → Configuration Properties → Environment*, set combo box *Configuration* in the top of the window to position *All Configurations* and provide the *Environment* field with parameter ``PATH=$(MATLAB_PATH)\bin\win32``.
	
	4.	Add MATLAB libraries to the solver’s project: 
	
		Select project with your solver in *solution explorer*, then choose *Project → Properties → Configuration Properties → Linker → Input → Additional Dependencies*, set combo box *Configuration* in the top of the window to position *All Configurations* and add following four libraries at the beginning of the input field: ``libmx.lib``, ``libmat.lib``, ``libeng.lib``, ``libmex.lib``.

	5.	Insert MATLAB’s header in ``Solver.h``: add the line :code:`#include "engine.h"` to the include section at the top of your ``Solver.h`` file.



