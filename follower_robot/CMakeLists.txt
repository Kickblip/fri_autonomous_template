cmake_minimum_required(VERSION 3.8)
project(follower_robot)

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

# find dependencies
find_package(ament_cmake REQUIRED)
find_package(apriltag_msgs REQUIRED)
find_package(cv_bridge REQUIRED)
find_package(Eigen3 REQUIRED)
find_package(geometry_msgs REQUIRED)
find_package(nav2_msgs REQUIRED)
find_package(rclcpp REQUIRED)
find_package(sensor_msgs REQUIRED)
find_package(spatial_utils REQUIRED)
find_package(tf2_ros REQUIRED)
find_package(OpenCV REQUIRED)

# If apriltag is installed system-wide and provides a pkg-config file, you can do:
# find_package(PkgConfig REQUIRED)
# pkg_check_modules(APRILTAG REQUIRED apriltag)

# Include directories for dependencies
include_directories(
  include
  ${EIGEN3_INCLUDE_DIRS}
  # ${OpenCV_INCLUDE_DIRS}
  # ${cv_bridge_INCLUDE_DIRS}
  # ${APRILTAG_INCLUDE_DIRS}
)

add_executable(follower_robot src/FollowerRobotNode.cpp src/MoveToTarget.cpp src/follower_robot.cpp)

ament_target_dependencies(follower_robot
  apriltag_msgs
  cv_bridge
  geometry_msgs
  nav2_msgs
  OpenCV
  rclcpp
  rclcpp_action
  sensor_msgs
  spatial_utils
  tf2_ros
)

target_link_libraries(follower_robot
  ${OpenCV_LIBS}
  # ${APRILTAG_LIBRARIES}  # Link against the apriltag library
)

install(TARGETS
  follower_robot
  DESTINATION lib/${PROJECT_NAME}
)

# install(
#   DIRECTORY launch
#   DESTINATION share/${PROJECT_NAME}/
# )

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
