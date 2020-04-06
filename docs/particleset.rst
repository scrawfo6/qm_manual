.. _sec:particleset:

Specifying the Particle Set
===========================

The ``particleset`` blocks specify the particles in the QMC simulations: their types,
attributes (mass, charge, valence), and positions.

Input Specification
-------------------

Particleset Elements:
~~~~~~~~~~~~~~~~~~~~~

+----------------------+---------------------+
| Parent elements:     | simulation          |
+----------------------+---------------------+
| Child elements:      | group, attrib       |
+----------------------+---------------------+

Attributes:

+-----------------------------------+--------------+------------------+-----------------+----------------------------------------------------+
| **Name**                          | **Datatype** | **Values**       | **Default**     | **Description**                                    |
+-----------------------------------+--------------+------------------+-----------------+----------------------------------------------------+
| name/ID                           | Text         | *any*            | e               | Name of particle set.                              |
+-----------------------------------+--------------+------------------+-----------------+----------------------------------------------------+
| :math:`size^0`                    | Integer      | *any*            | 0               | Number of particles in set.                        |
+-----------------------------------+--------------+------------------+-----------------+----------------------------------------------------+
| :math:`random^0`                  | Text         | Yes/No           | No              | Randomize starting positions.                      |
+-----------------------------------+--------------+------------------+-----------------+----------------------------------------------------+
| randomsrc/random_source:math:`^0` | Text         | particleset.name | *None*          | Particle set to randomize.                         |
+-----------------------------------+--------------+------------------+-----------------+----------------------------------------------------+


---------------


Group Elements:
~~~~~~~~~~~~~~~

+----------------------+---------------------+
| Parent elements:     | particleset         |
+----------------------+---------------------+
| Child elements:      | parameter, attrib   |
+----------------------+---------------------+

Attributes:

+-----------------------------------+--------------+------------------+-----------------+----------------------------------------------------+
| **Name**                          | **Datatype** | **Values**       | **Default**     | **Description**                                    |
+-----------------------------------+--------------+------------------+-----------------+----------------------------------------------------+
| name                              | Text         | *any*            | e               | Name of particle set.                              |
+-----------------------------------+--------------+------------------+-----------------+----------------------------------------------------+
| :math:`size^0`                    | Integer      | *any*            | 0               | Number of particles in set.                        |
+-----------------------------------+--------------+------------------+-----------------+----------------------------------------------------+
| :math:`mass^0`                    | Real         | *any*            | 1               | Mass of particles in set.                          |
+-----------------------------------+--------------+------------------+-----------------+----------------------------------------------------+
| :math:`unit^0`                    | Text         | au/amu           | au              | Units for mass of particles.                       |
+-----------------------------------+--------------+------------------+-----------------+----------------------------------------------------+

Parameters:

+-----------------------------------+--------------+------------------+-----------------+----------------------------------------------------+
| **Name**                          | **Datatype** | **Values**       | **Default**     | **Description**                                    |
+-----------------------------------+--------------+------------------+-----------------+----------------------------------------------------+
| charge                            | Real         | *any*            | 0               | Charge of particles in set.                        |
+-----------------------------------+--------------+------------------+-----------------+----------------------------------------------------+
| valence                           | Real         | *any*            | 0               | Valence charge of particles in set.                |
+-----------------------------------+--------------+------------------+-----------------+----------------------------------------------------+
| atomicnumber                      | Integer      | *any*            | 0               | Atomic number of particles in set.                 |
+-----------------------------------+--------------+------------------+-----------------+----------------------------------------------------+


----------------


Attrib Elements:
~~~~~~~~~~~~~~~~

+----------------------+---------------------+
| Parent elements:     | particleset, group  |
+----------------------+---------------------+

Attributes:

+-----------------------------------+--------------+------------------------+-----------------+----------------------------------------------------+
| **Name**                          | **Datatype** | **Values**             | **default**     | **description**                                    |
+-----------------------------------+--------------+------------------------+-----------------+----------------------------------------------------+
| name                              | String       | *any*                  | None            | Name of attrib.                                    |
+-----------------------------------+--------------+------------------------+-----------------+----------------------------------------------------+
| datatype                          | String       | IntArray, realArray    | None            | Type of data in attrib.                            |
|                                   |              | posArray, stringArray  |                 |                                                    |
+-----------------------------------+--------------+------------------------+-----------------+----------------------------------------------------+
|:math:`size^0`                     | String       | *any*                  | None            | Size of data in attrib.                            |
+-----------------------------------+--------------+------------------------+-----------------+----------------------------------------------------+



Detailed Attribute Description
------------------------------

Required Particleset Attributes
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

