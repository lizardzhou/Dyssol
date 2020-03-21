
.. _label-class:

=======
Classes
=======

Here you can get necessary information about how the elements in Dyssol (like unit, solver, :abbr:`PSD (Particle size distribution)` function, matrices) look like on a programming point of view. This is especially important if you want to develope new units and solvers. 

All elements mentioned above are defined as classes, which consists of member variables (data fields) and associated functions (methods). Every new independent instance generated under a class is called an object.

For more information on object-oriented programming appied in Dyssol, please refer to various internet resources about C++.

|

.. _label-baseUnit:

Basic unit
==========

Functions in class ``BaseUnit`` are introduced below.

.. Note:: Some definitions:

	-	**Compound**: a chemical substance defined by particular set of physical properties, calculation methods and data. Examples: water, hydrogen, oxygen.
	
	-	**Phase**: stable collection of compounds with a defined amount of substance and a homogeneous composition. It has an associated state of aggregation, e.g. liquid. 
	
	-	**State of aggregation**: the physical state in which the compounds in that phase occur. Possible state of aggregation: vapor, liquid, solid.

|

Basic information
-----------------

.. code-block:: cpp

	std::string GetUnitName()
	
Returns name of the unit. 

|

.. code-block:: cpp

	std::string GetAuthorName()

Returns name of the unit's author. 

|

.. code-block:: cpp

	std::string GetUniqueID()

Returns unique identifier of the unit. 

|

.. code-block:: cpp

	std::string GetUnitVersion()
	
Returns version of the unit.

|

Ports
-----

.. code-block:: cpp

	unsigned AddPort(std::string PortName, unsigned PortType)

Adds port with ``PortType`` (``INPUT_PORT`` or ``OUTPUT_PORT``) to the unit. Returns index of the port. Should be used in unit’s constructor only. ``PortName`` should be unique within the unit.

|

.. code-block:: cpp

	unsigned GetPortsNumber()
	
Returns number of ports in the unit. 

|

.. code-block:: cpp
	
	unsigned GetPortType(std::string PortName)
	
Returns type of the port with name PortName. Returning values: ``INPUT_PORT``, ``OUTPUT_PORT``. If port with such name has not been defined , ``UNDEFINED_PORT`` will be returned.

|

.. code-block:: cpp

	CMaterialStream* GetPortStream(std::string PortName)
	
Returns pointer to the stream which is connected to the port with name ``PortName``. Returns ``NULL`` if such port has not been defined.

|

Material streams and holdups
----------------------------

.. code-block:: cpp

	CHoldup* AddHoldup(std::string HoldupName, std::string StreamKey = "")
	
Adds new holdup with the specified name HoldupName to the unit. ``HoldupName`` should be unique within the unit. The structure of the holdup will be the same as the global stream’s structure (phases, grids, compounds etc.). 

Should be used in unit’s constructor; then the holdup will be automatically handled by the simulation system (saved and loaded during the simulation, cleared and removed after use). However, it is allowed to add holdups outside the constructor for temporal purposes, but afterwards you have to save, load or remove this holdup manually by calling functions ``SaveState()``, ``LoadState()`` and ``RemoveHoldup()``. Otherwise, all such holdups will be removed at the end of the simulation. 

Returns pointer to a created holdup. This pointer should not be used inside the constructor of the unit, since all changes of the holdup made through this pointer will be cancelled during the initialization of the unit. 

|

.. code-block:: cpp

	CHoldup* GetHoldup(std::string HoldupName)
	
Returns pointer to a holdup with the specified name ``HoldupName``. This pointer should not be used inside the constructor of the unit, since all changes of the holdup made through this pointer will be cancelled during the initialization of the unit. ``NULL`` will be returned if such holdup has not been defined.

|

.. code-block:: cpp

	std::vector<CHoldup*> GetHoldups()
	
Returns pointers to all holdups of the unit. These pointers should not be used inside the constructor of the unit, since all changes of the holdup made through them will be cancelled during the initialization of the unit.

|

.. code-block:: cpp

	void RemoveHoldup(std::string HoldupName)
	
Removes holdup with the specified name HoldupName from the unit. Should be used only for those holdups, which have been added to the unit outside the constructor.

|

.. code-block:: cpp

	CMaterialStream* AddMaterialStream(std::string StreamName, std::string StreamKey = "") 
	
Adds new material stream with the specified name ``StreamName`` for internal temporary use to the unit. ``StreamName`` should be unique within the unit. Structure of the stream will be the same as the global stream’s structure (phases, grids, compounds etc.). 

Should be used in unit’s constructor; then the material stream will be automatically handled by the simulation system, this means saved and loaded during the simulation, cleared and removed after use. However, it is allowed to add material outside the constructor for temporal purposes, but you have to save, load or remove this stream manually by calling functions ``SaveState()``, ``LoadState()`` and ``RemoveMaterialStream()``. Otherwise, all such material streams will be removed at the end of the simulation.

Returns pointer to a created material stream. 

|

.. code-block:: cpp

	CMaterialStream* GetMaterialStream(std::string StreamName)
	
Returns pointer to a material stream with specified name ``StreamName``. ``NULL`` will be returned if such stream has not been defined.

|

.. code-block:: cpp

	void RemoveMaterialStream(std::string StreamName)
	
Removes material stream with the specified name ``StreamName`` from the unit. Should be used only for those material streams, which have been added to the unit outside the constructor.

|

.. code-block:: cpp

	CMaterialStream* AddFeed(std::string FeedName, std::string StreamKey = "")
	
Adds new feed stream with the name ``FeedName`` to the unit. ``FeedName`` should be unique within the unit. The structure of the stream will be the same as the global stream structure (phases, grids, compounds etc.). 

Should be used only in constructor of the unit describing the feed. Returns pointer to a created stream. This pointer should not be used inside the constructor of the unit, since all changes of the stream made through this pointer will be cancelled during the initialization of the unit.

|

.. code-block:: cpp

	CMaterialStream* GetFeed(std::string FeedName)
	
Returns pointer to a feed with specified name ``FeedName``. This pointer should not be used inside the constructor of the unit, since all changes of the stream made through this pointer will be cancelled during the initialization of the unit. ``NULL`` will be returned if such feed has not been defined.

|

.. code-block:: cpp

	void CopyStreamToStream(CMaterialStream* SrcStream, CMaterialStream DstStream, double Time, bool DeleteDataAfter = true)
	
Copies all data from ``SrcStream`` to ``DestStream`` for specified time point. If flag ``DeleteDataAfter`` is set to true, all data after the time point ``Time`` in the destination stream will be removed before being copied.

|

.. code-block:: cpp

	void CopyStreamToStream(CMaterialStream* SrcStream, CMaterialStream DstStream, double StartTime, double EndTime, bool DeleteDataAfter = true)
	
Copies all data from ``SrcStream`` to ``DstStream`` on a specified time interval. If flag ``DeleteDataAfter`` is set to true, all data after the time point ``StartTime`` in the destination stream will be removed before being copied.

|

.. code-block:: cpp

	void CopyStreamToPort(CMaterialStream* Stream, std::string PortName, double Time, bool DeleteDataAfter = true)
	
Copies all data from ``Stream`` to a stream connected to an output port ``PortName`` for specified time point. If flag ``DeleteDataAfter`` is set to true, all data after the time point ``Time`` in the destination stream will be removed before being copied.

|

.. code-block:: cpp

	void CopyStreamToPort(CMaterialStream* Stream, std::string PortName, double StartTime, double EndTime, bool DeleteDataAfter = true)
	
Copies all data from ``Stream`` to a stream connected to an output port ``PortName`` for specified time interval. If flag ``DeleteDataAfter`` is set to true, all data after the time point ``StartTime`` in the destination stream will be removed before being copied.

|

.. code-block:: cpp

	void CopyPortToStream(std::string PortName, CMaterialStream* Stream, double Time, bool DeleteDataAfter = true)
	
Copies stream’s data of the input port ``PortName`` to another stream ``Stream`` for specified time point. If flag ``DeleteDataAfter`` is set to true, all data after the time point Time in the destination stream will be removed before being copied.

|

.. code-block:: cpp

	void CopyPortToStream(std::string PortName, CMaterialStream* Stream, double StartTime, double EndTime, bool DeleteDataAfter = true)
	
Copies stream’s data of the input port ``PortName`` to another stream ``Stream`` for specified time interval. If flag ``DeleteDataAfter`` is set to true, all data after the time point ``StartTime`` in the destination stream will be removed before being copied.

|

.. code-block:: cpp

	double CalcTemperatureFromProperty(ECompoundTPProperties Property, std::vector<double> CompoundFractions, double Value)
	
Returns temperature of a generic system of composition ``CompoundFractions`` for a specific value ``Value`` of the property ``Property``. Possible properties are those defined in :ref:`material database <label-materialDataDetailed>`. For more information, please refer to :ref:`label-thermo` on this page.

|

.. code-block:: cpp

	double CalcPressureFromProperty(ECompoundTPProperties Property, std::vector<double> CompoundFractions, double Value)
	
Returns pressure of a generic system of composition ``CompoundFractions`` for a specific value ``Value`` of the property ``Property``. Possible properties are those defined in :ref:`material database <label-materialDataDetailed>`. For more information, please refer to :ref:`label-thermo` on this page.

|

.. code-block:: cpp

	void HeatExchange(CMaterialStream* Stream1, CMaterialStream* Stream2, double Time, double Efficiency);
	
Performs a heat exchange between material streams ``Stream1`` and ``Stream2`` at specified time point ``Time`` with a specified efficiency ``Efficiency`` ranging between 0 and 1. For more information, please refer to :ref:`label-thermo` on this page.

|

Time points
-----------

.. code-block:: cpp

	std::vector<double> GetAllInputTimePoints(double StartTime, double EndTime, bool ForceStartBoundary = false, bool ForceEndBoundary = false)

Returns all time points on which input streams of the unit are defined for specified time interval. Input streams are all streams connected to the input ports. If ``ForceStartBoundary`` and/or ``ForceEndBoundary`` flag is enabled, corresponding boundary points will be forcibly added to the resulting vector.

|

.. code-block:: cpp

	std::vector<double> GetAllDefinedTimePoints(double StartTime, double EndTime, bool ForceStartBoundary = false, bool ForceEndBoundary = false)

Returns all time points for specified time interval on which input streams and unit parameters are defined. Input streams are all streams connected to the input ports. If ``ForceStartBoundary`` and/or ``ForceEndBoundary`` flag is enabled, corresponding boundary points will be forcibly added to the resulting vector.

|

.. code-block:: cpp

	std::vector<double> GetAllStreamsTimePoints(std::vector<CMaterialStream*> Srteams, double StartTime, double EndTime)

Returns all time points for specified time interval on which ``Stream``-s are defined. 

|

Unit parameters
---------------

.. code-block:: cpp

	unsigned AddConstParameter(std::string Name, double MinValue, double MaxValue, double InitValue, std::string Description = "")

Adds new constant unit parameter to the unit with the name ``Name``, boundary values ``MinValue`` and ``MaxValue``, description ``Description``, and initializes it with the value ``InitValue``. The name of the parameter should be unique within the unit. 

Should be used in unit’s constructor only. Returns index of the parameter. 

