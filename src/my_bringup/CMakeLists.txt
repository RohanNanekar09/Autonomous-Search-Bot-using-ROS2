cmake_minimum_required(VERSION 3.8)
project(my_bringup)

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

find_package(ament_cmake REQUIRED)
find_package(ament_cmake_python REQUIRED)
find_package(rclcpp REQUIRED)
find_package(sensor_msgs REQUIRED)
find_package(cv_bridge REQUIRED)
find_package(OpenCV REQUIRED)

# ---------- C++ Node ----------
add_executable(yolo_node IP/yolo_detection.cpp)

ament_target_dependencies(
  yolo_node
  rclcpp
  sensor_msgs
  cv_bridge
)

target_link_libraries(yolo_node ${OpenCV_LIBS})

install(TARGETS
  yolo_node
  DESTINATION lib/${PROJECT_NAME}
)

# ---------- Python Package ----------
ament_python_install_package(${PROJECT_NAME})

install(
  PROGRAMS
  my_bringup/yolo_node.py
  DESTINATION lib/${PROJECT_NAME}
)

# ---------- Other Resources ----------
install(
  DIRECTORY launch config worlds
  DESTINATION share/${PROJECT_NAME}
)

ament_package()