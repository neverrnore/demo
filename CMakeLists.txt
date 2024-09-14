
enable_language(ASM)

set(CMAKE_SYSTEM_NAME Linux)
#set(TOOLCHAIN_PATH /opt/pulpino/bin)

PROJECT(gprs_asic CXX C)
cmake_minimum_required(VERSION 3.5)

########################################################################
# Options
########################################################################
option(DEBUG_ENABLE    		        "compile to debug "                      OFF)
#DEMO
option(EAT_TEST    		            "compile to test eat"                    OFF)
option(TCP_DEMO1    		        "compile to tcp demo1"                   OFF)
option(HTTP_DEMO2    		        "compile to http demo2"                  OFF)
option(MQTT_DEMO3    		        "compile to mqtt demo3"                  OFF)
option(FTP_DEMO4    		        "compile to ftp demo4"                   OFF)
option(SMS_DEMO5    		        "compile to sms demo5"                   OFF)
option(MODE_SWITCH_DEMO6    		"compile to switch demo6"                OFF)
option(APP_DEMO7    		        "compile to app demo7"                   OFF)
option(APP_DEMO8    		        "compile to app demo8"                   OFF)
option(APP_DEMO9    		        "compile to app demo9"                   OFF)
option(APP_DEMO10    		        "compile to app demo10"                  OFF)



########################################################################
# compiler options   maybe modify
########################################################################
set(CMAKE_C_COMPILER /opt/pulpino/bin/riscv32-unknown-elf-gcc)
set(CMAKE_CXX_COMPILER /opt/pulpino/bin/riscv32-unknown-elf-g++)
#set link tool
set(CMAKE_AR /opt/pulpino/bin/riscv32-unknown-elf-ar)

#CMAKE_C_FLAGS //-ffunction-sections -fdata-sections
set(CMAKE_C_FLAGS "-ffunction-sections -fdata-sections -Os -Wall -march=rv32imfc -falign-functions=1 -D _ASIC_ -D UC_DEBUG -D MEM_OPT -D _FPGA_ -D H_LO=1 -D L_LO=0 -D LO_TYPE=H_LO -D IF_VALUE=100 -D HB_COE=1 -D FILTER_TAG=240 -D __AFC_SWITCH__ -D _HW_ROM_ -D _RACH_MOD_ -D _ON_WATCH_ -D _RF_CALI_SWITCH_")

if(DEBUG_ENABLE)
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -D _ALLOW_TRACE_PRITF_TEST_")
endif(DEBUG_ENABLE)

set(elf_name  "ASIC")

set(gprs_lib ${CMAKE_SOURCE_DIR}/PS/lib/lib_gprs/libGprs.a)

########################################################################
# demo contrl
########################################################################
if(TCP_DEMO1)
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -D TCP_DEMO1")
endif(TCP_DEMO1)

if(HTTP_DEMO2)
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -D HTTP_DEMO2")
endif(HTTP_DEMO2)

if(MQTT_DEMO3)
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -D MQTT_DEMO3")
endif(MQTT_DEMO3)

if(FTP_DEMO4)
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -D FTP_DEMO4")
endif(FTP_DEMO4)

if(SMS_DEMO5)
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -D SMS_DEMO5")
endif(SMS_DEMO5)

if(MODE_SWITCH_DEMO6)
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -D MODE_SWITCH_DEMO6")
endif(MODE_SWITCH_DEMO6)

if(APP_DEMO7)
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -D APP_DEMO7")
endif(APP_DEMO7)

if(APP_DEMO8)
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -D APP_DEMO8")
endif(APP_DEMO8)

if(APP_DEMO9)
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -D APP_DEMO9")
endif(APP_DEMO9)

if(APP_DEMO10)
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -D APP_DEMO10")
endif(APP_DEMO10)


#CMAKE_ASM_COMPILER
set(CMAKE_ASM_COMPILER ${CMAKE_C_COMPILER})

#set publik link //-Wl,--gc-sections
SET(CMAKE_EXE_LINKER_FLAGS "-nostartfiles -Wl,--gc-sections -T${CMAKE_SOURCE_DIR}/arch/asic/link/link.flash.ld")

