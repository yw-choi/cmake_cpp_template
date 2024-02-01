# TODO: Add test configuration here

# Add test_sample that run by MPI_PREFIX commands
add_executable(test_sample.x test_sample.cpp ${SOURCES} ${HEADERS})

# if MPI_PREFIX is not set, 
# use mpirun -np 4
if(NOT MPI_PREFIX)
    set(MPI_PREFIX mpirun -np 4)
endif()

add_test(NAME sample_test
    COMMAND ${MPI_PREFIX} ${CMAKE_CURRENT_BINARY_DIR}/test_sample.x)

