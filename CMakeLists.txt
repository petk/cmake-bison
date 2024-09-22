cmake_minimum_required(VERSION 3.25 FATAL_ERROR)

project(
  Bison
  VERSION 3.8.2
  DESCRIPTION "Parser generator"
  HOMEPAGE_URL "https://www.gnu.org/software/bison/"
)

set(file "${CMAKE_CURRENT_BINARY_DIR}/CMakeFiles/bison.tar.gz")

if(NOT EXISTS ${file})
  message(STATUS "Downloading GNU Bison ${Bison_VERSION}")

  file(
    DOWNLOAD
    "https://ftp.gnu.org/gnu/bison/bison-${Bison_VERSION}.tar.gz"
    "${file}"
    SHOW_PROGRESS
  )
endif()

set(Bison_extract_dir "${CMAKE_CURRENT_BINARY_DIR}/CMakeFiles/bison")
set(Bison_DIR "${Bison_extract_dir}/bison-${Bison_VERSION}")

message(STATUS "Extracting GNU Bison sources")

file(
  ARCHIVE_EXTRACT
  INPUT ${file}
  DESTINATION ${Bison_extract_dir}
)

include(GNUInstallDirs)

configure_file(lib/config.h.in lib/config.h)
configure_file(lib/configmake.h.in lib/configmake.h)

add_library(bison_configuration INTERFACE)
add_library(Bison::configuration ALIAS bison_configuration)
target_include_directories(
  bison_configuration
  INTERFACE
    ${Bison_DIR}
    ${Bison_DIR}/lib
    ${Bison_SOURCE_DIR}/lib
    ${Bison_BINARY_DIR}/lib
)
target_compile_definitions(
  bison_configuration
  INTERFACE
    GNULIB_NAMESPACE=bison
    _FORTIFY_SOURCE=1
    HAVE_CLOSEDIR=1
    HAVE_FSTATAT=1
    HAVE_WORKING_FSTATAT_ZERO_FLAG=1
    HAVE_FCHOWNAT=1
)

add_subdirectory(lib)

add_subdirectory(src)

target_link_libraries(bison PRIVATE y)

install(
  DIRECTORY ${Bison_DIR}/data/
  DESTINATION ${CMAKE_INSTALL_DATADIR}/bison
)
