cmake_minimum_required(VERSION 2.6)

set(CPPVECTORADD_SOURCES
    CPPVectorAdd.cpp )

set(CPPVECTORADD_HEADERS
	CPPVectorAddKernel.cl
	${CLU_SOURCE_DIR}/samples/CPP/CPPVectorAddKernel.cl.h)	

add_custom_command(
	OUTPUT CPPVectorAddKernel.cl.h
	COMMAND clu_generator -cpp CPPVectorAddKernel.cl
	DEPENDS CPPVectorAddKernel.cl
	COMMENT "Generating through CLU ..."
	WORKING_DIRECTORY "${CLU_SOURCE_DIR}/samples/CPP/")

include_directories(
   ${OPENCL_DIST_DIR}/include
   ${CLU_SOURCE_DIR}/clu_runtime)

if( CMAKE_SIZEOF_VOID_P EQUAL 8 )
  link_directories( ${OPENCL_DIST_DIR}/lib/x86_64 )
else( CMAKE_SIZEOF_VOID_P EQUAL 8 )
  link_directories( ${OPENCL_DIST_DIR}/lib/x86 )
endif( CMAKE_SIZEOF_VOID_P EQUAL 8 )

add_executable(CPPVectorAdd ${CPPVECTORADD_SOURCES} ${CPPVECTORADD_HEADERS})
add_dependencies(CPPVectorAdd 
	clu_generator
	clu_runtime)
target_link_libraries( CPPVectorAdd OpenCL.lib clu_runtime ) 