|

.. code-block:: cpp

	unsigned AddTDParameter(std::string Name, double MinValue, double MaxValue, double InitValue, std::string Description = "")

Adds new time-dependent unit parameter to the unit with the name ``Name``, boundary values ``MinValue`` and ``MaxValue``, description ``Description``, and initializes it in the time point 0 s with the value ``InitValue``. The name of the parameter should be unique within the unit. 

Should be used in unit’s constructor only. Returns index of the parameter. 

|

.. code-block:: cpp

	unsigned AddStringParameter(std::string Name, std::string InitValue = "", std::string Description = "")

Adds new string unit parameter to the unit with the name ``Name``, description ``Description``, and initializes it with the value ``InitValue``. The name of the parameter should be unique within the unit. 

Should be used in unit’s constructor only. Returns index of the parameter. 

|

.. code-block:: cpp

	unsigned AddSolverAggregation(std::string Name, std::string Description = "")

Adds new solver parameter of type ``SOLVER_AGGREGATION_1`` with name ``Name`` and description ``Description``. Allows choosing a specific solver with current type to use it in unit. The name of the parameter should be unique within the unit. 

Should be used in unit’s constructor only. Returns index of the parameter.

|

.. code-block:: cpp

	unsigned GetParametersNumber()

Returns number of unit parameters which have been defined in the unit. 

|

.. code-block:: cpp

	std::string GetParameterName(unsigned Index)

Returns name of the unit parameter with the specified index Index. Empty string is returned if such parameter has not been defined.

|

.. code-block:: cpp

	double GetParameterMinVal(std::string ParameterName)
	
Returns minimum allowable value of the time-dependent or constant unit parameter with the name ``ParameterName``. Returns ``0`` if such parameter has not been defined or this is not a constant or time-dependent parameter.

|

.. code-block:: cpp

	double GetParameterMaxVal(std::string ParameterName)

Returns maximum allowable value of the time-dependent or constant unit parameter with the name ``ParameterName``. Returns ``0`` if such parameter has not been defined or this is not a constant or time-dependent parameter.

|

.. code-block:: cpp

	double GetConstParameterValue(std::string ParameterName)
	
Returns value of a constant unit parameter with the name ``ParameterName``. Returns ``0`` if such constant parameter has not been defined.

|

.. code-block:: cpp

	double GetTDParameterValue(std::string ParameterName, double Time)

Returns value of a time-dependent unit parameter with the name ``ParameterName`` at the specified time point ``Time``. If the parameter is not defined at ``Time``, linear interpolation will be used to obtain the value. Returns ``0`` if such time-dependent parameter has not been defined.

|

.. code-block:: cpp

	std::string GetStringParameterValue(std::string ParameterName)
	
Returns value of a string unit parameter. If such string parameter has not been defined, empty string will be returned.

|

.. code-block:: cpp

	CAggregationSolver* GetSolverAggregation(std::string ParameterName)
	
Returns pointer to a chosen solver of ``SOLVER_AGGREGATION_1`` type. Returns ``nullptr`` if such unit parameter has not been defined.

|

.. code-block:: cpp

	void SetParameterValue(std::string ParameterName, double Value, double Time = 0)

Sets ``Value`` of a constant or a time dependent unit parameter in the specified time point ``Time``. If the time point does not exist, it will be created. If the time point already exists, its value will be overwritten.

|

.. code-block:: cpp

	void SetParameterValue(std::string ParameterName, std::string Value)
	
Sets new ``Value`` of a string unit parameter with the specified name ``ParameterName``. 

|

.. code-block:: cpp

	std::vector<double> GetParameterTimePoints(std::string ParameterName, double TimeStart, double TimeEnd)
	
Returns all time points for which time-dependent parameter is defined within the specified time interval [``TimeStart``; ``TimeEnd``]. Returns empty vector if such parameter has not been defined or this is not a time-dependent parameter.

|

State variables
---------------

.. code-block:: cpp

	unsigned AddStateVariable(std::string Name, double InitValue, bool SaveHistory = false)
	
Adds new state variable and initializes it with ``InitValue``. Name must be unique within the unit’s state variables. Parameter ``SaveHistory`` specifies if the history of all changes of variable should be saved during calculation for further post-processing. To save history, function ``SaveStateVariables()`` should be called. All variables which are added with this function will be automatically saved and restored during the simulation. Should be used in ``Initialize()`` function of the unit. Returns index of added variable.

|

.. code-block:: cpp

	unsigned GetStateVariablesNumber()	

Returns number of state variables which have been defined in this unit. 

|

.. code-block:: cpp

	std::string GetStateVariableName(unsigned Index)
	
Returns the name of the state variable with specified index. Returns empty string if such variable has not been defined.

|

.. code-block:: cpp

	double GetStateVariable(std::string Name)
	
Returns value of internal state variable with name Name. Returns ``0`` if such variable has not been defined.

|

.. code-block:: cpp

	void SetStateVariable(std::string Name, double Value)
	
Sets new value ``Value`` of a state variable ``Name``. 

|

.. code-block:: cpp

	void ClearStateVariables()
	
Removes all state variables and history of their changes. 

|

.. code-block:: cpp

	void SaveStateVariables(double Time)
	
Saves values of those internal variables defined as having history at the current time ``Time``.

|

Compounds
---------

.. code-block:: cpp

	std::vector<std::string> GetCompoundsList()
	
Returns unique keys of all compounds defined in the current flowsheet. 

|

.. code-block:: cpp

	std::vector<std::string> GetCompoundsNames()

Returns names of all compounds defined in the current flowsheet. 

|

.. code-block:: cpp

	unsigned GetCompoundsNumber()

Returns number of compounds which are defined in the current flowsheet. 

|

.. code-block:: cpp

	double GetCompoundConstant(std::string CompoundKey, unsigned Constant)

Returns value of constant physical property for specified compound. These properties are stored in the database of materials. Possible constants are listed below:

	-	``SOA_AT_NORMAL_CONDITIONS``
	
	-	``NORMAL_BOILING_POINT``
	
	-	``NORMAL_FREEZING_POINT``
	
	-	``CRITICAL_TEMPERATURE``
	
	-	``CRITICAL_PRESSURE``
	
	-	``MOLAR_MASS``
	
	-	``STANDARD_FORMATION_ENTHALPY``
	
	-	``HEAT_OF_FUSION_AT_NORMAL_FREEZING_POINT``
	
	-	``HEAT_OF_VAPORIZATION_AT_NORMAL_BOILING_POINT``
	
	-	``REACTIVITY_TYPE``
	
	-	``CONST_PROP_USER_DEFINED_XX``

.. Note:: Definition of constant properties:

	+----------------------------------------------------+------------------------------------------------+----------+
	|   Definition in class                              |   Name                                         |   Units  |
	+====================================================+================================================+==========+
	|   ``SOA_AT_NORMAL_CONDITIONS``                     |   State of aggregation at normal conditions    |   [-]    |
	+----------------------------------------------------+------------------------------------------------+----------+
	|   ``NORMAL_BOILING_POINT``                         |   Normal boiling point                         |   [K]    |
	+----------------------------------------------------+------------------------------------------------+----------+
	|   ``NORMAL_FREEZING_POINT``                        |   Normal freezing point                        |   [K]    |
	+----------------------------------------------------+------------------------------------------------+----------+
	|   ``CRITICAL_TEMPERATURE``                         |   Critical temperature                         |   [K]    |
	+----------------------------------------------------+------------------------------------------------+----------+
	|   ``CRITICAL_PRESSURE``                            |   Critical pressure                            |   [Pa]   |
	+----------------------------------------------------+------------------------------------------------+----------+
	|   ``MOLAR_MASS``                                   |   Molar mass                                   | [kg/mol] |
	+----------------------------------------------------+------------------------------------------------+----------+
	|   ``STANDARD_FORMATION_ENTHALPY``                  |   Standard formation enthalpy                  | [J/mol]  |
	+----------------------------------------------------+------------------------------------------------+----------+
	|   ``HEAT_OF_FUSION_AT_NORMAL_FREEZING_POINT``      |   Heat of fusion at normal freezing point      | [J/mol]  |
	+----------------------------------------------------+------------------------------------------------+----------+
	|   ``HEAT_OF_VAPORIZATION_AT_NORMAL_BOILING_POINT`` |   Heat of vaporization at normal boiling point | [J/mol]  |
	+----------------------------------------------------+------------------------------------------------+----------+
	|   ``REACTIVITY_TYPE``                              |   Reactivity type                              |   [-]    |
	+----------------------------------------------------+------------------------------------------------+----------+
	|   ``CONST_PROP_USER_DEFINED_XX``                   |   User defined property                        |   [-]    |
	+----------------------------------------------------+------------------------------------------------+----------+

|

.. code-block:: cpp

	double GetCompoundTPDProp(std::string CompoundKey, unsigned Property, double Temperature, double Pressure)

Returns value of temperature/pressure-dependent physical properties stored in the :ref:`material database <label-materialDataDetailed>` for compound with specified ``Temperature`` [K] and ``Pressure`` [Pa]. Possible properties are listed below:

	-	``DENSITY``
	
	-	``HEAT_CAPACITY_CP``
	
	-	``ENTHALPY``

	-	``VAPOR_PRESSURE``

	-	``VISCOSITY``

	-	``THERMAL_CONDUCTIVITY``
	
	-	``PERMITTIVITY``

	-	``TP_PROP_USER_DEFINED_XX``
	
.. Note:: Definition of emperature-dependent properties:

	+-----------------------------+------------------------+--------------------+
	| Define                      |   Name                 |   Units            |
	+=============================+========================+====================+
	|   ``DENSITY``               |   Density              |   [kg/m :math:`^3`]|
	+-----------------------------+------------------------+--------------------+
	|   ``HEAT_CAPACITY``         |   Heat                 |  [J/(kg·K)]        |
	+-----------------------------+------------------------+--------------------+
	|   ``ENTHALPY``              |   Enthalpy             |  [J/kg]            |
	+-----------------------------+------------------------+--------------------+
	|  ``VAPOR_PRESSURE``         |   Vapor                |  [Pa]              |
	+-----------------------------+------------------------+--------------------+
	|  ``VISCOSITY``              |   Viscosity            |  [Pa·s]            |
	+-----------------------------+------------------------+--------------------+
	|  ``THERMAL_CONDUCTIVITY``   |   Thermal              |  [W/(m·K)]         |
	+-----------------------------+------------------------+--------------------+
	| ``PERMITTIVITY``            |   Permittivity         |  [F/m]             |
	+-----------------------------+------------------------+--------------------+
	| ``TP_PROP_USER_DEFINED_XX`` |   User                 |  [-]               |
	+-----------------------------+------------------------+--------------------+

|

.. code-block:: cpp

	double GetCompoundsInteractionProp(std::string CompoundKey1, std::string CompoundKey2, unsigned Property, double Temperature, double Pressure)
	
Returns the value of the interaction property for selected compounds under the specified ``Temperature`` [K] and ``Pressure`` [Pa]. These properties are stored in the database of materials. Possible properties are listed below:

	-	``INTERFACE_TENSION``
	
	-	``INT_PROP_USER_DEFINED_XX``
	
