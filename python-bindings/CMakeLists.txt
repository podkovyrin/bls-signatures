CMAKE_MINIMUM_REQUIRED(VERSION 3.11 FATAL_ERROR)
set (CMAKE_CXX_STANDARD 11)

# CMake 3.11+
include(FetchContent)

FetchContent_Declare(
  pybind11
  GIT_REPOSITORY https://github.com/pybind/pybind11.git
  GIT_TAG        v2.5.0
)
FetchContent_MakeAvailable(pybind11 chia-relic)


include_directories(
  ${INCLUDE_DIRECTORIES}
  ${chia-relic_SOURCE_DIR}/include
  ${chia-relic_BINARY_DIR}/include
  ${CMAKE_CURRENT_SOURCE_DIR}/../contrib/catch
  ${GMP_INCLUDES}
  )

pybind11_add_module(blspy ${CMAKE_CURRENT_SOURCE_DIR}/pythonbindings.cpp)

if (SODIUM_FOUND)
  target_link_libraries(blspy PRIVATE blstmp relic_s sodium)
else()
  target_link_libraries(blspy PRIVATE blstmp relic_s)
endif()
