# Lokalisasi, Pemetaan, dan Navigasi Otonom Menggunakan ROS 2
Midterm_Exam-Localization_Mapping Turtlebot4

#Installation & Setup
Instalasi ROS2 Humble di Ubuntu 22.04

bash
$ sudo apt update && sudo apt install locales
$ sudo locale-gen en_US en_US.UTF-8
$ sudo update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8
$ export LANG=en_US.UTF-8

$ sudo apt install software-properties-common
$ sudo add-apt-repository universe
$ sudo apt update
$ sudo apt install curl -y
$ sudo curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key -o /usr/share/keyrings/ros-archive-keyring.gpg

$ echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] http://packages.ros.org/ros2/ubuntu $(. /etc/os-release && echo $UBUNTU_CODENAME) main" | sudo tee /etc/apt/sources.list.d/ros2.list

$ sudo apt update && sudo apt upgrade -y
$ sudo apt install ros-humble-desktop
$ sudo apt install ros-dev-tools

aktifkan ROS2

bash
echo "source /opt/ros/humble/setup.bash" >> ~/.bashrc
source ~/.bashrc

# Membuat Workspace & Menambahkan Package

