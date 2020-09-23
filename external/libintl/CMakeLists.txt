# This file is part of Desktop App Toolkit,
# a set of libraries for developing nice desktop applications.
#
# For license and copyright information please follow this link:
# https://github.com/desktop-app/legal/blob/master/LEGAL

add_library(external_libintl INTERFACE IMPORTED GLOBAL)
add_library(desktop-app::external_libintl ALIAS external_libintl)

if (NOT WIN32)
    find_path(LibIntl_INCLUDE_DIRS NAMES libintl.h REQUIRED)
    target_include_directories(external_libintl INTERFACE ${LibIntl_INCLUDE_DIRS})

    include(CheckCXXSymbolExists)
    check_cxx_symbol_exists(dngettext libintl.h LibIntl_SYMBOL_FOUND)

    if (NOT LibIntl_SYMBOL_FOUND)
        if (DESKTOP_APP_USE_PACKAGED)
            find_library(LibIntl_LIBRARIES NAMES intl libintl REQUIRED)
        else()
            find_library(LibIntl_LIBRARIES NAMES libintl.a REQUIRED)
        endif()
        target_link_libraries(external_libintl INTERFACE ${LibIntl_LIBRARIES})
    endif()
endif()
