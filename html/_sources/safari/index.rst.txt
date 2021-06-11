********
|SAFARI|
********

Preamble
########

|SAFARI|\ :cite:`old_safari` :cite:`new_safari` is an ion-surface scattering simulation, which is optimized for low and hyperthermal energies. |SAFARI| uses highly local molecular dynamics, where the particles of interest only interact with a relatively low number of others. This allows for greatly reducing the computational complexity of the simulation, thereby reducing runtimes.

|SAFARI| is a classical simulation. This is due to the momentum of the projectiles involved being sufficiently high that classical trajectories well describe their motion. This does limit the lower energy bounds of the simulation, but those limits are either in the thermal range, or in situations such as grazing incidences, which mostly apply for neutral projectiles, ie, not ions.

|SAFARI| Units
##############

|SAFARI| uses the following system of units. These units are chosen out of convenience, or general standards in literature. For single ions, the ``eV`` is a convenient measure of energy per ion, and the ``AMU`` is a convenient mass. For the lattice sizes under consideration, and the average atomic distances, the ``Å`` is a relevant length scale. Degrees are used for simplicity of reading angles on experimental setups. The remaining units are not directly measured.

.. table:: Units used in |SAFARI|, both internally and in the input files
    :name: SAFARI_units

    ============ ========================================
    **Quantity** **Units**
    ============ ========================================
    Energy       eV
    Distance     Å
    Mass         AMU
    Angles       Degrees
    Time         :math:`{Å}  \sqrt{\textrm{AMU / eV}}`
    Momentum     :math:`\sqrt{\textrm{eV AMU}}`
    Velocity     :math:`\sqrt{\textrm{eV / AMU}}`
    Force        eV / Å
    ============ ========================================

.. include:: ../.shared.rst