cmake_minimum_required(VERSION 3.10)

set(DISSECTOR_SRC packet-ptp.c)

if (WIN32)
    set(CMAKE_RC_COMPILER_INIT windres)
    enable_language(RC)
    set(DISSECTOR_SRC ${DISSECTOR_SRC} plugin.rc)
endif()

include_directories(${CMAKE_SOURCE_DIR})

# Find libpcap
find_package(PCAP REQUIRED)

add_library(ptp MODULE ${DISSECTOR_SRC})

set_target_properties(ptp PROPERTIES
    PREFIX ""
    LINK_FLAGS "${WS_LINK_FLAGS}"
)

install(TARGETS ptp
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}/wireshark/plugins/${PROJECT_VERSION_MAJOR}.${PROJECT_VERSION_MINOR}/epan
    RUNTIME DESTINATION ${CMAKE_INSTALL_LIBDIR}/wireshark/plugins/${PROJECT_VERSION_MAJOR}.${PROJECT_VERSION_MINOR}/epan
)