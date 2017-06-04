## add gtest
hunter_add_package(GTest)
find_package(GTest CONFIG REQUIRED)

# Test project:
ADD_EXECUTABLE(unit_tests test_main.cpp)

# Add the required libraries for linking:
TARGET_LINK_LIBRARIES(unit_tests nanoflann GTest::gtest)

## add test (run it)
enable_testing()
add_test(
  NAME "unit_tests"
  COMMAND "unit_tests"
  )
