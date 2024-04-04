---
id: kry5qx2lhech69tpqe2o6tt
title: Coredump_and_gdb
desc: ''
updated: 1709286546920
created: 1709279810797
---

#### record
```bash
#1. 在当前目录，source sdp目录下的环境变量脚本
#2. 直接使用命令 ntoaarch64-gdb ./objective_service_qnx  先加载程序
#3. 然后在gdb中输入命令 (gdb) core-file /home/xjf2613/code-space/qnx/pe-board_service/gdb/objective_service_qnx.4.core
#4. 输入 bt 发现提示Backtrace stopped: previous frame identical to this frame (corrupt stack?) 损坏
#使用(gdb)  x /50a $sp，    
(gdb) x /100a $sp
0xce4ac4360:    0xce4ac43c0     0x19af542754
0xce4ac4370:    0xf568ad140     0x19af5b9000
0xce4ac4380:    0x19af5b9000    0xf568ad150
0xce4ac4390:    0x1     0x19af57cff8
0xce4ac43a0:    0x39323131      0x19af5674f0
0xce4ac43b0:    0x19af5bab38    0xed00b9f85df6f68d
0xce4ac43c0:    0xce4ac4400     0x19af544c30
0xce4ac43d0:    0xf568ad8a8     0x19af5b9000
0xce4ac43e0:    0x19af5b9000    0xf568ad150
0xce4ac43f0:    0x1     0xf560aa729
0xce4ac4400:    0xce4ac4440     0x19b02fdfc8 <_ZNSt3__112basic_stringIcNS_11char_traitsIcEENS_9allocatorIcEEE7reserveEm+272>
0xce4ac4410:    0x16    0x0
0xce4ac4420:    0xf560aa728     0x0
0xce4ac4430:    0x1     0xf560aa729
0xce4ac4440:    0xce4ac4490     0x12c39386f4 <TestbedCmdParser::push_buitin_var_value<std::__1::basic_string<char, std::__1::char_traits<char>, std::__1::allocator<char> > >(std::__1::basic_string<char, std::__1::char_traits<char>, std::__1::allocator<char> > const&, std::__1::basic_string<char, std::__1::char_traits<char>, std::__1::allocator<char> >)+652>
0xce4ac4450:    0xd     0xce4ac4750
0xce4ac4460:    0xf560aa728     0x12c3de45c8
0xce4ac4470:    0x2     0xce4acb5f0
0xce4ac4480:    0x12c3de6d68    0xf568ad150
0xce4ac4490:    0xce4ac47b0     0x12c38e8014 <testbed_param::testbed_param(std::__1::basic_string<char, std::__1::char_traits<char>, std::__1::allocator<char> >, std::__1::basic_string<char, std::__1::char_traits<char>, std::__1::allocator<char> >, std::__1::basic_string<char, std::__1::char_traits<char>, std::__1::allocator<char> >, std::__1::basic_string<char, std::__1::char_traits<char>, std::__1::allocator<char> >, std::__1::basic_string<char, std::__1::char_traits<char>, std::__1::allocator<char> >, std::__1::basic_string<char, std::__1::char_traits<char>, std::__1::allocator<char> >)+53164>
0xce4ac44a0:    0x12c3ec77d8 <_ZZN2gf3log8instanceEvE3log>      0x12c3de3760
0xce4ac44b0:    0x31    0xce4ac9940
0xce4ac44c0:    0x12c3ec77d8 <_ZZN2gf3log8instanceEvE3log>      0xce4a


#查看线程信息 
info threas
Undefined info command: "threas".  Try "help info".
(gdb) info threads
  Id   Target Id         Frame 
  1    pid 1073157 tid 1 (STOPPED) 0x00000019af55b448 in ?? ()
  2    pid 1073157 tid 2 (STOPPED) 0x00000019af55b448 in ?? ()
  3    pid 1073157 tid 3 (MUTEX) 0x00000019af55b520 in ?? ()
  4    pid 1073157 tid 4 (MUTEX) 0x00000019af55b520 in ?? ()
* 5    pid 1073157 tid 5 (STOPPED) 0x00000019af55b510 in ?? ()
  6    pid 1073157 tid 6 (SIGWAITINFO) 0x00000019af55b350 in ?? ()
  7    pid 1073157 tid 7 (CONDVAR) 0x00000019af55b278 in ?? ()
  8    pid 1073157 tid 8 (CONDVAR) 0x00000019af55b278 in ?? ()


#列出crash地址所在的源代码位置
(gdb) list *0x12c38e8014
0x12c38e8014 is in testbed_param::testbed_param(std::__1::basic_string<char, std::__1::char_traits<char>, std::__1::allocator<char> >, std::__1::basic_string<char, std::__1::char_traits<char>, std::__1::allocator<char> >, std::__1::basic_string<char, std::__1::char_traits<char>, std::__1::allocator<char> >, std::__1::basic_string<char, std::__1::char_traits<char>, std::__1::allocator<char> >, std::__1::basic_string<char, std::__1::char_traits<char>, std::__1::allocator<char> >, std::__1::basic_string<char, std::__1::char_traits<char>, std::__1::allocator<char> >) (/home/xjf2613/code-space/qnx/pe-board_service/src/target/o_main_objective_service/run/testbed_param.h:245).
240             }
241             SLOGW("run testbed system enter, msgid:{} material type:{} cmd:{}, cmd_size:{}",msg_id, material_type, run_cmd, run_cmd.size());
242
243             auto& cmd_parser = TestbedCmdParser::ins();
244             //builtin
245             cmd_parser.push_buitin_var_value(CVAR_LOCAL_MATERIAL_PATH, testbed_path);
246             cmd_parser.push_buitin_var_value(CVAR_MATERIAL_URL, metrial_path_url);
247             cmd_parser.push_buitin_var_value(CVAR_RESULT_DIR, result_dir_path);
248             cmd_parser.push_buitin_var_value(CVAR_MSG_ID, msg_id);
249             //addtion
(gdb) 
```

### supplement
**查看所有回溯栈**
```bash
#所有
bt
#从最新栈帧开始计数的固定数量栈帧
bt <正数>
#从所有栈帧中最老帧开始计数，固定向前的栈帧
bt <负数>
```
**罗列栈帧信息后 切换对应帧的信息，切换完帧后，就将上下文移到了对应的帧环境中 <上下文切换>**
```bash
## 切换
frame <帧号> 
或者
f <帧号>
## 使用 up down来顺序切换帧
up 
down
```
**切换到对应的栈帧后，就可以打印在当前帧环境中的变量情况**
```bash
## 打印命令 printf
print <symbol>
p <symbol>
```
**其他的上下文切换: 切换线程上下文 <上下文切换>**
```bash
#先查看所有线程信息
info threads
或者
i threads
#切换对应线程
thread <线程号>
或者
t <线程号>
```