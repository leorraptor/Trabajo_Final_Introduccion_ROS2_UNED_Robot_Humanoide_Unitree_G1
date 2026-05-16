# Trabajo_Final_Introduccion_ROS2_UNED_Robot_Humanoide_Unitree_G1

Proyecto final para el curso de Introducción a la robótica con ROS 2 de la UNED en el que me planteo programar una simulación de un robot humanoide bípedo utilizando los recursos de los siguientes repositorios:
https://github.com/Atharva-05/unitree_ros2_sim

_El repositorio contiene tanto los paquetes originales del simulador de Unitree como las instrucciones para instalarlos y que el programa final compile, de momento no he podido hacer ningún cambio significativo al codigo original ni programar ninguno de los tests que me había propuesto, pero he incluido un par de archivos que en un principio deberían permitir el funcionamiento correcto de la simulación._

## Comenzando

Este proyecto sólo va a funcionar correctamente en Ubuntu 22.04.5 LTS Jammy Jellyfish. No me responsabilizo de ningún quebradero de cabeza que resulte de errores o fallos al usar otro sistema operativo.

### Pre-requisitos 

- [lcm] (https://lcm-proj.github.io/lcm/)
- [navigation2](https://github.com/ros-navigation/navigation2)
- [ros2_control](https://github.com/ros-controls/ros2_control)
- [ros2_controllers](https://github.com/ros-controls/ros2_controllers)
- [gazebo_plugins](https://github.com/ros-simulation/gazebo_ros_pkgs/tree/ros2/gazebo_plugins)

### Instalación 

Lo primero de todo, asegurarse de que tenemos [ROS2 Humble Hawksbill](https://docs.ros.org/en/humble/Installation/Ubuntu-Install-Debs.html) instalado.

Para ello empezamos por asegurarnos de que nuestro _locale_ soporta UTF-8

```
$ locale

$ sudo apt update && sudo apt install locales
$ sudo locale-gen en_US en_US.UTF-8
$ sudo update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8
$ export LANG=en_US.UTF-8

$ locale
```
Si lo hemos hecho bien al final la terminal nos devolverá UTF-8

Añadimos el repositorio de ROS2 al sistema:
```
$ sudo apt install software-properties-common
$ sudo add-apt-repository universe

$ sudo apt update && sudo apt install curl -y
$ export ROS_APT_SOURCE_VERSION=$(curl -s https://api.github.com/repos/ros-infrastructure/ros-apt-source/releases/latest | grep -F "tag_name" | awk -F'"' '{print $4}')
$ curl -L -o /tmp/ros2-apt-source.deb "https://github.com/ros-infrastructure/ros-apt-source/releases/download/${ROS_APT_SOURCE_VERSION}/ros2-apt-source_${ROS_APT_SOURCE_VERSION}.$(. /etc/os-release && echo ${UBUNTU_CODENAME:-${VERSION_CODENAME}})_all.deb"
$ sudo dpkg -i /tmp/ros2-apt-source.deb
```
Instalamos los paquetes de ROS2:
```
$ sudo apt update

$ sudo apt upgrade

$ sudo apt install ros-humble-desktop

$ sudo apt install ros-humble-ros-base

$ sudo apt install ros-dev-tools
```
Creamos el entorno de trabajo:
```
$ source /opt/ros/humble/setup.bash
```

_Comprobación de la instalación de ROS2_
```
$ source /opt/ros/humble/setup.bash
$ ros2 run demo_nodes_cpp talker
```
_En otra terminal ejecutamos:_
```
$ source /opt/ros/humble/setup.bash
$ ros2 run demo_nodes_py listener
```

También vamos a necesitar [colcon](https://docs.ros.org/en/humble/Tutorials/Beginner-Client-Libraries/Colcon-Tutorial.html), que esencialmente es el compilador de ROS2:
```
$ sudo apt install python3-colcon-common-extensions
```

_Comprobación de la instalación de ROS2_
_Creamos un espacio de trabajo con ejemplos:_
```
$ mkdir -p ~/ros2_ws/src
$ cd ~/ros2_ws

$ git clone https://github.com/ros2/examples src/examples -b humble

$ colcon build --symlink-install

$ colcon test

$ ros2 run examples_rclcpp_minimal_subscriber subscriber_member_function
```
_En otra terminal:_
```
$ ros2 run examples_rclcpp_minimal_publisher publisher_member_function

```

Luego necesitaremos [Gazebo Fortress](https://classic.gazebosim.org/tutorials?tut=ros2_installing), que es la versión inherentemente compatible con ROS2 Humble. 
```
$ sudo apt install ros-humble-gazebo-ros-pkgs

$ sudo apt install git

$ mkdir -p ~/ws/src

$ cd ~/ws
$ wget https://raw.githubusercontent.com/ros-simulation/gazebo_ros_pkgs/ros2/gazebo_ros_pkgs.repos

$ cd ~/ws
$ vcs import src < gazebo_ros_pkgs.repos

$ vcs custom --args checkout humble

$ cd ~/ws
$ rosdep install --from-paths src --ignore-src -r -y

$ cd ~/ws
$ colcon build --symlink-install

$ source ~/ws/install/setup.bash
```
_Comprobación de la instalación de Gazebo_
```
$ source /opt/ros/humble/setup.bash

$ sudo apt install ros-humble-ros-core ros-humble-geometry2

$ . ~/ws/install/setup.bash

$ gazebo --verbose /opt/ros/humble/share/gazebo_plugins/worlds/gazebo_ros_diff_drive_demo.world
```
_En una segunda terminal:_
```
$ gedit /opt/ros/humble/share/gazebo_plugins/worlds/gazebo_ros_diff_drive_demo.world

$ ros2 topic pub /demo/cmd_demo geometry_msgs/Twist '{linear: {x: 1.0}}' -1
```

Luego hace falta instalar todos los paquetes necesarios:

Empezamos por [lcm] (https://lcm-proj.github.io/lcm/)
```
$ sudo apt install liblcm-dev
```
Luego [navigation2](https://github.com/ros-navigation/navigation2)
```
$ sudo apt-get install ros-humble-navigation2
```
[Ros2_control](https://github.com/ros-controls/ros2_control)
```
$ sudo apt-get install ros-humble-ros2-control
```
Y [ros2_controllers](https://github.com/ros-controls/ros2_controllers)
```
$ sudo apt-get install ros-humble-ros2-controllers
```
Y finalmente [gazebo_plugins](https://github.com/ros-simulation/gazebo_ros_pkgs/tree/ros2/gazebo_plugins)
```
$ sudo apt-get install ros-humble-gazebo-plugins
```

Por último solo hace falta el [puente Gazebo-ROS2](https://gazebosim.org/docs/fortress/ros2_integration/):
```
$ sudo apt-get install ignition-fortress

$ sudo apt-get ros-humble-ros-gz

$ sudo apt install ros-humble-gz-ros2-control

$ sudo apt-get install ros-humble-ros-ign-bridge
```

Finalmente descomprimimos os clonamos el proyecto en la carpeta /src de un espacio de trabajo de nuestra elección.

## Pruebas 

Teniendo los paquetes del proyecto en un área de trabajo de ROS2, compilamos con colcon:
```
$ source /opt/ros/humble/setup.bash
$ cd ~/[TU ÁREA DE TRABAJO]
$ colcon build --symlink-install
```
Una vez compilados deberíamos poder ejecutar en una terminal:
```
$ cros2 launch go1_gazebo spawn_go1.launch.py
```
Con esto cargamos la simulación e inicializamos los controladores

**Una vez inicializados *TODOS* los controladores** en otra terminal ejecutamos:
```
$ ros2 run unitree_guide2 junior_ctrl
```
Esto activa la interfaz, permitiéndonos controlarlo con las teclas:
* **2** Pone al robot en modo *De pié* (fixed_stand)
* **5** El robot acepta los comandos de velocidad (move_base)

## Interfaz

Esto es una transcripción traducida de la sección de *Interface* del [OriginalREADME.md](OriginalREADME.md)

### Topics de subscripción

**Interfaz de control de velocidad:**
 La simulación del robot se mueve mediante comandos de velocidad recibidos del topic de la interfaz.
- Topic: `/cmd_vel`
- Message type: [geometry_msgs/Twist](https://docs.ros.org/en/ros2_packages/humble/api/geometry_msgs/interfaces/msg/Twist.html)

### Topics de Publicación

**Datos de odometría:**
 Los datos de odometría se interpretan de los datos recibidos del plugin *ground truth*
- Topic: `/odom`
- Message type: [nav_msgs/Odometry](https://docs.ros.org/en/humble/p/nav_msgs/interfaces/msg/Odometry.html)

**Datos del Lidar 2D:**
 Los datos del Lidar 2D se interpretan del plugin de *gazebo sensor*
- Topic: `/scan`
- Message type: [sensor_msgs/LaserScan](https://docs.ros.org/en/ros2_packages/humble/api/sensor_msgs/interfaces/msg/LaserScan.html)

**Transformaciones:**
 Ofrece transformaciones de  `odom` -> `base_link` y de `base_link` -> `base_footprint`
 - Topic: `/tf`
 - Message type: [tf2_msgs/TFMessage](https://docs.ros2.org/foxy/api/tf2_msgs/msg/TFMessage.html)

## Despliegue

El proyecto incluye el archivo [bridge.yaml] (/go1_sim/go1_description/config/bridge.yaml) que puentea todos los topics usados en el programa de ROS2 a ignition y viceversa.

## Construido con:

* [ROS2 Humble Hawksbill](https://docs.ros.org/en/humble/Installation/Ubuntu-Install-Debs.html)
* [Gazebo Fortress](https://classic.gazebosim.org/tutorials?tut=ros2_installing)

## Contribuyendo

Debido a la naturaleza del pryecto cualquier deseo de contribuír debería pasar por el creador [original](https://github.com/Atharva-05/unitree_ros2_sim) del código, aunque dejo este código abierto en caso de que alguien lo encuentre útil o necesario para llevar a cabo el suyo.

## Autores

_Menciona a todos aquellos que ayudaron a levantar el proyecto desde sus inicios_

* **Francisco Jose Mañas Álvarez** - *Tutor del trabajo* - [???](???)
* **Leonardo Romeo Carreras** - *Investigación, programación, documentación, traducción y publicación* - [leorraptor](https://github.com/leorraptor)
* **Andrés Villanueva** - *Plantilla del README* - [villanuevand](https://github.com/villanuevand)
* **Atharva Ghotavadekar** - *Código Inicial* - [Atharva-05](https://github.com/Atharva-05)

En [contribuyentes](https://github.com/your/project/contributors) encontrarás una lista de todos los que han participado en este proyecto. 

## Licencia

Este proyecto está bajo una Licencia BSD 3-Clause
 
Para más detalles: [LICENSE.md](LICENSE.md)

## Expresiones de Gratitud

_Gracias a mi profesor por el curso de iniciación a la robótica, y por ayudarme en todo lo posible a sacar este trabajo adelante. Y gracias a las comunidades de Reddit, Github y Stack Overflow por haberse topado con problemas similares a los míos y haber encontrado soluciones que me han servido para avanzar. Y, finalmente, gracias a mi familia y amigos por el apoyo incondicional a pesar de lo insoportable que he estado en los últimos 4 meses._
