Notes
=====


.. _remarks:

Remarks for Developers
------------
* To debug the code, it is suggested to:

  * turn off the parallization set in the main running script of each submodule, i.e., :ref:`dgep_run_HH`, :ref:`dgep_run_LC` and :ref:`dgep_run_DSO`
  * turn on the debug mode and the logging display by adjusting the sdpsetting of yalmip
  
* To reduce the computation time, parallization is used to run simulations for multiple regions at the same time. The number of workers for parallization can be decreased to reduce the memory requirements for the DistIv simulation, which however results in longer simulation time. In order to adjust the trade-off between the simulation time for each run and for all runs, the number of regions simulated for each run in LC and DSO submodules can be tuned. Furthermore, the spatial resolution and the number of clusters applied for household customers in the HH submodule can be adjusted to trade-off between accuracy and computational efficiency.

* In order to avoid the error of possibly writing on the same output file when using parallization in Matlab, outputs are saved only when there are no files with the same name stored in the specified output folder. Thus, it is suggested to delete the output folder whenever running new simulations. 



.. _todo:

Limitations and Future Work
------------
* Load profiles of different household customer groups are estimated using a number of synthetic load profiles that undoubtedly deviate from real-world data.  Thus, the variety of different consumption behaviors between various regions and sectors is not captured. Furthermore, due to the lack of input data, the annual electricity consumption of individual household customers is approximated using the warm water consumption data

* Aspects other than economic factors, e.g. peer effects and environmental considerations, could be integrated into the objective to better represent the motivations for investments made by different customer groups.

* Modeling of electric vehicles, which in the future are expected to account for a significant share of electricity consumption at the distribution level, needs to be incorporated.

* Please refer to reports and papers listed in :ref:`reference` Section for more detailed discussions regarding the limitations and future work for DistIv.
