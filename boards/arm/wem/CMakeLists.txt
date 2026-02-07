board_runner_args(nrfjprog "--nrf-family=NRF52")
include(${ZEPHYR_BASE}/boards/common/nrfjprog.board.cmake)
zephyr_include_directories(.)