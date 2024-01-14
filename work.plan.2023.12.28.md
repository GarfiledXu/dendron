---
id: xu0ifry2kdf87jfi6r6507h
title: '28'
desc: ''
updated: 1704286040817
created: 1703725512250
---

##### must
- [x] 打包 collection tool
- [x] objective_service
- [ ] AVM testapp
  - [x] 点亮设备
  - [x] 运行AVMdemo
  - [ ] 查看screen



QNX:
busybox telnet 192.168.118.2
root
fGh4dalvHp4ubmb2
mount -uw /apps

##### FX11 海外 note
- push exe file from android to qnx

        adb push \\wsl$\Ubuntu\home\xjf2613\code-space\qnx\pe-qnx_render_sample\out\avm_sample\avm_sample /data 
        adb shell mv /data/avm_sample /data/vendor/nfs/nfs_ota 
        adb shell busybox telnet -l root 192.168.118.2
    
        root

        fGh4dalvHp4ubmb2
        
        mv /otaupdate/avm_sample /apps/cluster/FX11_HEV/bin/

        export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/apps/cluster/FX11_HEV/lib/
        
        cd /apps/cluster/FX11_HEV/bin/ && chmod 777 * && ./avm_sample

- push lib file from android to qnx

        adb push \\wsl$\Ubuntu\home\xjf2613\code-space\qnx\pe-qnx_render_sample\src\module\m_avm_sdk\lib\libarcsoft_avm_sdk.so /data && adb shell mv /data/libarcsoft_avm_sdk.so /data/vendor/nfs/nfs_ota && adb shell busybox telnet -l root 192.168.118.2
    
        root

        fGh4dalvHp4ubmb2

        mv /otaupdate/libarcsoft_avm_sdk.so /apps/cluster/FX11_HEV/lib/

        export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/apps/cluster/FX11_HEV/lib/
- push screen sample
adb root
adb remount

        adb push \\wsl$\Ubuntu\home\xjf2613\code-space\qnx\pe-qnx_render_sample\out\screen_sample\screen_sample /data 
        
        adb shell
        
        mv /data/screen_sample   /data/vendor/nfs/nfs_ota 

        busybox telnet -l root 192.168.118.2
    
        root

        fGh4dalvHp4ubmb2
        
        mv /otaupdate/screen_sample /apps/cluster/FX11_HEV/bin/

        export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/apps/cluster/FX11_HEV/lib/
        
        cd /apps/cluster/FX11_HEV/bin/ && chmod 777 * && ./screen_sample