cmake_minimum_required(VERSION 3.8)
project(go2_controller)

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

# find dependencies
find_package(ament_cmake REQUIRED)
find_package(controller_interface REQUIRED)
find_package(hardware_interface REQUIRED)
find_package(pluginlib REQUIRED)
find_package(rclcpp REQUIRED)
find_package(rclcpp_lifecycle REQUIRED)
find_package(go2_interfaces REQUIRED)
find_package(std_msgs REQUIRED)


include_directories(
  ${CMAKE_CURRENT_SOURCE_DIR}/include
)

add_library( ${PROJECT_NAME}
  SHARED
    src/go2_controller.cpp
)

target_include_directories(${PROJECT_NAME} 
  PUBLIC
    include
    controller_interface
    hardware_interface
    pluginlib
    rclcpp
    rclcpp_lifecycle
    go2_interfaces
    std_msgs
)
 
ament_target_dependencies(${PROJECT_NAME}
  controller_interface
  pluginlib
  rclcpp
  rclcpp_lifecycle
  hardware_interface
  go2_interfaces
  std_msgs
)

  pluginlib_export_plugin_description_file(controller_interface go2_controller.xml)

  install( TARGETS ${PROJECT_NAME} DESTINATION
   lib
  )

  install (DIRECTORY include
  DESTINATION include/go2_controller
   )

   ament_export_include_directories(
    include
   )

   ament_export_libraries (
    go2_controller
   )
   
   ament_export_dependencies(
    controller_interface
    pluginlib
    rclcpp
    rclcpp_lifecycle
    go2_interfaces
   )

if(BUILD_TESTING)
  find_package(ament_lint_auto REQUIRED)
  # the following line skips the linter which checks for copyrights
  # comment the line when a copyright and license is added to all source files
  set(ament_cmake_copyright_FOUND TRUE)
  # the following line skips cpplint (only works in a git repo)
  # comment the line when this package is in a git repo and when
  # a copyright and license is added to all source files
  set(ament_cmake_cpplint_FOUND TRUE)
  ament_lint_auto_find_test_dependencies()
endif()

ament_package()
