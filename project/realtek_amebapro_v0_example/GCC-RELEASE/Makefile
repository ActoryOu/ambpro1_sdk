.DEFAULT_GOAL=all
.PHONY: mp
mp: ram_lp ram_mp
.PHONY: all
all: ram_lp ram_is

OS := $(shell uname)
.PRECIOUS: %.o %.d %.i
	
.PHONY: toolchain
toolchain:
ifeq ($(findstring CYGWIN, $(OS)), CYGWIN)
	if [ ! -d ../../../tools/arm-none-eabi-gcc/asdk ] ; then mkdir ../../../tools/arm-none-eabi-gcc/asdk; fi;\
	if [ ! -d ../../../tools/arm-none-eabi-gcc/asdk/cygwin ] ; then cat ../../../tools/arm-none-eabi-gcc/asdk-6.4.1-cygwin-newlib-build-2778-i686.tar.bz2* > ../../../tools/arm-none-eabi-gcc/asdk-6.4.1-cygwin-newlib-build-2778-i686.tar.bz2 ; tar -jxf ../../../tools/arm-none-eabi-gcc/asdk-6.4.1-cygwin-newlib-build-2778-i686.tar.bz2 -C ../../../tools/arm-none-eabi-gcc/ ; mv ../../../tools/arm-none-eabi-gcc/asdk-6.4.1/cygwin ../../../tools/arm-none-eabi-gcc/asdk/cygwin ;fi
endif

ifeq ($(findstring MINGW32, $(OS)), MINGW32)
	if [ ! -f /bin/unzip.exe ] ; then mingw-get.exe install msys-unzip; fi;\
	if [ ! -d ../../../tools/arm-none-eabi-gcc/asdk ] ; then mkdir ../../../tools/arm-none-eabi-gcc/asdk; fi;\
	if [ ! -d ../../../tools/arm-none-eabi-gcc/asdk/mingw32 ] ; then unzip -q -u ../../../tools/arm-none-eabi-gcc/asdk-5.5.1-mingw32-newlib-build-2754.zip ; mv asdk-5.5.1/mingw32 ../../../tools/arm-none-eabi-gcc/asdk/mingw32 ; rm -rf asdk-5.5.1 ;fi
endif

ifeq ($(findstring Linux, $(OS)), Linux)
	if [ ! -d ../../../tools/arm-none-eabi-gcc/asdk ] ; then mkdir ../../../tools/arm-none-eabi-gcc/asdk; fi;\
	if [ ! -d ../../../tools/arm-none-eabi-gcc/asdk/linux ] ; then cat ../../../tools/arm-none-eabi-gcc/asdk-6.4.1-linux-newlib-build-3026-x86_64.tar.bz2* > ../../../tools/arm-none-eabi-gcc/asdk-6.4.1-linux-newlib-build-3026-x86_64.tar.bz2 ; tar -jxf ../../../tools/arm-none-eabi-gcc/asdk-6.4.1-linux-newlib-build-3026-x86_64.tar.bz2 -C ../../../tools/arm-none-eabi-gcc/ ; mv ../../../tools/arm-none-eabi-gcc/asdk-6.4.1/linux ../../../tools/arm-none-eabi-gcc/asdk/linux ; rmdir ../../../tools/arm-none-eabi-gcc/asdk-6.4.1; fi
endif
.PHONY: ram_is
ram_is: toolchain
	@$(MAKE) -f lib_webrtc_pic.mk all
	@$(MAKE) -f lib_webrtc_producer_c.mk all
	@$(MAKE) -f libsrtp2.mk all
	@$(MAKE) -f libwebsockets.mk all
	@$(MAKE) -f lib_kvs_webrtc.mk all
	@$(MAKE) -f lib_kvs_producer.mk all
	@$(MAKE) -f lib_amazon_LTS.mk all
	rm -f ../../../component/soc/realtek/8195b/app/shell/cmd_shell.o ../../../component/soc/realtek/8195b/app/shell/cmd_shell.o ../../../component/soc/realtek/8195b/app/shell/cmd_shell.su
	@$(MAKE) -f application.is.mk all

.PHONY: kvs_lib
kvs_lib:
	@$(MAKE) -f lib_webrtc_pic.mk all -j4
	@$(MAKE) -f lib_webrtc_producer_c.mk all -j4
	@$(MAKE) -f libsrtp2.mk all -j4
	@$(MAKE) -f libwebsockets.mk all -j4
	@$(MAKE) -f lib_kvs_webrtc.mk all -j4
	@$(MAKE) -f lib_kvs_producer.mk all -j4
	@$(MAKE) -f lib_amazon_LTS.mk all -j4

.PHONY: ram_mp
ram_mp: toolchain
	@$(MAKE) -f application.is.mk clean mp

.PHONY: ram_lp
ram_lp: toolchain
	@$(MAKE) -f application.lp.mk clean
	@$(MAKE) -f application.lp.mk all

.PHONY: clean
clean:
	@$(MAKE) -f application.lp.mk clean
	@$(MAKE) -f application.is.mk clean
	@$(MAKE) -f lib_webrtc_pic.mk clean
	@$(MAKE) -f lib_webrtc_producer_c.mk clean
	@$(MAKE) -f libsrtp2.mk clean
	@$(MAKE) -f libwebsockets.mk clean
	@$(MAKE) -f lib_kvs_producer.mk clean
	@$(MAKE) -f lib_kvs_webrtc.mk clean
	@$(MAKE) -f lib_amazon_LTS.mk clean

.PHONY: clean_lp
clean_lp:
	@$(MAKE) -f application.lp.mk clean
	
.PHONY: clean_is
clean_is:
	@$(MAKE) -f application.is.mk clean


.PHONY: flash debug setup
setup:
	@$(MAKE) -f application.is.mk $(MAKECMDGOALS)

flash: toolchain
	@$(MAKE) -f application.is.mk flashburn

debug: toolchain
	@$(MAKE) -f application.is.mk debug