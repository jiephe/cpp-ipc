project(send_recv)

set(CMAKE_BUILD_TYPE Debug)

include_directories(${CMAKE_CURRENT_SOURCE_DIR}/../../include)

file(GLOB SRC_FILES ./*.cpp)
file(GLOB HEAD_FILES ./*.h)

# Library directories
link_directories(
    ${CMAKE_CURRENT_SOURCE_DIR}/../../build/lib
)

add_executable(${PROJECT_NAME} ${SRC_FILES} ${HEAD_FILES})

target_link_libraries(${PROJECT_NAME} ipc pthread rt)
