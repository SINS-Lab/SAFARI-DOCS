
.. _safari-outputs:

****************************
|SAFARI| Sample Output Files
****************************

These files are generally tab separated. The location and name of these files is specified via the ``-o`` argument, or is the same as for the input files if not specified. For the purpose of readability, the tab characters in the below files have been replaced with two spaces.

Production of :ref:`spec_file` and :ref:`data_file` can be controlled via using ``<type>`` on line |dtect_type| of the main input file :ref:`main_input`. This option is a bitmask, with the following flags:

-   1 - Enable :ref:`data_file` output
-   2 - Enable :ref:`spec_file` output

These can be combined to produce both files, where ``3`` will do this.

.. _data_file:

The .data file
##############

This is the primary data output file, it will contain the final results of the trajectories, though normally will only contain the detectable particles, this can be adjusted via settings in the :ref:`input files<main_input>`.

Here is a sample ``.data`` file.

.. literalinclude:: sample.data
    :language: PowerShell
    :linenos:

.. _data_columns:

The columns in this file are as follows:

    -   ``X0`` - the initial, target x coordinate for the run
    -   ``Y0`` - the initial, target y coordinate for the run
    -   ``Zm`` - the minimum z-coordinate of the projectile during the run
    -   ``E`` -  the final kinetic energy of the projectile
    -   ``THETA`` - the exit angle theta of the projectile
    -   ``PHI`` - the exit angle phi of the projectile
    -   ``ion index`` - a unique index for this particular projectile
    -   ``weight`` - In adaptive grid mode, this is the grid depth, otherwise is just 1
    -   ``max_n`` - Maximum number of particles considered for interaction for this projectile
    -   ``min_r`` - Minimum distance this projectile got to another particle
    -   ``steps`` - Total number of integration steps required, if these numbers are significantly less than the :ref:`maximum number of steps <important_lines>`, that value should probably be reduced in the :ref:`main_input`
    -   ``Max Error`` - Maximum change in energy encountered by the projectile during any timestep
    -   ``total time`` - Total trajctory time in |SAFARI| time units (|SAFARI Units|)

.. _spec_file:

The .spec file
##############

This file contains binned collections of the final detector positions of the various particles detected. This file starts with a header describing the energy and angle ranges contained in the file. It is then separated into blocks for each energy bin. For each block, there is a table of number of detections per angular bin. Each table has a header row and column with an approximate angle set for those bins, a more precise angle can be obtained from the information in the header.

A sample of the header is below:

::

    --------------------------------------------------------

    SAFARI Spectra File, Ranges in this file are as follows:
    Energy range: 0.50 to 250.00 eV
    Theta range: 0.00 to 90.00 Degrees
    Phi range: -15.00 to 15.00 Degrees

    This file is split into blocks for each energy section
    Each block contains a header row and column, which states
    the theta and phi angles for those blocks respectively

    Total Counts: 7800
    Largest Bin: 5
        (227.50eV, Theta: 73.0 Degrees, Phi: 12.3 Degrees)

    --------------------------------------------------------

This header contains the various ranges involved, as well as the total number of counts represented in the detector, as well as the bin containing the highest number of detections.

The .spec file can be processed more easily for generating spectra than :ref:`data_file`, the size of this file also does not depend strongly on the number of trajectories simulated, so it can be used more easily for calibrating energies, angles, image charges, etc. If :ref:`data_file` output is disabled, a run can be set which generates this file for an arbitrarily long time, until there is a significant number of detections per bin, without consuming infeasibly large amounts of disk space.

.. _dbug_file:
    
The .dbug file
##############

This file will contain general information about the run, and is to be used for assiting with configuration of some of the runtime parameters, such as error thresholds, etc.

The file starts with a trimmed copy of the input file, similar to the one found at :ref:`raw_sample`. It is then followed by some messages about runtime, and then some statistics about the various exit conditions encountered. It also included runtime information.

Here is a sample ``.dbug`` file.

.. literalinclude:: sample.dbug
    :linenos:

.. _dbug_flags:

Important to note in this file, are the error/exit conditions listed near the bottom (Lines 48-56 in the sample). These can be used for adjusting various input parameters for optimizing simulation runtime. The relevant exit conditions for adjusting input parameters are as follows:

-   ``Froze`` - If this number is significant, you might consider increasing the maximum allowed timesteps
-   ``OOB`` - This is how many left the edge of the crystal, if this is significant, you should increase the size of the generated crystal

If the runtimes are unacceptably large, you can consider decreasing the number of allowed timesteps until the ratio which return as ``Froze`` becomes almost significant, likewise if memory use is too large, you can decrease the generated crystal size until the ratio returining ``OOB`` becomes significant.

Exit conditions related to particles not being detectable are as follows:

-   ``Out of Phi`` - This particle was not in plane with the detector and incoming beam, the thresholds on this can be set based on the detector window settings, on line |dtect_window| of :ref:`input file<commented_sample>`
-   ``Trapped`` - The particle had left the surface, but was then re-trapped by the image potential
-   ``Stuck`` - The particle went below the Stuck Energy threshold (See first value on line |stuck_buried| of :ref:`input file<commented_sample>`)
-   ``Buried`` - The particle went more than the Buried Threshold below the surface (Second value on line |stuck_buried| of the same file)

