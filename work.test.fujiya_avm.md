---
id: dkxk7lbf2qaw4ypf943y5l8
title: Fujiya_avm
desc: ''
updated: 1694622581119
created: 1694574090783
---

#### result
----------------
##### checklicense
```c++
TEST(checklicense, device_online) {
    CallSys call_offline{"svc wifi enable"};
    call_offline.execute();
    if (!call_offline.if_return_success()) {
      throw std::logic_error("can't start the network!");
    }
    fs::remove_all(fs::path("tmp"));
    fs::path tmp_normal = "tmp/normal";
    fs::path tmp_empty= "tmp/empty";
    fs::path tmp_abnormal = "tmp/abnormal";
    fs::create_directories(tmp_normal);
    fs::create_directories(tmp_empty);
    fs::create_directories(tmp_abnormal);
    fs::remove(tmp_empty);
    fs::ofstream tmp_ab_file(tmp_abnormal/G_LICENSE_FILE_NAME);
    tmp_ab_file << "empty file a" << std::endl;
    tmp_ab_file.close();
    fs::copy((const char*)G_NORMAL_LICENSE_PATH, tmp_normal, fs::copy_options::recursive);
    //normal 
    EXPECT_EQ(0, AVMCheckLicense((uint8_t*)tmp_normal.c_str(), G_LICENSE_ID_CALLBACK));
    //file content error
    EXPECT_EQ(0, AVMCheckLicense((uint8_t*)tmp_abnormal.c_str(), G_LICENSE_ID_CALLBACK));
    //dir empty
    EXPECT_NE(0, AVMCheckLicense((uint8_t*)tmp_empty.c_str(), G_LICENSE_ID_CALLBACK));
}
```
> 第三个断言失败，当license文件内容非法时，并没有清空重新申请，而是返回错误码
> 需确认该逻辑是否为当前sdk正确逻辑

### set init param
- callback 为null时，不崩溃，但返回值正常
- initparam未做参数合法性验证

### set state mode
```c++
/home/xjf2613/code-space/android/pe-test_fuliya_avm/src/target/o_api_test2/single/set_state_mode.hpp:24: Failure
Expected equality of these values:
  ret
    Which is: 3
  0
/home/xjf2613/code-space/android/pe-test_fuliya_avm/src/target/o_api_test2/single/set_state_mode.hpp:30: Failure
Expected equality of these values:
  ret
    Which is: 5
  0

[==========] 11 tests from 2 test suites ran. (1651 ms total)
[  PASSED  ] 9 tests.
[  FAILED  ] 2 tests, listed below:
[  FAILED  ] set_state_mode1.param_normal/0, where GetParam() = (0)
[  FAILED  ] set_state_mode1.param_normal/6, where GetParam() = (6)

```
修改代码
```c++
//init
    ret = setAVMInitParam(&param, init_callback);
    EXPECT_EQ(ret, 0);
    if (ret) {
        return;
    }
    //state
    ret = setAVMStateMode(GETTP(0));
    if (GETTP(0) == AVM_VIEW_MODE_NO_REQUEST) {
        EXPECT_NE(ret, 0);
        return;
    }
    EXPECT_EQ(ret, 0);
    if (GETTP(0) == AVM_EXIT_VIEW_MODE_NONE) {
        return;
    }
```
all pass

#### set view mode
---------
all pass


#### set frame
--------
- normal param pass
- null data, not pass, fail!

#### set_undistortion

- all pass
  
#### calib
------------
```c++
[----------] Global test environment tear-down
[==========] 6 tests from 1 test suite ran. (44688 ms total)
[  PASSED  ] 2 tests.
[  FAILED  ] 4 tests, listed below:
[  FAILED  ] calib.normal_param
[  FAILED  ] calib.bin_path_null
[  FAILED  ] calib.bin_path_not_exist
[  FAILED  ] calib.version_normal
```

