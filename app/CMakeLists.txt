cmake_minimum_required(VERSION 3.4.1)
project(libuv C)

set(LIBUVDIR src/main/cpp)

include_directories(${LIBUVDIR}/include ${LIBUVDIR}/src)
set(SOURCES
    ${LIBUVDIR}/include/uv.h
    ${LIBUVDIR}/include/uv-errno.h
    ${LIBUVDIR}/include/uv-threadpool.h
    ${LIBUVDIR}/include/uv-version.h
    ${LIBUVDIR}/src/fs-poll.c
    ${LIBUVDIR}/src/heap-inl.h
    ${LIBUVDIR}/src/inet.c
    ${LIBUVDIR}/src/queue.h
    ${LIBUVDIR}/src/threadpool.c
    ${LIBUVDIR}/src/uv-common.c
    ${LIBUVDIR}/src/uv-common.h
    ${LIBUVDIR}/src/version.c
    )

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -g --std=gnu89 -pedantic -Wall -Wextra -Wno-unused-parameter -Wno-pedantic -Wno-sign-compare -Wno-implicit-function-declaration")
add_definitions(-D_LARGEFILE_SOURCE -D_FILE_OFFSET_BITS=64)
include_directories(${LIBUVDIR}/src/unix)
set(SOURCES ${SOURCES}
    ${LIBUVDIR}/include/uv-unix.h
    ${LIBUVDIR}/include/uv-linux.h
    ${LIBUVDIR}/include/uv-sunos.h
    ${LIBUVDIR}/include/uv-darwin.h
    ${LIBUVDIR}/include/uv-bsd.h
    ${LIBUVDIR}/include/uv-aix.h
    ${LIBUVDIR}/src/unix/async.c
    ${LIBUVDIR}/src/unix/atomic-ops.h
    ${LIBUVDIR}/src/unix/core.c
    ${LIBUVDIR}/src/unix/dl.c
    ${LIBUVDIR}/src/unix/fs.c
    ${LIBUVDIR}/src/unix/getaddrinfo.c
    ${LIBUVDIR}/src/unix/getnameinfo.c
    ${LIBUVDIR}/src/unix/internal.h
    ${LIBUVDIR}/src/unix/loop.c
    ${LIBUVDIR}/src/unix/loop-watcher.c
    ${LIBUVDIR}/src/unix/pipe.c
    ${LIBUVDIR}/src/unix/poll.c
    ${LIBUVDIR}/src/unix/process.c
    ${LIBUVDIR}/src/unix/signal.c
    ${LIBUVDIR}/src/unix/spinlock.h
    ${LIBUVDIR}/src/unix/stream.c
    ${LIBUVDIR}/src/unix/tcp.c
    ${LIBUVDIR}/src/unix/thread.c
    ${LIBUVDIR}/src/unix/timer.c
    ${LIBUVDIR}/src/unix/tty.c
    ${LIBUVDIR}/src/unix/udp.c
    ${LIBUVDIR}/src/unix/proctitle.c
    ${LIBUVDIR}/src/unix/linux-core.c
    ${LIBUVDIR}/src/unix/linux-inotify.c
    ${LIBUVDIR}/src/unix/linux-syscalls.c
    ${LIBUVDIR}/src/unix/linux-syscalls.h
    )

add_definitions(-Wstrict-aliasing)

set(SOURCES ${SOURCES}
    ${LIBUVDIR}/include/android-ifaddrs.h
    ${LIBUVDIR}/include/pthread-fixes.h
    ${LIBUVDIR}/src/unix/android-ifaddrs.c
    ${LIBUVDIR}/src/unix/pthread-fixes.c
    )

add_library(uv SHARED ${SOURCES})

find_library(M_LIB NAMES m)
find_package(Threads)
target_link_libraries(uv
    ${CMAKE_THREAD_LIBS_INIT}
    ${M_LIB}
    android
    log)