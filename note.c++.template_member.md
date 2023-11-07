---
id: e6ymhtao0bpbdt523haru5h
title: Template_memb
desc: ''
updated: 1696917704231
created: 1696916976074
---

#### keyword
`[class_name]::template [member_func]<args of tempate>`
```c++
struct synchronous_factory
{
    template<typename Sink, typename... SinkArgs>
    static std::shared_ptr<spdlog::logger> create(std::string logger_name, SinkArgs &&...args)
    {
        auto sink = std::make_shared<Sink>(std::forward<SinkArgs>(args)...);
        auto new_logger = std::make_shared<spdlog::logger>(std::move(logger_name), std::move(sink));
        details::registry::instance().initialize_logger(new_logger);
        return new_logger;
    }
};

template<typename Factory = spdlog::synchronous_factory>
inline std::shared_ptr<logger> gf_rotating_logger_mt(const std::string &logger_name, const filename_t &filename, size_t max_file_size,
    size_t max_files, bool rotate_on_open = false, const file_event_handlers &event_handlers = {})
{
    return Factory::template create<sinks::gf_rotating_file_sink_mt>(
        logger_name, filename, max_file_size, max_files, rotate_on_open, event_handlers);
}

template<typename Factory = spdlog::synchronous_factory>
inline std::shared_ptr<logger> hourly_logger_mt(const std::string &logger_name, const filename_t &filename, bool truncate = false,
    uint16_t max_files = 0, const file_event_handlers &event_handlers = {})
{
    return Factory::template create<sinks::hourly_file_sink_mt>(logger_name, filename, truncate, max_files, event_handlers);
}
```
#### refe
- [csdn-模板成员与template关键字]<https://blog.csdn.net/10km/article/details/123571339>


