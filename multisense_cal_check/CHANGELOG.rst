^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Changelog for package multisense_cal_check
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Forthcoming
-----------
* Add initial support for CRL's Mono IP Camera. Numerous fixes in catkin build infrastructure.
* main changes are:
  - add install to targets for packages.
  - add url for `package.xml`'s.
  - fix dynamic reconfigure specialized install.
  - added `CONTRIBUTING.md` and `README.md`.
  - added dummy urdf and `model.config` for `multisense_description`, so it can be used directly as a gazebo model resource.
  - moved intermediate files generated while compiling `sensor_api` from source directory into catkin devel directory so source directory remains untouched.
* Add support for catkin and rosbuild (Builds under Fuerte, Groovy, Hydro, and Indigo). Transitioned laser calibration from KDL and joint_state_publisher to pure ROS TF messages. Add support for multiple Multisene units via namespacing and tf_prefix's. Modified default topic names to reflect the new namespacing parameters (Default base namespace is now /multisense rather than /multisense_sl). Add support for 3.1_beta sensor firmware which includes support for Multisense-S21 units. Please note that the 3.1 ROS driver release is fully backwards compatible with all 2.X firmware versions.
* Release_2.3: Add support for 2.3 sensor firmware (IMU / CMV4000 support), add 'MultiSenseUpdater' firmware upgrade tool, add smart dynamic_reconfigure presentation, remove multisense_diagnostics/multisense_dashboard, wire protocol to version 3.0 (w/ support for forthcoming SGM core), misc. other bugfixes and feature enhancements.
* Release_2.1: fix a few minor files that were mistakenly changed
* -Add PPS topic: /multisense_sl/pps (std_msgs/Time)
  -Corrected step size of color images, which now display correctly using image_view
  -Add 'network_time_sync' option to dynamic reconfigure
* Updated calibration check utility with new version from JD Taylor.  New version is significantly more strict.
* Imported Release 2.0 of MultiSense-SL ROS driver.
* Contributors: David LaRose <dlr@carnegierobotics.com>, Eric Kratzer <ekratzer@carnegierobotics.com>, John Hsu <hsu@osrfoundation.org>, Matt Alvarado <malvarado@carnegierobotics.com>