The remaining exit conditions are as follows:

-   ``Errored`` - The particle encountered a large discontinuity in energy
-   ``Intersected`` - The particle intersected with another atom

.. _crys_files:

The .crys file
##############

After generating or loading the lattice, a ``.crys`` file will be generated with the initial, 0K state of the lattice. This file can be re-named to a :ref:`.crys_in<crys_in_files>` file for providing as an external lattice if needed.

This file is of identical format to the :ref:`.crys_in<crys_in_files>` files, but here is a sample anyway:

.. literalinclude:: sample.crys
    :language: PowerShell
    :linenos:

There is also a ``.xyz`` formatted version of the file generated, for use in external visualizers such as VMD\ :cite:`VMD`. This file is suffixed with ``.crys.xyz``, to separate it from the Single Shot mode files referenced :ref:`below<xyz_output_file>`.

The corresponding such file to the above ``.crys`` file is below:

.. literalinclude:: sample.crys.xyz
    :language: PowerShell
    :linenos:

The .sptr file
##############

The inter-lattice force type mentioned around line |pot_types| of :ref:`input files<commented_sample>` is a bitmask field. In the case where 16 (and montecarlo mode) or 32 is present in this bitmask, then |SAFARI| also produces a file containing surface atoms which have sputtered off. A sample such file is as follows:

.. literalinclude:: sample.sptr
    :language: PowerShell
    :linenos:

The columns here are similar to :ref:`.data columns<data_columns>`, with a few differences as noted below:

    -   ``X0`` - the 0 temperature x coordinate for the atom
    -   ``Y0`` - the 0 temperature y coordinate for the atom
    -   ``Zm`` - the 0 temperature z coordinate for the atom
    -   ``E`` -  the final kinetic energy of the atom
    -   ``THETA`` - the exit angle theta of the atom
    -   ``PHI`` - the exit angle phi of the atom
    -   ``ion index`` - a unique index for the projectile which produced this sputter
    -   ``weight`` - The error flag for the projectile (see :ref:`dbug flags<dbug_flags>`)
    -   ``max_n`` - Maximum number of particles considered for interaction for this atom
    -   ``min_r`` - Minimum distance this atom got to another particle
    -   ``steps`` - Total number of integration steps for the ion that caused the sputter
    -   ``Max Error`` - Maximum change in energy encountered by the projectile during any timestep
    -   ``total time`` - Total trajctory time in |SAFARI| time units for the projectile (|SAFARI Units|)

.. _ssm_files:

Single Shot Mode Files
######################

In :ref:`single_shot_mode`, two additional files are generated, the ``.traj`` file, and the ``.xyz`` file.

The .traj file
**************

The primary output file from single shot mod is the ``.traj`` file. This file contains a snapshot of the state of the projectile particle at each timestep of the simulation. This includes position, momentum, energy, timestep, and computed error.

Here is a sample of such a file:

.. literalinclude:: sample.traj
    :language: PowerShell
    :linenos:

The various columns in this file can be used to generate plots, which can then be used to characterize the trajectories, this gives basic information as to the number of significant interactions, how quickly the energy is lost, etc, during the simulation.

The columns in this file are as follows:

-   ``x y z`` - These three columns are the position of the projectile
-   ``px py pz`` - These three are the momentum of the projectile
-   ``t`` - This is the total trajctory time so far (see |SAFARI Units|)
-   ``n`` - The current step, this is generally just an index of rows in the table
-   ``T`` - The kinetic energy of the projectile
-   ``V`` - The total potential that the projectile is currently in
-   ``E`` - T + V, the total energy of the projectile
-   ``near`` - Number of lattice sites being considered for interaction
-   ``dt`` - Current timestep for the projectile
-   ``dr_max`` - maximum positional error encountered so far

Plots of ``T`` vs ``t`` or ``V`` vs ``t`` can be used to gauge a general indication of the number of significant collisions which occur, as well as to get an indication of how well approximations such as the Single Binary Collision Approximation (SBCA) would apply to that particular trajectory type.

.. figure:: ../../_images/traj/traj_qd_energy.png
   :alt: Quasi Double Traj
   :name: traj_qd_energy
   :width: 600
   
   An example of a plot of ``T``, ``V``, and ``E`` vs ``t``, where the time has be converted to fs. This shows two distinct collisions of the projectile.

.. _xyz_output_file:

The .xyz files
**************

This file is produced for the purpose of generating videos of the trajctory via external software such as VMD. A sample of such a file is below:

.. literalinclude:: sample.xyz
    :language: PowerShell
    :linenos:

Here is a sample of a video produced by VMD from an output `.xyz` file from a single shot run.

.. image:: ../../_images/safari/7-body.*

Some additions to this file
~~~~~~~~~~~~~~~~~~~~~~~~~~~

The timestamp for the frame is added as the "comment" line

Additional columns are added to this file from the standard files:

4.  x-component of momentum
5.  y-component of momentum
6.  z-component of momentum
7.  Mass of the particle (useful for isotope considerations)
8.  Index of the particle (used to sort/cleanup xyz afterwards)
9.  Interaction status, this is 0 or 1, depending on if the particle is in interaction range of the projectile


.. include:: ../../.shared.rst
