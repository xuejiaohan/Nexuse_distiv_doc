Usage
*****



.. note::
   The project is under active development and the document is subject to changed made to codes




.. _installation:

Installation
============

To run DistIv module on ETH server `Euler <https://scicomp.ethz.ch/wiki/Getting_started_with_clusters>`_, please refer to `Nexus-e document <https://nexus-e.readthedocs.io/en/latest/index.html>`_.

To run DistIv module on your own machine, please install/download:

* Matlab
* `YALMIP <https://yalmip.github.io>`_
* Gurobi
 



.. _structure:

Code Structure
==============

* input
* runHH_district (Check out the :ref:`HH` section)

  * testing_HH
  * ratio_municipalityOFcanton
  * readinput_CH
  * readinput_reg
  * readinput_sim
  * canton2municipality
  * dgep_run_HH
  * run_reg
  * run_struct2variable_reg
  * run_variables_reg
  * run_constraints_reg
  * run_objective_reg
  * run_postprocess_reg_full
  * run_saveoutput_reg
  * output_wrapper_HH


* runLC_municipality (Check out the :ref:`LC` section)

  * :ref:`dgep_run_LC`
  * :ref:`dgep_Run_LC_reg`	
  * :ref:`dgep_struct2variable_LC_reg`
  * :ref:`dgep_Variables_LC_reg`
  * :ref:`dgep_Constraints_LC_reg`	
  * :ref:`dgep_Objective_LC_reg`		
  * :ref:`dgep_saveoutput_LC_reg`
  * :ref:`output_wrapper_LC`
  * :ref:`testing_LC_reg`


* runDSO_municipality (Check out the :ref:`DSO` section)

  * :ref:`dgep_run_DSO`
  * :ref:`dgep_Run_DSO_reg`		
  * :ref:`dgep_Variables_DSO_reg`	
  * :ref:`dgep_struct2variable_DSO_reg`
  * :ref:`dgep_Constraints_DSO_reg`	
  * :ref:`dgep_Objective_DSO_reg`	
  * :ref:`dgep_saveoutput_DSO_reg`
  * :ref:`output_wrapper_DSO`
  * :ref:`testing_DSO_reg`


* :ref:`dgepModule`				
* :ref:`dgep_pullfromdatabase`			
* :ref:`dgep_convertDatabaseData`
* :ref:`dgep_ReadCgepData`
* :ref:`municipality2node`
* :ref:`dgep_saveoutput`	
* :ref:`results_check`


.. _HH:

HH submodule
------------------------

.. note::
   Some of the parameters or variables shared by different functions or scripted are described only once to avoid redundancy.



.. _LC:

LC submodule
-----------------------------

.. note::
   Some of the parameters or variables shared by different functions or scripted are described only once to avoid redundancy.


.. _dgep_run_LC:

dgep_run_LC
~~~~~~~~~~~~

.. code-block::

   resDistIv_LC_agg = dgep_run_LC(obj,CGEPtoDGEP, data, ScenarioId, ExaminedYear, ...
   T, RepresentPeriods, NumSameSimulate)

* Description

  * This is the main running script for the LC submodule

* Parameters

  * ``obj``: DistIv object
  * ``data``: input data structure retrieved from the database
  * ``CGEPtoDGEP``: CentIv-to-DistIv input data structure
  * ``ScenarioId``: index for the simulated scenario
  * ``ExaminedYear``: e.g., 2020, 2030, 2040 or 2050
  * ``T``: simulated hours for each examined year
  * ``RepresentPeriods``: set to 1 at the moment and can be set to other numbers when representative days/weeks are used
  * ``NumSameSimulate``: set to 2 if every one of the two days is simulated for the operational decisions (to reduce the computational burden) 


* What the function returns

  * Optimal investments and dispatch decisions made by large consumers in all regions (i.e., municipalities in Switzerland), candidate units considered currently include biomass (wood-based and manure-based) and CHP units



.. _dgep_Run_LC_reg:

dgep_Run_LC_reg
~~~~~~~~~~~~~~~

.. code-block::

   resDistIv_LC = dgep_Run_LC_reg(CGEPtoDGEP, data, ScenarioId, ExaminedYear, ...
   T, RepresentPeriods, NumSameSimulate, n_periods, nMuni,...
   i_Muni_start, i_Muni_end, alpha_ex, T_r, dgepfolder,dgepfolder_output)