message("-->gprs compiler:${CMAKE_C_COMPILER}")
message("-->asm:${CMAKE_ASM_COMPILER}  ar:${CMAKE_AR}")

########################################################################
# public head file   maybe modify
########################################################################
SET(pub_inc
    ./
    ../../driver/inc
    ../../OS/freertos/config
    ../../OS/freertos/src/include
    ../../OS/freertos/freertos_tcp/include
    ../../OS/freertos/freertos_tcp/portable/Compiler/GCC
    ../../OS/freertos/freertos_iot/demo/https/http_plain_text
    ../../OS/freertos/freertos_iot/abstractions/platform/freertos/include
    ../../OS/freertos/freertos_iot/c_sdk/standard/https/include
    ../../OS/freertos/freertos_iot/c_sdk/platform
    ../../OS/freertos/freertos_iot/c_sdk/standard/common/include
    ../../OS/freertos/freertos_iot/c_sdk
    ../../OS/freertos/freertos_iot/c_sdk/standard/http-parser
    ../../OS/freertos/freertos_iot/abstractions/platform/freertos/include
    ../../OS/freertos/freertos_iot/abstractions/mbedtls
    ../../OS/freertos/freertos_iot/c_sdk/standard/mbedtls/include
    ../../OS/freertos/freertos_iot/c_sdk/standard/mbedtls/include/mbedtls
    ../../OS/freertos/freertos_iot/c_sdk/standard/mqtt/include
    ../../OS/freertos/freertos_iot/c_sdk/standard/mqtt/src
    ../../platform/trace/TraceRecorder/include
    ../../platform/include
    ../../platform/msg
    ../../PS/gprs/cc
    ../../PS/gprs/dl
    ../../PS/gprs/common
    ../../PS/gprs/common/includes
    ../../PS/gprs/inc
    ../../PS/gprs/inc/com
    ../../PS/gprs/frame
    ../../PS/gprs/common/os_inc
    ../../PS/gprs/inc/cdg
    ../../PS/gprs/llc
    ../../PS/gprs/gea34
    ../../PS/gprs/cci
    ../../PS/gprs/mm
    ../../PS/gprs/sndcp
    ../../PS/gprs/ss
    ../../PS/gprs/rr
    ../../PS/gprs/dti
    ../../PS/gprs/til_alr
    ../../PS/gprs/l1/include
    ../../PS/gprs/sms
    ../../PS/app/net_service/inc
    ../../PS/app/at
    ../../PS/gprs/psa
    ../../PS/cce
    ../../PS/app/ppp
    ../../PS/app/mng
    ../../PS/app/trx
    ../../PS/app/gps
    ../../PS/app/led
    ../../PS/test/stat
    ../../PS/app/ftp
    ../../PS/app/eat
    ../../PS/platform/msg
    ../../PS/platform/trace/TraceRecorder/include
    ../../PS/platform/trace/config
    ../../PS/speech
    ../../PS/gprs/usim/inc
    ../../PS/gprs/usim/driver/inc
    ../../PS/gprs/usim/test
    ../../PS/gprs/usim
    ../../PS/gprs/usim/debug
    ../../PS/lpm/inc
    ../../PS/platform/inc
)

if(EAT_TEST)
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -D EAT_TEST")
    set(pub_inc "${pub_inc};
    ../../test_demo/ftp;
    ../../test_demo/gprs;
    ../../test_demo/http;
    ../../test_demo/main;
    ../../test_demo/mqtt;
    ../../test_demo/sms;
    ../../test_demo/gps;
    ../../test_demo/tcp")
    ADD_SUBDIRECTORY(test_demo)
endif(EAT_TEST)

message("+++++CMAKE_C_FLAGS:${CMAKE_C_FLAGS}")
message("+++++pub_inc:${pub_inc}")

### add compiler dir
ADD_SUBDIRECTORY(arch)
ADD_SUBDIRECTORY(driver)
ADD_SUBDIRECTORY(OS)
ADD_SUBDIRECTORY(platform)
ADD_SUBDIRECTORY(solution)
