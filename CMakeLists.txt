cmake_minimum_required(VERSION 3.5.0)
project(demo VERSION 0.1)

include (GNUInstallDirs)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

add_executable(demo main.cpp)
install(
    TARGETS demo
    RUNTIME DESTINATION bin
)

install(
    DIRECTORY config
    USE_SOURCE_PERMISSIONS DESTINATION demo
)

# CPack Configuration
set(CPACK_PACKAGING_INSTALL_PREFIX /usr)

set(POSTINST_IN ${CMAKE_CURRENT_LIST_DIR}/debian/postinst.in)
set(POSTINST_OUT ${CMAKE_CURRENT_BINARY_DIR}/postinst)
configure_file(${POSTINST_IN} ${POSTINST_OUT} @ONLY)

set(POSTRM_IN ${CMAKE_CURRENT_LIST_DIR}/debian/postrm.in)
set(POSTRM_OUT ${CMAKE_CURRENT_BINARY_DIR}/postrm)
configure_file(${POSTRM_IN} ${POSTRM_OUT} @ONLY)

set(PRERM_IN ${CMAKE_CURRENT_LIST_DIR}/debian/prerm.in)
set(PRERM_OUT ${CMAKE_CURRENT_BINARY_DIR}/prerm)
configure_file(${PRERM_IN} ${PRERM_OUT} @ONLY)

set(CPACK_DEBIAN_PACKAGE_CONTROL_EXTRA "${POSTINST_OUT};${POSTRM_OUT};${PRERM_OUT}")

set(CPACK_GENERATOR DEB)
set(CPACK_DEBIAN_PACKAGE_NAME "demo")
set(CPACK_DEBIAN_PACKAGE_VERSION 1.0.0)
set(CPACK_DEBIAN_PACKAGE_DESCRIPTION "CPack deb demo")
set(CPACK_DEBIAN_PACKAGE_MAINTAINER "liuxuenetmail@gmail.com")
set(CPACK_PACKAGE_VENDOR "${CPACK_DEBIAN_PACKAGE_MAINTAINER}")
set(CPACK_PACKAGE_CONTACT "liuxuenetmail@gmail.com")
set(CPACK_DEBIAN_PACKAGE_SECTION devel)

message(STATUS "  Package:      ${CPACK_DEBIAN_PACKAGE_NAME}")
message(STATUS "  Version:      ${CPACK_DEBIAN_PACKAGE_VERSION}")
message(STATUS "  Section:      ${CPACK_DEBIAN_PACKAGE_SECTION}")
message(STATUS "  Priority:     ${CPACK_DEBIAN_PACKAGE_PRIORITY}")
message(STATUS "  Architecture: ${CPACK_DEBIAN_PACKAGE_ARCHITECTURE}")
message(STATUS "  Depends:      ${CPACK_DEBIAN_PACKAGE_DEPENDS}")
message(STATUS "  Maintainer:   ${CPACK_DEBIAN_PACKAGE_MAINTAINER}")
message(STATUS "  Description:  ${CPACK_DEBIAN_PACKAGE_DESCRIPTION}")

# -- include cpack
include (CPack)