.. Note::Definition of interaction properties between two pure compounds:

	+------------------------------+-------------------------+---------+
	| Define                       |   Name                  |  Units  |
	+==============================+=========================+=========+
	| ``INTERFACE_TENSION``        |   Interface tension     |  [N/m]  |
	+------------------------------+-------------------------+---------+
	| ``INT_PROP_USER_DEFINED_XX`` |   User defined property |  [-]    |
	+------------------------------+-------------------------+---------+

|

.. code-block:: cpp

	bool IsCompoundNameDefined(std::string CompoundName)
	
Returns ``true`` if compound with specified name has been defined, otherwise returns ``false``.

|

.. code-block:: cpp

	bool IsCompoundKeyDefined(std::string CompoundKey)
	
Returns ``true`` if compound with specified unique key has been defined, otherwise returns ``false``.

|

Phases
------

.. code-block:: cpp

	unsigned GetPhasesNumber()
	
Returns number of phases which are currently defined in the flowsheet. 

|

.. code-block:: cpp

	std::string GetPhaseName(unsigned PhaseType)

Returns name of the specified phase. Possible values of ``PhaseType`` are: 
	
	- ``SOA_SOLID`` 
	
	- ``SOA_LIQUID``
	
	- ``SOA_LIQUID2``
	
	- ``SOA_VAPOR``
	
Returns empty string if such phase has not been defined.

|

.. code-block:: cpp

	unsigned GetPhaseSOA(unsigned PhaseIndex)
	
Returns state of aggregation for the phase with index ``PhaseIndex``. Returns ``SOA_UNDEFINED`` if the phase with specified index doesn’t exist.

|

.. code-block:: cpp

	size_t GetPhaseIndex(unsigned PhaseType)
	
Returns index of the specified phase. Returns ``-1`` if such phase has not been defined. Possible values of PhaseType are: 
	
	- ``SOA_SOLID`` 
	
	- ``SOA_LIQUID``
	
	- ``SOA_LIQUID2``
	
	- ``SOA_VAPOR``

|

.. code-block:: cpp

	bool IsPhaseDefined(unsigned PhaseType)
	
Returns ``true`` if such phase has been defined in the flowsheet. Possible values of PhaseType are: 

	- ``SOA_SOLID`` 
	
	- ``SOA_LIQUID``
	
	- ``SOA_LIQUID2``
	
	- ``SOA_VAPOR``

|

.. code-block:: cpp

	unsigned GetLiquidPhasesNumber()
	
Returns number of defined liquid phases.

|

Solid distributed properties
----------------------------

.. code-block:: cpp

	std::vector<EDistrTypes> GetDistributionsTypes()

Returns list of types of solid distributions, which have been defined in the flowsheet. Possible types are:

	-	``DISTR_COMPOUNDS``
	
	-	``DISTR_SIZE``
	
	-	``DISTR_PART_POROSITY``
	
	-	``DISTR_FORM_FACTOR``
	
	-	``DISTR_COLOR``

|

.. code-block:: cpp

	std::vector<unsigned> GetDistributionsClasses()
	
Returns list with number of classes for all defined distributions.

|

.. code-block:: cpp

	unsigned GetDistributionsNumber()
	
Retunrs number of solids distributions, which have been defined in the flowsheet.

|

.. code-block:: cpp

	EGridTypes GetDistributionGridType(EDistrTypes distrType)
	
Returns grid’s type, which was defined for specified solid distribution ``distrType``. Possible values are:

	-	``GRID_NUMERIC``
	
	-	``GRID_SYMBOLIC``
	
	-	``GRID_UNDEFINED``

|

.. code-block:: cpp

	std::vector<double> GetNumericGrid(EDistrTypes distrType)
	
Returns grid of classes for specified solid distribution for *Numeric grid*. Returns empty vector if this distribution has *Symbolic grid*.

|

.. code-block:: cpp

	std::vector<std::string> GetSymbolicGrid(EDistrTypes distrType)
	
Returns grid of classes for specified solid distribution for *Symbolic grid*. Retunrs empty vector if this distribution has *Numeric grid*.

|

.. code-block:: cpp

	unsigned GetClassesNumber(EDistrTypes distrType)
	
Returns number of classes for specified solid distribution. Returns ``0`` if such distribution has not been defined.

|

.. code-block:: cpp

	std::vector<double> GetClassesMeans(EDistrTypes distrType)
	
Returns means of classes for specified solid distribution with *Numeric grid*. Retunrs empty vector if such distribution has not been defined or has *Symbolic grid*.

|

.. code-block:: cpp

	std::vector<double> GetPSDGridDiameters()
	
Returns size grid for particle diameters. Returns empty vector if ``DISTR_SIZE`` distribution has not been defined.

|

.. code-block:: cpp

	std::vector<double> GetPSDGridVolumes()
	
Returns size grid for particle volumes. Returns empty vector if ``DISTR_SIZE`` distribution has not been defined.

|

.. code-block:: cpp

	std::vector<double> GetPSDMeanDiameters()
	
Returns mean particle diameters. Returns empty vector if ``DISTR_SIZE`` distribution has not been defined.

|

.. code-block:: cpp

	std::vector<double> GetPSDMeanSurfaces()
	
Returns mean particle surfaces. Returns empty vector if ``DISTR_SIZE`` distribution has not been defined. 

|

.. code-block:: cpp

	std::vector<double> GetPSDMeanVolumes()
	
Returns mean particle volumes. Returns empty vector if ``DISTR_SIZE`` distribution has not been defined. 

|

.. code-block:: cpp

	std::vector<double> GetClassesSizes(EDistrTypes distrType)
	
Returns sizes of classes for specified solid distribution with *Numeric grid*. Returns empty vector if such distribution has not been defined or has *Symbolic grid*.

|

.. code-block:: cpp

	bool IsDistributionDefined(EDistrTypes distrType)
	
Returns ``ture`` if such solids distribution has been defined in the flowsheet.

|

.. code-block:: cpp

	void CalculateTM(EDistrTypes distrType, std::vector<double> InDistr, std::vector<double> OutDistr, CTransformMatrix &outTM)
	
Calculates transformation matrix for one-dimensional distribution with type ``distrType`` according to input and output distributions. Obtained matrix can be applied to the stream instead of direct setting of distribution to retain secondary dimensions in multidimensional distribution.

Following algorithm is applied to setup transformation matrix:

	1.	Go through the classes of source and target distributions from left to right. 
	
	2.	The mostleft notempty class of the initial distribution proceeds to the mostleft notempty class of the output distribution.
	
	3.	Transition to the next class of the initial distribution is performed if the current class is completely transferred to the output distribution. 
	
	4.	Transition to the next class of the output distribution is performed if the current class is already full.

|

Tolerance
---------

.. code-block:: cpp

	double GetAbsTolerance()

Returns absolute tolerance, which has been defined for the flowsheet. 

|

.. code-block:: cpp

	double GetRelTolerance()

Returns relative tolerance, which has been defined for the flowsheet.

|

Errors and warnings
-------------------

.. code-block:: cpp

	void RaiseError(std::string Description = "")
	
Can be called to indicate that an error occurred. Description will be displayed in the simulation’s log and simulation will be stopped after setting.

|

.. code-block:: cpp

	void RaiseWarning(std::string Description = "")
	
Can be called to indicate warning. Description will be displayed in the simulation’s log and simulation will not be stopped.

|

.. code-block:: cpp

	void ShowInfo(std::string Description)

Can be called to write out messages to the simulation’s log screen during the simulation. Description will be displayed in the simulation’s log.

|

Plots
-----

.. code-block:: cpp

	int AddPlot(std::string PlotName, std::string XAxisName, std::string YAxisName)

Adds new 2-dimensional plot with specified name and axis, returns index of the plot. ``PlotName`` must be unique within the unit’s plots. Returns ``-1`` on error.

|

.. code-block:: cpp

	int AddPlot(std::string PlotName, std::string XAxisName, std::string YAxisName, std::string ZAxisName)

Adds new 3-dimensional plot with specified name and axis, returns index of the plot. ``PlotName`` must be unique within the unit’s plots. Returns ``-1`` on error.

|

.. code-block:: cpp

	int AddCurveOnPlot(std::string PlotName, std::string CurveName)

Adds new curve with specified name on the 2-dimensional plot with name ``PlotName``. Returns index of the curve within specified plot. Returns ``-1`` on error.

|

.. code-block:: cpp

	int AddCurveOnPlot(std::string PlotName, double ZValue)

Adds new curve with specified z-value on the 2- or 3-dimensional plot with name ``PlotName``. Returns index of the curve within specified plot. Returns ``-1`` on error.

|

.. code-block:: cpp

	void AddPointOnCurve(std::string PlotName, std::string CurveName, double X, double Y)

Adds new point on specified curve for 2-dimensional plot.

|

.. code-block:: cpp

	void AddPointOnCurve(std::string PlotName, double ZValue, double X, double Y)

Adds new point on specified curve for 3-dimensional plot

|

.. code-block:: cpp

	void AddPointOnCurve(std::string PlotName, std::string CurveName, std::vector<double> X, std::vector<double> Y)

Adds new points on specified curve for 2-dimensional plot.

|

.. code-block:: cpp

	void AddPointOnCurve(std::string PlotName, double ZValue, std::vector<double> X, std::vector<double> Y)

Adds new points on specified curve for 3-dimensional plot.

|

Virtual functions
-----------------

Virtual function is declared within the base class, which you can re-define in your derived class.

|

.. code-block:: cpp

	void Simulate(double Time)
	
Calculates unit on a specified time point ``Time`` for **steady-state units**. Is called by the suimulator iteratively for all time points for which this unit should be calculated. All logic of the unit’s model must be implemented here.

|

.. code-block:: cpp

	void Simulate(double StartTime, double EndTime)
	
Calculates unit for specified time interval for **dynamic units**. Is called by the suimulator iteratively for all time points for which this unit should be calculated. All logic of the unit’s model must be implemented here.

|

.. code-block:: cpp

	void Initialize(double Time)
	
Initializes unit at the time point Time. Is called by the simulator only once at the start of the simulation. Here some additionaly objects can be initialized (for example holdups, material streams or state variables).

|

.. code-block:: cpp

	void SaveState()

Saves current state of the unit. All time dependent variables which weren’t added to the unit be manually saved here with the help of ``AddStateVariable()``, ``AddMaterialStream()`` or ``AddHoldup()``. 

For flowsheets containing **recycled streams**, this function is called when the convergence on the current time interval is reached, this also ensures the return to the previous state of the unit if convergence fails during the calculation.

|

.. code-block:: cpp

	void LoadState()

Loads last saved state of the unit. All time dependent variables which were previously saved in ``SaveState()`` function should be manually loaded here. 

|

.. code-block:: cpp

	void Finalize()

Finalizes unit. Is called by the simulator only once at the end of the simulation. Here closing and cleaning operations can be performed.

|

.. _label-stream:

Stream
======

Functions in classes ``CMaterialStream`` and ``CHoldup`` are introduced below.

|

Basic stream properties
-----------------------

All functions in this section are for both ``CMaterialStream`` and ``CHoldup``.

.. code-block:: cpp

	std::string GetStreamName()
	
Returns the name of the material stream / holdup. 

