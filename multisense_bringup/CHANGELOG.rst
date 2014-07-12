^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Changelog for package multisense_bringup
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Forthcoming
-----------
* Add wire-protocol support for DirectedStreams on CRL's Monocular IP Camera. Misc. other bugfixes.
* Add initial support for CRL's Mono IP Camera. Numerous fixes in catkin build infrastructure.
* main changes are:
  - add install to targets for packages.
  - add url for `package.xml`'s.
  - fix dynamic reconfigure specialized install.
  - added `CONTRIBUTING.md` and `README.md`.
  - added dummy urdf and `model.config` for `multisense_description`, so it can be used directly as a gazebo model resource.
  - moved intermediate files generated while compiling `sensor_api` from source directory into catkin devel directory so source directory remains untouched.
* Add support for catkin and rosbuild (Builds under Fuerte, Groovy, Hydro, and Indigo). Transitioned laser calibration from KDL and joint_state_publisher to pure ROS TF messages. Add support for multiple Multisene units via namespacing and tf_prefix's. Modified default topic names to reflect the new namespacing parameters (Default base namespace is now /multisense rather than /multisense_sl). Add support for 3.1_beta sensor firmware which includes support for Multisense-S21 units. Please note that the 3.1 ROS driver release is fully backwards compatible with all 2.X firmware versions.
* Release_3.0_beta: Add support for 3.0_beta sensor firmware (SGM hardware stereo core: disparity at all resolutions, 2:1 rectangular pixel modes, 64/128/256 disparity modes, hardware bi-lateral post-stereo disparity filter support with tuning), add colorized points2 topic, add pointcloud egde and range filtering, add raw left/right disparitiy image topics, add stereo-cost image topic, misc other feature enhancements and bugfixes.  Please note that the 3.0_beta release is fully backwards compatiblie with all 2.X firmware versions.
* Release_2.3: Add support for 2.3 sensor firmware (IMU / CMV4000 support), add 'MultiSenseUpdater' firmware upgrade tool, add smart dynamic_reconfigure presentation, remove multisense_diagnostics/multisense_dashboard, wire protocol to version 3.0 (w/ support for forthcoming SGM core), misc. other bugfixes and feature enhancements.
* Imported Release 2.0 of MultiSense-SL ROS driver.
* Contributors: David LaRose <dlr@carnegierobotics.com>, Eric Kratzer <ekratzer@carnegierobotics.com>, John Hsu <hsu@osrfoundation.org>, Matt Alvarado <malvarado@carnegierobotics.com>
