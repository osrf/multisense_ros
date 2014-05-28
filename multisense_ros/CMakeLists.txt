cmake_minimum_required(VERSION 2.8.3)
project(multisense_ros)

include(FindPkgConfig)
if (PKG_CONFIG_FOUND)
  pkg_check_modules(opencv2 opencv2) # is this patch needed in Hydro
endif (PKG_CONFIG_FOUND)
find_package(OpenCV REQUIRED)
find_package(Boost REQUIRED COMPONENTS thread)

find_package(catkin REQUIRED COMPONENTS roscpp
                                        std_msgs
                                        genmsg
                                        tf
                                        multisense_lib
                                        rosbag
                                        sensor_msgs
                                        image_transport
                                        angles
                                        dynamic_reconfigure
                                        image_geometry
                                        message_generation)


# generate_dynamic_reconfigure_options(cfg/multisense.cfg.in)

###############################################
# custom dynamic reconfigure
catkin_destinations()  # get catkin variable defines for destinations
set(CFG_NAME sl_bm_cmv2000)
set(CFG_SGM FALSE)
set(CFG_IMU FALSE)
configure_file (cfg/multisense.cfg.in ${CATKIN_DEVEL_PREFIX}/${CATKIN_PACKAGE_SHARE_DESTINATION}/cfg/sl_bm_cmv2000.cfg @ONLY)
set(CFG_NAME sl_bm_cmv2000_imu)
set(CFG_SGM FALSE)
set(CFG_IMU TRUE)
configure_file (cfg/multisense.cfg.in ${CATKIN_DEVEL_PREFIX}/${CATKIN_PACKAGE_SHARE_DESTINATION}/cfg/sl_bm_cmv2000_imu.cfg @ONLY)
set(CFG_NAME sl_bm_cmv4000)
set(CFG_SGM FALSE)
set(CFG_IMU FALSE)
configure_file (cfg/multisense.cfg.in ${CATKIN_DEVEL_PREFIX}/${CATKIN_PACKAGE_SHARE_DESTINATION}/cfg/sl_bm_cmv4000.cfg @ONLY)
set(CFG_NAME sl_bm_cmv4000_imu)
set(CFG_SGM FALSE)
set(CFG_IMU TRUE)
configure_file (cfg/multisense.cfg.in ${CATKIN_DEVEL_PREFIX}/${CATKIN_PACKAGE_SHARE_DESTINATION}/cfg/sl_bm_cmv4000_imu.cfg @ONLY)
set(CFG_NAME sl_sgm_cmv2000_imu)
set(CFG_SGM TRUE)
set(CFG_IMU TRUE)
configure_file (cfg/multisense.cfg.in ${CATKIN_DEVEL_PREFIX}/${CATKIN_PACKAGE_SHARE_DESTINATION}/cfg/sl_sgm_cmv2000_imu.cfg @ONLY)
set(CFG_NAME sl_sgm_cmv4000_imu)
set(CFG_SGM TRUE)
set(CFG_IMU TRUE)
configure_file (cfg/multisense.cfg.in ${CATKIN_DEVEL_PREFIX}/${CATKIN_PACKAGE_SHARE_DESTINATION}/cfg/sl_sgm_cmv4000_imu.cfg @ONLY)

generate_dynamic_reconfigure_options(
  ${CATKIN_DEVEL_PREFIX}/${CATKIN_PACKAGE_SHARE_DESTINATION}/cfg/sl_bm_cmv2000.cfg
  ${CATKIN_DEVEL_PREFIX}/${CATKIN_PACKAGE_SHARE_DESTINATION}/cfg/sl_bm_cmv2000_imu.cfg
  ${CATKIN_DEVEL_PREFIX}/${CATKIN_PACKAGE_SHARE_DESTINATION}/cfg/sl_bm_cmv4000.cfg
  ${CATKIN_DEVEL_PREFIX}/${CATKIN_PACKAGE_SHARE_DESTINATION}/cfg/sl_bm_cmv4000_imu.cfg
  ${CATKIN_DEVEL_PREFIX}/${CATKIN_PACKAGE_SHARE_DESTINATION}/cfg/sl_sgm_cmv2000_imu.cfg
  ${CATKIN_DEVEL_PREFIX}/${CATKIN_PACKAGE_SHARE_DESTINATION}/cfg/sl_sgm_cmv4000_imu.cfg
)
###############################################

