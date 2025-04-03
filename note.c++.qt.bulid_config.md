---
id: 3y14wn1koi5hdsh8acj77ko
title: Bulid_config
desc: ''
updated: 1742959324350
created: 1742959247481
---

### .pro文件中设置的c++版本flag没有生效

```bash
# CONFIG += c++11
QMAKE_CXXFLAGS += -g  
QMAKE_CXXFLAGS += -std=c++11
# QMAKE_CXXFLAGS += -w
# QMAKE_CXXFLAGS += std=c++11
# QMAKE_CXXFLAGS += -DRKAIQ
QMAKE_CFLAGS += -w


cd target/o_rkmedia_muxer_test/ && ( test -e Makefile.rkmedia_muxer_test || /home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/bin/qmake -o Makefile.rkmedia_muxer_test /home/xjf1127/codespace/git_down/new_aplus/test/takephoto/src/target/o_rkmedia_muxer_test/rkmedia_muxer_test.pro -spec devices/linux-buildroot-g++ CONFIG+=debug CONFIG+=qml_debug ) && /usr/bin/make -f Makefile.rkmedia_muxer_test 
make[1]: Entering directory '/home/xjf1127/codespace/git_down/new_aplus/test/takephoto/build-test_root-rk1126_arm_linux-Debug/target/o_rkmedia_muxer_test'
/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/compiler2/gcc-arm-8.3-2019.03-x86_64-arm-linux-gnueabihf/bin/arm-linux-gnueabihf-gcc -c -pipe -D_LARGEFILE_SOURCE -D_LARGEFILE64_SOURCE -D_FILE_OFFSET_BITS=64 -Os -DUSE_UPDATEENGINE=ON -DSUCCESSFUL_BOOT=ON -mfpu=neon --sysroot=/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/buildroot/host/arm-buildroot-linux-gnueabihf/sysroot -w -Wall -Wextra -D_REENTRANT -fPIC -DQT_DEPRECATED_WARNINGS -DRKAIQ -DQT_QML_DEBUG -DQT_WIDGETS_LIB -DQT_GUI_LIB -DQT_NETWORK_LIB -DQT_CORE_LIB -I../../../src/target/o_rkmedia_muxer_test -I. -I/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/compiler2/gcc-arm-8.3-2019.03-x86_64-arm-linux-gnueabihf/arm-linux-gnueabihf/libc/usr/include -I/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/compiler2/gcc-arm-8.3-2019.03-x86_64-arm-linux-gnueabihf/arm-linux-gnueabihf/include/c++/8.3.0 -I/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/package/rockx/sdk/rockx-rk1806-Linux/include -I/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/package/rkmedia/include/rkmedia -I/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/package/rkmedia/examples/common -I/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/package/camera_engine_rkaiq-1.0/include/uAPI -I/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/package/camera_engine_rkaiq-1.0/include/xcore -I/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/package/camera_engine_rkaiq-1.0/include/common -I/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/package/camera_engine_rkaiq-1.0/include/algos -I/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/package/camera_engine_rkaiq-1.0/include/ipc_server -I/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/package/camera_engine_rkaiq-1.0/include/iq_parser -I../../../src/module/rkmedia -I../../../src/module/rkmedia/common -I../../../../rkmedia/include/easymedia -I../../../src/target/o_rkmedia_muxer_test -I/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/include/qt5 -I/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/include/qt5/QtWidgets -I/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/include/qt5/QtGui -I/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/include/qt5/QtNetwork -I/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/include/qt5/QtCore -I. -I/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/mkspecs/devices/linux-buildroot-g++ -o rkmedia_muxer_test.o ../../../src/target/o_rkmedia_muxer_test/rkmedia_muxer_test.c
In file included from /home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/compiler2/gcc-arm-8.3-2019.03-x86_64-arm-linux-gnueabihf/arm-linux-gnueabihf/include/c++/8.3.0/atomic:38,
                 from ../../../../rkmedia/include/easymedia/lock.h:11,
                 from ../../../../rkmedia/include/easymedia/flow.h:8,
                 from ../../../../rkmedia/include/easymedia/media_config.h:8,
                 from ../../../../rkmedia/include/easymedia/muxer.h:8,
                 from ../../../src/target/o_rkmedia_muxer_test/rkmedia_muxer_test.c:20:
/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/compiler2/gcc-arm-8.3-2019.03-x86_64-arm-linux-gnueabihf/arm-linux-gnueabihf/include/c++/8.3.0/bits/c++0x_warning.h:32:2: error: #error This file requires compiler and library support for the ISO C++ 2011 standard. This support must be enabled with the -std=c++11 or -std=gnu++11 compiler options.
 #error This file requires compiler and library support \


/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/compiler2/gcc-arm-8.3-2019.03-x86_64-arm-linux-gnueabihf/arm-linux-gnueabihf/include/c++/8.3.0/bits/c++0x_warning.h:32: error: #error This file requires compiler and library support for the ISO C++ 2011 standard. This support must be enabled with the -std=c++11 or -std=gnu++11 compiler options.
In file included from /home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/compiler2/gcc-arm-8.3-2019.03-x86_64-arm-linux-gnueabihf/arm-linux-gnueabihf/include/c++/8.3.0/atomic:38,
                 from ../../../../rkmedia/include/easymedia/lock.h:11,
                 from ../../../../rkmedia/include/easymedia/flow.h:8,
                 from ../../../../rkmedia/include/easymedia/media_config.h:8,
                 from ../../../../rkmedia/include/easymedia/muxer.h:8,
                 from ../../../src/target/o_rkmedia_muxer_test/rkmedia_muxer_test.c:20:
/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/compiler2/gcc-arm-8.3-2019.03-x86_64-arm-linux-gnueabihf/arm-linux-gnueabihf/include/c++/8.3.0/bits/c++0x_warning.h:32:2: error: #error This file requires compiler and library support for the ISO C++ 2011 standard. This support must be enabled with the -std=c++11 or -std=gnu++11 compiler options.
 #error This file requires compiler and library support \
  ^~~~~
```