#### similar
- similar_set_car_icon pass
- similar_set_dynamic_pgs_mode pass
- similar_gear_state pass
- similar_set_pause_state two fail
  ```c++
  [DBUG][65:io_read_bin_from_file]read binary file:./t1_5120x960_X.yuyv, file size:9830400
atelier: /vendor/etc/Assets///license_plate///licenseplate_background.tga open faile
/home/xjf2613/code-space/android/pe-test_fuliya_avm/src/target/o_api_test2/similar/set_pause_state.hpp:39: Failure
Expected: (cur_ret) != (0), actual: 0 vs 0
[  FAILED  ] similar_set_pause_state2.abnormal/0, where GetParam() = (-2) (3345 ms)
[ RUN      ] similar_set_pause_state2.abnormal/1
[DBUG][65:io_read_bin_from_file]read binary file:./t1_5120x960_X.yuyv, file size:9830400
atelier: /vendor/etc/Assets///license_plate///licenseplate_background.tga open faile
/home/xjf2613/code-space/android/pe-test_fuliya_avm/src/target/o_api_test2/similar/set_pause_state.hpp:39: Failure
Expected: (cur_ret) != (0), actual: 0 vs 0
[  FAILED  ] similar_set_pause_state2.abnormal/1, where GetParam() = (100) (3338 ms)
[----------] 2 tests from similar_set_pause_state2 (6685 ms total)

[----------] Global test environment tear-down
[==========] 4 tests from 2 test suites ran. (11559 ms total)
[  PASSED  ] 2 tests.
[  FAILED  ] 2 tests, listed below:
[  FAILED  ] similar_set_pause_state2.abnormal/0, where GetParam() = (-2)
[  FAILED  ] similar_set_pause_state2.abnormal/1, where GetParam() = (100)
```

