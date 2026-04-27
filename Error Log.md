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
  
- 23/04/2026
# SOLUCIÓN: Incluimos los paquetes de controladores y navegación dentro de la carpeta src
	navigation2-main (https://github.com/ros-navigation/navigation2)
	ros2_controllers-master (https://github.com/ros-controls/ros2_controllers)
	ros2_control-master (https://github.com/ros-controls/ros2_control)
	
===================================================================================================================================

- 22/04/2026
# Al intentar subir los archivos del proyecto a Github con 'git push -u origin main' me daba un error de autenticación como que no había introducido el password o el usuario correcto

- 22/04/2026
# SOLUCIÓN: Utilizar un token de acceso custom, en vez del "fine grained" según se explica en esta discusión; 
	https://github.com/orgs/community/discussions/71915
	
===================================================================================================================================

- 23/04/2026
# Al utilizar colcon para construir los paquetes que faltaban a la vez que los del paquete principal nos topamos de nuevo con otro problema con respecto a los directorios en CMakeLists.txt de joint_limits probablemente el problema no hubiera surgido de haber añadido las dependencias de una en una

[0.713s] WARNING:colcon.colcon_core.package_selection:Some selected packages are already built in one or more underlay workspaces:
	'hardware_interface' is in: /opt/ros/humble
	'controller_interface' is in: /opt/ros/humble
	'nav2_map_server' is in: /opt/ros/humble
	'controller_manager' is in: /opt/ros/humble
	'controller_manager_msgs' is in: /opt/ros/humble
If a package in a merged underlay workspace is overridden and it installs headers, then all packages in the overlay must sort their include directories by workspace order. Failure to do so may result in build failures or undefined behavior at run time.
If the overridden package is used by another package in any underlay, then the overriding package in the overlay must be API and ABI compatible or undefined behavior at run time may occur.

If you understand the risks and want to override a package anyways, add the following to the command line:
	--allow-overriding controller_interface controller_manager controller_manager_msgs hardware_interface nav2_map_server

This may be promoted to an error in a future release of colcon-override-check.
Starting >>> nav2_common
Starting >>> joint_limits
--- stderr: joint_limits
CMake Error: The source "/home/leorraptor/unitree_G1_sim_ros2/src/ros2_control-master/joint_limits/CMakeLists.txt" does not match the source "/home/leorraptor/unitree_G1_sim_ros2/ros2_control-master/joint_limits/CMakeLists.txt" used to generate cache.  Re-run cmake with a different source directory.
---
Failed   <<< joint_limits [0.05s, exited with code 1]
Aborted  <<< nav2_common [0.05s]

Summary: 0 packages finished [0.73s]
  1 package failed: joint_limits
  1 package aborted: nav2_common
  2 packages had stderr output: joint_limits nav2_common
  90 packages not processed
  
- 24/04/2026
# Borrar los archivos añadidos al usar 'colcon build --symlink-install' con el objetivo de asegurarnos de que todos los archivos se encuentran el el lugar correcto nos encontramos con este nuevo problema

[1.275s] WARNING:colcon.colcon_core.package_selection:Some selected packages are already built in one or more underlay workspaces:
	'controller_manager' is in: /opt/ros/humble
	'nav2_map_server' is in: /opt/ros/humble
	'controller_manager_msgs' is in: /opt/ros/humble
	'hardware_interface' is in: /opt/ros/humble
	'controller_interface' is in: /opt/ros/humble
If a package in a merged underlay workspace is overridden and it installs headers, then all packages in the overlay must sort their include directories by workspace order. Failure to do so may result in build failures or undefined behavior at run time.
If the overridden package is used by another package in any underlay, then the overriding package in the overlay must be API and ABI compatible or undefined behavior at run time may occur.

If you understand the risks and want to override a package anyways, add the following to the command line:
	--allow-overriding controller_interface controller_manager controller_manager_msgs hardware_interface nav2_map_server

This may be promoted to an error in a future release of colcon-override-check.
Starting >>> nav2_common
Starting >>> joint_limits
--- stderr: joint_limits                                         
CMake Error at CMakeLists.txt:4 (find_package):
  By not providing "Findros2_control_cmake.cmake" in CMAKE_MODULE_PATH this
  project has asked CMake to find a package configuration file provided by
  "ros2_control_cmake", but CMake did not find one.

  Could not find a package configuration file provided by
  "ros2_control_cmake" with any of the following names:

    ros2_control_cmakeConfig.cmake
    ros2_control_cmake-config.cmake

  Add the installation prefix of "ros2_control_cmake" to CMAKE_PREFIX_PATH or
  set "ros2_control_cmake_DIR" to a directory containing one of the above
  files.  If "ros2_control_cmake" provides a separate development package or
  SDK, be sure it has been installed.


---
Failed   <<< joint_limits [0.45s, exited with code 1]
Aborted  <<< nav2_common [0.56s]                            

Summary: 0 packages finished [1.59s]
  1 package failed: joint_limits
  1 package aborted: nav2_common
  1 package had stderr output: joint_limits
  90 packages not processed
  
# El paquete "ros2_control" existe en la ruta '~/unitree_G1_sim_ros2/src/ros2_control-master/ros2_control' pero no estoy muy seguro de por qué le falta la configuración ni como añadir una configuración nueva sin comprometer el paquete.

# Lo cierto es que al abrir el archivo 'CMakeLists.txt' sólo encontramos esto

cmake_minimum_required(VERSION 3.16)
project(ros2_control)

find_package(ament_cmake REQUIRED)
ament_package()

# Creo que es suficiente, pero quizás este es el origen del problema

- 24/04/2026
# Intentar construir el paquete de antemano no ha dado resultado

- 24/04/2026
# SOLUCIÓN: Estaba intentando instalar paquetes de la rama master en vez de la rama humble, me he dado cuenta gracias a este post 'https://github.com/ros2/rclpy/issues/972'

............................................________........................
....................................,.-‘”...................``~.,..................
.............................,.-”...................................“-.,............
.........................,/...............................................”:,........
.....................,?......................................................\,.....
.................../...........................................................,}....
................./......................................................,:`^`..}....
.............../...................................................,:”........./.....
..............?.....__.........................................:`.........../.....
............./__.(.....“~-,_..............................,:`........../........
.........../(_....”~,_........“~,_....................,:`........_/...........
..........{.._$;_......”=,_.......“-,_.......,.-~-,},.~”;/....}...........
...........((.....*~_.......”=-._......“;,,./`..../”............../............
...,,,___.\`~,......“~.,....................`.....}............../.............
............(....`=-,,.......`........................(......;_,,-”...............
............/.`~,......`-...............................\....../\...................
.............\`~.*-,.....................................|,./.....\,__...........
,,_..........}.>-._\...................................|..............`=~-,....
.....`=~-,_\_......`\,.................................\........................
...................`=~-,,.\,...............................\.......................
................................`:,,...........................`\..............__..
.....................................`=-,...................,%`>--==``.......
........................................_\..........._,-%.......`\...............
...................................,<`.._|_,-&``................`\.............. 

===================================================================================================================================

- 24/04/2026
# Nuevo problema, sigue sin compilar (┛ಠДಠ)┛彡┻━┻

[0.614s] WARNING:colcon.colcon_core.package_selection:Some selected packages are already built in one or more underlay workspaces:
	'controller_manager' is in: /opt/ros/humble
	'controller_interface' is in: /opt/ros/humble
	'controller_manager_msgs' is in: /opt/ros/humble
	'nav2_map_server' is in: /opt/ros/humble
	'hardware_interface' is in: /opt/ros/humble
If a package in a merged underlay workspace is overridden and it installs headers, then all packages in the overlay must sort their include directories by workspace order. Failure to do so may result in build failures or undefined behavior at run time.
If the overridden package is used by another package in any underlay, then the overriding package in the overlay must be API and ABI compatible or undefined behavior at run time may occur.

If you understand the risks and want to override a package anyways, add the following to the command line:
	--allow-overriding controller_interface controller_manager controller_manager_msgs hardware_interface nav2_map_server

This may be promoted to an error in a future release of colcon-override-check.
Starting >>> nav2_common
Starting >>> ros2_control_test_assets
Finished <<< ros2_control_test_assets [0.76s]                    
Starting >>> hardware_interface
Finished <<< nav2_common [1.14s]                                   
Starting >>> nav2_msgs
[Processing: hardware_interface, nav2_msgs]                                   
Finished <<< hardware_interface [44.9s]                                        
Starting >>> controller_manager_msgs
[Processing: controller_manager_msgs, nav2_msgs]                               
Finished <<< controller_manager_msgs [51.3s]                                   
Starting >>> controller_interface
[Processing: controller_interface, nav2_msgs]                                  
Finished <<< controller_interface [38.9s]                                      
Starting >>> hardware_interface_testing                        
Finished <<< hardware_interface_testing [25.0s]                                
Starting >>> nav2_voxel_grid
Finished <<< nav2_voxel_grid [23.3s]                                           
Starting >>> controller_manager
Finished <<< nav2_msgs [3min 15s]                                             
Starting >>> nav2_util
--- stderr: nav2_util                       
CMake Error at test/CMakeLists.txt:7 (find_package):
  By not providing "Findtest_msgs.cmake" in CMAKE_MODULE_PATH this project
  has asked CMake to find a package configuration file provided by
  "test_msgs", but CMake did not find one.

  Could not find a package configuration file provided by "test_msgs" with
  any of the following names:

    test_msgsConfig.cmake
    test_msgs-config.cmake

  Add the installation prefix of "test_msgs" to CMAKE_PREFIX_PATH or set
  "test_msgs_DIR" to a directory containing one of the above files.  If
  "test_msgs" provides a separate development package or SDK, be sure it has
  been installed.


---
Failed   <<< nav2_util [2.04s, exited with code 1]
Aborted  <<< controller_manager [2min 24s]                                    

Summary: 8 packages finished [5min 28s]
  1 package failed: nav2_util
  1 package aborted: controller_manager
  2 packages had stderr output: controller_manager nav2_util
  71 packages not processed
  
# Ok, ahora sí que no sé como continuar y ya no me da la cabeza para otra cosa que no sea expresar mi frustración mediante kaomojis y arte ascii

- 25/04/2026
# El paquete que falta es este 'https://docs.ros.org/en/humble/p/test_msgs/' y voy a proceder a descargarlo de 'https://github.com/ros2/rcl_interfaces/tree/humble/test_msgs' y añadirlo a '~/unitree_G1_sim_ros2/src/navigation2-humble/nav2_util'

# Me sigue dando exáctamente el mismo error en la misma linea del mismo archivo (┛ಠДಠ)┛彡┻━┻

# Añadir todos los paquetes de interfaces de rcl a '~/unitree_G1_sim_ros2/src' nos da este totalmente nuevo y para nada idéntico mensaje de rror, la diferencia siendo que he compilado como 20 paquetes más TTmTT

[0.608s] WARNING:colcon.colcon_core.package_selection:Some selected packages are already built in one or more underlay workspaces:
	'statistics_msgs' is in: /opt/ros/humble
	'lifecycle_msgs' is in: /opt/ros/humble
	'action_msgs' is in: /opt/ros/humble
	'rosgraph_msgs' is in: /opt/ros/humble
	'controller_manager_msgs' is in: /opt/ros/humble
	'composition_interfaces' is in: /opt/ros/humble
	'controller_interface' is in: /opt/ros/humble
	'hardware_interface' is in: /opt/ros/humble
	'nav2_map_server' is in: /opt/ros/humble
	'builtin_interfaces' is in: /opt/ros/humble
	'rcl_interfaces' is in: /opt/ros/humble
	'controller_manager' is in: /opt/ros/humble
If a package in a merged underlay workspace is overridden and it installs headers, then all packages in the overlay must sort their include directories by workspace order. Failure to do so may result in build failures or undefined behavior at run time.
If the overridden package is used by another package in any underlay, then the overriding package in the overlay must be API and ABI compatible or undefined behavior at run time may occur.

If you understand the risks and want to override a package anyways, add the following to the command line:
	--allow-overriding action_msgs builtin_interfaces composition_interfaces controller_interface controller_manager controller_manager_msgs hardware_interface lifecycle_msgs nav2_map_server rcl_interfaces rosgraph_msgs statistics_msgs

This may be promoted to an error in a future release of colcon-override-check.
Starting >>> builtin_interfaces
Starting >>> lifecycle_msgs
Finished <<< builtin_interfaces [13.6s]                                        
Starting >>> rcl_interfaces
Finished <<< lifecycle_msgs [39.5s]                                        
Starting >>> nav2_common
Finished <<< nav2_common [0.82s]                                          
Starting >>> action_msgs
Finished <<< action_msgs [24.8s]                                              
Starting >>> nav2_msgs
Finished <<< nav2_msgs [1.17s]                                                
Starting >>> test_msgs
--- stderr: test_msgs                                                         
CMake Error at CMakeLists.txt:21 (find_package):
  By not providing "Findtest_interface_files.cmake" in CMAKE_MODULE_PATH this
  project has asked CMake to find a package configuration file provided by
  "test_interface_files", but CMake did not find one.

  Could not find a package configuration file provided by
  "test_interface_files" with any of the following names:

    test_interface_filesConfig.cmake
    test_interface_files-config.cmake

  Add the installation prefix of "test_interface_files" to CMAKE_PREFIX_PATH
  or set "test_interface_files_DIR" to a directory containing one of the
  above files.  If "test_interface_files" provides a separate development
  package or SDK, be sure it has been installed.


---
Failed   <<< test_msgs [0.82s, exited with code 1]
Aborted  <<< rcl_interfaces [55.8s]                                      

Summary: 5 packages finished [1min 10s]
  1 package failed: test_msgs
  1 package aborted: rcl_interfaces
  1 package had stderr output: test_msgs
  83 packages not processed
  
# Hipótesis: Este problema viene por que no estoy usando la rama correcta del código para mi rama de ros.

# No hay más que una rama para el código y el propio readme dice que se ha testado en ros humble y Ubuntu 22.04

# Se supone que la versión que estoy usando de ros y de Ubuntu es la correcta también según lo que pone aquí 'https://www.reddit.com/r/UnitreeG1/comments/1qjzpto/which_ros_does_unitree_g1_supports/' por qué se me está resistiendo tanto!??!?!???!?!

# Me rindo por ahora, volveré cuando reamueble mi cabeza un poco o se me aparezca algún angel de dios con una explicación detallada de como solucionar esto
	
===================================================================================================================================

