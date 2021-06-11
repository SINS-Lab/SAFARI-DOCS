.. _safari-build:

*****************
Building |SAFARI|
*****************

The source code for |SAFARI| is available on Github at |SAFARI Github|_

You can build SAFARI from the following steps:

.. code:: Bash

    git clone https://github.com/SINS-Lab/SEA-SAFARI.git SAFARI
    cd SAFARI
    make

This will then generate the executable in ``./bin/Release/``

Executables generated:

    -   Sea-Safari - This is the main |SAFARI| executable
    -   XYZ - used to convert the ``.xyz`` files for the single shot trajectories to have a constant timestep, as during the simulation, the timestep is dynamic, which isn't conducive to producing nice videos.

If there are any issues with building, you might need to modify the ``CXXFLAGS`` in ``makefile`` depending on the specific hardware.

|SAFARI| is written using features of C++11, so at least that or higher is required, but otherwise only relies on the standard Libraries in C++11 and OpenMP, so no other dependancies are required to build.

Multithreading Considerations
#############################

The maximum number of OpenMP threads for |SAFARI| is defined in |safari_h_threads|_, via :const:`THREADCOUNT`, This should be adjusted according to the environment it is used in.
XYZ has a corresponding :const:`THREADCOUNT` in |xyz_threads|_, which is used when smoothing the outputs for use in VMD\ :cite:`VMD`.


.. |SAFARI Github| replace:: |SAFARI| Github
.. _SAFARI Github: https://github.com/SINS-Lab/SEA-SAFARI

.. |safari_h_threads| replace:: safari.h
.. _safari_h_threads: https://github.com/SINS-Lab/SEA-SAFARI/blob/342e157b98dab71ffc16bd00e38337cc62cd8170/src/safari.h#L7

.. |xyz_threads| replace:: xyz_process.cpp
.. _xyz_threads: https://github.com/SINS-Lab/SEA-SAFARI/blob/master/src/xyz_process.cpp#L7

.. include:: ../../.shared.rst