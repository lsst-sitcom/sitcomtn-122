#################################################
Laser Tracker Alignment System at Vera C. Rubin Observatory
#################################################

.. abstract::

   This technical note provides an overview of the Laser Tracker Alignment System. 
   It details the system's operational framework, software interfaces, and guidelines to operate it.

Introduction
============

At the Vera C. Rubin Observatory, the Laser Tracker plays a crucial role 
in the precision alignment of the observatory's optical elements. 
Positioned at the center of the M1M3 cell, it utilizes a laser to accurately 
measure the positions of Spherically Mounted Retroreflectors (SMRs) distributed 
around the Camera, M2, and M1M3.

Operational Overview
====================

The operation of the Laser Tracker is facilitated through a Commandable SAL Component (CSC), 
which interfaces with the custom T2SA software installed on the laser tracker computer. 
The T2SA software on its turn interfaces with Spatial Analyzer, the proprietary software developed 
by the laser tracker's manufacturer. Remote desktop access to the laser tracker computer is 
available for operational control and monitoring, with access details provided within the 
"lasetracker" Slack channel. This setup ensures a seamless operational 
flow and allows for efficient management and troubleshooting of the system.

Regular Operations
==================

The Laser Tracker system is routinely operated at the beginning of each night's observations 
to align all optical elements within the observatory. This operation is supported by a series 
of specialized scripts designed to facilitate the alignment process:

- ``maintel/lasertracker/measure.py``: This script measures a target 
and calculates the offset of that target (either M2 or Camera) with respect to M1M3.
- ``maintel/lasertracker/align.py``: With inputs for a target and a tolerance level, 
this script measures the chosen target and iteratively realigns the telescope using the 
measured offsets until the specified tolerance is achieved.

Special Procedures
==================

In addition to routine alignments, specific procedures are in place to ensure 
comprehensive system readiness and operational integrity:

- **BLOCK-246**: Tests that all targets are measurable by the laser tracker.
- **BLOCK-197**: Tests the sequential misalignment and realignment of all optical elements, 
testing and confirming the system's capability to accurately realign after intentional displacement.


System Maintenance
==================

The laser tracker computer should not be powered down using the ``maintel/lasertracker/shut_down.py`` script. 
Restarting the system requires physical presence at the observatory for manual reboot, a procedure designed 
to minimize downtime and ensure continuous operational readiness.

.. note:: For detailed instructions on accessing and operating the Laser Tracker Alignment System, 
   please refer to the "lasetracker" Slack channel, where updates and access credentials are regularly provided.
