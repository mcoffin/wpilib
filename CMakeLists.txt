cmake_minimum_required(VERSION 2.8)

project(WPILib)

set(CMAKE_CXX_FLAGS "-g -Og -std=c++11 -Wall")

set(HEADER_DIR include/WPILib)
set(LIB_NAME wpilib)
set(LIB_DIR lib)
set(VERSION "2013.4")

configure_file(WPILibConfig.cmake.in ${PROJECT_BINARY_DIR}/WPILibConfig.cmake @ONLY)
configure_file(WPILibConfigVersion.cmake.in ${PROJECT_BINARY_DIR}/WPILibConfigVersion.cmake @ONLY)

file(GLOB_RECURSE WPI_SOURCES *.cpp)
set(WPI_INCLUDES ${CMAKE_SOURCE_DIR})
set(WPI_LIBS "")
add_library(${LIB_NAME} ${WPI_SOURCES})
include_directories(${WPI_INCLUDES})
target_link_libraries(${LIB_NAME} ${WPI_LIBS})
install(TARGETS ${LIB_NAME} 
	LIBRARY DESTINATION ${LIB_DIR} 
	ARCHIVE DESTINATION ${LIB_DIR} 
	RUNTIME DESTINATION bin
)
install(DIRECTORY ${CMAKE_SOURCE_DIR}/ 
	DESTINATION ${HEADER_DIR}
	FILES_MATCHING
		PATTERN *.h
		PATTERN ".git" EXCLUDE
		PATTERN ".settings" EXCLUDE
		#prevent infinite recursion if DESTDIR is subdir of SRCDIR
		PATTERN "${HEADER_DIR}" EXCLUDE
		PATTERN share/WPILib/src EXCLUDE
)
install(DIRECTORY ${CMAKE_SOURCE_DIR}/
	DESTINATION share/WPILib/src
	FILES_MATCHING
		PATTERN *.h
		PATTERN *.cpp
		PATTERN ".git" EXCLUDE
		PATTERN ".settings" EXCLUDE
		PATTERN "${HEADER_DIR}" EXCLUDE
		PATTERN share/WPILib/src EXCLUDE
)
install(FILES
	"${PROJECT_BINARY_DIR}/WPILibConfig.cmake"
	"${PROJECT_BINARY_DIR}/WPILibConfigVersion.cmake"
	DESTINATION "share/WPILib/cmake"
)

# CPack Package Generation
set(CPACK_PACKAGE_NAME wpilib)
set(CPACK_PACKAGE_DESCRIPTION_SUMMARY "WPILib for the FIRST Robotics Competition
 WPILib, developed by Worcester Polytechnic Institute for FIRST, is a
 library designed to simplify interfacing with hardware for the FIRST
 Robotics Competition (FRC).  It is part of a cross development
 environment to a powerpc-wrs-vxworks system."
)
set(CPACK_SET_DESTDIR "ON")
set(CPACK_INSTALL_PREFIX "${CMAKE_INSTALL_PREFIX}")
set(CPACK_PACKAGE_VERSION ${VERSION})
set(CPACK_PACKAGE_VENDOR "FRC")
set(CPACK_PACKAGE_MAINTAINER "Matt Coffin")
set(CPACK_PACKAGE_CONTACT "mcoffin@irys.com")
set(CPACK_PACKAGE_INSTALL_DIRECTORY "/usr/powerpc-wrs-vxworks")
include(CPack)
