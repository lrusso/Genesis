cmake_minimum_required(VERSION 3.5)

include(./macros.cmake)
project(genplus C CXX)

header_directories(./)

add_source_files(
    ./core/z80/z80.c
    ./core/m68k/m68kcpu.c
    ./core/m68k/s68kcpu.c
    ./core/genesis.c
    ./core/vdp_ctrl.c
    ./core/vdp_render.c
    ./core/system.c
    ./core/io_ctrl.c
    ./core/mem68k.c
    ./core/memz80.c
    ./core/membnk.c
    ./core/state.c
    ./core/loadrom.c
    ./core/input_hw/input.c
    ./core/input_hw/gamepad.c
    ./core/input_hw/lightgun.c
    ./core/input_hw/mouse.c
    ./core/input_hw/activator.c
    ./core/input_hw/xe_1ap.c
    ./core/input_hw/teamplayer.c
    ./core/input_hw/paddle.c
    ./core/input_hw/sportspad.c
    ./core/input_hw/terebi_oekaki.c
    ./core/input_hw/graphic_board.c
    ./core/sound/sound.c
    ./core/sound/psg.c
    ./core/sound/ym2413.c
    ./core/sound/ym2612.c
    ./core/sound/ym3438.c
    ./core/sound/blip_buf.c
    ./core/sound/eq.c
    ./core/cart_hw/sram.c
    ./core/cart_hw/svp/svp.c
    ./core/cart_hw/svp/ssp16.c
    ./core/cart_hw/ggenie.c
    ./core/cart_hw/areplay.c
    ./core/cart_hw/eeprom_93c.c
    ./core/cart_hw/eeprom_i2c.c
    ./core/cart_hw/eeprom_spi.c
    ./core/cart_hw/megasd.c
    ./core/cart_hw/flash_cfi.c
    ./core/cart_hw/yx5200.c
    ./core/cart_hw/md_cart.c
    ./core/cart_hw/sms_cart.c
    ./core/cd_hw/cd_cart.c
    ./core/cd_hw/cdc.c
    ./core/cd_hw/cdd.c
    ./core/cd_hw/gfx.c
    ./core/cd_hw/pcm.c
    ./core/cd_hw/scd.c
    ./core/ntsc/sms_ntsc.c
    ./core/ntsc/md_ntsc.c
    ./wasm/config.c
    ./wasm/error.c
    ./wasm/fileio.c
    ./wasm/scrc32.c
    ./wasm/wasm.c
)

# source map option
#   # -Oz
#   -g
#   -fdebug-compilation-dir='../'
#   -gseparate-dwarf='../genplus.dbg'

add_compile_flags(C
    -Oz
    -std=gnu11
    -fomit-frame-pointer
    -Wno-strict-aliasing
    -Wno-unused-parameter
    -Wno-unused-function
    -Wno-sign-compare
    -DLSB_FIRST
    -DUSE_32BPP_RENDERING
    -DHAVE_YM3438_CORE
    -DWASM_GENPLUS
    -DALIGN_LONG
)

add_compile_flags(LD
    "-s WASM=0"
    "-s ALLOW_MEMORY_GROWTH=1"
    "-s MODULARIZE=1"
    "-s TOTAL_MEMORY=32MB"
    "-s FILESYSTEM=0"
    "-s EXPORTED_RUNTIME_METHODS=['HEAPU8','HEAPF32','HEAP8','HEAP16','HEAP32']"
    "-s EXPORTED_FUNCTIONS=['_init','_start','_tick','_sound','_wasm_input_update','_get_rom_buffer_ref','_get_frame_buffer_ref','_get_web_audio_l_ref','_get_web_audio_r_ref','_get_input_buffer_ref','_get_state_buffer_ref','_save_state','_load_state','_get_state_size']"
)

set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ../)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ../)
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ../)

add_executable(${PROJECT_NAME} ${SOURCE_FILES})

# Post-build step to transpile with Babel to ES5
add_custom_command(TARGET ${PROJECT_NAME} POST_BUILD
    COMMAND node ./ConverterES5.js ../genplus.js
    WORKING_DIRECTORY ${CMAKE_SOURCE_DIR}
    COMMENT "Transpiling output to ES5 with Babel"
)
