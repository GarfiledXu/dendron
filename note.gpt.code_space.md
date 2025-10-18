---
id: 3dvx79sp70jo3rh18x0rkzi
title: Code_space
desc: ''
updated: 1757748414907
created: 1751013357879
---

## c++

要求类名使用大驼峰，成员变量和成员函数使用小写加下划线，private的成员变量和函数，在末尾追加一个下划线
如:

```c++
    class RkVencMuxer {

    public:
        RkVencMuxer(VencMuxerConf in_venc_muxer_conf);
        virtual ~RkVencMuxer();

        int venc_init();
        int venc_data_outcb_set(const std::string& in_save_file_path);
        int venc_muxer_init(const std::string& in_save_file_prefix = "muxer_test_", const std::string& in_save_file_dir_path = "/userdata");
        int venc_muxer_eventcb_set();

        int start_venc();
        int stop_venc();
        int start_muxer();
        int stop_muxer();

        int venc_uninit();
        int venc_muxer_uninit();

    private:
        VencMuxerConf venc_muxer_conf_;

    };
```

日志风格 字符串格式化格式是spdlog的, 如下

```c++
#include "logg.h"
SLOGD("[server] new session connected");
SLOGI("[server] new session connected");
```
