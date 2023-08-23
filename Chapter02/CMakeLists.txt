cmake_minimum_required(VERSION 3.26)
project(Chapter02)
set(CMAKE_CXX_STANDARD 17)
include_directories(
    ../External/SDL2-devel-2.28.2-mingw/x86_64-w64-mingw32/include
    ../External/SDL2-devel-2.28.2-mingw/x86_64-w64-mingw32/include/SDL2
    ../External/SDL2_image-devel-2.6.3-mingw/x86_64-w64-mingw32/include
)
link_directories(
    ../External/SDL2-devel-2.28.2-mingw/x86_64-w64-mingw32/lib
    ../External/SDL2_image-devel-2.6.3-mingw/x86_64-w64-mingw32/lib
)

file(GLOB SOURCES *.cpp)
add_executable(Chapter2 ${SOURCES})

# copy assets files
file(GLOB ASSETS
    Assets/*.csv
    Assets/*.txt
    Assets/*.png
)
add_custom_target(
    Assets
    DEPENDS ${ASSETS}
)
add_dependencies(Chapter02 Assets)
add_custom_command(TARGET Assets POST_BUILD
    COMMAND ${CMAKE_COMMAND} -E copy_directory
    "$<TARGET_PROPERTY:Chapter02,SOURCE_DIR>/Assets"
    "$<TARGET_PROPERTY:Chapter02,BINARY_DIR>/Assets"
)

file(GLOB SO_OR_DLL_FILES
    ../External/SDL2-devel-2.28.2-mingw/x86_64-w64-mingw32/bin
    ../External/SDL2_image-devel-2.6.3-mingw/x86_64-w64-mingw32/bin
)
add_custom_command(TARGET Chapter02 POST_BUILD
    COMMAND ${CMAKE_COMMAND} -E copy_directory
    ${SO_OR_DLL_FILES}
    "$<TARGET_PROPERTY:Chapter02,BINARY_DIR>"
)
target_link_libraries(Chapter02 SDL2 SDL2_image)