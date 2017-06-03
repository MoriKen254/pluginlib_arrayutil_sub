cmake_minimum_required(VERSION 2.8.3)
project(pluginlib_arrayutil_sub)

find_package(catkin REQUIRED COMPONENTS
  pluginlib
  roscpp
  pluginlib_arrayutil_base
)

catkin_package(
  INCLUDE_DIRS include
  LIBRARIES pluginlib_arrayutil_sub
  CATKIN_DEPENDS pluginlib roscpp 
  DEPENDS Boost system_lib
)

include_directories(
  include ${catkin_INCLUDE_DIRS} ${Boost_INCLUDE_DIRS}
)

# Declare a C++ library
add_library(${PROJECT_NAME} src/sum.cpp src/ave.cpp src/min.cpp src/max.cpp)
target_link_libraries(${PROJECT_NAME} ${catkin_LIBRARIES})

install(TARGETS ${PROJECT_NAME} #pluginlib_client
  ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION})

## Mark cpp header files for installation
install(DIRECTORY include/${PROJECT_NAME}/
  DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
  FILES_MATCHING PATTERN "*.h"
  PATTERN ".svn" EXCLUDE)

## Mark other files for installation (e.g. launch and bag files, etc.)
install(FILES arrayutil_plugins.xml
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION})

if (CATKIN_ENABLE_TESTING)
  find_package(catkin REQUIRED COMPONENTS
    rostest
    pluginlib
    roscpp
    std_msgs
    pluginlib_arrayutil_base
  )

  add_rostest_gtest(${PROJECT_NAME}_test
    test/pluginlib_arrayutil_sub.test
    test/pluginlib_arrayutil_sub_test.cpp)
  target_link_libraries(${PROJECT_NAME}_test ${catkin_LIBRARIES})
endif()