|

.. code-block:: cpp

	void SetStreamName(std::string Name)
	
Sets the name of the material stream / holdup. 

|

Time points
-----------

All functions in this section are for both ``CMaterialStream`` and ``CHoldup``.

.. code-block:: cpp

	void AddTimePoint(double Time, double SourceTime = -1)
	
Adds new time point ``Time`` to the material stream / holdup. Data for this time point is copied from ``SourceTime``. By default (``SourceTime = -1``) data will be copied from the previous time point. If this is the first time point in the material stream / holdup, all data will be set to ``0``. If such time point already exists, nothing will be done.

|

.. code-block:: cpp

	void RemoveTimePoint(double Time)
	
Removes time point ``Time`` from the material stream / holdup, if such point exists. 

|

.. code-block:: cpp

	void RemoveTimePoints(double Start, double End)
	
Removes all time points from the specified interval, including boundaries.

|

.. code-block:: cpp

	void RemoveTimePointsAfter(double Start, bool IncludeStart = false)
	
Removes all data after the specified time point including (if ``IncludeStart`` is set to ``true``) or excluding (``IncludeStart`` is set to ``false``) point ``Start``.

|


.. code-block:: cpp

	std::vector<double> GetAllTimePoints()

Returns all time points which are defined in the material stream / holdup. 

|


.. code-block:: cpp

	std::vector<double> GetTimePointsForInterval(double Start, double End, bool ForceInclBoudaries = false)
	
Returns the list of time points for the specified time interval (incl. boundary points ``Start`` and ``End``). If ``ForceInclBoudaries`` is set to ``true``, resulting vector will contain boundary points even if they have not been defined in the material stream / holdup.

|


.. code-block:: cpp

	double GetLastTimePoint()

Returns last defined time point in the material stream / holdup. Returns ``-1`` if no time points have been defined.

|

.. code-block:: cpp

	double GetPreviousTimePoint(double Time)
	
Returns the nearest time point before ``Time``. Returns ``-1`` if there is no time points before the specified value.

|

Overall properties
------------------

.. _label-massFlow:

.. code-block:: cpp

	double GetMassFlow(double Time, unsigned Basis)
	
Specific function for ``CMaterialStream``. 

Returns mass or mole flow of the material stream at the specified time point ``Time``. If such time point has not been defined, interpolation of data will be done. 

``Basis`` is a basis of results (``BASIS_MASS`` in [kg/s] or ``BASIS_MOLL`` in [mol/s]):

``BASIS_MASS``: :math:`\dot m` in [kg/s], total mass flow of the material stream.

``BASIS_MOLL``: :math:`\sum\limits_i \dfrac{\dot m \cdot w_i}{M_i}` in [mol/s], with :math:`w_i` mass fraction of the phase :math:`i`, and :math:`M_i` molar mass of the phase :math:`i`.

|

.. _label-mass:

.. code-block:: cpp

	double GetMass(double Time, unsigned Basis) 
	
Specific function for ``CHoldup``. 

Returns mass or mole of the holdup at the specified time point ``Time``. If such time point has not been defined, interpolation of data will be done. 

``Basis`` is a basis of results (``BASIS_MASS`` in [kg] or ``BASIS_MOLL`` in [mol]):

``BASIS_MASS``: :math:`m` in [kg], total mass of the holdup.

``BASIS_MOLL``: :math:`\sum\limits_i \dfrac{m \cdot w_i}{M_i}` in [mol], with :math:`w_i` mass fraction of the phase :math:`i`, and :math:`M_i` molar mass of the phase :math:`i`.

|

.. _label-setMassFlow:

.. code-block:: cpp

	void SetMassFlow(double Time, double Value, unsigned Basis)
	
Specific function for ``CMaterialStream``. 
	
Sets mass flow of the material stream at the time point ``Time``. Negative values before setting will be converted to ``0``. If the time point Time has not been defined in the material stream, then the value will not be set. 

``Basis`` is a basis of results (``BASIS_MASS`` in [kg/s] or ``BASIS_MOLL`` in [mol/s]):

``BASIS_MASS``: :math:`\dot{m} =` ``Value`` in [kg/s], total mass flow of the material stream.

``BASIS_MOLL``: :math:`\dot{n} =` ``Value`` :math:`\cdot \sum\limits_i M_i \cdot w_i` in [mol/s], with :math:`w_i` mass fraction of the phase :math:`i`, and :math:`M_i` molar mass of the phase :math:`i`.

|

.. _label-setMass:

.. code-block:: cpp

	void SetMass(double Time, double Value, unsigned Basis)
	
Specific function for ``CHoldup``. 

Sets mass of the holdup at the time point ``Time``. Previously set negative values will be converted to ``0``. If the time point ``Time`` has not been defined in the holdup, then the value will not be set. 

``Basis`` is a basis of results (``BASIS_MASS`` in [kg] or ``BASIS_MOLL`` in [mol]):

``BASIS_MASS``: :math:`m =` ``Value`` in [kg], total mass of the holdup.

``BASIS_MOLL``: :math:`n =` ``Value`` :math:`\cdot \sum\limits_i M_i \cdot w_i` in [mol], with :math:`w_i` mass fraction of the phase :math:`i`, and :math:`M_i` molar mass of the phase :math:`i`.

|

.. _label-temp:

.. code-block:: cpp

	double GetTemperature(double Time)
	
Function for both ``CMaterialStream`` and ``CHoldup``.

Returns temperature of the material stream / holdup at the specified time point ``Time`` in [K]. If such time point has not been defined, interpolation of data will be done.

|

.. _label-setTemp:

.. code-block:: cpp

	void SetTemperature(double Time, double Value)

Function for both ``CMaterialStream`` and ``CHoldup``.

Sets temperature of the material stream / holdup at the time point ``Time`` in [K]. Negative values before setting will be converted to ``0``. If time the point ``Time`` has not been defined in the material stream / holdup, the value will not be set.

|

.. _label-pressure:

.. code-block:: cpp

	double GetPressure(double Time)
	
Function for both ``CMaterialStream`` and ``CHoldup``.

Returns pressure of the material stream / holdup at the specified time point ``Time`` in [Pa]. If such time point has not been defined, interpolation of data will be done.

|

.. _label-setPressure:

.. code-block:: cpp

	void SetPressure(double Time, double Value)

Function for both ``CMaterialStream`` and ``CHoldup``.

Sets pressure of the material stream / holdup in the time point ``Time`` in [Pa]. Negative values before setting will be converted to ``0``. If the time point ``Time`` has not been defined in the material stream / holdup, the value will not be set.

|

.. code-block:: cpp

	double GetOverallProperty(double Time, unsigned Property, unsigned Basis)
		
Returns non-constant physical property value for the overall mixture at the specified time point ``Time``. If such time point has not been defined, interpolation of data will be done. 

``Basis`` is a basis of results (``BASIS_MASS`` or ``BASIS_MOLL``).

``Property`` is an identifier of a physical property. Available properties are:

	-	``FLOW`` and ``TOTAL_FLOW`` for material stream ``CMaterialStream``: refer to function :ref:`getMassFlow <label-massFlow>`.
	
	-	``MASS`` and ``TOTAL_MASS`` for holdup ``CHoldup``: refer to function :ref:`getMass <label-mass>`.
	
	-	``TEMPERATURE``: refer to function :ref:`getTemperature <label-temp>`.
	
	-	``PRESSURE``: refer to function :ref:`getPressure <label-pressure>`.
	
	-	``MOLAR_MASS``: :math:`\sum\limits_i M_i \cdot w_i`, with :math:`M` molar mass of the total flow, :math:`w_i` mass fraction of the phase :math:`i`, and :math:`M_i` molar mass of the phase :math:`i`.
	
	-	``ENTHALPY``:
	
		- Set ``Basis`` as ``BASIS_MASS``: :math:`\sum\limits_i H_i \cdot w_i`, with :math:`H_i` the enthalpy of the phase :math:`i`, and :math:`w_i` the mass fraction of the phase :math:`i`.
		
		- Set ``Basis`` as ``BASIS_MOLL``: :math:`\sum\limits_i H_i \cdot x_i`, with :math:`H_i` the enthalpy of the phase :math:`i`, and :math:`x_i` the mole fraction of the phase :math:`i`.


.. Note:: Definition of overall mixture properties:

	+--------------------------+--------------------+---------------------------------------------------------------------------+
	|   Define                 |   Name             |   Units                                                                   |
	+==========================+====================+===========================================================================+
	|``FLOW``, ``TOTAL_FLOW``  | Mass / mole flow   | [kg/s] or  [mol/s]                                                        |
	+--------------------------+--------------------+---------------------------------------------------------------------------+
	| ``MASS``, ``TOTAL_MASS`` | Mass / mole        | [kg] or [mol]                                                             |
	+--------------------------+--------------------+---------------------------------------------------------------------------+
	|   ``TEMPERATURE``        |   Temperature      |   [K]                                                                     |
	+--------------------------+--------------------+---------------------------------------------------------------------------+
	|   ``PRESSURE``           |   Pressure         |   [Pa]                                                                    |
	+--------------------------+--------------------+---------------------------------------------------------------------------+
	|   ``MOLAR_MASS``         |   Molar mass       |   [kg/mol]                                                                |
	+--------------------------+--------------------+---------------------------------------------------------------------------+
	|   ``ENTHALPY``           |   Enthalpy         |   [J/kg/s] or [J/mol/s] for material stream, [J/kg] or [J/mol] for holdup |
	+--------------------------+--------------------+---------------------------------------------------------------------------+

|

.. code-block:: cpp

	double SetOverallProperty(double Time, unsigned Property, double Value, unsigned Basis)
	
Sets non-constant physical property value for the overall mixture at the specified time point ``Time``.

Basis is a basis of the value (``BASIS_MASS`` or ``BASIS_MOLL``). 

``Property`` is an identifier of a physical property. Available properties are:

	-	``FLOW`` and ``TOTAL_FLOW`` for material stream ``CMaterialStream``: refer to function :ref:`setMassFlow <label-setMassFlow>`.
	
	-	``MASS`` and ``TOTAL_MASS`` for holdup ``CHoldup``: refer to function :ref:`setMass <label-setMass>`.
	
	-	``TEMPERATURE``: refer to function :ref:`setTemperature <label-setTemp>`.
	
	-	``PRESSURE``: refer to function :ref:`setPressure <label-setPressure>`.


.. Note:: Definition of overall mixture properties:

	+--------------------------+--------------------+-------------------------+
	|   Define                 |   Name             |   Units                 |
	+==========================+====================+=========================+
	|``FLOW``, ``TOTAL_FLOW``  |   Mass / mole flow | [kg/s] or  [mol/s]      |
	+--------------------------+--------------------+-------------------------+
	| ``MASS``, ``TOTAL_MASS`` | Mass / mole        | [kg] or [mol]           |
	+--------------------------+--------------------+-------------------------+
	|   ``TEMPERATURE``        |   Temperature      |   [K]                   |
	+--------------------------+--------------------+-------------------------+
	|   ``PRESSURE``           |   Pressure         |   [Pa]                  |
	+--------------------------+--------------------+-------------------------+
	|   ``MOLAR_MASS``         |   Molar mass       |   [kg/mol]              |
	+--------------------------+--------------------+-------------------------+
	|   ``ENTHALPY``           |   Enthalpy         |   [J/kg/s] or [J/mol/s] |
	+--------------------------+--------------------+-------------------------+
	
