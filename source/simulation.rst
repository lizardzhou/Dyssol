
.. _label-simulation:

===============
Simulation core
===============

In simulation core, all data are processed discretely on the time scale. Different material streams and operation units are combined into the flowsheet, which is then simulated in the simulator.


Discretization
--------------

A continuous processes is discretized using time points, and each time point is a snapshot of the process state, as shown below.

.. image:: ./pics/discrete.png
   :width: 900px
   :alt: screen
   :align: center

|

Material streams
----------------

All units in Dyssol are connected by material streams, which are described by a set of time points (time discretisation).

Overall properties define parameters for all selected phases. Three phases - solid, liquid and gas phases - are available. Each phase is distributed along compounds content.

Solid phase can be distributed along several multidimensional properties. Each stream on the flowsheet has the same set of compounds, phases and multidimensional properties.

All variables in material streams are time-dependent.

The structure of material streams is illustrated in the figure below. The information is transferred between operation units.

.. image:: ./pics/timePoint.jpg
   :width: 800px
   :alt: screen
   :align: center

|

.. seealso:: 

	a demostration file at ``<Help\Program interfaces\Stream.pdf>``.



Simulator
---------

In this section, you can find the information about the main calculation algorithm.

.. image:: ./pics/algorithm.png
   :width: 900px
   :alt: screen
   :align: center


Main method and approaches
""""""""""""""""""""""""""

Following methods are applied in Dyssol for simulation. Click the corresponding names for more background theoretical information.

- :ref:`label-seqModule`: each model is solved separately.

- Dividing of a flowsheet into :ref:`label-partition`.

- :ref:`label-waveRelax` (WRM) for dynamic calculation of recycle streams: dividing simulation time into shorter intervals.

- :ref:`label-extrapolation` to initialize each time window.

- :ref:`label-convergence` to initialize each iteration of WRM.

.. seealso:: V. Skorych et al., Novel system for dynamic flowsheet simulation of solids processes, 2017






