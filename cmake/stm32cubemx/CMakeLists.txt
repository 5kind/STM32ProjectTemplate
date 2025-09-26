cmake_minimum_required(VERSION 3.22)
# Enable CMake support for ASM and C languages
enable_language(C ASM)
# STM32CubeMX generated symbols (macros)
set(MX_Defines_Syms 
    USE_STDPERIPH_DRIVER
    STM32F10X_MD
    $<$<CONFIG:Debug>:DEBUG>
)

set(Core_Src ${CMAKE_CURRENT_SOURCE_DIR}/../../Core/Src)
set(StdPeriphLibraries ${CMAKE_CURRENT_SOURCE_DIR}/../../Libraries)

# STM32CubeMX generated include paths
set(MX_Include_Dirs
    ${CMAKE_CURRENT_SOURCE_DIR}/../../Core/Inc
    ${StdPeriphLibraries}/CMSIS/CM3/CoreSupport
    ${StdPeriphLibraries}/CMSIS/CM3/DeviceSupport/ST/STM32F10x
    ${StdPeriphLibraries}/STM32F10x_StdPeriph_Driver/inc
)

# STM32CubeMX generated application sources
file(GLOB MX_Application_Src
    ${Core_Src}/*.c
    ${Core_Src}/*.cpp
    ${StdPeriphLibraries}/CMSIS/CM3/DeviceSupport/ST/STM32F10x/startup/gcc_ride7/startup_stm32f10x_md.s
)

# STM32 SPL Drivers
file(GLOB STM32_Drivers_Src
    # Incompatible with newer versions of GCC, 
    # see Core/Src/core_cm3.c for replacement.
    # ${StdPeriphLibraries}/CMSIS/CM3/CoreSupport/core_cm3.c
    ${StdPeriphLibraries}/CMSIS/CM3/DeviceSupport/ST/STM32F10x/system_stm32f10x.c
    ${StdPeriphLibraries}/STM32F10x_StdPeriph_Driver/src/*.c
)

# Drivers Midllewares



# Link directories setup
set(MX_LINK_DIRS

)
# Project static libraries
set(MX_LINK_LIBS 
    STM32_Drivers
    ${TOOLCHAIN_LINK_LIBRARIES}
    
)
# Interface library for includes and symbols
add_library(stm32cubemx INTERFACE)
target_include_directories(stm32cubemx INTERFACE ${MX_Include_Dirs})
target_compile_definitions(stm32cubemx INTERFACE ${MX_Defines_Syms})

# Create STM32_Drivers static library
add_library(STM32_Drivers OBJECT)
target_sources(STM32_Drivers PRIVATE ${STM32_Drivers_Src})
target_link_libraries(STM32_Drivers PUBLIC stm32cubemx)


# Add STM32CubeMX generated application sources to the project
target_sources(${CMAKE_PROJECT_NAME} PRIVATE ${MX_Application_Src})

# Link directories setup
target_link_directories(${CMAKE_PROJECT_NAME} PRIVATE ${MX_LINK_DIRS})

# Add libraries to the project
target_link_libraries(${CMAKE_PROJECT_NAME} ${MX_LINK_LIBS})

# Add the map file to the list of files to be removed with 'clean' target
set_target_properties(${CMAKE_PROJECT_NAME} PROPERTIES ADDITIONAL_CLEAN_FILES ${CMAKE_PROJECT_NAME}.map)

# Validate that STM32CubeMX code is compatible with C standard
if((CMAKE_C_STANDARD EQUAL 90) OR (CMAKE_C_STANDARD EQUAL 99))
    message(ERROR "Generated code requires C11 or higher")
endif()
