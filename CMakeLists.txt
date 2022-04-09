PROJECT(MyConsumer)
cmake_minimum_required(VERSION 3.16.3)

SET(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/bin)
SET(CMAKE_RUNTIME_OUTPUT_DIRECTORY_RELEASE ${CMAKE_CURRENT_SOURCE_DIR}/bin)
SET(CMAKE_RUNTIME_OUTPUT_DIRECTORY_DEBUG ${CMAKE_CURRENT_SOURCE_DIR}/bin)

SET(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/lib)
SET(CMAKE_ARCHIVE_OUTPUT_DIRECTORY_RELEASE ${CMAKE_ARCHIVE_OUTPUT_DIRECTORY})
SET(CMAKE_ARCHIVE_OUTPUT_DIRECTORY_DEBUG ${CMAKE_ARCHIVE_OUTPUT_DIRECTORY})

# Magia 1 de 2 para que lea librerias de .conan/data
include(${CMAKE_BINARY_DIR}/conanbuildinfo.cmake)
conan_basic_setup()


#KO  (ERROR ADD_LIBRARY hello_conan cannot find source file No source given to target hello_conan. Es porque esto seria si construimos aqui la libreria
#ADD_LIBRARY(hello_conan hello_conan.cpp)


#OK. Con el imported le decimos que aqui no construimos la libreria, que solo la usamos
#ADD_LIBRARY(hello_conan hello_conan.cpp)

#[[Mandatory after added the target. Notar que no se usa en el CMakeLists.txt top y sin embargo main.cpp podra hacer #include "addition.h"  sin tener que proveer ninguan relative path. ¡¡CMake se encarga!! y lo puede hacer gracias a este scope PUBLIC]] #Agrega la propiedad INTERFACE_INCLUDE_DIRECTORIE y al hacerlo como public los CMakeList.txt padres (aqui el general) reciben tb la propiedad y por eso en los fuentes del ppal puedo incluir el h exactamente igual que en los fuents de las librerias (basta poner el .h sin subdirectorio include por esto ...
#target_include_directories (hello_conan PUBLIC include)
# ... bueno y sin subdir con el de la libreria ("hello_conan") por la delegacion ya vista en el raiz de poner sendos add_subdirectory cada uno con el nombre del subdir en el que poner un nuevo CMakeLists.txt que se encargara de generar el .a de cada una de las libre rias




# Make sure the compiler can find include files for our Hello library
# when other libraries or executables link to Hello
#target_include_directories (hello_conan PUBLIC include)


ADD_EXECUTABLE(consumer consumer.cpp)

#TARGET_LINK_LIBRARIES(consumer hello_conan)

# Magia 1 de 2 para que lea librerias de .conan/data
target_link_libraries(consumer ${CONAN_LIBS})




IF (WIN32 AND "${CMAKE_CXX_COMPILER_ID}" STREQUAL "Clang")
	TARGET_LINK_LIBRARIES(hello_conan stdc++)
ENDIF()
