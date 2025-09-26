# STM32 项目模板
中文 | [English](doc/README_en.md)

- 这是一个基于 STM32CubeMX 生成的项目模板，使用 CMake 管理（`Libraries  Project`使用类似`STM32F10x_StdPeriph_Lib`项目的方式组织，方便更新/替换）  
- 采用芯片型号：`STM32F103C8T6`  
- 如需`HAL`库，请使用[主分支](https://github.com/5kind/STM32ProjectTemplate)  


## 使用方法

1. 通过[Use the template](https://github.com/new?template_name=STM32ProjectTemplate&template_owner=5kind)创建新的仓库，不要fork；  
2. [main.c](Core/Src/main.c)中包含了一个简单的点灯程序，可以替换/添加到`Core/{Inc,Src}`下的文件，已在[CMakeLists.txt](cmake/stm32cubemx/CMakeLists.txt)包含（`Core/Src/*.{c,cpp} Core/Inc/`）。  
3. 默认包含所有SPL驱动库，[core_cm3.c](Libraries/CMSIS/CM3/CoreSupport/core_cm3.c)有修改，见[CMakeLists.txt](cmake/stm32cubemx/CMakeLists.txt)或[core_cm3.c 修改版本](Core/Src/core_cm3.c)：
```
# STM32 SPL Drivers
file(GLOB STM32_Drivers_Src
    # Incompatible with newer versions of GCC, 
    # see Core/Src/core_cm3.c for replacement.
    # ${StdPeriphLibraries}/CMSIS/CM3/CoreSupport/core_cm3.c
    ${StdPeriphLibraries}/CMSIS/CM3/DeviceSupport/ST/STM32F10x/system_stm32f10x.c
    ${StdPeriphLibraries}/STM32F10x_StdPeriph_Driver/src/*.c
)
```
```diff Libraries/CMSIS/CM3/CoreSupport/core_cm3.c Core/Src/core_cm3.c 
736c736
<    __ASM volatile ("strexb %0, %2, [%1]" : "=r" (result) : "r" (addr), "r" (value) );
---
>    __ASM volatile ("strexb %0, %2, [%1]" : "=&r" (result) : "r" (addr), "r" (value) );
753c753
<    __ASM volatile ("strexh %0, %2, [%1]" : "=r" (result) : "r" (addr), "r" (value) );
---
>    __ASM volatile ("strexh %0, %2, [%1]" : "=&r" (result) : "r" (addr), "r" (value) );
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
# BSD-3-Clause
Libraries/CMSIS/CM3/DeviceSupport/ST/STM32F10x/LICENSE.txt
# ST: SLA0044
Libraries/STM32F10x_StdPeriph_Driver/LICENSE.txt
Project/STM32F10x_StdPeriph_Template/LICENSE.txt
# ARM: LEC-PRE-00425-V2.0
Libraries/CMSIS/License.doc
```