add_message_files(DIRECTORY msg
                  FILES
                  DeviceInfo.msg
                  RawCamConfig.msg
                  RawImuData.msg
                  RawLidarData.msg
                  RawCamCal.msg
                  RawCamData.msg
                  RawLidarCal.msg)

generate_messages(DEPENDENCIES sensor_msgs)

catkin_package(INCLUDE_DIRS   include
               CATKIN_DEPENDS roscpp
                              std_msgs
                              genmsg
                              tf
                              multisense_lib
                              rosbag
                              sensor_msgs
                              angles
                              dynamic_reconfigure
                              image_geometry
                              image_transport
                              message_runtime
                              message_generation
               LIBRARIES      ${PROJECT_NAME}
               DEPENDS        system_lib)

include_directories(include
                    ${catkin_INCLUDE_DIRS}
                    ${Boost_INCLUDE_DIRS}
                    ${multisense_lib_INCLUDE_DIRS})


add_library(${PROJECT_NAME} src/camera.cpp
                          src/imu.cpp
                          src/laser.cpp
                          src/pps.cpp
                          src/reconfigure.cpp)
add_dependencies(${PROJECT_NAME} ${PROJECT_NAME}_gencfg)
add_dependencies(${PROJECT_NAME} ${PROJECT_NAME}_generate_messages_cpp)
add_dependencies(${PROJECT_NAME} multisense_lib)
target_link_libraries(${PROJECT_NAME} ${catkin_LIBRARIES}
                                      ${OpenCV_LIBS}
                                      ${Boost_LIBRARIES}
                                      ${multisense_lib_LIBRARIES})
target_link_libraries(${PROJECT_NAME} MultiSense)
set_target_properties(${PROJECT_NAME}
  PROPERTIES LINK_FLAGS "-L${PROJECT_SOURCE_DIR}/../multisense_lib/sensor_api/bin/")
set_target_properties(${PROJECT_NAME}
  PROPERTIES COMPILE_FLAGS "-I${PROJECT_SOURCE_DIR}/../multisense_lib/sensor_api/source/LibMultiSense")

add_executable(ros_driver src/ros_driver.cpp)
target_link_libraries(ros_driver ${PROJECT_NAME})
target_link_libraries(ros_driver yaml-cpp)
set_target_properties(ros_driver
  PROPERTIES COMPILE_FLAGS "-I${PROJECT_SOURCE_DIR}/../multisense_lib/sensor_api/source/LibMultiSense")

add_executable(raw_snapshot src/raw_snapshot.cpp)
target_link_libraries(raw_snapshot ${catkin_LIBRARIES}
                                   ${OpenCV_LIBS}
                                   ${Boost_LIBRARIES})
add_dependencies(raw_snapshot ${PROJECT_NAME}_generate_messages_cpp)

## Install
## Mark executables and/or libraries for installation
install(TARGETS ${PROJECT_NAME} ros_driver raw_snapshot
  ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)

## Mark cpp header files for installation
install(DIRECTORY include/${PROJECT_NAME}/
  DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
  FILES_MATCHING PATTERN "*.h"
  PATTERN ".svn" EXCLUDE
)