* Description

  * This is the function to run the optimization for a number of regions (i.e., municipalities)

* Parameters

  * ``data``: input data structure retrieved from the database
  * ``CGEPtoDGEP``: CentIv-to-DistIv input data structure
  * ``ScenarioId``: index for the simulated scenario
  * ``ExaminedYear``: e.g., 2020, 2030, 2040 or 2050
  * ``T``: simulated hours for each examined year
  * ``RepresentPeriods``: set to 1 at the moment and can be set to other numbers when representative days/weeks are used
  * ``NumSameSimulate``: set to 2 if every one of the two days is simulated for the operational decisions (to reduce the computational burden) 
  * ``n_periods``: 
  * ``nMuni``: number of municipalities to be optimized for each run (adjusted considering a trade-off between the time consumed for each run and the total number of runnings, as optimizations for each municipality can be made in parallel)
  * ``i_Muni_start``: index of the municipality to start with 
  * ``i_Muni_end``: index of the municipality to end with
  * ``alpha_ex``: ratio used to approximate the transformer capacity together with the peak demand (i.e., trafo capacity = alpha_ex * peak demand)
  * ``T_r``: time length of the reserve product
  * ``dgepfolder``: path of the DistIv folder
  * ``dgepfolder_output``: path of the DistIv output folder
  

* What the function returns

  * Optimal investments and dispatch decisions made by large consumers for a number of regions (number = nMuni)



.. _dgep_struct2variable_LC_reg:

dgep_struct2variable_LC_reg
~~~~~~~~~~~~~~~~~~~~~~~~~~~

* Description

  * This script is used to unwrapp the input data/parameters so as to be used for the LC submodule

* Data structure

  * ``TimeSeries``: time series data
  * ``units``: characteristics of the candidate units
  * ``general``: general identifications of the candidate units
  * ``costs``: cost related data
  * ``region``: region related data
  * ``dsm``: demand side management related data
  * ``mapping_table``: mapping data defining the relationship between canton, district, municipality, and transmission nodes at the tranmission level


.. _dgep_Variables_LC_reg:

dgep_Variables_LC_reg
~~~~~~~~~~~~~~~~~~~~~

* Description

  * This script is used to define variables used for the LC submodule

.. warning::
    Currently system-controlled DSM is implemented in the DSO submodule, changes need to be made if later self-controlled DSM in LC submodule is needed.

* Variables

  * ``x_exist``: existing capacities per technology type and per simulated municipality
  * ``p_exist``: power output of existing units per technology and per simulated municipality
  * ``p_exist_curt``:curtailment of existing units per technology and per simulated municipality
  * ``p_exist_yearly``: power output of existing units per technology per installed year
  * ``x_exist``: existing capacities per technology type and per simulated municipality
  * ``R_Ug_exist``: upward reserve provided by existing conventional generation units per simulated municipality
  * ``R_Dg_exist``: downward reserve provided by existing conventional generation units per simulated municipality

  * ``x_inv``: continuous investment capacities per technology type and per simulated municipality
  * ``p_unit``: power output of newly invested units per technology type and per simulated municipality
  * ``p_unit_curt``: power curtailment of newly invested units per technology type and per simulated municipality

  * ``p_lnew``: updated load profile after shifting
  * ``r_Lu``: upward load shifting (load increase)
  * ``r_Ld``: downward load shifting (load decrease)

  * ``P_DA``: hourly electricity exchange with selling indicated by positive values per simulated municipality 
  * ``P_DAs``: hourly electricity sale per simulated municipality
  * ``P_DAb``: hourly electricity purchase per simulated municipality

  * ``R_Ug``: hourly upward reserve provided by the dispatchable generation unit per simulated municipality 
  * ``R_Dg``: hourly downward reserve provided by the dispatchable generation unit per simulated municipality 
  * ``R_Ul``: hourly upward reserve provided by flexible load units (DSM) per simulated municipality 
  * ``R_Dl``: hourly downward reserve provided by flexible load units (DSM) per simulated municipality 

  * ``P_RMu_res``: total hourly residual upward reserve requirement after considering the DER reserve provision
  * ``P_RMd_res``: total hourly residual downward reserve requirement after considering the DER reserve provision
  * ``P_RMu_bid``: total hourly upward reserve bidding per simulated municipality 
  * ``P_RMd_bid``: total hourly downward reserve bidding per simulated municipality 

  * ``C_inv``: investment cost
  * ``C_foc``: fixed operating cost
  * ``C_voc``: variable operating cost
  * ``C_fuel``: fuel cost
  * ``C_discom``: discomfort cost for dispatching DSM
  * ``C_emi``: emission cost
  * ``R_selfcon``: revenues from self-consumption savings
  * ``R_markets``: revenues from bidding energy/reserve into the markets
  * ``R_subsidy``: revenues from subsidy
  * ``R_heat``: revenues from heat credit


