cmake_minimum_required(VERSION 3.10)

# Project name
project(smartfusion_cmake C ASM)

# Define the CPU and compiler flags
set(LINKER_SCRIPT "${CMAKE_SOURCE_DIR}/firmware/CMSIS/startup_gcc/debug-in-microsemi-smartfusion2-envm.ld")
set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} -T ${LINKER_SCRIPT}")

# Set cpu to cortex-m3
set(CPU_FLAGS "-mcpu=cortex-m3")
# Set compiler flags
add_compile_options(${CPU_FLAGS} ${COMMON_FLAGS})

# Include directories
include_directories(
    ${CMAKE_SOURCE_DIR}/firmware/
    ${CMAKE_SOURCE_DIR}/inc/ARM_CMSIS/CMSIS/Include
)

# Source files
file(GLOB_RECURSE SOURCES
    ${CMAKE_SOURCE_DIR}/firmware/hal/CortexM3/*.c
    ${CMAKE_SOURCE_DIR}/firmware/drivers/**/*.c
    ${CMAKE_SOURCE_DIR}/firmware/CMSIS/startup_gcc/*.c
    ${CMAKE_SOURCE_DIR}/firmware/CMSIS/startup_gcc/*.S
    ${CMAKE_SOURCE_DIR}/firmware/drivers_config/**/*.c
    ${CMAKE_SOURCE_DIR}/firmware/*.c
    ${CMAKE_SOURCE_DIR}/src/main.c
)

file(GLOB_RECURSE ASMSOURCES
    ${CMAKE_SOURCE_DIR}/hal/CortexM3/*.s
    ${CMAKE_SOURCE_DIR}/CMSIS/**/*.s
)

set(TARGET_NAME ${PROJECT_NAME}.elf)
# Add executable
add_executable(${TARGET_NAME} ${SOURCES} ${ASMSOURCES})

# Post-build steps
add_custom_command(TARGET ${TARGET_NAME} POST_BUILD
    COMMAND arm-none-eabi-objcopy -O ihex ${TARGET_NAME} ${CMAKE_BINARY_DIR}/${PROJECT_NAME}.hex
    COMMAND arm-none-eabi-size --format=berkeley ${TARGET_NAME}
    COMMENT "Generating HEX and size information"
)

# set (OPENOCD_OPTIONS --command "set DEVICE M2S010" --file board/microsemi-cortex-m3.cfg)
# # Add upload command
# add_custom_command(TARGET ${PROJECT_NAME} 
#     COMMAND openocd ${OPENOCD_OPTIONS} 
#     COMMENT "Uploading to target"
# )
