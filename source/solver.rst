
.. _label-solver:

======
Solver
======

Basic solver provides interfaces to the simulation system and to units. You can implement your own solver and add it to the solvers library. The solver can also be added to a unit as a parameter.

.. image:: ./pics/solver-structure.png
   :width: 600px
   :alt: 
   :align: center 

The :ref:`label-agg-solvers` for agglomeration are currently available in Dyssol.

|

Built-in equation solvers
-------------------------

Dyssol uses IDA and KINSOL solvers in `SUNDIALS package <https://computation.llnl.gov/projects/sundials>`_.

.. image:: ./pics/solver.png
   :width: 400px
   :alt: 
   :align: center 

`IDA solver <https://computing.llnl.gov/projects/sundials/ida>`_ is used for automatic calculation of DAE systems inside the units, which applies variable-order, variable-coefficient backward differentiation formulas, in fixed-leading-coefficient form.

`KINSOL solver <https://computing.llnl.gov/projects/sundials/kinsol>`_ is used for calculation of nonlinear algebraic systems, which applies a fixed-point iteration with Anderson acceleration.

.. seealso:: Skorych et al., Investigation of an FFT-based solver applied to dynamic flowsheet simulation of agglomeration processes, Advanced Powder Technology, 30 (2019).

|

Solver development
------------------





















