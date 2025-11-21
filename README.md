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
Membuat worspace
bash:
$ mkdir kelompok4b_uts/src/
$ cd kelompok4b_uts/src
$ source /opt/ros/humble/setup.bash
$ ros2 pkg create --build-type ament_cmake --node-name nav_kel4b nav_kel4b –dependencies rclcpp  nav2_msgs rclcpp_action tf2 
$ cd ..
$ code .
$ colcon build
$ ls 
(keluarannya akan berupa (build install log src”)
$ gedit ~/.bashrc
$ gedit ~/.bashrc
Tambahkan ke .bashrc:

bash
echo "source ~/kelompok4b_uts/install/setup.bash" >> ~/.bashrc


Pada CMakeLists.txt edit, lalu tambahkan di atas if(BUILD_TESTING):

bash:
“install(
  DIRECTORY launch config maps
  DESTINATION share/${PROJECT_NAME}
)”

Selanjutnya disini kita akan menambahkan pkg atau biasa disebut folder yaitu pkg config, launch maps di dalam nav_kel4b:
config isinya localization.yaml, nav2_params.yaml
launch isinya localization.launch.py, navigation.launch.py, run_nav.launch.py
maps isinya kel4b.pgm dan kel4b.yaml
pada src nav_kel4b tambahkan nav_kel4b.cpp

# Menjalankan Lokalisasi, Navigasi, dan Node pick & place
Karena Anda menjalankan node ROS 2 secara remote pada robot (dengan IP 192.168.185.3), setiap terminal baru harus menjalankan langkah-langkah koneksi dan setup yang sama.
1. Koneksi SSH ke robot:
bash:
$ ssh ubuntu@192.168.185.3

2. Pindah ke Workspace:
bash:
$ cd ~/kelompok4b_uts

3. Source Workspace:
bash:
source install/setup.bash

Setelah persiapan ini, lanjutkan ke langkah eksekusi di bawah.

Tab pertama: Lokalisasi(Memuat Peta) 
Tujuan: Mengaktifkan node Lokalisasi (AMCL) untuk menentukan posisi awal robot di dalam peta. Lokalisasi harus menjadi node pertama yang berjalan.
bash:
# ros2 launch nav_kel4b localization.launch.py map:=src/nav_kel4b/maps/kel4b.yaml
(Meluncurkan AMCL (melalui nav2_bringup/localization_launch.py) dan memuat peta kel4b.yaml serta parameter lokalisasi dari localization.yaml).

Tab Kedua: Navigasi Penuh (Perencanaan & Kontrol)
Tujuan: Mengaktifkan semua node perencanaan jalur, kontroler, dan manajemen perilaku yang membentuk Nav2 Stack.
bash:
ros2 launch nav_kel4b run_nav.launch.py
(Meluncurkan seluruh Nav2 Stack. File ini memastikan topic LiDAR (/scan) di-remap dengan benar ke costmap Nav2 dan memuat parameter perencanaan dari nav2_params.yaml).

Note: Node ini harus aktif sebelum node Goal Sender dijalankan, karena ia menyediakan Action Server (/navigate_to_pose) yang akan dihubungi oleh Goal Sender.

Tab Ketiga: Node Pengantar Barang
Tujuan: Menjalankan node kustom Anda yang mengirimkan tujuan navigasi secara berurutan dan otomatis.
bash:
ros2 run nav_kel4b navkel4b
(Menjalankan node C++ Pose_nav yang berfungsi sebagai Action Client).

🚨 Penanganan Error "Rejected"
Jika Anda mengalami pesan "rejected" saat menjalankan ros2 run nav_kel4b navkel4b di awal, itu sering kali berarti Nav2 Stack (Tab Kedua) belum sepenuhnya up dan running (yaitu, Action Server belum siap).

Solusi:

Matikan node dengan Ctrl + C.


Tunggu beberapa detik hingga pesan dari Tab Kedua menunjukkan bahwa Lifecycle Manager telah berhasil mengaktifkan semua node Nav2 (misalnya, semua node telah beralih ke status active).

Jalankan kembali perintah ros2 run nav_kel4b navkel4b.

Dengan menjalankan ketiga tab ini, Anda telah mengaktifkan seluruh pipeline otonom Anda: Lokalisasi memberikan posisi, Navigasi merencanakan dan mengontrol, dan Node Kustom Anda secara otomatis memicu tugas pengiriman barang.

Berikut link Demonstrasi: https://youtu.be/UsAYJ0VoVkE 
