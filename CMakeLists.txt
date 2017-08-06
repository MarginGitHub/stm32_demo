cmake_minimum_required (VERSION 2.6)
project(stm32f7xx_libs)

set(CMAKE_C_COMPILER "arm-none-eabi-gcc")
set(CMAKE_CXX_COMPILER "arm-none-eabi-g++")

set(CMAKE_C_FLAGS "-mthumb -march=armv7e-m -mfloat-abi=hard -std=c99 -mfpu=fpv5-sp-d16 ${CMAKE_C_FLAGS}")
set(CMAKE_CXX_FLAGS "-mthumb -march=armv7e-m -mfloat-abi=hard -std=c++11 -mfpu=fpv5-sp-d16 ${CMAKE_CXX_FLAGS}")


include_directories(./inc ./inc/core ./inc/legacy ./app/inc ./board)

add_subdirectory(lib)
add_subdirectory(boot)



 target_link_libraries(main libstm32f7xx.a libapp.a libboot.a)

 add_custom_command(TARGET main
                     PRE_BUILD
                     COMMAND arm-none-eabi-gcc ${CMAKE_C_FLAGS} -c startup_stm32f767xx.s
                     COMMAND ar -r libboot.a startup_stm32f767xx.o
                    )