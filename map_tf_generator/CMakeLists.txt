cmake_minimum_required(VERSION 3.14)
project(map_tf_generator)

if(NOT CMAKE_CXX_STANDARD)
  set(CMAKE_CXX_STANDARD 17)
  set(CMAKE_CXX_STANDARD_REQUIRED ON)
  set(CMAKE_CXX_EXTENSIONS OFF)
endif()

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic -Werror)
endif()

find_package(PCL REQUIRED)
find_package(ament_cmake_auto REQUIRED)
ament_auto_find_build_dependencies()
include_directories(
  include
  SYSTEM
  ${PCL_INCLUDE_DIRS}
)

ament_auto_add_library(pcd_map_tf_generator_node SHARED
  src/pcd_map_tf_generator_node.cpp
)
target_link_libraries(pcd_map_tf_generator_node ${PCL_LIBRARIES})

rclcpp_components_register_node(pcd_map_tf_generator_node
  PLUGIN "PcdMapTFGeneratorNode"
  EXECUTABLE pcd_map_tf_generator
)


ament_auto_package(INSTALL_TO_SHARE
  launch
)
