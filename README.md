# STM32 项目模板
中文 | [English](doc/README_en.md)

- 这是一个基于 STM32CubeMX 生成的项目模板，使用 CMake 管理（使用类似 STM32CubeMX 项目的方式组织，方便更新/替换）  
- 推荐使用：`STM32Cube for Visual Studio Code`  
- 采用芯片型号：`STM32F103C8T6`  
- 如需`SPL`库，请使用[SPL 分支](https://github.com/5kind/STM32ProjectTemplate/tree/SPL)  


## 使用方法

1. 通过[Use the template](https://github.com/new?template_name=STM32ProjectTemplate&template_owner=5kind)创建新的仓库，不要fork；  
2. [main.c](Core/Src/main.c)中包含了一个简单的点灯程序，可以替换/添加到`Core/{Inc,Src}`下的文件，已在[CMakeLists.txt](cmake/stm32cubemx/CMakeLists.txt)包含（`Core/Src/*.{c,cpp} Core/Inc/`）。  
3. 默认包含的驱动库，见[CMakeLists.txt](cmake/stm32cubemx/CMakeLists.txt)，自行添加并定义：
```
# STM32 HAL/LL Drivers
file(GLOB STM32_Drivers_Src
    ${HAL_Src}_gpio*.c
    ${HAL_Src}.c
    ${HAL_Src}_rcc*.c
    ${HAL_Src}_dma.c
    ${HAL_Src}_cortex.c
    ${HAL_Src}_pwr.c
    ${HAL_Src}_flash*.c
    ${HAL_Src}_exti.c
)
```


## 编译方法

```
# 推荐使用ninja
# Debug:
cmake -DCMAKE_TOOLCHAIN_FILE=cmake/gcc-arm-none-eabi.cmake -S ./ -B build/Debug -G"Ninja" -DCMAKE_BUILD_TYPE=Debug && ninja -C build/Debug
# Release:
cmake -DCMAKE_TOOLCHAIN_FILE=cmake/gcc-arm-none-eabi.cmake -S ./ -B build/Release -G"Ninja" -DCMAKE_BUILD_TYPE=Release && ninja -C build/Release
```

# 许可证或用户协议
参见
```
# Apache-2.0
Drivers/CMSIS/Device/ST/STM32F1xx/LICENSE.txt
Drivers/CMSIS/docs/General/html/LICENSE.txt
Drivers/CMSIS/LICENSE.txt
# BSD-3-Clause
Drivers/STM32F1xx_HAL_Driver/LICENSE.txt
```