.. _dgep_Constraints_LC_reg:

dgep_Constraints_LC_reg
~~~~~~~~~~~~~~~~~~~~~~~

* Description

  * This script is used to define constraints used for the optimization



.. _dgep_Objective_LC_reg:

dgep_Objective_LC_reg
~~~~~~~~~~~~~~~~~~~~~

* Description

  * This script is used to define objective used for the optimization



.. _dgep_saveoutput_LC_reg:

dgep_saveoutput_LC_reg
~~~~~~~~~~~~~~~~~~~~~

* Description

  * This sript is used to save and process important results into the output structure



.. _output_wrapper_LC:

output_wrapper_LC
~~~~~~~~~~~~~~~~~

.. code-block::

   resDistIv_LC_agg = output_wrapper_LC(ScenarioId,ExaminedYear,dgepfolder,dgepfolder_output)

* Description

  * This is the function to combine LC submodule outputs for different regions (i.e., municipalities) into one output structure
  


.. _testing_LC_reg:

testing_LC_reg
~~~~~~~~~~~~~~

* Description

  * This is the stanadlone running script for the LC submodule
  
  
  


.. _DSO:

DSO submodule
--------------------------------------------

.. note::
   Some of the parameters or variables shared by different functions or scripted are described only once to avoid redundancy.

  
.. _dgep_run_DSO:

dgep_run_DSO
~~~~~~~~~~~~~~

.. code-block::

   resDistIv_DSO_agg = dgep_run_DSO(obj,CGEPtoDGEP, data, ScenarioId, ExaminedYear, ...
   T, RepresentPeriods, NumSameSimulate)

* Description

  * This is the main running script for the DSO submodule

* Parameters

  * ``obj``: DistIv object
  * ``data``: input data structure retrieved from the database
  * ``CGEPtoDGEP``: CentIv-to-DistIv input data structure
  * ``ScenarioId``: index for the simulated scenario
  * ``ExaminedYear``: e.g., 2020, 2030, 2040 or 2050
  * ``T``: simulated hours for each examined year
  * ``RepresentPeriods``: set to 1 at the moment and can be set to other numbers when representative days/weeks are used
  * ``NumSameSimulate``: set to 2 if every one of the two days is simulated for the operational decisions (to reduce the computational burden) 


* What the function returns

  * Optimal investments and dispatch decisions made by DSO in all regions (i.e., municipalities in Switzerland), investment options considered currently include grid-batteries, demand side management and transformer upgrade





.. _dgep_Run_DSO_reg:

dgep_Run_DSO_reg
~~~~~~~~~~~~~~

.. code-block::

   resDistIv_DSO = dgep_Run_DSO_reg(CGEPtoDGEP, data, ScenarioId, ExaminedYear, ...
   T, RepresentPeriods, NumSameSimulate, n_periods, nMuni,...
   i_Muni_start, i_Muni_end, alpha_ex, T_r, dgepfolder,dgepfolder_output)

* Description

  * This is the function to run the optimization for a number of regions (i.e., municipalities)

