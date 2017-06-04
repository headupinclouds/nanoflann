# examples:
ADD_EXECUTABLE(pointcloud_example pointcloud_example.cpp)
target_link_libraries(pointcloud_example nanoflann)

ADD_EXECUTABLE(pointcloud_kdd_radius pointcloud_kdd_radius.cpp)
target_link_libraries(pointcloud_kdd_radius nanoflann)

ADD_EXECUTABLE(pointcloud_adaptor_example pointcloud_adaptor_example.cpp)
target_link_libraries(pointcloud_adaptor_example nanoflann)

ADD_EXECUTABLE(saveload_example saveload_example.cpp)
target_link_libraries(saveload_example nanoflann)

ADD_EXECUTABLE(vector_of_vectors_example vector_of_vectors_example.cpp KDTreeVectorOfVectorsAdaptor.h)
target_link_libraries(vector_of_vectors_example nanoflann)

# add eigen
IF(EIGEN3_FOUND)
  hunter_add_package(Eigen)
  find_package(Eigen3 REQUIRED)
  
  ADD_EXECUTABLE(matrix_example matrix_example.cpp)
  target_link_libraries(matrix_example nanoflann Eigen3::Eigen)
ENDIF()

