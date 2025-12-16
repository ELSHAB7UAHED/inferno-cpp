cmake_minimum_required(VERSION 3.15)
project(Inferno VERSION 3.0.0)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# إعدادات خاصة بـ Windows
if(WIN32)
    add_definitions(-DWIN32_LEAN_AND_MEAN)
    add_definitions(-DUNICODE -D_UNICODE)
endif()

# البحث عن المكتبات
find_package(OpenSSL REQUIRED)
find_package(CURL REQUIRED)
find_package(ZLIB REQUIRED)
find_package(JsonCpp REQUIRED)

# ملفات المصدر
set(SOURCES
    inferno.cpp
)

# إعدادات الربط
add_executable(Inferno ${SOURCES})

target_link_libraries(Inferno
    PRIVATE
    OpenSSL::SSL
    OpenSSL::Crypto
    CURL::libcurl
    ZLIB::ZLIB
    jsoncpp_lib
    shlwapi
    shell32
    comctl32
    setupapi
    cfgmgr32
    winmm
    crypt32
    ws2_32
    wldap32
    normaliz
    advapi32
)

# إعدادات المترجم
if(MSVC)
    target_compile_options(Inferno PRIVATE /W4 /WX)
else()
    target_compile_options(Inferno PRIVATE -Wall -Wextra -Werror)
endif()

# تضمين ملف الأيقونة
if(WIN32)
    set(RESOURCE_FILE ${CMAKE_CURRENT_SOURCE_DIR}/inferno.rc)
    if(EXISTS ${RESOURCE_FILE})
        target_sources(Inferno PRIVATE ${RESOURCE_FILE})
    endif()
endif()
