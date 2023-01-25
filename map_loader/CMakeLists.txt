cmake_minimum_required(VERSION 3.14)
project(map_loader)

if(NOT CMAKE_CXX_STANDARD)
  set(CMAKE_CXX_STANDARD 17)
  set(CMAKE_CXX_STANDARD_REQUIRED ON)
  set(CMAKE_CXX_EXTENSIONS OFF)
endif()

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic -Werror)
endif()

find_package(ament_cmake_auto REQUIRED)
ament_auto_find_build_dependencies()
find_package(PCL REQUIRED COMPONENTS io)

ament_auto_add_library(pointcloud_map_loader_node SHARED
  src/pointcloud_map_loader/pointcloud_map_loader_node.cpp
  src/pointcloud_map_loader/pointcloud_map_loader_module.cpp
  src/pointcloud_map_loader/partial_map_loader_module.cpp
  src/pointcloud_map_loader/differential_map_loader_module.cpp
  src/pointcloud_map_loader/utils.cpp
)
target_link_libraries(pointcloud_map_loader_node ${PCL_LIBRARIES})

target_include_directories(pointcloud_map_loader_node
  SYSTEM PUBLIC
    ${PCL_INCLUDE_DIRS}
)

rclcpp_components_register_node(pointcloud_map_loader_node
  PLUGIN "PointCloudMapLoaderNode"
  EXECUTABLE pointcloud_map_loader
)

ament_auto_package(INSTALL_TO_SHARE
  launch
  config
)
