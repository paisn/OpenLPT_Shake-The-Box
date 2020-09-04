cmake_minimum_required(VERSION 3.10)

# use g++
# find_program(CMAKE_CXX_COMPILER NAMES $ENV{CXX} g++ PATHS ENV PATH NO_DEFAULT_PATH)

# set the project name
project(ShakeTheBox VERSION 1.0)

# specify the C++ standard
# CAUTION if you specifiy the C11 standard the binary will be ~30 to 50% slower
# Still do not know the reason
# set(CMAKE_CXX_STANDARD 11)
# set(CMAKE_CXX_STANDARD_REQUIRED True)

# add some necessary options for the compiler e.g. -fpermissive
add_compile_options(-fopenmp -fpermissive -std=c++11 -O3 -c -Wno-deprecated -fmessage-length=0)

# add the executable
add_executable(ShakeTheBox src/CPVGDFTracker.cpp)

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

# The original fork uses the tif sources to link against the binary instead of
# using the installed libtiff. Tried to increase the speed by doing it as well,
# only a little speedup achieved compared to link it with standard installed libtiff
# But: Left it here in order to reduce the dependencies

# add the libTiff library
add_library(Tiff inc/libtiff/tif_aux.c
		 inc/libtiff/tif_close.c
		 inc/libtiff/tif_codec.c
		 inc/libtiff/tif_color.c
		 inc/libtiff/tif_compress.c
		 inc/libtiff/tif_dir.c
		 inc/libtiff/tif_dirinfo.c
		 inc/libtiff/tif_dirread.c
		 inc/libtiff/tif_dirwrite.c
		 inc/libtiff/tif_dumpmode.c
		 inc/libtiff/tif_error.c
		 inc/libtiff/tif_extension.c
		 inc/libtiff/tif_fax3.c
		 inc/libtiff/tif_fax3sm.c
		 inc/libtiff/tif_flush.c
		 inc/libtiff/tif_getimage.c
		 inc/libtiff/tif_jbig.c
		 inc/libtiff/tif_jpeg.c
		 inc/libtiff/tif_jpeg_12.c
		 inc/libtiff/tif_luv.c
		 inc/libtiff/tif_lzma.c
		 inc/libtiff/tif_lzw.c
		 inc/libtiff/tif_next.c
		 inc/libtiff/tif_ojpeg.c
		 inc/libtiff/tif_open.c
		 inc/libtiff/tif_packbits.c
		 inc/libtiff/tif_pixarlog.c
		 inc/libtiff/tif_predict.c
		 inc/libtiff/tif_print.c
		 inc/libtiff/tif_read.c
		 inc/libtiff/tif_stream.cxx
		 inc/libtiff/tif_strip.c
		 inc/libtiff/tif_swab.c
		 inc/libtiff/tif_thunder.c
		 inc/libtiff/tif_tile.c
		 inc/libtiff/tif_unix.c
		 inc/libtiff/tif_version.c
		 inc/libtiff/tif_warning.c
		 inc/libtiff/tif_write.c
		 inc/libtiff/tif_zip.c
)

# add the include directories
target_include_directories(Tiff
    PUBLIC 
        ${PROJECT_SOURCE_DIR}/inc    
        ${PROJECT_SOURCE_DIR}/inc/libtiff
)

# where to look for libraries
# link_directories(/usr/lib/x86_64-linux-gnu)

# link the libs with the ShakeTheBox main
target_link_libraries(ShakeTheBox LINK_PUBLIC STB Math DataIO gomp Tiff)
