cmake_minimum_required(VERSION 3.6)

project(jbigkit VERSION 2.1)

set(CMAKE_C_STANDARD 99)
set(CMAKE_C_STANDARD_REQUIRED True)

set(JBIG_SOURCE
    libjbig/jbig.c
    libjbig/jbig.h
    libjbig/jbig_ar.c
    libjbig/jbig_ar.h
    )

set(JBIG85_SOURCE
    libjbig/jbig85.c
    libjbig/jbig85.h
    libjbig/jbig_ar.c
    libjbig/jbig_ar.h
    )

add_executable(tstcodec
    libjbig/tstcodec.c
    ${JBIG_SOURCE}
    )

add_executable(tstcodec85
    libjbig/tstcodec85.c
    ${JBIG85_SOURCE}
    )

add_executable(tstjoint
    libjbig/tstjoint.c
    ${JBIG_SOURCE}
    ${JBIG85_SOURCE}
    )

add_library(jbig STATIC
    ${JBIG_SOURCE}
    )
add_library(jbig85 STATIC 
    ${JBIG85_SOURCE}
    )

add_executable(pbmtojbg
    pbmtools/pbmtojbg.c
    ${JBIG_SOURCE}
    )
target_link_libraries(pbmtojbg jbig)
target_include_directories(pbmtojbg PUBLIC
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>
)

add_executable(jbgtopbm 
    pbmtools/jbgtopbm.c
    ${JBIG_SOURCE}
    )
target_link_libraries(jbgtopbm jbig)
target_include_directories(jbgtopbm PUBLIC
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>
)

add_executable(pbmtojbg85
    pbmtools/pbmtojbg85.c
    ${JBIG85_SOURCE}
    )
target_link_libraries(pbmtojbg85 jbig85)
target_include_directories(pbmtojbg85 PUBLIC
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>
)

add_executable(jbgtopbm85
    pbmtools/jbgtopbm85.c
    ${JBIG85_SOURCE}
    )
target_link_libraries(jbgtopbm85 jbig85)
target_include_directories(jbgtopbm85 PUBLIC
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>
)

if(NOT MSVC)
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -O2 -W -Wno-unused-result")
endif()

include(GNUInstallDirs)
install(
          FILES ${CMAKE_CURRENT_SOURCE_DIR}/libjbig/jbig.h
          FILES ${CMAKE_CURRENT_SOURCE_DIR}/libjbig/jbig85.h
                ${CMAKE_CURRENT_SOURCE_DIR}/libjbig/jbig_ar.h
          DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
  )

install(TARGETS jbig jbig85
        LIBRARY DESTINATION lib)

# uninstall target
configure_file(
    "${CMAKE_CURRENT_SOURCE_DIR}/cmake_uninstall.cmake.in"
    "${CMAKE_CURRENT_BINARY_DIR}/cmake_uninstall.cmake"
    IMMEDIATE @ONLY)

add_custom_target(uninstall
    COMMAND ${CMAKE_COMMAND} -P ${CMAKE_CURRENT_BINARY_DIR}/cmake_uninstall.cmake)
