# SPDX-License-Identifier: MIT

cmake_minimum_required(VERSION 3.13)

if(DEFINED ENV{PICO_SDK_PATH} AND EXISTS "$ENV{PICO_SDK_PATH}/external/pico_sdk_import.cmake")
	include($ENV{PICO_SDK_PATH}/external/pico_sdk_import.cmake)
elseif(EXISTS "${CMAKE_CURRENT_LIST_DIR}/pico-sdk/external/pico_sdk_import.cmake")
	include(${CMAKE_CURRENT_LIST_DIR}/pico-sdk/external/pico_sdk_import.cmake)
else()
	message(FATAL_ERROR "Unable to locate Pico SDK. Set PICO_SDK_PATH or initialize the pico-sdk submodule.")
endif()

project(pico_uart_bridge)

if (NOT PICO_BOARD)
	set(PICO_BOARD pico CACHE STRING "Target board")
endif()

pico_sdk_init()

add_executable(uart_bridge uart-bridge.c usb-descriptors.c)

target_include_directories(uart_bridge PUBLIC
	./
	${PICO_SDK_PATH}/lib/tinyusb/src)

target_link_libraries(uart_bridge
	hardware_flash
	pico_multicore
	pico_stdlib
	tinyusb_device)

pico_add_extra_outputs(uart_bridge)