|

.. code-block:: cpp

	double CalcTemperatureFromProperty(ECompoundTPProperties Property, double Time, double Value)

Function for both ``CMaterialStream`` and ``CHoldup``.

Returns temperature of the material stream / holdup for a specific value ``Value`` of the property ``Property`` at the time point ``Time``. Available properties are those defined in :ref:`material database <label-materialDataDetailed>`. 

For further information, please refer to :ref:`label-thermo` on this page.

|

.. code-block:: cpp

	double CalcPressureFromProperty(ECompoundTPProperties Property, double Time, double Value)

Function for both ``CMaterialStream`` and ``CHoldup``.
	
Returns pressure of the material stream / holdup for a specific value ``Value`` of the property ``Property`` at the time point ``Time``. Available properties are those defined in :ref:`material database <label-materialDataDetailed>`. 

For further information, please refer to :ref:`label-thermo` on this page.

|

Compounds
---------

.. code-block:: cpp

	double GetCompoundFraction(double Time , std::string CompoundKey, unsigned Basis)

Function for both ``CMaterialStream`` and ``CHoldup``.
	
Returns total fraction of the compound with key ``CompoundKey`` at the time point ``Time``. If such time point has not been defined, interpolation of data will be done.

Basis can be ``BASIS_MASS`` or ``BASIS_MOLL``.

``BASIS_MASS``: :math:`f_c = \sum \limits_i w_i \cdot f_{i,c}`, with :math:`f_i` the mass fraction of compound :math:`i`, and :math:`w_i` the mass fraction of phase :math:`i`.

``BASIS_MOLL``: :math:`f_c^{mol} = \sum \limits_i w_i \dfrac{f_{i,c}}{M_c \cdot \sum\limits_j \frac{f_{i,j}}{M_j}}`, with :math:`f_i^{mol}` the mole fraction of compound :math:`i`, :math:`f_{i,j}` the mass fraction of compound :math:`j` in phase :math:`i`, and :math:`M_j` the molar mass of compound :math:`j`.

|

.. code-block:: cpp

	double GetCompoundPhaseFraction(double Time, std::string CompoundKey, unsigned Phase, unsigned Basis)

Function for both ``CMaterialStream`` and ``CHoldup``.

Returns fraction of the compound with the key ``CompoundKey`` in the phase ``Phase`` (``SOA_SOLID``, ``SOA_LIQUID``, ``SOA_VAPOR``) for the time point ``Time``. If such time point has not been defined, interpolation of data will be done.

Basis can be ``BASIS_MASS`` or ``BASIS_MOLL``.

``BASIS_MASS``: :math:`f_{i,j}`, mass fraction of compound :math:`j` in phase :math:`i`.

``BASIS_MOLL``: :math:`f_{i,c}^{mol} = \sum \limits_i w_i \dfrac{f_{i,c}}{M_c \cdot \sum\limits_j \frac{f_{i,j}}{M_j}}`, with :math:`f_{i,c}^{mol}` the mole fraction of compound :math:`j` in phase :math:`i`, and :math:`M_j` the molar mass of compound :math:`j`.

|

.. code-block:: cpp

	void SetCompoundPhaseFraction (double Time, std::string CompoundKey, unsigned Phase, double Fraction, unsigned Basis)

Function for both ``CMaterialStream`` and ``CHoldup``.

Sets fraction of the compound with key ``CompoundKey`` in phase ``Phase`` (``SOA_SOLID``, ``SOA_LIQUID``, ``SOA_VAPOR``) for the time point ``Time``. If such time point has not been defined, nothing will be done. Negative values before setting will be converted to ``0``.

Basis can be ``BASIS_MASS`` or ``BASIS_MOLL``.

``BASIS_MASS``: :math:`f_{i,j} = Fraction`, mass fraction of compound :math:`j` in phase :math:`i`.

``BASIS_MOLL``: :math:`f_{i,c} = \dfrac{Fraction \cdot M_c}{\sum\limits_j \frac{f_{i,j}}{M_j}}`, with :math:`f_{i,c}^{mol}` the mole fraction of compound :math:`j` in phase :math:`i`, and :math:`M_j` the molar mass of compound :math:`j`.

|

.. code-block:: cpp

	double GetCompoundMassFlow(double Time, std::string CompoundKey, unsigned Phase, unsigned Basis)

Specific function for ``CMaterialStream``. 

Returns mass flow of the compound with key CompoundKey in phase Phase (SOA_SOLID, SOA_LIQUID, SOA_VAPOR) for the time point ``Time``. If such time point has not been defined, interpolation of data will be done. 

Basis is a basis of value (``BASIS_MASS`` in [kg/s] or ``BASIS_MOLL`` in [mol/s]).

``BASIS_MASS``: :math:`\dot{m}_{i,j} = w_i \cdot f_{i,j} \cdot \dot{m}`, with :math:`m_{i,j}` the mass flow of compound :math:`j` in phase :math:`i`, :math:`w_i` the mass fraction of phase :math:`i`, and :math:`f_{i,j}` the mass fraction of compound :math:`j` in phase :math:`i`.

``BASIS_MOLL``: :math:`\dot{m}_{i,j} = w_i \cdot f_{i,j} \cdot \sum\limits_k \frac{\dot{m} \cdot w_k}{M_k}`, with :math:`m` the total mass flow of the material stream, and :math:`M_k` the molar mass of phase :math:`k`.

|

.. code-block:: cpp

	double GetCompoundMass(double Time, std::string CompoundKey, unsigned Phase, unsigned Basis)

Specific function for ``CHoldup``. 

Returns mass of the compound with key CompoundKey in phase Phase (SOA_SOLID, SOA_LIQUID, SOA_VAPOR) for the time point ``Time``. If such time point has not been defined, interpolation of data will be done. 

Basis is a basis of value (``BASIS_MASS`` in [kg] or ``BASIS_MOLL`` in [mol]).

``BASIS_MASS``: :math:`m_{i,j} = w_i \cdot f_{i,j} \cdot m`, with :math:`m_{i,j}` the mass of compound :math:`j` in phase :math:`i`, :math:`w_i` the mass fraction of phase :math:`i`, and :math:`f_{i,j}` the mass fraction of compound :math:`j` in phase :math:`i`.

``BASIS_MOLL``: :math:`m_{i,j} = w_i \cdot f_{i,j} \cdot \sum\limits_k \frac{m \cdot w_k}{M_k}`, with :math:`m` the total mass of the holdup, and :math:`M_k` the molar mass of phase :math:`k`.

|

.. code-block:: cpp

	double GetCompoundConstant(std::string CompoundKey, ECompoundConstProperties ConstProperty)

Function for both ``CMaterialStream`` and ``CHoldup``.
	
Returns value of the constant physical property ``ConstProperty`` for the specified compound. These properties are stored in :ref:`material database <label-materialDataDetailed>`. Available constants are:
	
	-	``SOA_AT_NORMAL_CONDITIONS``

	-	``NORMAL_BOILING_POINT``
	
	-	``NORMAL_FREEZING_POINT``
	
	-	``CRITICAL_TEMPERATURE``
	
	-	``CRITICAL_PRESSURE``
	
	-	``MOLAR_MASS``
	
	-	``STANDARD_FORMATION_ENTHALPY``
	
	-	``HEAT_OF_FUSION_AT_NORMAL_FREEZING_POINT``
	
	-	``HEAT_OF_VAPORIZATION_AT_NORMAL_BOILING_POINT``
		
	-	``REACTIVITY_TYPE``
	
	-	``CONST_PROP_USER_DEFINED_XX``


.. Note:: Definition of constant properties for pure compounds:

	+----------------------------------------------------+------------------------------------------------+------------+
	|   Define                                           |   Name                                         |   Unit     |
	+====================================================+================================================+============+
	|   ``SOA_AT_NORMAL_CONDITIONS``                     |   State of aggregation at normal conditions    |   [-]      |
	+----------------------------------------------------+------------------------------------------------+------------+
	|   ``NORMAL_BOILING_POINT``                         |   Normal boiling point                         |   [K]      |
	+----------------------------------------------------+------------------------------------------------+------------+
	|   ``NORMAL_FREEZING_POINT``                        |   Normal freezing point                        |   [K]      |
	+----------------------------------------------------+------------------------------------------------+------------+
	|   ``CRITICAL_TEMPERATURE``                         |   Critical temperature                         |  [K]       |
	+----------------------------------------------------+------------------------------------------------+------------+
	|   ``CRITICAL_PRESSURE``                            |   Critical pressure                            |   [Pa]     |
	+----------------------------------------------------+------------------------------------------------+------------+
	|   ``MOLAR_MASS``                                   |   Molar mass                                   |   [kg/mol] |
	+----------------------------------------------------+------------------------------------------------+------------+
	|   ``STANDARD_FORMATION_ENTHALPY``                  |   Standard formation enthalpy                  |   [J/mol]  |
	+----------------------------------------------------+------------------------------------------------+------------+
	|   ``HEAT_OF_FUSION_AT_NORMAL_FREEZING_POINT``      |   Heat of fusion at normal freezing point      |   [J/mol]  |
	+----------------------------------------------------+------------------------------------------------+------------+
	|   ``HEAT_OF_VAPORIZATION_AT_NORMAL_BOILING_POINT`` |   Heat of vaporization at normal boiling point |   [J/mol]  |
	+----------------------------------------------------+------------------------------------------------+------------+
	|   ``REACTIVITY_TYPE``                              |   Reactivity type                              |   [-]      |
	+----------------------------------------------------+------------------------------------------------+------------+
	|   ``CONST_PROP_USER_DEFINED_XX``                   |   User defined property                        |   [-]      |
	+----------------------------------------------------+------------------------------------------------+------------+

|

.. code-block:: cpp

	double GetCompoundTPDProp(std::string CompoundKey, unsigned Property, double Temperature, double Pressure)

Function for both ``CMaterialStream`` and ``CHoldup``.
	
Returns value of the temperature / pressure-dependent physical Property (which are stored in the database of materials) for the compound with the specified ``Temperature`` in [K] and ``Pressure`` in [Pa]. Available properties are:

	-	``DENSITY``
	
	-	``HEAT_CAPACITY_CP``
	
	-	``VAPOR_PRESSURE``
	
	-	``VISCOSITY``
	
	-	``THERMAL_CONDUCTIVITY``
	
	-	``PERMITTIVITY``
	
	-	``ENTHALPY``
	
	-	``TP_PROP_USER_DEFINED_XX``


