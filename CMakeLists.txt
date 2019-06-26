cmake_minimum_required(VERSION 2.8.3)
project(ceres_ros)

find_package(catkin_simple REQUIRED)
catkin_simple()

include(ExternalProject)

# The configure step fails at catkin_package() if this directory
# doesn't exist yet. Create it at configure time!
# file(MAKE_DIRECTORY ${CMAKE_INSTALL_PREFIX}/include)

ExternalProject_Add(ceres_src
  GIT_REPOSITORY https://ceres-solver.googlesource.com/ceres-solver
  GIT_TAG aeebc66bd8ff5db65b852dac0cbac28f618ac5d8 # 1.14.0
  CONFIGURE_COMMAND cmake 
    -DCMAKE_BUILD_TYPE=Release
    -DCMAKE_INSTALL_PREFIX=/usr/local #${CATKIN_DEVEL_PREFIX}
    -DCERES_THREADING_MODEL=OPENMP
    -DEIGENSPARSE=ON
    -DCXSPARSE=OFF
    -DSUITESPARSE=OFF
    -DBUILD_EXAMPLES=OFF
    -DBUILD_TESTING=OFF
    -DBUILD_SHARED_LIBS=ON
    -DCMAKE_CXX_FLAGS=-fPIC
    ../ceres_src
  BUILD_COMMAND $(MAKE) --silent
  INSTALL_COMMAND $(MAKE) install
)

cs_install()
cs_export(INCLUDE_DIRS /usr/local/include
          LIBRARIES ceres) 
