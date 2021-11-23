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

  * :ref:`ratio_municipalityOFcanton`
  * :ref:`canton2municipality`
  * :ref:`dgep_run_HH`
  * :ref:`run_reg`
  * :ref:`readinput_CH`
  * :ref:`readinput_reg`
  * :ref:`readinput_sim`
  * :ref:`run_struct2variable_reg`
  * :ref:`run_variables_reg`
  * :ref:`run_constraints_reg`
  * :ref:`run_objective_reg`
  * :ref:`run_saveoutput_reg`
  * :ref:`output_wrapper_HH`
  * :ref:`testing_HH`


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
  



.. _ratio_municipalityOFcanton:

ratio_municipalityOFcanton
~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block::

   ratio_bfs = ratio_municipalityOFcanton(list_bfs)

* Description

  * This script calculates the ratio of the municipality results to the cantonal ones using Sonnendach dataset

* Parameters

  * ``list_bfs``:  municipality index (bfs number) or name
 
* What the function returns

  * return the solar deployment potential ratio of specific municipality out of the canton [area, irr, consumption]

.. note::
  This function is currently not used as the conversion from the cantonal or district-level outputs to the municipal ones are done directly in ``output_wrapper_HH``.
  
  
  
.. _canton2municipality: 
 
canton2municipality
~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block::

   res_bfs = canton2municipality(list_bfs)

* Description

  * This script converts cantonal results into the ones of municipality using the calculated municipality ratio

* Parameters

  * ``list_bfs``:  municipality index (bfs number) or name
 
* What the function returns

  * converted municipal results based on the corresponding cantonal results and the municipality ratio

.. note::
  This function is currently not used as the conversion from the cantonal or district-level outputs to the municipal ones are done directly in ``output_wrapper_HH``.
 
 

.. _dgep_run_HH:

dgep_run_HH
~~~~~~~~~~~~

.. code-block::

   resDistIv_HH_agg = dgep_run_HH(obj, CGEPtoDGEP, data, ScenarioId, ExaminedYear, ...
   T, RepresentPeriods,NumSameSimulate)

* Description

  * This is the main running script for the HH submodule

* Parameters

  * ``obj``: DistIv object
  * ``CGEPtoDGEP``: CentIv-to-DistIv input data structure
  * ``data``: input data structure retrieved from the database
  * ``ScenarioId``: index for the simulated scenario
  * ``ExaminedYear``: e.g., 2020, 2030, 2040 or 2050
  * ``T``: simulated hours for each examined year
  * ``RepresentPeriods``: set to 1 at the moment and can be set to other numbers when representative days/weeks are used
  * ``NumSameSimulate``: set to 2 if every one of the two days is simulated for the operational decisions (to reduce the computational burden) 


* What the function returns

  * Optimal investments and dispatch decisions made by households in all regions (i.e., municipalities in Switzerland), candidate units considered currently include rooftop solar and PV-battery units



.. _run_reg:

run_reg
~~~~~~~~

.. code-block::

   res = run_reg(obj, input, i_reg, i_scen, ExaminedYear, i_consume, NumSameSimulate, ...
   dgepfolder, dgepfolder_output, i_geores)


* Description

  * This is the function to run the optimization for residential customer groups at a specific region (i.e., municipality, district or canton) with certain annual electricity consumption level

* Parameters

  * ``obj``: DistIv objective
  * ``input``: input data structure
  * ``ExaminedYear``: e.g., 2020, 2030, 2040 or 2050
  * ``i_consume``: index of the predefined annual electricity consumption level
  * ``NumSameSimulate``: set to 2 if every one of the two days is simulated for the operational decisions (to reduce the computational burden) 
  * ``dgepfolder``: path of the DistIv folder
  * ``dgepfolder_output``: path of the DistIv output folder
  * ``i_geores``: index of the spatial resolution level (i.e., 1~3 corresponding to canton-, district- and municipality-level, respectively)
  

* What the function returns

  * Optimal investments and dispatch decisions made by residential customer groups at a specific region with certain annual electricity consumption level



.. _readinput_CH:

readinput_CH
~~~~~~~~~~~~~~~~~~~~~~~

.. code-block::

   input = readinput_CH(obj, CGEPtoDGEP, data, ScenarioId, ExaminedYear, T, NumSameSimulate, dgepfolder)

* Description

  * This script is used to load the general simulation input data and parameters for all regions in Switzerland used by DistIv
  
* What the function returns

  * General input data structure applied for Switzerland
  
  
.. _readinput_reg:

readinput_reg
~~~~~~~~~~~~~~~~~~~~~~~

.. code-block::

   input = readinput_reg(input, i_reg, index_reg, i_canton, dgepfolder, dgepfolder_output, i_geores)

