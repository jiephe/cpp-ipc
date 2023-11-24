cmake_minimum_required(VERSION 3.10)
project(cpp-ipc)

option(LIBIPC_BUILD_TESTS       "Build all of libipc's own tests."                      OFF)
option(LIBIPC_BUILD_DEMOS       "Build all of libipc's own demos."                      OFF)
option(LIBIPC_BUILD_SHARED_LIBS "Build shared libraries (DLLs)."                        OFF)
option(LIBIPC_USE_STATIC_CRT    "Set to ON to build with static CRT on Windows (/MT)."  OFF)

set(CMAKE_POSITION_INDEPENDENT_CODE ON)
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_BUILD_TYPE Debug)

set(LIBRARY_OUTPUT_PATH    ${CMAKE_BINARY_DIR}/bin)
set(EXECUTABLE_OUTPUT_PATH ${CMAKE_BINARY_DIR}/bin)
set(LIBIPC_PROJECT_DIR     ${PROJECT_SOURCE_DIR})

# Unicode Support
add_definitions(-DUNICODE -D_UNICODE)

add_subdirectory(src)

if (LIBIPC_BUILD_TESTS)
    set(GOOGLETEST_VERSION 1.10.0)
    if (LIBIPC_USE_STATIC_CRT)
        set(gtest_force_shared_crt OFF)
    else()
        set(gtest_force_shared_crt ON)
    endif()
    add_subdirectory(3rdparty/gtest)
    add_subdirectory(test)
endif()

if (LIBIPC_BUILD_DEMOS)
    add_subdirectory(demo/chat)
    add_subdirectory(demo/msg_que)
    add_subdirectory(demo/send_recv)
    if (MSVC)
        add_subdirectory(demo/win_service/service)
        add_subdirectory(demo/win_service/client)
    else()
        add_subdirectory(demo/linux_service/service)
        add_subdirectory(demo/linux_service/client)
    endif()
endif()

install(
    DIRECTORY "include/"
    DESTINATION "include"
)
