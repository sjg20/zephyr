# SPDX-License-Identifier: Apache-2.0

cmake_minimum_required(VERSION 3.13.1)

include($ENV{ZEPHYR_BASE}/cmake/app/boilerplate.cmake NO_POLICY_SCOPE)
project(sockets_echo_server)

target_sources(                     app PRIVATE src/echo-server.c)
target_sources_ifdef(CONFIG_NET_UDP app PRIVATE src/udp.c)
target_sources_ifdef(CONFIG_NET_TCP app PRIVATE src/tcp.c)
target_sources_ifdef(CONFIG_NET_VLAN app PRIVATE src/vlan.c)

include($ENV{ZEPHYR_BASE}/samples/net/common/common.cmake)

set(gen_dir ${ZEPHYR_BINARY_DIR}/include/generated/)

foreach(inc_file
	echo-apps-cert.der
	echo-apps-key.der
    )
  generate_inc_file_for_target(
    app
    src/${inc_file}
    ${gen_dir}/${inc_file}.inc
    )
endforeach()