-  | ``name/id``
   | Unique name for the particle set. Default is “e" for electrons. “i"
     or “ion0" is typically used for ions. For special cases where an
     empty particle set is needed, the special name “empty" can be used
     to bypass the zero-size error check.

Optional particleset attributes
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

-  | ``size``
   | Number of particles in set.

-  | ``random``
   | Randomize starting positions of particles. Each component of each
     particle’s position is randomized independently in the range of the
     simulation cell in that component’s direction.

-  | ``randomsrc/random_source``
   | Specify source particle set around which to randomize the initial
     positions of this particle set.

Required name attributes
~~~~~~~~~~~~~~~~~~~~~~~~

-  | ``name/id``
   | Unique name for the particle set group. Typically, element symbols
     are used for ions and “u" or “d" for spin-up and spin-down electron
     groups, respectively.

Optional group attributes
~~~~~~~~~~~~~~~~~~~~~~~~~

-  | ``mass``
   | Mass of particles in set.

-  | ``unit``
   | Units for mass of particles in set (au[:math:`m_e` = 1] or
     amu[:math:`\frac{1}{12}m_{\rm ^{12}C}` = 1]).

Example use cases
-----------------
Particleset elements for ions and electrons randomizing electron start positions.

::

   <particleset name="i" size="2">
       <group name="Li">
         <parameter name="charge">3.000000</parameter>
         <parameter name="valence">3.000000</parameter>
         <parameter name="atomicnumber">3.000000</parameter>
       </group>
       <group name="H">
         <parameter name="charge">1.000000</parameter>
         <parameter name="valence">1.000000</parameter>
         <parameter name="atomicnumber">1.000000</parameter>
       </group>
       <attrib name="position" datatype="posArray" condition="1">
       0.0   0.0   0.0
       0.5   0.5   0.5
       </attrib>
       <attrib name="ionid" datatype="stringArray">
          Li H
       </attrib>
     </particleset>
     <particleset name="e" random="yes" randomsrc="i">
       <group name="u" size="2">
         <parameter name="charge">-1</parameter>
       </group>
       <group name="d" size="2">
         <parameter name="charge">-1</parameter>
       </group>
     </particleset>

Particleset elements for ions and electrons specifying electron start positions.

::

   <particleset name="e">
       <group name="u" size="4">
         <parameter name="charge">-1</parameter>
         <attrib name="position" datatype="posArray">
       2.9151687332e-01 -6.5123272502e-01 -1.2188463918e-01
       5.8423636048e-01  4.2730406357e-01 -4.5964306231e-03
       3.5228575807e-01 -3.5027014639e-01  5.2644808295e-01
          -5.1686250912e-01 -1.6648002292e+00  6.5837023441e-01
         </attrib>
       </group>
       <group name="d" size="4">
         <parameter name="charge">-1</parameter>
         <attrib name="position" datatype="posArray">
       3.1443445436e-01  6.5068682609e-01 -4.0983449009e-02
          -3.8686061749e-01 -9.3744432997e-02 -6.0456005388e-01
       2.4978241724e-02 -3.2862514649e-02 -7.2266047173e-01
          -4.0352404772e-01  1.1927734805e+00  5.5610824921e-01
         </attrib>
       </group>
     </particleset>
     <particleset name="ion0" size="3">
       <group name="O">
         <parameter name="charge">6</parameter>
         <parameter name="valence">4</parameter>
         <parameter name="atomicnumber">8</parameter>
       </group>
       <group name="H">
         <parameter name="charge">1</parameter>
         <parameter name="valence">1</parameter>
         <parameter name="atomicnumber">1</parameter>
       </group>
       <attrib name="position" datatype="posArray">
         0.0000000000e+00  0.0000000000e+00  0.0000000000e+00
         0.0000000000e+00 -1.4308249289e+00  1.1078707576e+00
         0.0000000000e+00  1.4308249289e+00  1.1078707576e+00
       </attrib>
       <attrib name="ionid" datatype="stringArray">
         O H H
       </attrib>
     </particleset>

Particleset elements for ions specifying positions by ion type.

::

   <particleset name="ion0">
       <group name="O" size="1">
         <parameter name="charge">6</parameter>
         <parameter name="valence">4</parameter>
         <parameter name="atomicnumber">8</parameter>
         <attrib name="position" datatype="posArray">
           0.0000000000e+00  0.0000000000e+00  0.0000000000e+00
         </attrib>
       </group>
       <group name="H" size="2">
         <parameter name="charge">1</parameter>
         <parameter name="valence">1</parameter>
         <parameter name="atomicnumber">1</parameter>
         <attrib name="position" datatype="posArray">
           0.0000000000e+00 -1.4308249289e+00  1.1078707576e+00
           0.0000000000e+00  1.4308249289e+00  1.1078707576e+00
         </attrib>
       </group>
     </particleset>
