cmake_minimum_required(VERSION 3.9 FATAL_ERROR)

project(VulkanFunctionLoaderProject LANGUAGES CXX)

add_library(VulkanFunctionLoader "src/VulkanFunctions.cpp")
target_include_directories(VulkanFunctionLoader PUBLIC "${CMAKE_CURRENT_SOURCE_DIR}/include")
target_compile_definitions(VulkanFunctionLoader PRIVATE VK_NO_PROTOTYPES)
if(WIN32)
target_compile_definitions(VulkanFunctionLoader PRIVATE VK_USE_PLATFORM_WIN32_KHR)
endif(WIN32)

find_package(Vulkan REQUIRED)

if(Vulkan_FOUND)
  message(STATUS ${Vulkan_LIBRARY})
else(Vulkan_FOUND)
  message("Vulkan was not found.")
endif(Vulkan_FOUND)

add_library(VulkanHeaders INTERFACE)
target_include_directories(VulkanHeaders INTERFACE ${Vulkan_INCLUDE_DIRS})
target_link_libraries(VulkanFunctionLoader PUBLIC VulkanHeaders)

set(VFL_CMAKE_DIR "${CMAKE_CURRENT_SOURCE_DIR}/cmake")
export(TARGETS VulkanFunctionLoader VulkanHeaders FILE ${VFL_CMAKE_DIR}/VulkanFunctionLoaderTargets.cmake)

#configure_file(VulkanFunctionLoaderConfig.cmake.in "${VFL_CMAKE_DIR}/VulkanFunctionLoaderConfig.cmake" @ONLY)