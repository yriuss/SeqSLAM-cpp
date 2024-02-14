# CMakeLists files in this project can
# refer to the root source directory of the project as ${HELLO_SOURCE_DIR} and
# to the root binary directory of the project as ${HELLO_BINARY_DIR}.
cmake_minimum_required (VERSION 3.14.0)
project (BOW_opencv C CXX)
find_package (GeographicLib REQUIRED)
# enable_language(CUDA)

find_package(Matplot++ REQUIRED)


include(CheckLanguage)
set(CMAKE_CXX_STANDARD 17)

find_package(OpenCV REQUIRED)
   if(NOT OpenCV_FOUND)
      message(FATAL_ERROR "OpenCV not found.")
   endif()

set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${PROJECT_SOURCE_DIR}/lib)
include_directories(
${PROJECT_SOURCE_DIR}
${GeographicLib_INCLUDE_DIRS}
${PROJECT_SOURCE_DIR}/include
)


#set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${PROJECT_SOURCE_DIR}/lib)
add_library(${PROJECT_NAME} SHARED
src/Timer.cc
src/BOWExtractor.cc
src/GT.cc
include/Timer.h
include/BOWExtractor.h
include/GT.h

)

target_link_libraries (${PROJECT_NAME}
${MATPLOT_LIBRARIES}
${OpenCV_LIBS}
${GeographicLib_LIBRARIES}
${PROJECT_SOURCE_DIR}/Thirdparty/DBoW2/build/libDBoW2.so
)

#

# Add executable called "helloDemo" that is built from the source files
# "demo.cxx" and "demo_b.cxx". The extensions are automatically found.
add_executable (createVocabulary ../createVocabulary.cc)
target_link_libraries(createVocabulary ${PROJECT_NAME})

add_executable (createDiffMatrix ../createDiffMatrix.cc)
target_link_libraries(createDiffMatrix ${PROJECT_NAME})

add_executable (createTrainingAndTestData ../createTrainingAndTestData.cc)
target_link_libraries(createTrainingAndTestData ${PROJECT_NAME})


add_executable (createTrainingAndTestDataSUM ../createTrainingAndTestDataSUM.cc)
target_link_libraries(createTrainingAndTestDataSUM ${PROJECT_NAME})

add_executable (teste ../teste.cc)
target_link_libraries(teste ${PROJECT_NAME})


add_executable (generateGT ../generateGT.cc)
target_link_libraries(generateGT ${PROJECT_NAME} Matplot++::matplot)


add_executable (plotGT ../for_paper/plot_exps.cc)
target_link_libraries(plotGT ${PROJECT_NAME} Matplot++::matplot)