* Parameters

  * ``data``: input data structure retrieved from the database
  * ``CGEPtoDGEP``: CentIv-to-DistIv input data structure
  * ``ScenarioId``: index for the simulated scenario
  * ``ExaminedYear``: e.g., 2020, 2030, 2040 or 2050
  * ``T``: simulated hours for each examined year
  * ``RepresentPeriods``: set to 1 at the moment and can be set to other numbers when representative days/weeks are used
  * ``NumSameSimulate``: set to 2 if every one of the two days is simulated for the operational decisions (to reduce the computational burden) 
  * ``n_periods``: 
  * ``nMuni``: number of municipalities to be optimized for each run (adjusted considering a trade-off between the time consumed for each run and the total number of runnings, as optimizations for each municipality can be made in parallel)
  * ``i_Muni_start``: index of the municipality to start with 
  * ``i_Muni_end``: index of the municipality to end with
  * ``alpha_ex``: ratio used to approximate the transformer capacity together with the peak demand (i.e., trafo capacity = alpha_ex * peak demand)
  * ``T_r``: time length of the reserve product
  * ``dgepfolder``: path of the DistIv folder
  * ``dgepfolder_output``: path of the DistIv output folder
  

* What the function returns

  * Optimal investments and dispatch decisions made by DSOs for a number of regions (number = nMuni)




.. _dgep_Variables_DSO_reg:

dgep_Variables_DSO_reg
~~~~~~~~~~~~~~~~~~~~~~
* Description

  * This script is used to define variables used for the DSO submodule

* Variables

  * ``x_exist``: existing capacities per technology type and per simulated municipality
  * ``p_exist``: power output of existing units per technology and per simulated municipality
  * ``p_exist_curt``:curtailment of existing units per technology and per simulated municipality
  * ``p_exist_yearly``: power output of existing units per technology per installed year
  * ``x_exist``: existing capacities per technology type and per simulated municipality
  * ``p_h_exist``: discharging power of existing storage units
  * ``p_hc_exist``: charging power of existing storage units
  * ``u_h_exist``: binary variable for charging/discharging status
  * ``E_h_exist``: SOC of the existing storage units

  * ``R_Us_exist``: upward reserve provided by existing storage units per simulated municipality
  * ``R_Ds_exist``: downward reserve provided by existing storage units per simulated municipality


  * ``x_inv``: continuous investment capacities per technology type and per simulated municipality
  * ``x_inv_trafo``: continuous investment capacities for transformer upgrade per simulated municipality
  * ``p_unit``: power output of newly invested units per technology type and per simulated municipality
  * ``p_unit_curt``: power curtailment of newly invested units per technology type and per simulated municipality

  * ``p_h``: discharging power of the newly invested storage units
  * ``p_hc``: charging power of the newly invested storage units
  * ``u_h``: binary variable for charging/discharging status
  * ``E_h``: SOC of the newly invested storage units

  * ``p_lnew``: updated load profile after shifting
  * ``r_Lu``: upward load shifting (load increase)
  * ``r_Ld``: downward load shifting (load decrease)

  * ``P_DA``: hourly electricity exchange with selling indicated by positive values per simulated municipality 
  * ``P_DAs``: hourly electricity sale per simulated municipality
  * ``P_DAb``: hourly electricity purchase per simulated municipality

  * ``R_Us``: hourly upward reserve provided by the storage unit per simulated municipality 
  * ``R_Ds``: hourly downward reserve provided by the storage unit per simulated municipality 
  * ``R_Ul``: hourly upward reserve provided by flexible load units (DSM) per simulated municipality 
  * ``R_Dl``: hourly downward reserve provided by flexible load units (DSM) per simulated municipality 

  * ``P_RMu_res``: total hourly residual upward reserve requirement after considering the DER reserve provision
  * ``P_RMd_res``: total hourly residual downward reserve requirement after considering the DER reserve provision
  * ``P_RMu_bid``: total hourly upward reserve bidding per simulated municipality 
  * ``P_RMd_bid``: total hourly downward reserve bidding per simulated municipality 

  * ``C_inv``: investment cost
  * ``C_foc``: fixed operating cost
  * ``C_voc``: variable operating cost
  * ``C_discom``: discomfort cost for dispatching DSM
  * ``R_markets``: revenues from bidding energy/reserve into the markets



.. _dgep_struct2variable_DSO_reg:

dgep_struct2variable_DSO_reg
~~~~~~~~~~~~~~~~~~~~~~


* Description

  * This script is used to unwrapp the input data/parameters so as to be used for the DSO submodule

