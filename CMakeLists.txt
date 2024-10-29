cmake_minimum_required(VERSION 3.12)
project(3DEngine)

# Establece el estándar de C++
set(CMAKE_CXX_STANDARD 17)

# Añade el ejecutable del proyecto
add_executable(3DEngine src/main.cpp)

# Busca Vulkan
find_package(Vulkan REQUIRED)

# Vincula Vulkan al proyecto
if(Vulkan_FOUND)
    message(STATUS "Vulkan library found!")
    target_include_directories(3DEngine PRIVATE ${Vulkan_INCLUDE_DIRS})
    #target_link_libraries(My3DEngine PRIVATE ${Vulkan_LIBRARIES})
else()
    message(FATAL_ERROR "Vulkan not found! Please install Vulkan SDK.")
endif()


# PhysX-specific CMake project setup
set(NV_USE_DEBUG_WINCRT ON CACHE BOOL "Use the debug version of the CRT")
set(PHYSX_ROOT_DIR ${CMAKE_SOURCE_DIR}/external/PhysX/physx)
set(PXSHARED_PATH ${PHYSX_ROOT_DIR}/../pxshared)
set(PXSHARED_INSTALL_PREFIX ${CMAKE_INSTALL_PREFIX})
set(CMAKEMODULES_VERSION "1.27")
set(CMAKEMODULES_PATH ${PHYSX_ROOT_DIR}/../externals/cmakemodules)
set(PX_OUTPUT_LIB_DIR ${CMAKE_CURRENT_BINARY_DIR}/externals/physx)
set(PX_OUTPUT_BIN_DIR ${CMAKE_CURRENT_BINARY_DIR}/externals/physx)
    set(TARGET_BUILD_PLATFORM "windows" CACHE INTERNAL "")
    set(PLATFORM "Windows")

# Configura PhysX
#set(PHYSX_ROOT_DIR ${CMAKE_SOURCE_DIR}/external/PhysX)
add_subdirectory(${PHYSX_ROOT_DIR}/source/compiler/cmake 3DEngine_PhysX)
include_directories(${PHYSX_ROOT_DIR}/include)

# Añade PhysX como dependencia
target_link_libraries(3DEngine PUBLIC PhysX ${Vulkan_LIBRARIES})
