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
* runHH_district (Check out the :HH:`HH submodule` section)

  * with a nested list
  * and some subitems
  
* runLC_municipality (Check out the :LC:`LC submodule` section)

  * dgep_run_LC.m
  * dgep_Run_LC_reg.m		
  * dgep_struct2variable_LC_reg.m
  * dgep_Variables_LC_reg.m		
  * dgep_Constraints_LC_reg.m	
  * dgep_Objective_LC_reg.m		
  * dgep_saveoutput_LC_reg.m
  * output_wrapper_LC.m
  * testing_LC_reg.m
  
* runDSO_municipality (Check out the :DSO:`DSO submodule` section)

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



.. _DSO:

DSO (distribution system operator) submodule
--------------------------------------------


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

