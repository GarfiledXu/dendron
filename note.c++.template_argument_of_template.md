---
id: ofkw2nkadayguky32lsjn5a
title: Template_argument_of_template
desc: ''
updated: 1699335130127
created: 1699325767887
---

#### sence
```c++
//source section from toml11 get.hpp and value.hpp
template<typename C,
         template<typename ...> class M, template<typename ...> class V>
basic_value<C, M, V> const& find(const basic_value<C, M, V>& v, const key& ky)
{
    const auto& tab = v.as_table();
    if(tab.count(ky) == 0)
    {
        detail::throw_key_not_found_error(v, ky);
    }
    return tab.at(ky);
}
template<typename C,
         template<typename ...> class M, template<typename ...> class V>
basic_value<C, M, V>& find(basic_value<C, M, V>& v, const key& ky)
{
    auto& tab = v.as_table();
    if(tab.count(ky) == 0)
    {
        detail::throw_key_not_found_error(v, ky);
    }
    return tab.at(ky);
}


template<typename Comment, // discard/preserve_comment
         template<typename ...> class Table = std::unordered_map,
         template<typename ...> class Array = std::vector>
class basic_value
{
    template<typename T, typename U>
    static void assigner(T& dst, U&& v)
    {
        const auto tmp = ::new(std::addressof(dst)) T(std::forward<U>(v));
        assert(tmp == std::addressof(dst));
        (void)tmp;
    }

    using region_base = detail::region_base;

    template<typename C, template<typename ...> class T,
             template<typename ...> class A>
    friend class basic_value;

  public:

    using comment_type         = Comment;
    using key_type             = ::toml::key;
    using value_type           = basic_value<comment_type, Table, Array>;
    using boolean_type         = ::toml::boolean;
    using integer_type         = ::toml::integer;
.....
}
```

#### refe
[C++11 模板元编程 - 模板的模板参数](https://www.jianshu.com/p/c94184e295d7)
[C++ 模板的模板参数和模板别名](https://mysteriouspreserve.com/blog/2021/08/17/Cpp-Alias-Template-and-Template-Parameter/)
[blog:模板模板参数](https://www.cnblogs.com/5iedu/p/12755651.html)