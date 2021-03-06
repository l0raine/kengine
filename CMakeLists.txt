cmake_minimum_required(VERSION 3.0)
project(kengine)
set(CMAKE_CXX_STANDARD 17)
if(WIN32)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /DNOMINMAX")
endif()

if (KENGINE_SFML)
    set(TGUI TRUE)
    set(PUTILS_BUILD_PSE TRUE)
endif ()

if(KENGINE_LUA)
    set(PUTILS_BUILD_LUA TRUE)
endif()

if(KENGINE_PYTHON)
    set(PUTILS_BUILD_PYTHON TRUE)
endif()

if (KENGINE_NO_SHADER_DEBUG)
    set(PUTILS_NO_SHADER_DEBUG TRUE)
endif()

set(PUTILS_BUILD_MEDIATOR TRUE)
add_subdirectory(putils)

file(GLOB src_files
    *.cpp *.hpp
    common/components/*.cpp common/components/*.hpp
    common/functions/*.cpp common/functions/*.hpp
    common/packets/*.cpp common/packets/*.hpp
    common/systems/*.cpp common/systems/*.hpp
    common/helpers/*.cpp common/helpers/*.hpp)

add_library(kengine STATIC ${src_files})
target_link_libraries(kengine PUBLIC putils)

if (KENGINE_SFML)
    add_subdirectory(common/systems/sfml)
    target_link_libraries(kengine PUBLIC kengine_sfml)
endif ()

if (KENGINE_IMGUI_OVERLAY OR KENGINE_OPENGL)
    set(BUILD_UTILS FALSE)
    set(GLEW_PATH common/systems/opengl/libs/glew)
    add_subdirectory(${GLEW_PATH}/build/cmake)
    target_include_directories(kengine PUBLIC ${GLEW_PATH}/include)

    set(GLFW_BUILD_EXAMPLES OFF CACHE BOOL "" FORCE)
    set(GLFW_BUILD_TESTS OFF CACHE BOOL "" FORCE)
    add_subdirectory(common/systems/opengl/libs/glfw)
endif()

if (KENGINE_IMGUI_OVERLAY)
    add_subdirectory(common/systems/imgui_overlay)
    target_link_libraries(kengine PUBLIC kengine_imgui_overlay)
endif()

if (KENGINE_OPENGL)
    add_subdirectory(common/systems/opengl)
    target_link_libraries(kengine PUBLIC kengine_opengl)

    add_subdirectory(common/systems/opengl_sprites)
    target_link_libraries(kengine PUBLIC kengine_opengl_sprites)
endif ()

if (KENGINE_ASSIMP)
    add_subdirectory(common/systems/assimp)
    target_link_libraries(kengine PUBLIC kengine_assimp)
endif()

if (KENGINE_POLYVOX)
    add_subdirectory(common/systems/polyvox)
    target_link_libraries(kengine PUBLIC kengine_polyvox)
endif()

if (KENGINE_BULLET)
    add_subdirectory(common/systems/bullet)
    target_link_libraries(kengine PUBLIC kengine_bullet)
endif()

set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} PARENT_SCOPE)
target_include_directories(kengine PUBLIC . common)