* Data structure

  * ``TimeSeries``: time series data
  * ``units``: characteristics of the candidate units
  * ``general``: general identifications of the candidate units
  * ``costs``: cost related data
  * ``region``: region related data
  * ``dsm``: demand side management related data
  * ``mapping_table``: mapping data defining the relationship between canton, district, municipality, and transmission nodes at the tranmission level



.. _dgep_Constraints_DSO_reg:

dgep_Constraints_DSO_reg
~~~~~~~~~~~~~~~~~~~~~~
* Description

  * This script is used to define constraints used for the optimization


.. _dgep_Objective_DSO_reg:

dgep_Objective_DSO_reg
~~~~~~~~~~~~~~~~~~~~~~
* Description

  * This script is used to define objective used for the optimization



.. _dgep_saveoutput_DSO_reg:

dgep_saveoutput_DSO_reg
~~~~~~~~~~~~~~~~~~~~~~

* Description

  * This sript is used to save and process important results into the output structure


.. _output_wrapper_DSO:

output_wrapper_DSO
~~~~~~~~~~~~~~~~~~

.. code-block::

   resDistIv_DSO_agg = output_wrapper_DSO(ScenarioId,ExaminedYear,dgepfolder,dgepfolder_output)

* Description

  * This is the function to combine DSO submodule outputs for different regions (i.e., municipalities) into one output structure



.. _testing_DSO_reg:

testing_DSO_reg
~~~~~~~~~~~~~~

* Description

  * This is the stanadlone running script for the DSO submodule


  
.. _others:

Other scripts
--------------------------------------------

.. _dgepModule:

dgepModule
~~~~~~~~~~~~~~

.. code-block::

   data_distiv = convertDatabaseData(input_database)

* Description

  * This is used to restructure the data from the MySQL database to be used in DistIv



.. _dgep_pullfromdatabase:

dgep_pullfromdatabase
~~~~~~~~~~~~~~

.. code-block::

   input_database = dgep_pullfromdatabase(wspace,conn)

* Description

  * This file is used to pull DistIv data from the MySQL database into Matlab

* What the function returns

  * Input data structure with data that is used by distiv from MySQL
  
  
.. _convertDatabaseData:

convertDatabaseData
~~~~~~~~~~~~~~

.. code-block::

   data_distiv = convertDatabaseData(input_database)

* Description

  * This is used to restructure the raw DistIv data from the MySQL database to be used in DistIv

* What the function returns

  * Input data structure with the distiv inputs



.. _dgep_ReadCgepData:

dgep_ReadCgepData
~~~~~~~~~~~~~~

.. code-block::

   [ demand, pr_DAtotal, pr_DA, pr_cRMu, pr_cRMd, T, RepresentPeriods, nNode, node2muni, ...
   muni2node, demandMuni, P_RMu_req, P_RMd_req, raw_demand, raw_Reserves ]...
    = dgep_ReadCgepData(data, NumSameSimulate, RepresentPeriods, T, pr_RM_add, CGEPtoDGEP, mapping_table)

* Description

  * This function is used to read and process and input data from CentIv

* Parameters

  * ``data``: input data structure retrieved from the database
  * ``NumSameSimulate``: set to 2 if every one of the two days is simulated for the operational decisions (to reduce the computational burden) 
  * ``RepresentPeriods``: set to 1 at the moment and can be set to other numbers when representative days/weeks are used
  * ``T``: simulated hours for each examined year
  * ``pr_RM_add``: adjustment added to the reserve prices from CentIv (set to zero currently)
  * ``CGEPtoDGEP``: CentIv-to-DistIv input data structure
  * ``ScenarioId``: index for the simulated scenario
  * ``mapping_table``: 
  

* What the function returns

  * Processed data that need to be used by DistIv


.. _municipality2node:

municipality2node
~~~~~~~~~~~~~~

.. code-block::

   data_distiv = convertDatabaseData(input_database)

* Description


.. _dgep_saveoutput:

dgep_saveoutput
~~~~~~~~~~~~~~

.. code-block::

   data_distiv = convertDatabaseData(input_database)

* Description

 
 
.. _results_check:

results_check
~~~~~~~~~~~~~~

.. code-block::

   data_distiv = convertDatabaseData(input_database)

* Description



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

