cmake_minimum_required(VERSION 3.22)
project(briscola_test)

set(CMAKE_CXX_STANDARD 20)

# Asio library
add_library(asio INTERFACE)
target_include_directories(asio INTERFACE ${CMAKE_CURRENT_SOURCE_DIR}/asio/asio/include)
target_compile_options(asio INTERFACE -DASIO_STANDALONE)

if(UNIX)
    target_compile_options(asio INTERFACE -lpthread)
elseif(WIN32 OR MINGW)
    target_link_libraries(asio INTERFACE ws2_32 wsock32)
endif()

# SFML library
#add_library(SFML PUBLIC)

# spdlog library

# doctest library
add_library(doctest INTERFACE)
target_include_directories(doctest INTERFACE ${CMAKE_CURRENT_SOURCE_DIR}/doctest/doctest/)

# main program
add_executable(briscola-test src/main.cpp)
target_link_libraries(briscola-test PRIVATE asio)
target_link_libraries(briscola-test PRIVATE doctest)

#add_executable(testing src/main.test.cpp)
#target_link_libraries(testing PRIVATE asio)
#target_link_libraries(testing PRIVATE doctest)

add_executable(server src/chat_server.cpp)
target_link_libraries(server PRIVATE asio)

add_executable(client src/chat_client.cpp)
target_link_libraries(client PRIVATE asio)