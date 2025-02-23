---
id: tphqsbw3474tgn74j0t3lrk
title: Init_throw
desc: ''
updated: 1719282688837
created: 1719281493762
---

##### vector 初始化列表抛出长度异常
```c++
#define DEF_SUB_CMD(name, descript, ...) \
auto* sub_cmd_##name= cli_root.add_subcommand(#name, descript); \
std::vector<Cli11OptionBase*> vec_option_##name{__VA_ARGS__}; \
for (auto& cur_option : vec_option_##name) { \
    cur_option->to_add_option(*sub_cmd_##name); \
} \

 DEF_SUB_CMD(function, "call function directly",
         new Cli11Option_unzip_testbed, new Cli11Option_download_http
         , new Cli11Option_mv_file, new Cli11Option_http_filesize
         , new Cli11Option_mearsure_url_space, new Cli11Option_query_file_size
         , new Cli11Option_query_dir_remain_space
         , new Cli11Option_test_http_server, new Cli11Option_test_http_objective_server
         , new Cli11Option_test_threadloop_back, new Cli11Option_test_threadloop_front
         , new Cli11Option_test_threadloop_back_detach, new Cli11Option_test_threadloop_back_join
         , new Cli11Option_test_http_server_with_state_manager, new Cli11Option_update_testbed_and_replace
         , new Cli11Option_check_smb_url_file_type, new Cli11Option_check_smb_upload_file
        //  , new Cli11Option_test_rabbitmq_cpp_recevier
        //  , new Cli11Option_test_rabbitmq_sender
         , new Cli11Option_normal_remove_all
         , new Cli11Option_recursive_rm
        //  , new Cli11Option_read_file_publish_to_queue
        //  , new Cli11Option_read_queue_write_to_file
        //  , new Cli11Option_rabbitmq_c_read_queue_print
         //  , new Cli11Option_test_loop_thread
         //  , new Cli11Option_test_single_thread
        //  , new Cli11Option_read_queue_write_to_file_by_inner_queue
         , new Cli11Option_test_safe_queue
         , new Cli11Option_test_testbedmng
        //  , new Cli11Option_publish_multi_thread
        //  , new Cli11Option_publish_multi_thread_rabbitmq_c
        , new Cli11Option_smb_download_file
        , new Cli11Option_smb_download_file2
        , new Cli11Option_smb_download_file3
        , new Cli11Option_smb_check_remote

         , new Cli11Option_my_amqp_receiver_test1
         , new Cli11Option_my_amqp_receiver_test2
         , new Cli11Option_my_amqp_publisher_test1
         , new Cli11Option_my_amqp_publisher_test2
         , new Cli11Option_my_amqp_publisher_test3
         , new Cli11Option_my_amqp_publisher_test4
         , new Cli11Option_my_amqp_publisher_test5
         , new Cli11Option_my_amqp_publisher_test6

         , new Cli11Option_test_cfg_parse
         , new Cli11Option_test_cfg_toml_parse
         , new Cli11Option_test_cfg_mng

         ,new Cli11Option_mongoose_mqtt_client_demo
         ,new Cli11Option_mongoose_mqtt_server_demo
         ,new Cli11Option_mongoose_mqtt_server_demo2

     );

     #define DEFINE_CMD_OPTION_3(cmd_name, arg1_type, arg1, arg2_type, arg2, arg3_type, arg3) \
        class  Cli11Option_##cmd_name : public Cli11OptionBase { \
        public: \
            arg1_type arg1 = arg1; \
            arg2_type arg2 = arg2; \
            arg3_type arg3 = arg3; \
            virtual std::string get_sub_cmd_name() override { \
                return #cmd_name; \
            }; \
            virtual int to_add_option(CLI::App& inapp) override { \
                auto* cur_sub = inapp.add_subcommand(#cmd_name, #cmd_name); \
                cur_sub->add_option("--"#arg1, arg1, #arg1) -> required(true); \
                cur_sub->add_option("--"#arg2, arg2, #arg2) -> required(true); \
                cur_sub->add_option("--"#arg3, arg3, #arg3) -> required(true); \
            }; \
            virtual int call_funtion() override; \
        }; \
        inline int Cli11Option_##cmd_name::call_funtion() \

```
**相同代码，在qnx能够编译运行，在arm linux上会报错，抛出std::length_error异常, 暂时可以确定是生成的类在初始化的时候发生错误**