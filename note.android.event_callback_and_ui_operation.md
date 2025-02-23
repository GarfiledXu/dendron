---
id: s6fs6m8x6p0ll4eqddemie4
title: Event_callback_and_ui_operation
desc: ''
updated: 1717323203469
created: 1717320638311
---

#### scene
本意是实现按钮点击后置灰，完成点击回调中的事情后再置正常
```java
mBinding.buttonAutoCalib.setOnClickListener(v -> {

            //handler.post(() -> {
            ////Thread curThread = new Thread(() -> {
            //    try{
            //        sleep(2000);
            //        ArrayList<String> inputList = new ArrayList<>();
            //        inputList.add(mAutoCalibFileDir + "/auto_calib_image_front_1280x720.NV21");
            //        inputList.add(mAutoCalibFileDir + "/auto_calib_image_right_1280x720.NV21");
            //        inputList.add(mAutoCalibFileDir + "/auto_calib_image_left_1280x720.NV21");
            //        ArcAvmSdkBase.ARC_ImageInfo_t[] inputImages= prepareImage(inputList);
            //        if(inputImages == null){
            //            Log.e(TAG, "read image is null");
            //            return;
            //        }
            //        int ret = mAvmSdkLogic.autoAvmCalib(inputImages, new ArcAvmSdkBase.ARC_AVM_ChessInfo_t()
            //                , mAutoCalibResultData, mAutoCalibLookupTable);
            //        if(ret!=0){
            //            mAutoCalibString =  "auto calib fail, ret:" + ret;
            //            mBinding.textOutAutoCalibFilePath.setText(mAutoCalibString);
            //        }
            //        else {
            //            mAutoCalibString =  "auto calib success, ret:" + ret;
            //            mBinding.textOutAutoCalibFilePath.setText(mAutoCalibString);
            //        }
            //    }catch (Exception e){
            //        Log.e(TAG, "will exception what: " + e.getMessage());
            //    }
            //    finally {
            //
            //    }
            //
            //});
            handler.post(() -> {
                mBinding.buttonAutoCalib.setEnabled(false);
                mBinding.buttonMaunalCalib.setEnabled(false);
            });
            handler.post(() -> {
                try {
                    sleep(3000);
                } catch (InterruptedException e) {
                    throw new RuntimeException(e);
                }
                //handler.post(() -> {
                    mBinding.buttonAutoCalib.setEnabled(true);
                    mBinding.buttonMaunalCalib.setEnabled(true);
                //});
            });




            //});
            //curThread.start();
        });
    }
```
看现象是 button 的属性设置被覆盖了，只有onclick中最后一个才生效
猜测：在点击事件生效后，会有一个流程组不可被打断: ui更新1，click事件回调 ui 更新2 执行之前所有的的message，导致回调中的ui操作效果上看来就只有最后一条生效