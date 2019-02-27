cmake_minimum_required(VERSION 3.13)
project(testodbcpp11)

set(CMAKE_CXX_STANDARD 11)

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11 -DDATABASE_MYSQL")

add_executable(testodbcpp11 driver.cxx employee-odb.cxx)

target_link_libraries(testodbcpp11 libodb-mysql.so)

target_link_libraries(testodbcpp11 libodb.so)