## Mark other files for installation (e.g. launch and bag files, etc.)
install(FILES
    ${CATKIN_DEVEL_PREFIX}/${CATKIN_PACKAGE_SHARE_DESTINATION}/docs/sl_bm_cmv2000Config-usage.dox
    ${CATKIN_DEVEL_PREFIX}/${CATKIN_PACKAGE_SHARE_DESTINATION}/docs/sl_bm_cmv2000Config.dox
    ${CATKIN_DEVEL_PREFIX}/${CATKIN_PACKAGE_SHARE_DESTINATION}/docs/sl_bm_cmv2000Config.wikidoc
    ${CATKIN_DEVEL_PREFIX}/${CATKIN_PACKAGE_SHARE_DESTINATION}/docs/sl_bm_cmv2000_imuConfig-usage.dox
    ${CATKIN_DEVEL_PREFIX}/${CATKIN_PACKAGE_SHARE_DESTINATION}/docs/sl_bm_cmv2000_imuConfig.dox
    ${CATKIN_DEVEL_PREFIX}/${CATKIN_PACKAGE_SHARE_DESTINATION}/docs/sl_bm_cmv2000_imuConfig.wikidoc
    ${CATKIN_DEVEL_PREFIX}/${CATKIN_PACKAGE_SHARE_DESTINATION}/docs/sl_bm_cmv4000Config-usage.dox
    ${CATKIN_DEVEL_PREFIX}/${CATKIN_PACKAGE_SHARE_DESTINATION}/docs/sl_bm_cmv4000Config.dox
    ${CATKIN_DEVEL_PREFIX}/${CATKIN_PACKAGE_SHARE_DESTINATION}/docs/sl_bm_cmv4000Config.wikidoc
    ${CATKIN_DEVEL_PREFIX}/${CATKIN_PACKAGE_SHARE_DESTINATION}/docs/sl_bm_cmv4000_imuConfig-usage.dox
    ${CATKIN_DEVEL_PREFIX}/${CATKIN_PACKAGE_SHARE_DESTINATION}/docs/sl_bm_cmv4000_imuConfig.dox
    ${CATKIN_DEVEL_PREFIX}/${CATKIN_PACKAGE_SHARE_DESTINATION}/docs/sl_bm_cmv4000_imuConfig.wikidoc
    ${CATKIN_DEVEL_PREFIX}/${CATKIN_PACKAGE_SHARE_DESTINATION}/docs/sl_sgm_cmv2000_imuConfig-usage.dox
    ${CATKIN_DEVEL_PREFIX}/${CATKIN_PACKAGE_SHARE_DESTINATION}/docs/sl_sgm_cmv2000_imuConfig.dox
    ${CATKIN_DEVEL_PREFIX}/${CATKIN_PACKAGE_SHARE_DESTINATION}/docs/sl_sgm_cmv2000_imuConfig.wikidoc
    ${CATKIN_DEVEL_PREFIX}/${CATKIN_PACKAGE_SHARE_DESTINATION}/docs/sl_sgm_cmv4000_imuConfig-usage.dox
    ${CATKIN_DEVEL_PREFIX}/${CATKIN_PACKAGE_SHARE_DESTINATION}/docs/sl_sgm_cmv4000_imuConfig.dox
    ${CATKIN_DEVEL_PREFIX}/${CATKIN_PACKAGE_SHARE_DESTINATION}/docs/sl_sgm_cmv4000_imuConfig.wikidoc
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}/docs
)

install(FILES
    ${CATKIN_DEVEL_PREFIX}/${CATKIN_PACKAGE_SHARE_DESTINATION}/cfg/sl_bm_cmv2000.cfg
    ${CATKIN_DEVEL_PREFIX}/${CATKIN_PACKAGE_SHARE_DESTINATION}/cfg/sl_bm_cmv2000_imu.cfg
    ${CATKIN_DEVEL_PREFIX}/${CATKIN_PACKAGE_SHARE_DESTINATION}/cfg/sl_bm_cmv4000.cfg
    ${CATKIN_DEVEL_PREFIX}/${CATKIN_PACKAGE_SHARE_DESTINATION}/cfg/sl_bm_cmv4000_imu.cfg
    ${CATKIN_DEVEL_PREFIX}/${CATKIN_PACKAGE_SHARE_DESTINATION}/cfg/sl_sgm_cmv2000_imu.cfg
    ${CATKIN_DEVEL_PREFIX}/${CATKIN_PACKAGE_SHARE_DESTINATION}/cfg/sl_sgm_cmv4000_imu.cfg
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}/cfg
)