- similar_avm_sdk_steeringangle pass
- similar_avm_sdk_steeringangle
```c++
[DBUG][65:io_read_bin_from_file]read binary file:./t1_5120x960_X.yuyv, file size:9830400
atelier: /vendor/etc/Assets///license_plate///licenseplate_background.tga open faile
/home/xjf2613/code-space/android/pe-test_fuliya_avm/src/target/o_api_test2/similar/set_steering_angle.hpp:46: Failure
Expected: (cur_ret) != (0), actual: 0 vs 0
[  FAILED  ] similar_avm_sdk_steeringangle_2.abnormal/0, where GetParam() = (-1, -1, -1) (2281 ms)
[ RUN      ] similar_avm_sdk_steeringangle_2.abnormal/1
[DBUG][65:io_read_bin_from_file]read binary file:./t1_5120x960_X.yuyv, file size:9830400
atelier: /vendor/etc/Assets///license_plate///licenseplate_background.tga open faile
/home/xjf2613/code-space/android/pe-test_fuliya_avm/src/target/o_api_test2/similar/set_steering_angle.hpp:46: Failure
Expected: (cur_ret) != (0), actual: 0 vs 0
[  FAILED  ] similar_avm_sdk_steeringangle_2.abnormal/1, where GetParam() = (-1, -1, -2) (2285 ms)
[ RUN      ] similar_avm_sdk_steeringangle_2.abnormal/2
[DBUG][65:io_read_bin_from_file]read binary file:./t1_5120x960_X.yuyv, file size:9830400
atelier: /vendor/etc/Assets///license_plate///licenseplate_background.tga open faile
/home/xjf2613/code-space/android/pe-test_fuliya_avm/src/target/o_api_test2/similar/set_steering_angle.hpp:46: Failure
Expected: (cur_ret) != (0), actual: 0 vs 0
[  FAILED  ] similar_avm_sdk_steeringangle_2.abnormal/2, where GetParam() = (-1, -2, -1) (2285 ms)
[ RUN      ] similar_avm_sdk_steeringangle_2.abnormal/3
[DBUG][65:io_read_bin_from_file]read binary file:./t1_5120x960_X.yuyv, file size:9830400
atelier: /vendor/etc/Assets///license_plate///licenseplate_background.tga open faile
/home/xjf2613/code-space/android/pe-test_fuliya_avm/src/target/o_api_test2/similar/set_steering_angle.hpp:46: Failure
Expected: (cur_ret) != (0), actual: 0 vs 0
[  FAILED  ] similar_avm_sdk_steeringangle_2.abnormal/3, where GetParam() = (-1, -2, -2) (2272 ms)
[ RUN      ] similar_avm_sdk_steeringangle_2.abnormal/4
[DBUG][65:io_read_bin_from_file]read binary file:./t1_5120x960_X.yuyv, file size:9830400
atelier: /vendor/etc/Assets///license_plate///licenseplate_background.tga open faile
/home/xjf2613/code-space/android/pe-test_fuliya_avm/src/target/o_api_test2/similar/set_steering_angle.hpp:46: Failure
Expected: (cur_ret) != (0), actual: 0 vs 0
[  FAILED  ] similar_avm_sdk_steeringangle_2.abnormal/4, where GetParam() = (100, -1, -1) (2283 ms)
[ RUN      ] similar_avm_sdk_steeringangle_2.abnormal/5
[DBUG][65:io_read_bin_from_file]read binary file:./t1_5120x960_X.yuyv, file size:9830400
atelier: /vendor/etc/Assets///license_plate///licenseplate_background.tga open faile
/home/xjf2613/code-space/android/pe-test_fuliya_avm/src/target/o_api_test2/similar/set_steering_angle.hpp:46: Failure
Expected: (cur_ret) != (0), actual: 0 vs 0
[  FAILED  ] similar_avm_sdk_steeringangle_2.abnormal/5, where GetParam() = (100, -1, -2) (2280 ms)
[ RUN      ] similar_avm_sdk_steeringangle_2.abnormal/6
[DBUG][65:io_read_bin_from_file]read binary file:./t1_5120x960_X.yuyv, file size:9830400
atelier: /vendor/etc/Assets///license_plate///licenseplate_background.tga open faile
/home/xjf2613/code-space/android/pe-test_fuliya_avm/src/target/o_api_test2/similar/set_steering_angle.hpp:46: Failure
Expected: (cur_ret) != (0), actual: 0 vs 0
[  FAILED  ] similar_avm_sdk_steeringangle_2.abnormal/6, where GetParam() = (100, -2, -1) (2275 ms)
[ RUN      ] similar_avm_sdk_steeringangle_2.abnormal/7
[DBUG][65:io_read_bin_from_file]read binary file:./t1_5120x960_X.yuyv, file size:9830400
atelier: /vendor/etc/Assets///license_plate///licenseplate_background.tga open faile
/home/xjf2613/code-space/android/pe-test_fuliya_avm/src/target/o_api_test2/similar/set_steering_angle.hpp:46: Failure
Expected: (cur_ret) != (0), actual: 0 vs 0
[  FAILED  ] similar_avm_sdk_steeringangle_2.abnormal/7, where GetParam() = (100, -2, -2) (2275 ms)
[----------] 8 tests from similar_avm_sdk_steeringangle_2 (18243 ms total)

[----------] Global test environment tear-down
[==========] 41 tests from 3 test suites ran. (93832 ms total)
[  PASSED  ] 33 tests.
[  FAILED  ] 8 tests, listed below:
[  FAILED  ] similar_avm_sdk_steeringangle_2.abnormal/0, where GetParam() = (-1, -1, -1)
[  FAILED  ] similar_avm_sdk_steeringangle_2.abnormal/1, where GetParam() = (-1, -1, -2)
[  FAILED  ] similar_avm_sdk_steeringangle_2.abnormal/2, where GetParam() = (-1, -2, -1)
[  FAILED  ] similar_avm_sdk_steeringangle_2.abnormal/3, where GetParam() = (-1, -2, -2)
[  FAILED  ] similar_avm_sdk_steeringangle_2.abnormal/4, where GetParam() = (100, -1, -1)
[  FAILED  ] similar_avm_sdk_steeringangle_2.abnormal/5, where GetParam() = (100, -1, -2)
[  FAILED  ] similar_avm_sdk_steeringangle_2.abnormal/6, where GetParam() = (100, -2, -1)
[  FAILED  ] similar_avm_sdk_steeringangle_2.abnormal/7, where GetParam() = (100, -2, -2)

 8 FAILED TESTS
```

