# Basic CMake that uses All.c
cmake_minimum_required(VERSION 3.7)

project(UltrawideMania)
set(MOD_NAME MaxControl CACHE STRING "The mod directory to look into")

set(OUTPUT_NAME "MaxControl" CACHE STRING "The name of the built library")

set(sources
    GameAPI/C/GameAPI/Game.c
    ${MOD_NAME}/dllmain.c
    ${MOD_NAME}/Objects/All.c
)

if (VITA)
    set(VITASDK_CMAKE_FILE "$ENV{VITASDK}/share/vita.cmake" CACHE PATH "VitaSDK CMake functions file")
    include("${VITASDK_CMAKE_FILE}" REQUIRED)

    add_executable(MaxControl
        ${sources}
    )
else()
    add_library(MaxControl SHARED
        ${sources}
    )
endif()

target_include_directories(MaxControl PRIVATE
    ${MOD_NAME}/
    ${MOD_NAME}/Objects/
    GameAPI/C/
)

set_target_properties(MaxControl PROPERTIES OUTPUT_NAME ${OUTPUT_NAME})

if (VITA)
    vita_create_self(${OUTPUT_NAME}.suprx ${MOD_NAME}
      GEN_EXPORTS export.yml    # This is required or else nothing will be exported
      UNSAFE
    )
endif()

unset(MOD_NAME CACHE)
unset(OUTPUT_NAME CACHE)