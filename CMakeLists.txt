cmake_minimum_required(VERSION 3.10)
set(PATCH_VERSION "1" CACHE INTERNAL "Patch version")
set(PROJECT_VERSION 0.0.${PATCH_VERSION})
project(lab7 VERSION ${PROJECT_VERSION})
add_executable(lab7 main.cpp)
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
if (CMAKE_CXX_COMPILER_ID STREQUAL "MSVC")    # Флаг для компилятора MSVC (Windows)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /std:c++17")
    else()
    # Флаг для g++ и clang++ (Linux, MacOS)    
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++17")
endif()
install(TARGETS lab7 DESTINATION bin)
set(CPACK_GENERATOR "ZIP;DEB")
set(CPACK_PACKAGE_VERSION "${PROJECT_VERSION}")
set(CPACK_PACKAGE_NAME "lab7")
set(CPACK_PACKAGE_DESCRIPTION "Описание вашего проекта")
set(CPACK_PACKAGE_VENDOR "Ваше имя или компания")
set(CPACK_PACKAGE_CONTACT "Ваш контактный email")
include(CPack)

# set(BOOST_ROOT "C:/Users/Sopha/Downloads/boost") 
set(BOOST_ROOT "${PROJECT_SOURCE_DIR}/boost") # Относительный путь
set(Boost_INCLUDE_DIRS "${BOOST_ROOT}/boost/spirit/include/")
set(Boost_LIBRARIES "${BOOST_ROOT}/libs") 

# Проверка, существуют ли указанные директории
if(NOT EXISTS "${Boost_INCLUDE_DIRS}")
  message(FATAL_ERROR "Boost include directory not found: ${Boost_INCLUDE_DIRS}")
endif()
if(NOT EXISTS "${Boost_LIBRARIES}")
  message(FATAL_ERROR "Boost libraries directory not found: ${Boost_LIBRARIES}")
endif()

# Находим Boost и проверяем, что он найден
find_package(Boost REQUIRED COMPONENTS filesystem program_options)

# Проверяем, что Boost найден успешно
if(Boost_FOUND)
 # Добавляем необходимые include директории
 target_include_directories(lab7 PRIVATE ${Boost_INCLUDE_DIRS})
 # Линкуем необходимые библиотеки Boost
 target_link_libraries(lab7 PRIVATE ${Boost_LIBRARIES})
else()
 message(FATAL_ERROR "Boost not found! Please install Boost and make sure it's in your system's include and library paths.")
endif()

# find_package(Boost COMPONENTS unit_test_framework REQUIRED)

# configure_file(version.h.in version.h)

# add_executable(lab7_cli main.cpp)
# add_library(lab7 lib.cpp)
# add_executable(test_version test_version.cpp)

# set_target_properties(lab7_cli lab7 test_version PROPERTIES
#     CXX_STANDARD 14
#     CXX_STANDARD_REQUIRED ON
# )
# target_include_directories(lab7
#     PRIVATE "${CMAKE_BINARY_DIR}"
# )
# set_target_properties(test_version PROPERTIES
#     COMPILE_DEFINITIONS BOOST_TEST_DYN_LINK
#     INCLUDE_DIRECTORIES ${Boost_INCLUDE_DIR}
# )

# target_link_libraries(lab7_cli PRIVATE
#     lab7
# )
# target_link_libraries(test_version
#     ${Boost_LIBRARIES}
#     lab7
# )

# enable_testing()

# add_test(test_version test_version)
