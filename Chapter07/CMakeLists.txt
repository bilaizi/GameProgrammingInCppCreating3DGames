cmake_minimum_required(VERSION 3.22)
project(Chapter07)
set(CMAKE_CXX_STANDARD 17)
include_directories(
    ../External
    ../External/FMOD/inc
    ../External/glew-2.2.0/include
    ../External/rapidjson/include
    ../External/SDL2-devel-2.28.2-mingw/x86_64-w64-mingw32/include
    ../External/SDL2-devel-2.28.2-mingw/x86_64-w64-mingw32/include/SDL2
    ../External/SDL2_image-devel-2.6.3-mingw/x86_64-w64-mingw32/include
)
link_directories(
    ../External/FMOD/lib
    ../External/glew-2.2.0/lib
    ../External/SDL2-devel-2.28.2-mingw/x86_64-w64-mingw32/lib
    ../External/SDL2_image-devel-2.6.3-mingw/x86_64-w64-mingw32/lib
)

file(GLOB SOURCES *.cpp)
add_executable(Chapter07 ${SOURCES})

# copy assets files
file(GLOB ASSETS
    Assets/*.png
    Assets/*.txt
)
add_custom_target(
    Assets
    DEPENDS ${ASSETS}
)
add_dependencies(Chapter07 Assets)
add_custom_command(TARGET Assets POST_BUILD
    COMMAND ${CMAKE_COMMAND} -E copy_directory
    "$<TARGET_PROPERTY:Chapter07,SOURCE_DIR>/Assets"
    "$<TARGET_PROPERTY:Chapter07,BINARY_DIR>/Assets"
)

# copy shader files
file(GLOB GLSL_SOURCE_FILES
    Shaders/*.frag
    Shaders/*.vert
)
add_custom_target(
    Shaders
    DEPENDS ${GLSL_SOURCE_FILES}
)
add_dependencies(Chapter07 Shaders)
add_custom_command(TARGET Shaders POST_BUILD
    COMMAND ${CMAKE_COMMAND} -E copy_directory
    "$<TARGET_PROPERTY:Chapter07,SOURCE_DIR>/Shaders"
    "$<TARGET_PROPERTY:Chapter07,BINARY_DIR>/Shaders"
)

file(GLOB SO_OR_DLL_FILES
    ../External/FMOD/bin
    ../External/glew-2.2.0/bin
    ../External/SDL2-devel-2.28.2-mingw/x86_64-w64-mingw32/bin
    ../External/SDL2_image-devel-2.6.3-mingw/x86_64-w64-mingw32/bin
)
add_custom_command(TARGET Chapter07 POST_BUILD
    COMMAND ${CMAKE_COMMAND} -E copy_directory
    ${SO_OR_DLL_FILES}
    "$<TARGET_PROPERTY:Chapter07,BINARY_DIR>"
)
target_link_libraries(Chapter07 SDL2 SDL2_image glew32 opengl32 fmod fmodstudio)