* Parameters

  * ``input``: input data structure
  * ``i_reg``: index of the region
  * ``index_reg``: official index of the region (this is official index published by the government)
  * ``i_canton``: official index of the canton the region is associated to
  * ``dgepfolder``: path of the main DistIv folder
  * ``dgepfolder_output``: path of the main DistIv output folder
  * ``i_geores``: index of the spatial resolution (1~3 corresponding to cantonal, district and municipal level)

* Description

  * This script is used to load the general simulation input data and parameters for a specific region considering predefined spatial resolution
  
* What the function returns

  * Input data structure for a specific region


.. _readinput_sim:

readinput_sim
~~~~~~~~~~~~~~~~~~~~~~~

.. code-block::

   input = readinput_sim(input, i_reg, i_consume, ExaminedYear, n_periods, dgepfolder, dgepfolder_output, i_geores)

* Parameters

  * ``i_consume``: index of the annual electricity consumption level

* Description

  * This script is used to load the input data and parameters for the specific simulation corresponding to a specific residential customer group
  
* What the function returns

  * Input data structure for a specific customer group
  

.. _run_struct2variable_reg:

run_struct2variable_reg
~~~~~~~~~~~~~~~~~~~~~~~~~~~

* Description

  * This script is used to unwrapp the input data/parameters so as to be used for the HH submodule



.. _run_variables_reg:

run_variables_reg
~~~~~~~~~~~~~~~~~~~~~

* Description

  * This script is used to define variables used for the HH submodule

.. warning::
    Currently system-controlled DSM is implemented in the DSO submodule, 

* Variables

  * ``x_inv_pv``: continuous investment capacities for PVs per size category type, per roof size area and per irradiation level
  * ``u_inv_pv``: binary investment decisions for PVs per size category type, per roof size area and per irradiation level
  * ``x_inv_bat_e``: invested energy capacity for PV-battery per battery category type, per roof size area and per irradiation level
  * ``x_inv_bat_p``: invested power capacity for PV-battery per battery category type, per roof size area and per irradiation level

  * ``PV_selfconsume``: hourly self-consumed PV generation per roof size area and per irradiation level
  * ``PV_gen_ann``: annual PV generation per PV size category, per roof size area and per irradiation level
  * ``PV_generation``:hourly PV generation per roof size area and per irradiation level
  * ``PV_curtail``: hourly PV curtailment per roof size area and per irradiation level
  * ``p_l_solar``: hourly demand per roof size area and per irradiation level
  * ``r_Lu``: hourly upward load shifting per roof size area and per irradiation level
  * ``r_Ld``: hourly downward load shifting per roof size area and per irradiation level
  * ``p_l_solar_new``: hourly updated demand per roof size area and per irradiation level

  * ``p_pv2grid``: hourly power flow from PV system to grid per roof size area and per irradiation level
  * ``p_grid2pv``: hourly power flow from grid system to PV per roof size area and per irradiation level
  * ``p_pv2load``: hourly power flow from PV system to load per roof size area and per irradiation level
  * ``p_pv2bat``: hourly power flow from PV system to battery per roof size area and per irradiation level
  * ``p_bat2load``: hourly power flow from battery to load per roof size area and per irradiation level

  * ``p_h_solar``: hourly discharging power of pv-battery per roof size area and per irradiation level
  * ``p_hc_solar``: hourly charging power of pv-battery per roof size area and per irradiation level
  * ``u_h_solar``: binary variable for charging/discharging status of pv-battery per roof size area and per irradiation level
  * ``E_h_solar``: hourly SOC (state-of-charge) of the pv-battery per roof size area and per irradiation level


.. _run_constraints_reg:

run_constraints_reg
~~~~~~~~~~~~~~~~~~~~~~~

* Description

  * This script is used to define constraints used for the optimization



.. _run_objective_reg:

run_objective_reg
~~~~~~~~~~~~~~~~~~~~~

* Description

  * This script is used to define objective used for the optimization



.. _run_saveoutput_reg:

run_saveoutput_reg
~~~~~~~~~~~~~~~~~~

* Description

  * This sript is used to save and process important results into the output structure



.. _output_wrapper_HH:

output_wrapper_HH
~~~~~~~~~~~~~~~~~

.. code-block::

   resDistIv_HH_agg = output_wrapper_HH(mapping_table, ScenarioId, ExaminedYear, dgepfolder, ...
   dgepfolder_output, i_geores)

* Description

  * This is the function to combine HH submodule outputs for different regions into one output structure
  


.. _testing_HH_reg:

testing_HH_reg
~~~~~~~~~~~~~~

* Description

  * This is the stanadlone running script for the HH submodule




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
  * ``n_periods``: number of simulation periods examined
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
~~~~~~~~~~~

* Description

  * This is the module wrapper class for the DistIv module

.. code-block::

   obj = dgepModule(wspace,conn)
   
* Description

  * This is the function for initializing the running DistIv module
  
