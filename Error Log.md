===================================================================================================================================

- 22/04/2026
# Al construir los paquetes del simulador de Unitree del repositorio de Github ha saltado el siguiente error, pues falta el Joint trajectory controller

leorraptor@UbuntuJJ:~/unitree_G1_sim_ros2$ colcon build --symlink-install
Starting >>> ros2_unitree_legged_msgs
Starting >>> go1_description
Finished <<< go1_description [2.69s]                                         
Starting >>> go1_gazebo
Finished <<< go1_gazebo [5.50s]      
Starting >>> go1_navigation
Finished <<< go1_navigation [2.35s]   
Finished <<< ros2_unitree_legged_msgs [28.1s]                       
Starting >>> ros2_unitree_legged_control
Starting >>> unitree_guide2
--- stderr: unitree_guide2            
CMake Error at CMakeLists.txt:84 (find_package):
  By not providing "Findjoint_trajectory_controller.cmake" in
  CMAKE_MODULE_PATH this project has asked CMake to find a package
  configuration file provided by "joint_trajectory_controller", but CMake did
  not find one.

  Could not find a package configuration file provided by
  "joint_trajectory_controller" with any of the following names:

    joint_trajectory_controllerConfig.cmake
    joint_trajectory_controller-config.cmake

  Add the installation prefix of "joint_trajectory_controller" to
  CMAKE_PREFIX_PATH or set "joint_trajectory_controller_DIR" to a directory
  containing one of the above files.  If "joint_trajectory_controller"
  provides a separate development package or SDK, be sure it has been
  installed.


---
Failed   <<< unitree_guide2 [1.61s, exited with code 1]
Aborted  <<< ros2_unitree_legged_control [1.63s]

Summary: 4 packages finished [30.2s]
  1 package failed: unitree_guide2
  1 package aborted: ros2_unitree_legged_control
  1 package had stderr output: unitree_guide2

===================================================================================================================================
