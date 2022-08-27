cmake_minimum_required(VERSION 3.5)
project(ccat)

set(CMAKE_CONFIGURATION_TYPES "Debug;Release" CACHE STRING "" FORCE)

if(NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE Release)
endif()

if(MSVC)
else()
    set(CMAKE_CXX_FLAGS "-Wall -Wextra")
    set(CMAKE_CXX_FLAGS_DEBUG "-g -O0 -march=native")
    set(CMAKE_CXX_FLAGS_RELEASE "-O3 -march=native")
endif()

set(CMAKE_CXX_STANDARD 11)

set(THREADS_PREFER_PTHREAD_FLAG ON)
find_package(Threads REQUIRED)

# Zstd library source files
set(ZSTD_LIB_SRCFILES
        src/zstd/bitstream.h
        src/zstd/compiler.h
        src/zstd/cover.c
        src/zstd/cpu.h
        src/zstd/divsufsort.c
        src/zstd/divsufsort.h
        src/zstd/entropy_common.c
        src/zstd/error_private.c
        src/zstd/error_private.h
        src/zstd/fse.h
        src/zstd/fse_compress.c
        src/zstd/fse_decompress.c
        src/zstd/huf.h
        src/zstd/huf_compress.c
        src/zstd/huf_decompress.c
        src/zstd/mem.h
        src/zstd/pool.c
        src/zstd/pool.h
        src/zstd/threading.c
        src/zstd/threading.h
        src/zstd/xxhash.c
        src/zstd/xxhash.h
        src/zstd/zdict.c
        src/zstd/zdict.h
        src/zstd/zstd.h
        src/zstd/zstdmt_compress.c
        src/zstd/zstdmt_compress.h
        src/zstd/zstd_common.c
        src/zstd/zstd_compress.c
        src/zstd/zstd_compress_internal.h
        src/zstd/zstd_decompress.c
        src/zstd/zstd_double_fast.c
        src/zstd/zstd_double_fast.h
        src/zstd/zstd_errors.h
        src/zstd/zstd_fast.c
        src/zstd/zstd_fast.h
        src/zstd/zstd_internal.h
        src/zstd/zstd_lazy.c
        src/zstd/zstd_lazy.h
        src/zstd/zstd_ldm.c
        src/zstd/zstd_ldm.h
        src/zstd/zstd_opt.c
        src/zstd/zstd_opt.h
)

# Zpng library source files
set(ZPNG_LIB_SRCFILES
        src/zpng.cpp
        src/zpng.h
)

# Zpng unit tester
set(ZPNG_TEST_SRCFILES
        src/apps/zpng_test.cpp
)

# Zpng app
set(ZPNG_APP_SRCFILES
        src/apps/zpng_app.cpp
)

add_library(zpnglib ${ZPNG_LIB_SRCFILES} ${ZSTD_LIB_SRCFILES})

add_executable(unit_test ${ZPNG_TEST_SRCFILES})
target_link_libraries(unit_test zpnglib Threads::Threads)

add_executable(zpng ${ZPNG_APP_SRCFILES})
target_link_libraries(zpng zpnglib Threads::Threads)
