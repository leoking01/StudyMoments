cmake_minimum_required(VERSION 2.8)

#project(StudyMoments)
#add_executable(${PROJECT_NAME} "main.cpp")

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -march=native -m64 -Ofast -flto")
#set(CMAKE_CXX_FLAGS "CMAKE_CXX_FLAGS: -march=native -m64 -Ofast -flto" )
#CMAKE_CXX_FLAGS:STRING="-march=native -m64 -Ofast -flto"
#判断编译器类型,如果是gcc编译器,则在编译选项中加入c++11支持
if(CMAKE_COMPILER_IS_GNUCXX)
    set(CMAKE_CXX_FLAGS " ${CMAKE_CXX_FLAGS} -std=c++11")
    message(STATUS "optional:-std=c++11")
endif(CMAKE_COMPILER_IS_GNUCXX)


if (CMAKE_VERSION VERSION_LESS 3.1)
    if (NOT MSVC)
        set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")
    endif()
else()
    set(CMAKE_CXX_STANDARD 11)
endif()

aux_source_directory(. SRC_LIST)
aux_source_directory(./common SRC_LIST)
aux_source_directory(./PreProcessor SRC_LIST)
#aux_source_directory(./skeleton SRC_LIST)
aux_source_directory(./SDFFD SRC_LIST)
aux_source_directory(./utility SRC_LIST)
#aux_source_directory(./demo SRC_LIST)
#aux_source_directory(./findNearestPoint SRC_LIST)

INCLUDE_DIRECTORIES (${PROJECT_SOURCE_DIR})


#set(dep_lib_path  "c:/AwsOneDriver/aStudioRD/WrapedShapeMatchingGenericMLS/depedence")
set(dep_lib_path  "C:/win10")
#C:\win10\opencv24136\build\x64\vc14\lib

if( 0 )
    set(OPencv_base_dir  "${dep_lib_path}/opencv24136")
    set(OpenCV_DIR  "${OPencv_base_dir}/x64/vc14/lib"  )
    #    EXACT 2.4.13.6
    find_package(OpenCV   EXACT 2.4.13.6 REQUIRED  PATHS "${OPencv_base_dir}" )
    LINK_DIRECTORIES( "${OPencv_base_dir}/x64/vc14/lib" )
    INCLUDE_DIRECTORIES("${OPencv_base_dir}/include")
endif()
if( 1 )
    set(OPencv_base_dir  "${dep_lib_path}/opencv24136/build")
    set(OpenCV_DIR  "${OPencv_base_dir}/x64/vc14/lib"  )
    #    EXACT 2.4.13.6
    find_package(OpenCV   EXACT 2.4.13.6 REQUIRED  PATHS "${OPencv_base_dir}" )
    LINK_DIRECTORIES( "${OPencv_base_dir}/x64/vc14/lib" )
    INCLUDE_DIRECTORIES("${OPencv_base_dir}/include")
endif()
if( 0 )
    set(OPencv_base_dir  "c:/win10/opencv24136/opencv/build")
    set(OpenCV_DIR  "${OPencv_base_dir}/x64/vc14/lib"  )
    find_package(OpenCV   EXACT 2.4.13.6 REQUIRED  PATHS "${OPencv_base_dir}" )
    #    EXACT 2.4.13.6
    LINK_DIRECTORIES( "${OPencv_base_dir}/x64/vc14/lib" )
    INCLUDE_DIRECTORIES("${OPencv_base_dir}/include")
endif()
if( 0 )
    set(OPencv_base_dir  "D:/win10/opencv24136Modi/opencv/build")
    set(OpenCV_DIR  "${OPencv_base_dir}/x64/vc14/lib"  )
    find_package(OpenCV EXACT 2.4.13.6 REQUIRED  PATHS "${OPencv_base_dir}" )
    LINK_DIRECTORIES( "${OPencv_base_dir}/x64/vc14/lib" )
    INCLUDE_DIRECTORIES("${OPencv_base_dir}/include")
endif()
message( "\n\n")
message( "__________________________________________________________________")
message( "opencv info(lk):  ")
message( "___________________OpenCV_INCLUDE_DIRS____________________________")
message( ${OpenCV_INCLUDE_DIRS})
message( "___________________ OpenCV_CONFIG_PATH ___________________________")
message( ${OpenCV_CONFIG_PATH})
message( "___________________ OpenCV_SHARED ________________________________")
message( ${OpenCV_SHARED})
message( "___________________ OpenCV_USE_MANGLED_PATHS _____________________")
message( ${OpenCV_USE_MANGLED_PATHS})
#message( "___________________ OpenCV_COMPUTE_CAPABILITIES _________________")
#message( ${OpenCV_COMPUTE_CAPABILITIES})
message( "___________________OpenCV_LIBRARIES_______________________________")
message( ${OpenCV_LIBRARIES})
message( "___________________ OpenCV_LIBS __________________________________")
message( ${OpenCV_LIBS})
message( "___________________ OpenCV_VERSION _______________________________")
message( ${OpenCV_VERSION})
message( "___________________ OpenCV_LIB_COMPONENTS ________________________")
message( ${OpenCV_LIB_COMPONENTS})
message( "___________________var in  OpenCV_LIBRARIES ______________________")
foreach( var ${OpenCV_LIBRARIES} )
    message( ${var} )
endforeach()
message( "\n\n")


set(eigen_dir "${dep_lib_path}/aspDep/eigen-3.3.7\\x64\\share\\eigen3\\cmake")
find_package(  EIGEN3 REQUIRED PATHS  "${dep_lib_path}/aspDep\\eigen-3.3.7/x64" )
INCLUDE_DIRECTORIES (${EIGEN3_INCLUDE_DIRS})
INCLUDE_DIRECTORIES ( "${dep_lib_path}/aspDep\\eigen-3.3.7\\x64\\include\\eigen3")
add_definitions( ${EIGEN3_DEFINITIONS})

#add_executable(${PROJECT_NAME}  ${SRC_LIST} ${HEADERS} ${SOURCES} ${RESOURCES} ${QMLS})
SET(EXECUTABLE_OUTPUT_PATH ${PROJECT_BINARY_DIR}/bin)
#add_executable(${PROJECT_NAME}  ${SRC_LIST} ${HEADERS} ${SOURCES} ${FORMS}  )
#target_link_libraries(${PROJECT_NAME}  Qt5::Core Qt5::Gui Qt5::Qml Qt5::Quick  Qt5::Widgets  ${OpenCV_LIBS})


add_executable(${PROJECT_NAME}  ${SRC_LIST}    )
target_link_libraries(${PROJECT_NAME}   ${OpenCV_LIBS})


#######################################################
#Add some multithreaded build support
MARK_AS_ADVANCED(MULTITHREADED_BUILD)
set(MULTITHREADED_BUILD   8  CACHE STRING "How many threads are used to build the project")
if(MULTITHREADED_BUILD)
    if(${CMAKE_GENERATOR} MATCHES "Unix Makefiles")
        message(STATUS ${CMAKE_BUILD_TOOL})
        set(CMAKE_MAKE_PROGRAM  "${CMAKE_MAKE_PROGRAM} -j${MULTITHREADED_BUILD}")
        message(STATUS "Added arguments to CMAKE_BUILD_TOOL: ${CMAKE_MAKE_PROGRAM}")
    elseif(MSVC)
        set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS}/MP")
        message(STATUS "Added parallel build arguments to CMAKE_CXX_FLAGS: ${CMAKE_CXX_FLAGS}")
    endif()
endif()
#######################################################



