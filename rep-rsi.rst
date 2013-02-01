REP: XXX
Title: Robot System Identification
Author: William Morris <bill@iheartengineering.com>
Status: Draft
Type: Process
Content-Type: text/x-rst
Created: 31-Jan-2013
Post-History: 31-Jan-2013


Abstract
========

Robots with ROS installed at the system level have different requirements from users manually starting and stopping ROS and this REP describes file formats for storing information about these robotic systems. This will provide users and applications with a source of information that is common across robot hardware platforms and software installations.

Rationale
=========

Production robotic systems that ship with ROS pre-installed have different requirements from the needs of developers and researchers installing ROS onto a single robot or computer.

Hardware vendor support staff need a way to determine information about the state of the system to speed troubleshooting. Software developers can make use of a common means of introspecting hardware to determine what functionality is available and improve the out of the box experience for users.

Specification
=============

This REP builds on the concepts [1]_ used for Linux operating system identification [2]_ and attempts to provide a means of describing a robotic system that will be common across hardware and software platforms.

The system identification information will be stored in configuration files for ease of parsing in adverse support conditions. The files, ``/etc/ros-release`` describes the installed state of ROS and ``/etc/robot-release`` will provide information about the hardware.

ros-release Options
-------

NAME=
'''''

This is the unversioned human readable name of the robot operating system, the default is ``NAME=ROS``. Forks of ROS that patch core components or have an independent update and release cycle should specify a unique identifier such as ``NAME=ROS Industrial``.

VERSION=
''''''''

The version string specifies the version of ROS that is installed. Currently this would be ``VERSION=Groovy``. 

ID=
'''

The machine parsable name of the robot operating system. This string must only contain ['a-z', 0-9, '.', '_' and '-'] and should not include version information. The default is ``ID=ros`` and reasonable values might include something like ``ID=ros_industrial``.

VERSION_ID=
'''''''''''

A machine readable string containing ['a-z', 0-9, '.', '_' and '-'] that identifies the version of ROS that is installed. This is equivalent to the command ``rosversion roslaunch`` and currently this would be ``VERSION_ID=1.9.41`` 

PRETTY_NAME=
''''''''''''

The human readable operating system name as determined by the marketing department. This may be something like ``PRETTY_NAME="ROS (Groovy Galapagos)"``, the default is ``PRETTY_NAME="ROS (Robot Operating System)"``

HOME_URL=, SUPPORT_URL=, BUG_REPORT_URL=
''''''''''''''''''''''''''''''''''''''''

These are conceptually the same as [2]_, with the defaults of ``HOME_URL="http://www.ros.org"``, ``SUPPORT_URL="http://www.ros.org/wiki/Support"``, and ``BUG_REPORT_URL="http://www.ros.org/wiki/Tickets"``.

BUILD_SOURCE=
'''''''''''''

The machine parseable source of the robot operating system. Current valid values include, ``BUILD_SOURCE=deb``, ``BUILD_SOURCE=iso``, and ``BUILD_SOURCE=source``. This field along with the other ``BUILD_*`` fields should be specified by the initial install medium and should be updated when the operating system is updated.

BUILD_DATE=
'''''''''''''

The date the operating system was built in ``dd-mmm-yyyy`` format. Today this would be ``BUILD_DATE=31-Jan-2013``. For ISO installs this should be the date the image was assembled and for Debian package repositories this should be the date the repository was synced.

BUILD_ID=
'''''''''

Optional human readable field to identify a specific build. This could be used to denote the source of the build ``BUILD_ID="Built by humans"`` or information to identify the release ``BUILD_ID="RC1"``

/etc/robot-release Options
-------

NAME=
'''''

This is the unversioned human readable name of the robot. A valid example includes ``NAME=TurtleBot``

VERSION=
''''''''

An optional version string, such as ``VERSION=2.0`` to describe the hardware revision of the system.

ID=
'''

A lower-case machine readable version of the robot name. ``ID=turtlebot`` 

VERSION_ID=
'''''''''''

A machine readable string containing ['a-z', 0-9, '.', '_' and '-'] that identifies the hardware revision of the robot. This could be ``VERSION_ID=2.0.0_a`` 

VENDOR=
'''''''

Human readable name of the hardware vendor. Hopefully for your robot this will be ``VENDOR="I Heart Engineering"``

VENDOR_ID=
''''''''''

A machine readable string containing ['a-z', 0-9, '.', '_' and '-'] and identifies the vendor. This may be the same string used identify the vendor's ROS repository in a name such as ``ihe-ros-pkg``, which would yield ``VENDOR_ID="ihe"``.

PRETTY_NAME=
''''''''''''

The full human readable name of the robot. ``PRETTY_NAME="TurtleBot 2 - [Luxury Edition]"``

HOME_URL=, SUPPORT_URL=, BUG_REPORT_URL=
''''''''''''''''''''''''''''''''''''''''

These are conceptually the same as [2]_, the PR2 could provide something like the following, ``HOME_URL="https://www.willowgarage.com/pages/pr2/overview"``, ``SUPPORT_URL="http://pr2support.willowgarage.com/wiki/"``, and ``BUG_REPORT_URL="http://wgsupport.willowgarage.com/access/unauthenticated"``.

TYPE=
'''''

This describes the general class of the robot. Examples include, ``TYPE="Mobile"``, ``TYPE="Manipulator"``, ``TYPE="Unmanned Aircraft System"``, and ``TYPE="Autonomous Underwater Vehicle"``.

TYPE_ID=
''''''''

Machine readable lower-case type information, as an example ``TYPE="Unmanned Aircraft System"`` would become ``TYPE_ID="uas"``.

DRIVE=
''''''

Optional machine parsable examples include ``DRIVE="differential"``, ``DRIVE="ackermann"``, and ``DRIVE="holonomic"``.

DOF=
''''

Optional number of degrees of freedom, a robot arm for example might have ``DOF=6``.

MODEL=
''''''

This is a human readable identifier to denote a specific model of a robot, and can be used to designate different feature sets. A common use of this would be to denote the difference between the research edition ``MODEL="RX"`` and the standard model ``MODEL="SX"``. Custom options should be used to specify each feature, such as ``TURTLEBOT_3D_SENSOR`` explained below.

These configuration files are meant to be forwards compatible and undefined or vendor specific options should be ignored. The TurtleBot for example may use ``TURTLEBOT_3D_SENSOR="kinect"`` and ``TURTLEBOT_3D_SENSOR="xtion"`` to optimize performance based on which sensor the robot ships with.

Future Work
===========

ros-release
-----------
This file should be installed by the variant or metapackage for installation from Debian Packages and be automatically generated when building from source.

References
==========

.. [1] /etc/os-release Announcement
   (http://0pointer.de/blog/projects/os-release)
.. [2] os-release — Operating system identification
   (http://www.freedesktop.org/software/systemd/man/os-release.html)

Copyright
=========

This document has been placed in the public domain.



..
   Local Variables:
   mode: indented-text
   indent-tabs-mode: nil
   sentence-end-double-space: t
   fill-column: 70
   coding: utf-8
   End:
