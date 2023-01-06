cmake_minimum_required(VERSION 3.15...3.23)

project(CmakeConfigPackageTests LANGUAGES CXX)

# ---- Test as standalone project the exported config package ----

if(PROJECT_IS_TOP_LEVEL OR TEST_INSTALLED_VERSION)
  enable_testing()

  find_package(test_cookiecutter_cpp_cmake_conan CONFIG REQUIRED) # for intro, project_options, ...

  if(NOT TARGET test_cookiecutter_cpp_cmake_conan::project_options)
    message(FATAL_ERROR "Required config package not found!")
    return() # be strictly paranoid for Template Janitor github action! CK
  endif()
endif()

# ---- Dependencies ----
find_package(GTest CONFIG REQUIRED)

add_executable(tests tests.cpp)
target_link_libraries(tests PRIVATE test_cookiecutter_cpp_cmake_conan::project_warnings test_cookiecutter_cpp_cmake_conan::project_options GTest::GTest)

include(GoogleTest)
gtest_discover_tests(tests
  # WORKING_DIRECTORY Testing
)
