cmake_minimum_required(VERSION 3.10)
project(FTServo_Linux)

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic -std=c++11 -O3)
endif()

# Find dependencies
find_package(ament_cmake REQUIRED)

# Collect source and header files from src/ directory
file(GLOB hdrs src/*.h)
file(GLOB srcs src/*.cpp)

# Build static library
add_library(SCServo STATIC ${hdrs} ${srcs})

# Enable position-independent code for linking into shared libraries
set_target_properties(SCServo PROPERTIES POSITION_INDEPENDENT_CODE ON)

target_include_directories(SCServo PUBLIC
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/src>
  $<INSTALL_INTERFACE:include/FTServo_Linux>
)

# Install library
install(TARGETS SCServo
  EXPORT export_${PROJECT_NAME}
  ARCHIVE DESTINATION lib
  LIBRARY DESTINATION lib
  RUNTIME DESTINATION bin
)

# Install headers
install(FILES
  ${hdrs}
  DESTINATION include/FTServo_Linux
)

# Export targets
ament_export_targets(export_${PROJECT_NAME} HAS_LIBRARY_TARGET)
ament_export_include_directories(include/FTServo_Linux)

ament_package()
