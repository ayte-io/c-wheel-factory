cmake_minimum_required(VERSION 3.14)
project(wheel_factory C)

set(CMAKE_C_STANDARD 11)

add_library(wheel_factory SHARED library.c library.h)