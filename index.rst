###############################
Laser Tracker Alignment System
###############################

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

Computing Target offsets
========================

In our previous approach to computing offsets on M2 and the Camera, we employed a 
method where we would measure the target (we will choose "Camera" for this explanation) points and update the reference `FrameCAM`
by creating the `Frame_CAM_meas`. Notably, `FrameCAM` was centered at the origin of M1M3. 
Due to this configuration, `Frame_CAM_meas` was also centered at the origin of M1M3, leading to 
observed displacements in the frame caused by rotation in the hexapod, since the hexapod point of rotation is 
at the Camera instead of at that origin. These rotational displacements
were artifacts that would not have been apparent if FrameCAM had been centered directly at 
the center of the CAM target. 

Indeed, take the example of 0.15deg rotation about x in the hexapod. This rotation would cause the
FrameCAM at the center of M1M3 to move, 

.. math::
   
   dY = \sin(0.15^\circ) \times \text{{distance between M1M3 and Camera}} \approx 0.15^\circ \times 4.06\,m \approx 10\,mm.

To avoid these artifacts, we need to adopt a new method. 
This method is detailed in one of the initial schmeatic drawings the vendor made, as seen below.
Initially, we measure M1M3 and establish this measurement as the base or working 
frame by setting the tracker at the center of M1M3. All subsequent measurements are 
then made with respect to this M1M3 frame. Then, we proceed to measure M2 and the camera. 
The measurement frame for M2 (`Frame_M2_meas`) is compared to the default `FrameM2`, 
but crucially, this time the frame is centered at the actual location of the M2 target center. 
This adjustment allows us to correct for any real displacements without the interference of 
assumed distances between M1M3 and the target. It ensures that the default distance between 
M1M3 and the target is not erroneously generating displacement artifacts.

.. image:: /Users/gmegias/Desktop/LSST_Developer/technotes/sitcomtn-122/_static/drawing.png
   :align: center


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
  It goes through all the targets and ensures no errors are found when measuring them.


- **BLOCK-197**: Tests the sequential misalignment and realignment of all optical elements,   
  testing and confirming the system's capability to accurately realign after intentional displacement.



System Maintenance
==================

The laser tracker computer should not be powered down using the ``maintel/lasertracker/shut_down.py`` script. 
Restarting the system requires physical presence at the observatory for manual reboot, a procedure designed 
to minimize downtime and ensure continuous operational readiness.

.. note:: For detailed instructions on accessing and operating the Laser Tracker Alignment System, 
   please refer to the "lasetracker" Slack channel, where updates and access credentials are regularly provided.