.. Note:: Definition of temperature-dependent compound properties:

	+-------------------------------+-----------------------------+---------------------------------------------------------------------------+
	|   Define                      |   Name                      |   Unit                                                                    |
	+===============================+=============================+===========================================================================+
	|   ``DENSITY``                 |   Density                   |   [kg/m :math:`^3`]                                                       |
	+-------------------------------+-----------------------------+---------------------------------------------------------------------------+
	|   ``HEAT_CAPACITY_CP``        |   Heat capacity :math:`C_p` |   [J/(kg·K)]                                                              |
	+-------------------------------+-----------------------------+---------------------------------------------------------------------------+
	|   ``VAPOR_PRESSURE``          |   Vapor pressure            |   [Pa]                                                                    |
	+-------------------------------+-----------------------------+---------------------------------------------------------------------------+
	|   ``VISCOSITY``               |   Viscosity                 |   [Pa·s]                                                                  |
	+-------------------------------+-----------------------------+---------------------------------------------------------------------------+
	|   ``THERMAL_CONDUCTIVITY``    |   Thermal conductivity      |   [W/(m·K)]                                                               |
	+-------------------------------+-----------------------------+---------------------------------------------------------------------------+
	|   ``PERMITTIVITY``            |   Permittivity              |   [F/m]                                                                   |
	+-------------------------------+-----------------------------+---------------------------------------------------------------------------+
	|   ``ENTHALPY``                |   Enthalpy                  |   [J/kg/s] or [J/mol/s] for material stream, [J/kg] or [J/mol] for holdup |
	+-------------------------------+-----------------------------+---------------------------------------------------------------------------+
	|   ``TP_PROP_USER_DEFINED_XX`` |   User defined property     |  [-]                                                                      |
	+-------------------------------+-----------------------------+---------------------------------------------------------------------------+

|

.. code-block:: cpp

	double GetCompoundTPDProp(double Time, std::string CompoundKey, unsigned Property)
	
Function for both ``CMaterialStream`` and ``CHoldup``.

Returns value of the temperature / pressure-dependent physical ``Property`` (which are stored in the database of materials) for the compound with the current temperature and pressure. Available properties are:

	-	``DENSITY``
	
	-	``HEAT_CAPACITY_CP``
	
	-	``VAPOR_PRESSURE``
	
	-	``VISCOSITY``
	
	-	``THERMAL_CONDUCTIVITY``
	
	-	``PERMITTIVITY``
	
	-	``ENTHALPY``
	
	-	``TP_PROP_USER_DEFINED_XX``


.. Note:: Definition of temperature-dependent compound properties:

	+-------------------------------+-----------------------------+---------------------------------------------------------------------------+
	|   Define                      |   Name                      |   Unit                                                                    |
	+===============================+=============================+===========================================================================+
	|   ``DENSITY``                 |   Density                   |   [kg/m :math:`^3`]                                                       |
	+-------------------------------+-----------------------------+---------------------------------------------------------------------------+
	|   ``HEAT_CAPACITY_CP``        |   Heat capacity :math:`C_p` |   [J/(kg·K)]                                                              |
	+-------------------------------+-----------------------------+---------------------------------------------------------------------------+
	|   ``VAPOR_PRESSURE``          |   Vapor pressure            |   [Pa]                                                                    |
	+-------------------------------+-----------------------------+---------------------------------------------------------------------------+
	|   ``VISCOSITY``               |   Viscosity                 |   [Pa·s]                                                                  |
	+-------------------------------+-----------------------------+---------------------------------------------------------------------------+
	|   ``THERMAL_CONDUCTIVITY``    |   Thermal conductivity      |   [W/(m·K)]                                                               |
	+-------------------------------+-----------------------------+---------------------------------------------------------------------------+
	|   ``PERMITTIVITY``            |   Permittivity              |   [F/m]                                                                   |
	+-------------------------------+-----------------------------+---------------------------------------------------------------------------+
	|   ``ENTHALPY``                |   Enthalpy                  |   [J/kg/s] or [J/mol/s] for material stream, [J/kg] or [J/mol] for holdup |
	+-------------------------------+-----------------------------+---------------------------------------------------------------------------+
	|   ``TP_PROP_USER_DEFINED_XX`` |   User defined property     |  [-]                                                                      |
	+-------------------------------+-----------------------------+---------------------------------------------------------------------------+

|

.. code-block:: cpp

	double GetCompoundInteractionProp(std::string CompoundKey1, std::string CompoundKey2, unsigned Property, double Temperature, double Pressure)
	
Function for both ``CMaterialStream`` and ``CHoldup``.

Returns the value of the interaction property ``Property`` for the selected compounds under the specified ``Temperature`` in [K] and ``Pressure`` in [Pa]. These properties are stored in the :ref:`material database <label-materialDataDetailed>`. Available properties are:

	-	``INTERFACE_TENSION``
	
	-	``INT_PROP_USER_DEFINED_XX``
	
.. Note:: Definition of interaction properties between two pure compounds:
	
	+--------------------------------+-------------------------+---------+
	|   Define                       |   Name                  |   Unit  |
	+================================+=========================+=========+
	|   ``INTERFACE_TENSION``        |   Interface tension     |   [N/m] |
	+--------------------------------+-------------------------+---------+
	|   ``INT_PROP_USER_DEFINED_XX`` |   User defined property |   [-]   |
	+--------------------------------+-------------------------+---------+	

.. code-block:: cpp

	double GetCompoundInteractionProp(double Time, std::string CompoundKey1, std::string CompoundKey2, unsigned Property)
	
Function for both ``CMaterialStream`` and ``CHoldup``.

Returns the value of the interaction property ``Property`` for the selected compounds under the current temperature and pressure. These properties are stored in the :ref:`material database <label-materialDataDetailed>`. Available properties are:

	-	``INTERFACE_TENSION``
	
	-	``INT_PROP_USER_DEFINED_XX``
	
.. Note:: Definition of interaction properties between two pure compounds:
	
	+--------------------------------+-------------------------+---------+
	|   Define                       |   Name                  |   Unit  |
	+================================+=========================+=========+
	|   ``INTERFACE_TENSION``        |   Interface tension     |   [N/m] |
	+--------------------------------+-------------------------+---------+
	|   ``INT_PROP_USER_DEFINED_XX`` |   User defined property |   [-]   |
	+--------------------------------+-------------------------+---------+	

|

Phases
------

.. code-block:: cpp

	double GetPhaseMassFlow(double Time, unsigned Phase, unsigned Basis = BASIS_MASS)

Specific function for ``CMaterialStream``. 

Returns mass flow of the specified phase ``Phase`` (``SOA_SOLID``, ``SOA_LIQUID``, ``SOA_VAPOR``) in the material stream for the time point ``Time``. If such time point has not been defined, the value will be interpolated. 

Basis is a basis of value (``BASIS_MASS`` in [kg/s] or ``BASIS_MOLL`` in [mol/s]).

``BASIS_MASS``: :math:`\dot{m}_i = \dot{m} \cdot w_i`, with :math:`\dot{m}_i` the mass flow of phase :math:`i`, :math:`w_i` the mass fraction of phase :math:`i`, and :math:`\dot{m}` the total mass flow of the material stream.

``BASIS_MOLL``: :math:`\dot{n}_i = \frac{\dot{m} \cdot w_i}{M_i}`, with :math:`\dot{n}_i` the mole flow of phase :math:`i`, :math:`w_i` the mass fraction of phase :math:`i`, :math:`\dot{m}` the total mass flow of the material stream, and :math:`M_i` the molar mass of phase :math:`i`.

|

.. code-block:: cpp

	double GetPhaseMass(double Time, unsigned Phase, unsigned Basis = BASIS_MASS)

Specific function for ``CHoldup``. 

Returns mass of the specified phase ``Phase`` (``SOA_SOLID``, ``SOA_LIQUID``, ``SOA_VAPOR``) in the holdup for the time point ``Time``. If such time point has not been defined, the value will be interpolated.

Basis is a basis of value (``BASIS_MASS`` in [kg] or ``BASIS_MOLL`` in [mol]).

``BASIS_MASS``: :math:`m_i = m \cdot w_i`, with :math:`m_i` the mass of phase :math:`i`, :math:`w_i` the mass fraction of phase :math:`i`, and :math:`m` the total mass flow of the material stream.

``BASIS_MOLL``: :math:`n_i = \dfrac{m \cdot w_i}{M_i}`, with :math:`n_i` the mole of phase :math:`i`, :math:`w_i` the mass fraction of phase :math:`i`, :math:`m` the total mass flow of the material stream, and :math:`M_i` the molar mass of phase :math:`i`.

|

.. _label-setPhaseMassFlow:

.. code-block:: cpp
	
	void SetPhaseMassFlow(double Time, unsigned Phase, double Value, unsigned Basis)

Specific function for ``CMaterialStream``. 
	
Sets mass flow of the specified phase ``Phase`` (``SOA_SOLID``, ``SOA_LIQUID``, ``SOA_VAPOR``) in the material stream for the time point ``Time``. 

Is performed by calculation and setting of a new total mass flow of the material stream and new phase fractions (according to the new mass flow of the specified phase). Negative values before setting will be converted to ``0``. If there is no specified time point or phase in the material stream, the value will not be set. 

Basis is a basis of value (``BASIS_MASS`` [kg/s] or ``BASIS_MOLL`` [mol/s]).

``BASIS_MASS``: for each defined phase. :math:`\dot{m} = \dot{m} + (` ``Value`` :math:` - \dot{m}_i)`, :math:`m_i =` ``Value``, :math:`w_i = m_i / m`. With :math:`\dot{m}_i` the mass flow of phase :math:`i`, :math:`w_i` the mass fraction of phase :math:`i`, and :math:`\dot{m}` the total mass flow of the material stream.

``BASIS_MOLL``: for each defined phase. :math:`\dot{n} = \dot{n} + (` ``Value`` :math:`\cdot M_i - \dot{m}_i)`, :math:`m_i = M_i \cdot` ``Value``, :math:`w_i = m_i / m`. With :math:`\dot{n}_i` the mole flow of phase :math:`i`, :math:`w_i` the mass fraction of phase :math:`i`, :math:`\dot{m}` the total mass flow of the material stream, and :math:`M_i` the molar mass of phase :math:`i`.



|

.. _label-setPhaseMass:

.. code-block:: cpp
	
	void SetPhaseMass(double Time, unsigned Phase, double Value, unsigned Basis)

Specific function for ``CHoldup``. 
	
Sets mass of the specified phase ``Phase`` (``SOA_SOLID``, ``SOA_LIQUID``, ``SOA_VAPOR``) in the holdup for the time point ``Time``. 

Is performed by calculation and setting of a new total mass flow of the holdup and new phase fractions (according to the new mass of the specified phase). Negative values before setting will be converted to ``0``. If there is no specified time point or phase in the holdup, the value will not be set. 

Basis is a basis of value (``BASIS_MASS`` [kg] or ``BASIS_MOLL`` [mol]).

``BASIS_MASS``: for each defined phase. :math:`m = m + (` ``Value`` :math:`- m_i)`, :math:`m_i =` ``Value``, :math:`w_i = m_i / m`. With :math:`m_i` the mass of phase :math:`i`, :math:`w_i` the mass fraction of phase :math:`i`, and :math:`m` the total mass flow of the material stream.

``BASIS_MOLL``: for each defined phase. :math:`n = n + (` ``Value`` :math:`\cdot M_i - m_i)`, :math:`m_i = M_i \cdot` ``Value``, :math:`w_i = m_i / m`. With :math:`n_i` the mole of phase :math:`i`, :math:`w_i` the mass fraction of phase :math:`i`, :math:`m` the total mass flow of the material stream, and :math:`M_i` the molar mass of phase :math:`i`.

