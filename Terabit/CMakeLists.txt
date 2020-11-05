cmake_minimum_required (VERSION 2.6)
project (CTSCE)
message (STATUS "CMAKE_INSTALL_PREFIX is ${CMAKE_INSTALL_PREFIX}")

set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_SOURCE_DIR}/cmake/")

find_package ( ACE REQUIRED )

# Make sure ACE is the right version and built with timed message blocks.
file (READ "${ACE_INCLUDE_DIR}/ace/Version.h" _ACE_Version_h_contents)

string (REGEX REPLACE ".*#define ACE_VERSION \"([0-9]+.[0-9]+.[0-9]+)\".*" \\1 ACE_VERSION "${_ACE_Version_h_contents}")

#if ( ${ACE_VERSION} VERSION_GREATER 6.0.0 AND ${ACE_VERSION} VERSION_LESS 6.0.7 )
#	message ("Good, ACE is version ${ACE_VERSION}.")
#else ( ${ACE_VERSION} VERSION_GREATER 6.0.0 AND ${ACE_VERSION} VERSION_LESS 6.0.7 )
#	message (FATAL_ERROR "Please install one of ACE version 6.0.1 - 6.0.6 instead of $#{ACE_VERSION}.")
#endif ( ${ACE_VERSION} VERSION_GREATER 6.0.0 AND ${ACE_VERSION} VERSION_LESS 6.0.7 )

#include (CheckSymbolExists)
#check_symbol_exists(ACE_HAS_TIMED_MESSAGE_BLOCKS "${ACE_INCLUDE_DIR}/ace/config.h" ACE_HAS_TMB)

#if ( ACE_HAS_TMB )
# message( STATUS "ACE library has timed message blocks, good." )
#else ( ACE_HAS_TMB )
#  message( STATUS "ACE library does not have timed message blocks." )
#  message( FATAL_ERROR "Please rebuild ACE with ACE_HAS_TIMED_MESSAGE_BLOCKS defined." )
#endif ( ACE_HAS_TMB )

# set(CMAKE_CXX_FLAGS "-g -ggdb -O3")
set(CMAKE_CXX_FLAGS "-g -ggdb -w -O0 -O3")

set(TProactor_SRC ${CMAKE_SOURCE_DIR}/TProactor/Asynch_Acceptor2.cpp 
${CMAKE_SOURCE_DIR}/TProactor/Asynch_Connector2.cpp 
${CMAKE_SOURCE_DIR}/TProactor/Asynch_File_Transmitter.cpp 
${CMAKE_SOURCE_DIR}/TProactor/Asynch_IO.cpp 
${CMAKE_SOURCE_DIR}/TProactor/Asynch_IO_Impl.cpp 
${CMAKE_SOURCE_DIR}/TProactor/POSIX_AIO_Config.cpp 
${CMAKE_SOURCE_DIR}/TProactor/POSIX_AIO_Dev_Poll_Provider.cpp 
${CMAKE_SOURCE_DIR}/TProactor/POSIX_AIO_Dispatcher.cpp 
${CMAKE_SOURCE_DIR}/TProactor/POSIX_AIO_Emulation.cpp 
${CMAKE_SOURCE_DIR}/TProactor/POSIX_AIO_Emulation_Provider.cpp 
${CMAKE_SOURCE_DIR}/TProactor/POSIX_AIO_Event_Poll_Provider.cpp 
${CMAKE_SOURCE_DIR}/TProactor/POSIX_AIO_FD_Info.cpp 
${CMAKE_SOURCE_DIR}/TProactor/POSIX_AIO_Kqueue_Provider.cpp 
${CMAKE_SOURCE_DIR}/TProactor/POSIX_AIO_LINUX_NAIO_Provider.cpp 
${CMAKE_SOURCE_DIR}/TProactor/POSIX_AIO_Poll_Provider.cpp 
${CMAKE_SOURCE_DIR}/TProactor/POSIX_AIO_Processor.cpp 
${CMAKE_SOURCE_DIR}/TProactor/POSIX_AIO_Provider.cpp 
${CMAKE_SOURCE_DIR}/TProactor/POSIX_AIO_STD_Provider.cpp 
${CMAKE_SOURCE_DIR}/TProactor/POSIX_AIO_SUN_Port_Provider.cpp 
${CMAKE_SOURCE_DIR}/TProactor/POSIX_AIO_SUN_Provider.cpp 
${CMAKE_SOURCE_DIR}/TProactor/POSIX_AIO_Select_Provider.cpp 
${CMAKE_SOURCE_DIR}/TProactor/POSIX_Asynch_IO.cpp 
${CMAKE_SOURCE_DIR}/TProactor/POSIX_Proactor.cpp 
${CMAKE_SOURCE_DIR}/TProactor/Proactor.cpp 
${CMAKE_SOURCE_DIR}/TProactor/Proactor_Impl.cpp 
${CMAKE_SOURCE_DIR}/TProactor/Sock_Addr.cpp 
${CMAKE_SOURCE_DIR}/TProactor/WIN32_Asynch_IO.cpp 
${CMAKE_SOURCE_DIR}/TProactor/WIN32_Proactor.cpp)

include_directories(${CMAKE_SOURCE_DIR})
include_directories(${ACE_INCLUDE_DIR})

LINK_DIRECTORIES(/usr/lib/x86_64-linux-gnu)

add_library(TProactor ${TProactor_SRC})
#target_link_libraries(${ACE_LIBRARY} ${ACE_EXTRA_LIBRARIES} -laio)
target_link_libraries(TProactor ${ACE_LIBRARY} -laio)