* Parameters

  * ``obj``: DistIv object
  * ``wspace``: structure with information of the simulated scenario 
  * ``conn``: structure with MySQL database connection information


.. code-block::

   DGEPtoCGEP = sendToCgep(obj)
   
* Description

  * This is the function for generating the interface data used by CentIv (used to call CGEP)


.. code-block::

   DGEPtoCGE = sendToCge(obj) 
   
* Description

  * This is the function for generating the interface data used by Gmel (used to call CGE)


.. code-block::

   DGEPtoEEM = sendToEEM(obj) 
   
* Description

  * This is the function for generating the interface data used by eMark (used to call EEM)

.. note::
   This interface with the market module, i.e., eMark, was not established yet and remains as the future work.



.. _dgep_pullfromdatabase:

dgep_pullfromdatabase
~~~~~~~~~~~~~~

.. code-block::

   input_database = dgep_pullfromdatabase(wspace,conn)

* Description

  * This file is used to pull DistIv data from the MySQL database into Matlab

* What the function returns

  * Input data structure with data that is used by distiv from MySQL
  
  
.. _dgep_convertDatabaseData:

dgep_convertDatabaseData
~~~~~~~~~~~~~~~~~~~~

.. code-block::

   data_distiv = dgep_convertDatabaseData(input_database)

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

* Input Parameters

  * ``data``: input data structure retrieved from the database
  * ``NumSameSimulate``: set to 2 if every one of the two days is simulated for the operational decisions (to reduce the computational burden) 
  * ``RepresentPeriods``: set to 1 at the moment and can be set to other numbers when representative days/weeks are used
  * ``T``: simulated hours for each examined year
  * ``pr_RM_add``: adjustment added to the reserve prices from CentIv (set to zero currently)
  * ``CGEPtoDGEP``: CentIv-to-DistIv input data structure
  * ``ScenarioId``: index for the simulated scenario
  * ``mapping_table``:  mapping data defining the relationship between canton, district, municipality, and transmission nodes at the tranmission level
  
* Output Parameters

  * ``demand``: demand structure including ``pr_DAtotal`` and ``pr_DA``
  * ``pr_DAtotal``: hourly wholesale energy price profile per transmission node from CentIv [hour*node]
  * ``pr_DA``: converted hourly wholesale energy price profile per municipality [hour*municipality]
  * ``pr_cRMu``: converted hourly upward reserve capacity provision price profile per municipality [hour*municipality]
  * ``pr_cRMd``: converted hourly downward reserve capacity provision price profile per municipality [hour*municipality]
  * ``T``: simulated hours for each examined year
  * ``RepresentPeriods``: set to 1 at the moment and can be set to other numbers when representative days/weeks are used
  * ``nNode``: number of transmission nodes in CentIv for the examined year
  * ``node2muni``: matrix to map the tranmission node in CentIv to the municipality in Switzerland [node*municipality]
  * ``muni2node``: matrix to map the municipality in Switzerland to the tranmission node in CentIv [municipality*node]
  * ``demandMuni``: converted hourly demand profile per municipality [hour*municipality]
  * ``P_RMu_req``: hourly upward reserve bidding requirement from CentIv [hour*1]
  * ``P_RMd_req``: hourly downward reserve bidding requirement from CentIv [hour*1]
  * ``raw_demand``: raw data of the excel related to demand from CentIv
  * ``raw_Reserves``: raw data of the excel related to reserves from CentIv


* What the function returns

  * Processed interface data from CentIv and solve the inconsistency in spatial resolution so as to be used by DistIv


.. _municipality2node:

municipality2node
~~~~~~~~~~~~~~

* Description: the script is used to create mapping between municipalities and transmission nodes, i.e., mapping table ``mapping_table_b2020`` before 2020 and mapping table ``mapping_table_a2020`` after 2020

.. note::
   The script needs to run once to create the input mapping matrix and need to only run again if a new set of tranmission nodes are used;
   
   The process is differentiated before and after 2020 due to the changes of transmission nodes in CentIv.
   
   

.. _dgep_saveoutput:

dgep_saveoutput
~~~~~~~~~~~~~~

.. code-block::

   resDistIv = dgep_saveoutput(obj, resDistIv_HH_agg, resDistIv_LC_agg, resDistIv_DSO_agg)

* Description: this script is used to save and process important results from three submodules (i.e., HH, LC and DSO) into the output structure ``resDistIv``

* Input Parameters

  * ``obj``: DistIv object
  * ``resDistIv_HH_agg``: aggregated output structure of HH submodule
  * ``resDistIv_LC_agg``: aggregated output structure of LC submodule
  * ``resDistIv_DSO_agg``: aggregated output structure of DSO submodule

* What the function returns

  * Processed aggregated output structure of DistIv
 
 
.. _results_check:

results_check
~~~~~~~~~~~~~~


* Description: This script is used to quickly check the DistIV resuklts based on the generation and consumption missmatch



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