|

.. code-block:: cpp
	
	double GetSinglePhaseProp(double Time, unsigned Property, unsigned Phase, unsigned Basis)

Function for both ``CMaterialStream`` and ``CHoldup``.

Returns non-constant physical property value for the phase mixture ``Phase`` (``SOA_SOLID``, ``SOA_LIQUID``, ``SOA_VAPOR``) for the specified time point. If such time point has not been defined, interpolation of data will be done. 

Basis is a basis of results (``BASIS_MASS`` or ``BASIS_MOLL``).

``Property`` is an identifier of a physical property. Available properties are:

	-	``FLOW``: only for class ``CMaterialStream``. Refer to function :ref:`getMassFlow <label-massFlow>`.
	
	-	``MASS``: only for class ``CHoldup``. Refer to function :ref:`getMass <label-mass>`.
	
	-	``TEMPERATURE``: refer to function :ref:`getTemperature <label-temp>`.
	
	-	``PRESSURE``: refer to function :ref:`getPressure <label-pressure>`.
	
	-	``PHASE_FRACTION``, ``FRACTION``: 
		
		- ``BASIS_MASS``: function returns :math:`w_i`, mass fraction of phase :math:`i`.
		
		- ``BASIS_MOLL``: function returns result of :math:`\left ( \dfrac{w_i}{M_i \cdot \sum\limits_j \frac{w_j}{M_j}} \right )`, with :math:`w_i` the mass fraction of phase :math:`i`, and :math:`M_i` the molar mass of phase :math:`i`.
	
	-	``MOLAR_MASS``: calculate the molar mass of the phase :math:`M` by :math:`\left ( \frac{1}{M} = \sum\limits_i \frac{w_i}{M_i} \right )`, with :math:`M_i` the molar mass of phase :math:`i`, and :math:`w_i` the mass fraction of phase :math:`i`.
	
	-	``DENSITY``: refer to function :ref:`getPhaseTPDProp <label-getPhaseTPD>`.
	
	-	``HEAT_CAPACITY_CP``: refer to function :ref:`getPhaseTPDProp <label-getPhaseTPD>`.
	
	-	``THERMAL_CONDUCTIVITY``: refer to function :ref:`getPhaseTPDProp <label-getPhaseTPD>`.
	
	-	``VISCOSITY``: refer to function :ref:`getPhaseTPDProp <label-getPhaseTPD>`.
	
	-	``VAPOR_PRESSURE``: refer to function :ref:`getPhaseTPDProp <label-getPhaseTPD>`.
	
	-	``PERMITTIVITY``: refer to function :ref:`getPhaseTPDProp <label-getPhaseTPD>`.
	
	-	``TP_PROP_USER_DEFINED_XX``: refer to function :ref:`getPhaseTPDProp <label-getPhaseTPD>`.
	
	-	``ENTHALPY``:
		
		For solid and liquid phase: :math:`h = h_0 + C_p \cdot \Delta T + \frac{M}{\rho} (P - P_0)`
		
		- ``BASIS_MASS``: :math:`H = \sum\limits_i \frac{h_i \cdot f_i}{M_i}`
		
		- ``BASIS_MOLL``: :math:`H = \sum\limits_i h_i \cdot f_i`
		
		For vapor phase: :math:`h = h_0 + C_p \cdot \Delta T`
		
		- ``BASIS_MASS``: :math:`H = \sum\limits_i \frac{h_i \cdot f_i}{M_i}`
		
		- ``BASIS_MOLL``: :math:`H = \sum\limits_i h_i \cdot f_i`
		
		.. Note:: Notations for enthalpy:
		
			:math:`H` – enthalpy of the phase. [J/kg/s] or [J/mol/s] for material stream; [J/kg] or [J/mol] for holdup
			
			:math:`h_i` – enthalpy of the compound :math:`i` [J/mol]
			
			:math:`f_i` – mass fraction of the compound :math:`i` in phase
			
			:math:`M_i` – molar mass of the compound :math:`i`
			
			:math:`h_0` – formation enthalpy [J/mol]
			
			:math:`C_p` – heat capacity for constant pressure of the compound
			
			:math:`\Delta T` – difference between the temperature at normal conditions (298.15 K) and current temperature
			
			:math:`P` – current pressure
			
			:math:`P_0` – pressure at normal conditions (101325 Pa)

|

.. Note:: Definition of single-phase mixture properties:

	+------------------------------------+-----------------------------+--------------------------------------------------------------------------+
	|   Define                           |   Name                      |   Unit                                                                   |
	+====================================+=============================+==========================================================================+
	|   ``FLOW``                         |   Mass flow                 |   [kg/s] or [mol/s]                                                      |
	+------------------------------------+-----------------------------+--------------------------------------------------------------------------+
	| ``MASS``                           | Mass                        | [kg] or [mol]                                                            |
	+------------------------------------+-----------------------------+--------------------------------------------------------------------------+
	|   ``TEMPERATURE``                  |   Temperature               |   [K]                                                                    |
	+------------------------------------+-----------------------------+--------------------------------------------------------------------------+
	|   ``PRESSURE``                     |   Pressure                  |   [Pa]                                                                   |
	+------------------------------------+-----------------------------+--------------------------------------------------------------------------+
	|   ``PHASE_FRACTION``, ``FRACTION`` |   Phase fraction            |   [-]                                                                    |
	+------------------------------------+-----------------------------+--------------------------------------------------------------------------+
	|   ``MOLAR_MASS``                   |   Molar mass                |   [kg/mol]                                                               |
	+------------------------------------+-----------------------------+--------------------------------------------------------------------------+
	|   ``DENSITY``                      |   Density                   |   [kg/m :math:`^3`]                                                      |
	+------------------------------------+-----------------------------+--------------------------------------------------------------------------+
	|   ``HEAT_CAPACITY_CP``             |   Heat capacity :math:`C_p` |   [J/(kg·K)]                                                             |
	+------------------------------------+-----------------------------+--------------------------------------------------------------------------+
	|   ``THERMAL_CONDUCTIVITY``         |   Thermal conductivity      |   [W/(m·K)]                                                              |
	+------------------------------------+-----------------------------+--------------------------------------------------------------------------+
	|  ``VISCOSITY``                     |   Viscosity                 |   [Pa·s]                                                                 |
	+------------------------------------+-----------------------------+--------------------------------------------------------------------------+
	|  ``VAPOR_PRESSURE``                |   Vapor pressure            |   [Pa]                                                                   |
	+------------------------------------+-----------------------------+--------------------------------------------------------------------------+
	|   ``ENTHALPY``                     |   Enthalpy                  |   [J/kg/s]or [J/mol/s] for material stream, [J/kg] or [J/mol] for holdup |
	+------------------------------------+-----------------------------+--------------------------------------------------------------------------+
	|   ``PERMITTIVITY``                 |   Permittivity              |   [F/m]                                                                  |
	+------------------------------------+-----------------------------+--------------------------------------------------------------------------+
	|   ``TP_PROP_USER_DEFINED_XX``      |   User defined property     |   [-]                                                                    |
	+------------------------------------+-----------------------------+--------------------------------------------------------------------------+

|

.. code-block:: cpp

	void SetSinglePhaseProp(double Time, unsigned Property, unsigned Phase, double Value, unsigned Basis)

Function for both ``CMaterialStream`` and ``CHoldup``.
	
Sets non-constant physical property value for phase mixture ``Phase`` (``SOA_SOLID``, ``SOA_LIQUID``, ``SOA_VAPOR``) for the specified time point ``Time``. If there is no specified time point or phase in the material stream or holdup, the value will not be set. 

``Property`` is an identifier of a physical property. Available properties are:

	-	``FLOW``: only for class ``CMaterialStream``. Refer to function :ref:`setPhaseMassFlow <label-setPhaseMassFlow>`.
	
	-	``MASS``: only for class ``CHoldup``. Refer to function :ref:`setPhaseMass <label-setPhaseMass>`.
	
	-	``FRACTION``: mass fraction of the ``Phase`` is set to ``Value``.


Basis is a basis of value (``BASIS_MASS`` or ``BASIS_MOLL``).

|

.. _label-getPhaseTPD:

.. code-block:: cpp

		double GetPhaseTPDProp(double Time, unsigned Property, unsigned Phase)

Function for both ``CMaterialStream`` and ``CHoldup``.
		
Returns value of temperature / pressure-dependent physical property for specified phase (``SOA_SOLID``, ``SOA_LIQUID``, ``SOA_VAPOR``) for the time point ``Time``. If such time point has not been defined, interpolation of data will be done.

Available properties are:

	-	``DENSITY``:
		
		- For solid phase: is calculated by :math:`\rho = \sum\limits_{i,j} \rho_i \, (1 - \varepsilon_j)\,f_{i,j}`, with :math:`\varepsilon_j` the porosity in interval :math:`j`, and :math:`f_{i,j}` the mass fraction of compound :math:`i` with porosity :math:`j`.
		
		- For liquid and vapor phase: is calculated by :math:`\frac{1}{\rho} = \sum\limits_i \frac{w_i}{\rho_i}`, with :math:`w_i` the mass fraction of compound :math:`i` in ``Phase``.
	
	-	``HEAT_CAPACITY_CP``: is calculated by :math:`C_p = \sum\limits_i w_i \cdot C_{p,i}`, with :math:`C_{p,i}` the heat capacity of compound :math:`i`, and :math:`w_i` the mass fraction of compound :math:`i` in ``Phase``.
	
	-	``VAPOR_PRESSURE``: is calculated by :math:`P_v = \min\limits_{i} (P_v)_i`, with :math:`(P_v)_i` vapor pressure of compound :math:`i`.
	
	-	``VISCOSITY``: 
		
		- For solid phase: is calculated by :math:`\eta = \sum\limits_i w_i\, \eta_i`, with :math:`\eta_i` the viscosity of compound :math:`i`, and :math:`w_i` the mass fraction of compound :math:`i`.
		
		- For liquid phase: is calculated by :math:`\ln \eta = \dfrac{\sum\limits_i w_i\,\ln \eta_i}{\sum\limits_i x_i\,\sqrt{M_i}}`, with :math:`\eta_i` the viscosity of compound :math:`i`, :math:`w_i` the mass fraction of compound :math:`i` in `Phase` and :math:`x_i` the mole fraction of compound :math:`i` in ``Phase``.
		
		
		- For vapor phase: :math:`\eta = \dfrac{\sum\limits_i x_i\,\sqrt{M_i}\,\eta_i}{\sum\limits_i x_i\,\sqrt{M_i}}`, with :math:`\eta_i` the viscosity of compound :math:`i`, :math:`w_i` the mass fraction of compound :math:`i` in `Phase`, and :math:`x_i` the mole fraction of compound :math:`i` in ``Phase``.
		
	
	-	``THERMAL_CONDUCTIVITY``:
	
		- For solid phase: is calculated by :math:`\lambda = \sum\limits_i w_i \, \lambda_i`, with :math:`\lambda_i` the thermal conductivity of compound :math:`i`.
		
		- For liquid phase: is calculated by :math:`\lambda = \dfrac{1}{\sqrt{\sum\limits_i x_i \, \lambda_i^{-2}}}`, with :math:`\lambda_i` the thermal conductivity of compound :math:`i`.
		
		- For vapor phase: is calculated by :math:`\lambda = \sum\limits_i \dfrac{x_i\,\lambda_i}{\sum\limits_j x_j\, F_{i,j}}`, :math:`F_{i,j} = \frac{(1 + \sqrt{\lambda_i^4 / \lambda_j} \sqrt{M_j / M_i})^2}{\sqrt{8(1 + M_i / M_j)}}`. With :math:`M_i` the molar mass of compound :math:`i`.

	-	``PERMITTIVITY``: is calculated by :math:`\varepsilon = \sum\limits_i w_i\,\varepsilon_i`, with :math:`\varepsilon_i` the permittivity of compound :math:`i`, and :math:`w_i` the mass fraction of compound :math:`i` in ``Phase``.
	
	-	``ENTHALPY``: is calculated by :math:`H = \sum\limits_i w_i\,H_i`, with :math:`H_i` the enthalpy of compound :math:`i`, and :math:`w_i` the mass fraction of compound :math:`i` in ``Phase``.
	
	-	``TP_PROP_USER_DEFINED_XX``: is calculated by :math:`Y = \sum\limits_i w_i\,Y_i`, with :math:`Y_i` the property value of compound :math:`i`, and :math:`w_i` the mass fraction of compound :math:`i` in ``Phase``.

