Usage
=====

.. note:: 

add note here....




.. _installation:

Installation
------------

To run DistIv module on ETH server `Euler <https://scicomp.ethz.ch/wiki/Getting_started_with_clusters>`_, please refer to `Nexus-e document <https://nexus-e.readthedocs.io/en/latest/index.html>`_.

To run DistIv module on your own machine, please install/download:

* Matlab
* `YALMIP <https://yalmip.github.io>`_
* Gurobi
 



.. _structure:

Code Structure
--------------

* input
* runHH_district (Check out the :ref:`HH submodule` section)

  * with a nested list
  * and some subitems
  
* runLC_municipality (Check out the :ref:`LC submodule` section)

  * dgep_run_LC.m :ref:`dgep_run_LC`
  * dgep_Run_LC_reg.m		
  * dgep_struct2variable_LC_reg.m
  * dgep_Variables_LC_reg.m		
  * dgep_Constraints_LC_reg.m	
  * dgep_Objective_LC_reg.m		
  * dgep_saveoutput_LC_reg.m
  * output_wrapper_LC.m
  * testing_LC_reg.m
  
* runDSO_municipality (Check out the :ref:`DSO submodule` section)

  * dgep_run_DSO.m
  * dgep_Run_DSO_reg.m		
  * dgep_Variables_DSO_reg.m	
  * dgep_struct2variable_DSO_reg.m
  * dgep_Constraints_DSO_reg.m	
  * dgep_Objective_DSO_reg.m	
  * dgep_saveoutput_DSO_reg.m
  * output_wrapper_DSO.m
  * testing_DSO_reg.m


* and here the parent list continues


.. _HH:

HH (household) submodule
------------------------

.. _LC:

LC (large consumer) submodule
-----------------------------

.. _dgep_run_LC:

To retrieve the optimal investments and dispatch decisions made by large consumers, you can run the ``dgep_run_LC()`` function:

.. code-block::

   resDistIv_LC_agg = dgep_run_LC(obj,CGEPtoDGEP, data, ScenarioId, ExaminedYear, T, RepresentPeriods, NumSameSimulate)

* Description

  * This is the main running script for the LC submodule

* Parameters

  * ``obj``: DistIv object
  * ``CGEPtoDGEP``: CentIv-to-DistIv input data structure
  * ``ScenarioId``: index for the simulated scenario
  * ``ExaminedYear``: e.g., 2020, 2030, 2040 or 2050
  * ``T``: simulated hours for each examined year
  * ``RepresentPeriods``: set to 1 at the moment and can be set to other numbers when representative days/weeks are used
  * ``NumSameSimulate``: set to 2 if every one of the two days is simulated for the operational decisions (to reduce the computational burden) 


* What the function returns

  * Optimal investments and dispatch decisions made by large consumers for each region (i.e., municipalities in Switzerland)



To test the standalone version of the LC submodule, you can run the ``testing_LC_reg()`` function.

* Description

  * This is the stanadlone running script for the LC submodule
  
  
  


.. _DSO:

DSO (distribution system operator) submodule
--------------------------------------------

To retrieve a list of random ingredients,
you can use the ``lumache.get_random_ingredients()`` function:

.. code-block::

   function resDistIv_LC_agg = dgep_run_LC(obj,CGEPtoDGEP, data, ScenarioId, ExaminedYear, T,...
    RepresentPeriods,NumSameSimulate)

* Description

* Parameters

* What the function returns

  
  


Creating recipes
----------------

To retrieve a list of random ingredients,
you can use the ``lumache.get_random_ingredients()`` function:

.. autofunction:: lumache.get_random_ingredients

The ``kind`` parameter should be either ``"meat"``, ``"fish"``,
or ``"veggies"``. Otherwise, :py:func:`lumache.get_random_ingredients`
will raise an exception.

.. autoexception:: lumache.InvalidKindError

For example:

>>> import lumache
>>> lumache.get_random_ingredients()
['shells', 'gorgonzola', 'parsley']

