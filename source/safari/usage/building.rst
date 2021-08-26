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

Instructions for Setup and use on WSL in Windows 10
###################################################

A convienient way to run SAFARI on Windows is using the Windows Subsystem for Linux (WSL). These instructions are for use with Ubuntu, as it has a relatively simple installation process on Windows 10.

Step 1: Install Linux on Windows 10
***********************************

For using Ubuntu, this is fairly simple, you can locate the Ubuntu App in the Microsoft Store, and install from there. You also need to enable the WSL, instructions for this should be on the Ubuntu App page. This process may require a Windows reboot. You should launch it via the Ubuntu App's store page to do the initial setup.

Once this setup has finished, run the following commands from the Ubuntu App to ensure things are updated:

.. code:: Bash

    sudo apt update
    sudo apt upgrade

Next, run the following commands to install g++ and make

.. code:: Bash

    sudo apt install g++ make

Step 2: Download and Setup for SAFARI
*************************************

Once WSL is enabled, and a Linux distribution is installed, open it to the directory where you want to setup your SAFARI installation. This can be done via either the ``wsl`` command in one of the windows terminals, or via shift right clicking in the required directory, and then clicking ``Open Linux shell here``

In the Linux shell, run the following commands to download the SEA-SAFARI and SAFARI-ANALYSIS repositories.

.. code:: Bash

    git clone https://github.com/SINS-Lab/SEA-SAFARI.git SAFARI
    git clone https://github.com/SINS-Lab/SAFARI-ANALYSIS.git SAFARI-ANALYSIS

Next, try making SAFARI:

.. code:: Bash

    cd SAFARI
    make

Once that successfully finishes, copy the ``Sea-Safari`` executable from ``/SAFARI/bin/Release/``, and the ``XYZ`` executable from ``/SAFARI/utility_scripts/`` to ``/SAFARI-ANALYSIS/``

.. code:: Bash

    cd ..
    cp ./SAFARI/bin/Release/Sea-Safari ./SAFARI-ANALYSIS/Sea-Safari
    cp ./SAFARI/utility_scripts/XYZ ./SAFARI-ANALYSIS/XYZ

Now, try running the test runs for SAFARI.

.. code:: Bash

    cd SAFARI/bin/Release
    ./test_sample.py

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