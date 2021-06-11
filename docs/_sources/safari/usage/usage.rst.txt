.. _safari-running:

****************
Running |SAFARI|
****************

|SAFARI| is a command line application, and has no graphical user interaction. Adaptive grid mode does have a commandline indication of progress for the first thermal iteration, but otherwise there is no direct progress information for the run until it has finished running.

Required Input Files
====================

Most of the configuration options for |SAFARI| are provided via a primary input file, there are then some optional additional files, which are used depending on the command line arguments, or options in the main input file.

An sample input file can be located at :ref:`main_input`.

Optional Input Files
--------------------

Other optional files include:

-   :ref:`pots_files` for providing potentials from a table rather than built in methods
-   :ref:`crys_in_files` for providing externally generated surfaces, rather than auto-generating the surface

Produced Output Files
#####################

|SAFARI| outputs the data in a plain-text format, the specific files produced depend on the :ref:`run mode <safari_modes>`. More information about the produced files is discussed in :ref:`safari-outputs`, and general analysis of these files is discussed in :ref:`safari-analysis`.

.. _cmd_args:

Command Line Arguments
######################

-i file          Sets the :ref:`main_input` to ``file.input``. If this argument is not present, it will look in a file called ``safari.input``, and attempt to read this argument from there
-o file          Sets the name of the various output files, if not present, uses the same as input
-t temperature   Sets the temperature for the run
-n number        Sets the number of runs (trajectories in :ref:`monte_carlo_mode`, Thermal Iteration in :ref:`adaptive_grid_mode`)
-e Energy        Sets the initial energy (E0)
-p file          Enables loading external potentials from ``file.pots``
-s               Forces the run to be in :ref:`single_shot_mode`
-x number        x target for :ref:`single_shot_mode`
-y number        y target for :ref:`single_shot_mode`
-r               restricted :ref:`single_shot_mode`, only will output nearby lattice sites

Command Line examples
---------------------

Here are a few examples of commands to use for running |SAFARI|, in the following examples, the safari executable is ``Sea-Safari``, which is the name of the executable produced via the instructions in :ref:`safari-build`.

The following will use ``./sample.input`` for the :ref:`main_input`, and produce output files in ``./tests/``, with names starting with ``sample``:

.. code:: Bash

    ./Sea-Safari -i sample -o tests/sample

The example below will use the same input and output files as the above, however will do a single-shot run at the origin:

.. code:: Bash

    ./Sea-Safari -i sample -o tests/sample -s -x 0 -y 0

.. _safari_modes:

|SAFARI| Modes
##############

|SAFARI| has multiple modes it can run in, these are as follows:

-   :ref:`monte_carlo_mode`
-   :ref:`fixed_grid`
-   :ref:`adaptive_grid_mode`
-   :ref:`chain_mode`
-   :ref:`single_shot_mode`

These modes, and how to enable them will be discussed below.

.. _monte_carlo_mode:

Monte Carlo
-----------

Monte Carlo selects random sites on the surface, and fires projectiles at it. This mode is enabled by setting ``SCAT_TYPE`` to ``666``, see line |scat_types| of :ref:`input file<commented_sample>`.

In this mode, the run number |numcha_long| is the number of such particles fired, and it is evenly distributed over the specified surface (lines |x_range|, |y_range| of :ref:`input file<commented_sample>`).

.. _fixed_grid:

Fixed Grid
----------

This mode is enabled by setting ``SCAT_TYPE`` to ``777``, see |scat_loc|.

In this mode, the trajectories are made via a raster scan across the surface, where it scans in the step sizes specified on |x_y_range|. In this mode, the run number is ignored.

.. _adaptive_grid_mode:

Adaptive Grid
-------------

This is similar to :ref:`fixed_grid` in the initial pass of site selection, however then produces finer and finer grids around the successful trajectories. The number of such bifurcations is specified via the value set for ``SCAT_TYPE`` on |scat_loc|.

In this mode, the run number specifies the number of iterations to use for a non 0K surface, and is otherwise ignored if the temperature is set to 0.

.. _chain_mode:

Chain Mode
----------

Chain mode fires a line of particles, starting at the initial x, y coordinates, and ending at the final ones, these are specified via |x_y_range|. This mode fires the number of specified particles |numcha_long|, with even spacing.

This mode is enabled by setting |scat_loc| to ``888``.

.. _single_shot_mode:

Single Shot Mode
----------------

A Single Shot run can either be enabled via the commandline argument ``-s``, or by setting the trajctory number to 1. In this mode, |SAFARI| will log information about the entire trajectory. The run is done with a target location specified by |x_y_range|, or via the ``-x`` and ``-y`` arguments.

The default behaviour of this mode depends on the value for ``SCAT_TYPE`` (|scat_loc|), if this is 0, it will produce the entire lattice this can result in extremely large ``.xyz`` files. To only include the "relevant" lattice sites, you can use the ``-r`` argument or set ``SCAT_TYPE`` to a non-zero value.

More information about the specific output files for this run can be located at :ref:`ssm_files`.

.. include:: ../../.shared.rst

.. |numcha_long| replace:: (line |numcha| of :ref:`input file<commented_sample>`)

.. |x_y_range| replace:: lines |x_range| and |y_range| of :ref:`input file<commented_sample>`

.. |scat_loc| replace:: line |scat_types| of :ref:`input file<commented_sample>`