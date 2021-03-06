cmake_minimum_required(VERSION 2.6)
PROJECT(CatalystFortran90FullExample CXX Fortran)

find_package(ParaView 4.1 REQUIRED COMPONENTS vtkPVPythonCatalyst)

INCLUDE(${PARAVIEW_USE_FILE})
if(NOT PARAVIEW_USE_MPI)
  message(SEND_ERROR "ParaView must be built with MPI enabled")
endif()
if(NOT MPI_Fortran_LIBRARIES)
  find_package(MPI)
endif()

ADD_EXECUTABLE(Fortran90FullExample FEDriver.f90 FEFortranAdaptor.f90 FECxxAdaptor.cxx)
TARGET_LINK_LIBRARIES(Fortran90FullExample vtkPVPythonCatalyst ${MPI_Fortran_LIBRARIES})

file(COPY SampleScripts/coproc.py DESTINATION ${CMAKE_BINARY_DIR})
option(BUILD_TESTING "Build Testing" OFF)
# Setup testing.
if (BUILD_TESTING)
  include(CTest)
  add_test(Fortran90FullExampleTest Fortran90FullExample)
endif()
