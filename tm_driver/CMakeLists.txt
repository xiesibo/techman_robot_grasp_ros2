cmake_minimum_required(VERSION 3.5)
project(tm_driver)

# Default to C99
if(NOT CMAKE_C_STANDARD)
  set(CMAKE_C_STANDARD 99)
endif()

# Default to C++14
if(NOT CMAKE_CXX_STANDARD)
  set(CMAKE_CXX_STANDARD 14)
endif()

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

# find dependencies
find_package(ament_cmake REQUIRED)
find_package(example_interfaces REQUIRED)
find_package(rclcpp REQUIRED)
find_package(rclcpp_action REQUIRED)
find_package(tm_msgs)
find_package(trajectory_msgs REQUIRED)
find_package(sensor_msgs REQUIRED)
find_package(OpenCV REQUIRED)


include_directories(include
  ${trajectory_msgs_INCLUDE_DIRS}
)

add_executable(listen_robot_status src/listen_robot_status.cpp)
ament_target_dependencies(listen_robot_status
  "rclcpp"
  "tm_msgs")
add_executable(send_tm_command src/send_command.cpp)
  ament_target_dependencies(send_tm_command
  "rclcpp"
  "rclcpp_action"
  "trajectory_msgs"
  "tm_msgs"
  "example_interfaces"
  )

  add_executable(tm_get_img src/tm_get_camera.cpp)
  ament_target_dependencies(tm_get_img
    "rclcpp"
    "sensor_msgs"
    "std_msgs"
    "OpenCV")
install(TARGETS
  listen_robot_status
  send_tm_command
  tm_get_img
  DESTINATION lib/${PROJECT_NAME})

if(BUILD_TESTING)
  find_package(ament_lint_auto REQUIRED)
  # the following line skips the linter which checks for copyrights
  # uncomment the line when a copyright and license is not present in all source files
  #set(ament_cmake_copyright_FOUND TRUE)
  # the following line skips cpplint (only works in a git repo)
  # uncomment the line when this package is not in a git repo
  #set(ament_cmake_cpplint_FOUND TRUE)
  ament_lint_auto_find_test_dependencies()
endif()

ament_package()
