cmake_minimum_required(VERSION 3.10)

# set the project name
project(ShakeTheBox VERSION 1.0)

# specify the C++ standard
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)

# add the executable
add_executable(ShakeTheBox src/CPVGDFTracker.cpp)

# add some necessary options for the compiler e.g. -fpermissive
add_compile_options(-fopenmp -fpermissive -std=c++11 -Wdeprecated)

# add the include directories
target_include_directories(ShakeTheBox
    PUBLIC 
        ${PROJECT_SOURCE_DIR}/inc    
        ${PROJECT_SOURCE_DIR}/inc/DataIO
        ${PROJECT_SOURCE_DIR}/inc/libMath    
        ${PROJECT_SOURCE_DIR}/inc/libSTB
        ${PROJECT_SOURCE_DIR}/inc/libtiff    
)

# message("Here the PROJECT_SOURCE_DIR: ${PROJECT_SOURCE_DIR}")

# add the DataIO library
add_library(DataIO src/srcDataIO/GDF.cpp
		   src/srcDataIO/NumDataIO.cpp)

# add the include directories
target_include_directories(DataIO
    PUBLIC 
        ${PROJECT_SOURCE_DIR}/inc    
        ${PROJECT_SOURCE_DIR}/inc/DataIO
        ${PROJECT_SOURCE_DIR}/inc/libSTB
)

# add the libMath library
add_library(Math src/srcMath/Matrix.cpp)

# add the include directories
target_include_directories(Math
    PUBLIC 
        ${PROJECT_SOURCE_DIR}/inc    
        ${PROJECT_SOURCE_DIR}/inc/libMath
        ${PROJECT_SOURCE_DIR}/inc/libSTB
)

# add the libSTB library
add_library(STB src/srcSTB/BackSTB.cpp
		src/srcSTB/BoundaryCheck.cpp
		src/srcSTB/Calibration.cpp
		src/srcSTB/Camera.cpp
		src/srcSTB/ForwardSTB.cpp
		src/srcSTB/Frame.cpp
		src/srcSTB/Image.cpp
		src/srcSTB/IPR.cpp
		src/srcSTB/OTF.cpp
		src/srcSTB/ParticleFinder.cpp
		src/srcSTB/Position.cpp
		src/srcSTB/PredictiveField.cpp
		src/srcSTB/Shaking.cpp
		src/srcSTB/STB.cpp
		src/srcSTB/Tiff2DFinder.cpp
		src/srcSTB/Tiffload.cpp
		src/srcSTB/Track.cpp
		src/srcSTB/WesleyanCPV.cpp
		)

# add the include directories
target_include_directories(STB
    PUBLIC 
        ${PROJECT_SOURCE_DIR}/inc    
        ${PROJECT_SOURCE_DIR}/inc/libSTB
	${PROJECT_SOURCE_DIR}/inc/libMath
        ${PROJECT_SOURCE_DIR}/inc/DataIO
)

# where to look for libraries
# link_directories(/usr/lib/x86_64-linux-gnu)

# link the libs with the ShakeTheBox main
target_link_libraries(ShakeTheBox LINK_PUBLIC STB Math DataIO tiff gomp)
