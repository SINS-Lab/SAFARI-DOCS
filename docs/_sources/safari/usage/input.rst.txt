.. _safari-sample-input-file:

***************************
|SAFARI| Sample Input Files
***************************

These files are generally whitespace separated. The location and name of these files is specified via the ``-i`` argument, or is specified on the first line of a file ``safari.input`` in the run directory.

.. _main_input:

Main Input File
###############

The fields in the input file are whitespace separated, with the input lines being read in a specific order. Lines beginning with a ``#``, or entirely consisting of whitespace are ignored for the purposes of the indexed order while reading.

Boolean values are specified via ``t`` for ``true``, and anything else for ``false``, usually a ``f`` is used in this case. Floating point values use `atof <http://www.cplusplus.com/reference/cstdlib/atof/>`_, so any input valid for that format is accepted. Integer values use `atoi <http://www.cplusplus.com/reference/cstdlib/atoi/>`_.

.. _commented_sample:

Commented Input File
********************

Here is a sample input file for |SAFARI|, it is an extended one including comments.

.. literalinclude:: sample.input
    :language: PowerShell
    :linenos:

.. _important_lines:

Important Lines
~~~~~~~~~~~~~~~

-   |initial_conditions| - Initial conditions for the beam
-   |dtect_type| - the ``cull flag`` can be used to get specific details as to the projectiles which trigger the various failure conditions, and the ``type`` can enable producing :ref:`spec_file`
-   |dtect_window| - Detector window
-   |nmax| - This controls the maximum number of particles to consider for interaction, smaller numbers greatly reduce runtimes
-   |Z0| - This controls the initial z position of the projectile, adjusting this may be nessisary depending on surface roughness
-   |max_steps| - This controls the maximum number if integration steps, see :ref:`data_file` more details
-   |numcha| - Run Number, ignored in :ref:`fixed_grid` Mode
-   |x_range| - X Range
-   |y_range| - Y Range
-   |scat_types| - Scattering Modes and Options
-   |pot_types| - Potential type parameters
-   |stuck_buried| - Stuck/Buried thresholds

.. raw:: latex

    \clearpage

.. _raw_sample:

Compact Input File
******************

Here is the same input file, however with the comments removed, this is an example of a compact input file for |SAFARI|. This is also reproduced in :ref:`dbug_file`.

.. literalinclude:: sample_compact.input
    :language: PowerShell
    :linenos:

.. _pots_files:

External Potentials File
########################

|SAFARI| calculates potentials and forces by linearly interpolating between values in pre-computed lookup tables. These tables range from ``r_step`` to ``r_max`` (see line |r_range| of :ref:`commented_sample`). They start at ``r_step``, as the potentials are not well defined at 0 separation. The values of ``r_max`` and ``r_step`` should be chosen such that the particles will never approach ``r_step`` as a separation distance (see :ref:`data_file` for how to determine minimum separation distance), and such that a linear interpolation between points separated by ``r_step`` well fits the potential. ``r_max`` should be chosen such that the potential at that ditance is sufficiently close to 0, 10Å is generally a good value for this.

It should be noted that the tables in the external potential file are assumed to have the following ranges:

r = r_step -> r_max, where the first point is the value at r_step (generally undefined at 0 anyway).

The values in the table are in the natural order, however no order is required for the different tables.

Below are some examples of organization in a ``.pots`` file

.. literalinclude:: sample.pots
    :language: PowerShell
    :linenos:

In the above file, the ``...`` represent omitted rows to make this file more compact for example purposes.

Here it demonstrates including multiple sets of potentials, each pairing of atoms is treated as a separate table, so they can either be separated entirely, such as the first block with ``Na Au``, or they can be interlaced, as in the ``Cs Cu`` and ``Cu Cu`` cases.

.. _crys_in_files:

External Crystal File
#####################

Surfaces can be specified via an external ``.crys_in`` file, below the format for that file will be discussed. A :ref:`similar file<crys_files>` is also produced as an output file, which can be a useful way to generate these files. This file will be loaded if the 4th parameter on line |lat_orient| of :ref:`commented_sample` is ``t``. In this case, it will assume that the provided crystal is oriented according to the last three values on that same line, and will then rotate the crystal to the orientation provided by the first three values. This allows loading in an externally provided surface, and then orienting it to an arbitrary direction.

Each line in the ``.crys_in`` file specifes an atom at a location. The atom is specified via atomic number and atomic mass, in the following order:

.. code:: PowerShell

    [X] [Y] [Z] [Atomic Number] [Mass]

Here is an example of such a file:

.. literalinclude:: sample.crys_in
    :language: PowerShell
    :linenos:


.. include:: ../../.shared.rst