.. Note:: Definition of temperature-dependent compound properties:

	+-------------------------------+-----------------------------+---------------------------------------------------------------------------+
	|   Define                      |   Name                      |   Unit                                                                    |
	+===============================+=============================+===========================================================================+
	|   ``DENSITY``                 |   Density                   |   [kg/m :math:`^3`]                                                       |
	+-------------------------------+-----------------------------+---------------------------------------------------------------------------+
	|   ``HEAT_CAPACITY_CP``        |   Heat capacity :math:`C_p` |   [J/(kg·K)]                                                              |
	+-------------------------------+-----------------------------+---------------------------------------------------------------------------+
	|   ``VAPOR_PRESSURE``          |   Vapor pressure            |   [Pa]                                                                    |
	+-------------------------------+-----------------------------+---------------------------------------------------------------------------+
	|   ``VISCOSITY``               |   Viscosity                 |   [Pa·s]                                                                  |
	+-------------------------------+-----------------------------+---------------------------------------------------------------------------+
	|   ``THERMAL_CONDUCTIVITY``    |   Thermal conductivity      |   [W/(m·K)]                                                               |
	+-------------------------------+-----------------------------+---------------------------------------------------------------------------+
	|   ``PERMITTIVITY``            |   Permittivity              |   [F/m]                                                                   |
	+-------------------------------+-----------------------------+---------------------------------------------------------------------------+
	|   ``ENTHALPY``                |   Enthalpy                  |   [J/kg/s] or [J/mol/s] for material stream, [J/kg] or [J/mol] for holdup |
	+-------------------------------+-----------------------------+---------------------------------------------------------------------------+
	|   ``TP_PROP_USER_DEFINED_XX`` |   User defined property     |  [-]                                                                      |
	+-------------------------------+-----------------------------+---------------------------------------------------------------------------+

|

Solid distributed properties
----------------------------

All functions in this section are for both ``CMaterialStream`` and ``CHoldup``.

.. code-block:: cpp

	double GetFraction(double Time, std::vector<unsigned> Coords)

Returns solid mass fraction by specified coordinates according to all defined distributions. If such time point has not been defined, interpolation of data will be done.

|

.. code-block:: cpp

	void SetFraction(double Time, std::vector<unsigned> Coords, double Value)
	
Sets solid mass fraction by specified coordinates according to all defined distributions. If such time point has not been defined in the material stream / holdup, nothing will be done. 

Direct setting of fractions to the material stream / holdup leads to a change of all dependent distributions. Approach with transformation matrix should be used to avoid this.

|

.. code-block:: cpp

	bool GetDistribution(double Time, EDistrTypes Dim, std::vector<double>& Result)
	
Returns vector of distributed property for specified time point ``Time`` and dimension ``Dim``. If such time point has not been defined in the material stream / holdup, then linear interpolation will be used to obtain data. 

Returns ``false`` on error.

|

.. code-block:: cpp

	bool GetDistribution(double Time, EDistrTypes Dim1, EDistrTypes Dim2, CDense2DMatrix& Result)
	
Returns matrix of two distributed dependent properties ``Dim1`` and ``Dim2`` for the specified time point ``Time``. 

If such time point has not been defined in the material stream / holdup, then linear interpolation will be used to obtain data. Rows of resulting matrix will correspond to ``Dim1``, columns – to ``Dim2``. 

Returns ``false`` on error.

|

.. code-block:: cpp

	bool GetDistribution(double Time, std::vector<EDistrTypes> Dims, CDenseMDMatrix& Result)

Returns multidimensional matrix of distributed dependent properties for specified time point ``Time`` and dimensions ``Dims``. If such time point has not been defined in the material stream / holdup, then linear interpolation will be used to obtain data. 

Returns ``false`` on error.

|

.. code-block:: cpp

	bool GetDistribution(double Time, EDistrTypes Dim, std::string Compound, std::vector<double>& Result)
	
Returns vector of distributed property for specified time point ``Time``, dimension ``Dim`` and compound ``Compound``. 

Input dimensions should not include distribution by compounds (``DISTR_COMPOUNDS``). If specified compound has not been defined in the material stream / holdup, nothing will be done. If specified time point has not been defined, then linear interpolation will be used to obtain data. 

Returns ``false`` on error.

|

.. code-block:: cpp

	bool GetDistribution(double Time, EDistrTypes Dim1, EDistrTypes Dim2, std::string Compound, CDense2DMatrix& 2DResult)
	
Returns matrix of two distributed dependent properties ``Dim1`` and ``Dim2`` for specified compound ``Compound`` and time point ``Time``. 

Input dimensions should not include distribution by compounds (DISTR_COMPOUNDS). If specified compound has not been defined in the material stream / holdup, nothing will be done. If specified time point has not been defined, then linear interpolation will be used to obtain data. Rows of resulting matrix will correspond to ``Dim1``, columns to ``Dim2``. 

Returns ``false`` on error.

|

.. code-block:: cpp

	bool GetDistribution(double Time, std::vector<EDistrTypes> Dims, std::string Compound, CDenseMDMatrix& MDResult)
	
Returns multidimensional matrix of distributed dependent properties for specified time point ``Time``, dimensions ``Dims`` and compound ``Compound``.

Input dimensions should not include distribution by compounds (``DISTR_COMPOUNDS``). If specified compound has not been defined in the material stream / holdup, nothing will be done. If specified time point has not been defined, then linear interpolation will be used to obtain data. 

Returns ``false`` on error.

|

.. code-block:: cpp

	bool SetDistribution(double Time, EDistrTypes Dim, std::vector<double> Distr)
	
Sets distributed property ``Distr`` of type ``Dim`` for specified time point ``Time``. 

If such time point or dimension doesn’t exist, nothing will be done. Returns ``false`` on error. 

Direct setting of distribution to the material stream / holdup leads to a change of all dependent distributions. Approach with transformation matrix should be used to avoid this.

|

.. code-block:: cpp

	bool SetDistribution(double Time, EDistrTypes Dim1, EDistrTypes Dim2, CDense2DMatrixDistr)
	
Sets matrix ``Distr`` of two dependent distributed properties of types ``Dim1`` and ``Dim2`` for specified time point ``Time``. If such time point or dimensions don’t exist nothing will be done. 

Returns ``false`` on error. 

Direct setting of distribution to the material stream / holdup leads to a change of all dependent distributions. Approach with transformation matrix should be used to avoid this.

|

.. code-block:: cpp

	bool SetDistribution(double Time,  CDenseMDMatrix Distr)
	
Sets multidimensional matrix ``Distr`` of dependent distributed properties for specified time point ``Time``. If such time point or dimensions, which are specified in ``Distr``, don’t exist, nothing will be done. 

Returns ``false`` on error. 

Direct setting of distribution to the material stream / holdup leads to a change of all dependent distributions. Approach with transformation matrix should be used to avoid this.

|

.. code-block:: cpp

	bool SetDistribution(double Time, EDistrTypes Dim, std::string Compound, std::vector<double> Distr)
	
Sets distributed property ``Distr`` of type ``Dim`` for specified compound ``Compound`` and time point ``Time``. If such time point, compound or dimension doesn’t exist, nothing will be done. Input dimensions should not include distribution by compounds (``DISTR_COMPOUNDS``). 

Returns ``false`` on error. 

Direct setting of distribution to the holdup leads to a change of all dependent distributions. Approach with transformation matrix should be used to avoid this.

|

.. code-block:: cpp

	bool SetDistribution(double Time, EDistrTypes Dim1, EDistrTypes Dim2, std::string Compound, CDense2DMatrix 2DDistr)
	
Sets matrix ``2DDistr`` of two dependent distributed properties of types ``Dim1`` and ``Dim2`` for specified compound ``Compound`` and time point ``Time``. If such time point, compound or dimensions don’t exist, nothing will be done. Input dimensions should not include distribution by compounds (``DISTR_COMPOUNDS``). 

Returns ``false`` on error. 

Direct setting of distribution to the holdup leads to a change of all dependent distributions. Approach with transformation matrix should be used to avoid this.

|

.. code-block:: cpp

	bool SetDistribution(double Time, std::string Compound, CDenseMDMatrix MDDistr)

Sets multidimensional matrix ``MDDistr`` of dependent distributed properties for specified compound ``Compound`` and time point ``Time``. If such time point, compound or dimensions, which are specified in ``MDDistr``, don’t exist, nothing will be done. Input dimensions should not include distribution by compounds (``DISTR_COMPOUNDS``). 

Returns ``false`` on error. 

Direct setting of distribution to the holdup leads to a change of all dependent distributions. Approach with transformation matrix should be used to avoid this.

|

.. code-block:: cpp

	bool ApplyTM(double Time, CTransformMatrix Transformation)

Transforms matrix of distributed parameters of solids for time point ``Time`` by applying a movement matrix ``Transformation``. Returns ``true`` if the transformation was successful.

|

.. code-block:: cpp

	bool ApplyTM (double Time, std::string Compound, CTransformMatrix Transformation)

Transforms matrix of distributed parameters of solids for specified compound ``Compound`` and time point ``Time`` by applying a movement matrix ``Transformation``. Dimensions of transformation matrix should not include distribution by compounds (``DISTR_COMPOUNDS``). Returns ``true`` if the transformation was successful.

|

.. code-block:: cpp

	void NormalizeDistribution(double Time)
	
Normalizes data in solid distribution matrix at the specified time point ``Time``. If ``Time`` has not been defined, nothing will be done.

|

.. code-block:: cpp

	void NormalizeDistribution(double Start, double End)
	
Normalizes data in solid distribution matrix in each time point from interval [``Start``; ``End``]. 

|

.. code-block:: cpp

	void NormalizeDistribution()
	
Normalizes data in solid distribution matrix in all defined time points.

|

Praticle size distribution
--------------------------








.. _label-thermo:

Thermodynamics
==============