- similar_set_trunlight
```c++
[ RUN      ] similar_set_trunlight2.abnormal/0
[DBUG][65:io_read_bin_from_file]read binary file:./t1_5120x960_X.yuyv, file size:9830400
atelier: /vendor/etc/Assets///license_plate///licenseplate_background.tga open faile
/home/xjf2613/code-space/android/pe-test_fuliya_avm/src/target/o_api_test2/similar/set_turnlight_sate.hpp:39: Failure
Expected: (cur_ret) != (0), actual: 0 vs 0
[  FAILED  ] similar_set_trunlight2.abnormal/0, where GetParam() = (0) (3306 ms)
[ RUN      ] similar_set_trunlight2.abnormal/1
[DBUG][65:io_read_bin_from_file]read binary file:./t1_5120x960_X.yuyv, file size:9830400
atelier: /vendor/etc/Assets///license_plate///licenseplate_background.tga open faile
/home/xjf2613/code-space/android/pe-test_fuliya_avm/src/target/o_api_test2/similar/set_turnlight_sate.hpp:39: Failure
Expected: (cur_ret) != (0), actual: 0 vs 0
[  FAILED  ] similar_set_trunlight2.abnormal/1, where GetParam() = (1) (3303 ms)
[ RUN      ] similar_set_trunlight2.abnormal/2
[DBUG][65:io_read_bin_from_file]read binary file:./t1_5120x960_X.yuyv, file size:9830400
atelier: /vendor/etc/Assets///license_plate///licenseplate_background.tga open faile
/home/xjf2613/code-space/android/pe-test_fuliya_avm/src/target/o_api_test2/similar/set_turnlight_sate.hpp:39: Failure
Expected: (cur_ret) != (0), actual: 0 vs 0
[  FAILED  ] similar_set_trunlight2.abnormal/2, where GetParam() = (4) (3277 ms)
[ RUN      ] similar_set_trunlight2.abnormal/3
[DBUG][65:io_read_bin_from_file]read binary file:./t1_5120x960_X.yuyv, file size:9830400
atelier: /vendor/etc/Assets///license_plate///licenseplate_background.tga open faile
/home/xjf2613/code-space/android/pe-test_fuliya_avm/src/target/o_api_test2/similar/set_turnlight_sate.hpp:39: Failure
Expected: (cur_ret) != (0), actual: 0 vs 0
[  FAILED  ] similar_set_trunlight2.abnormal/3, where GetParam() = (5) (3276 ms)
[----------] 4 tests from similar_set_trunlight2 (13163 ms total)

[----------] Global test environment tear-down
[==========] 8 tests from 2 test suites ran. (22562 ms total)
[  PASSED  ] 4 tests.
[  FAILED  ] 4 tests, listed below:
[  FAILED  ] similar_set_trunlight2.abnormal/0, where GetParam() = (0)
[  FAILED  ] similar_set_trunlight2.abnormal/1, where GetParam() = (1)
[  FAILED  ] similar_set_trunlight2.abnormal/2, where GetParam() = (4)
[  FAILED  ] similar_set_trunlight2.abnormal/3, where GetParam() = (5)

 4 FAILED TESTS
- similar_set_wheel_pulse
```
[       OK ] similar_set_wheel_pulse1.normal_param/255 (3269 ms)
[----------] 256 tests from similar_set_wheel_pulse1 (840697 ms total)

[----------] 256 tests from similar_set_wheel_pulse2
[ RUN      ] similar_set_wheel_pulse2.abnormal/0

[DBUG][65:io_read_bin_from_file]read binary file:./t1_5120x960_X.yuyv, file size:9830400
atelier: /vendor/etc/Assets///license_plate///licenseplate_background.tga open faile
/home/xjf2613/code-space/android/pe-test_fuliya_avm/src/target/o_api_test2/similar/set_wheel_pulse.hpp:72: Failure
Expected: (cur_ret) != (0), actual: 0 vs 0
[  FAILED  ] similar_set_wheel_pulse2.abnormal/29, where GetParam() = (-100, -100, -100, -1, -1, -1, -100, -1) (3297 ms)
[ RUN      ] similar_set_wheel_pulse2.abnormal/30
[DBUG][65:io_read_bin_from_file]read binary file:./t1_5120x960_X.yuyv, file size:9830400
atelier: /vendor/etc/Assets///license_plate///license

[       OK ] similar_set_wheel_pulse3.null (2736 ms)
[----------] 1 test from similar_set_wheel_pulse3 (2736 ms total)

[----------] Global test environment tear-down
[==========] 1 test from 1 test suite ran. (2737 ms total)
[  PASSED  ] 1 test.

- similar_set_wheel_speed


top
 PID USER         PR  NI VIRT  RES  SHR S[%CPU] %MEM     TIME+ ARGS
  592 root         20   0  54M 200K 200K S  100   0.0 281:21.71 smartplatformserver
 8344 system       20   0 7.8G 8.2M 8.2M S 22.6   0.1  65:36.24 com.iflytek.cutefly.speechclient.hmi
 7113 audioserver  20   0  53M 212K 212K S 11.0   0.0  29:08.66 android.hardware.audio@5.0-service-mediatek
