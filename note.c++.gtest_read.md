---
id: mkxykzsap9yb8e4qpc6rx6l
title: Gtest_read
desc: ''
updated: 1703208015242
created: 1703157222094
---

### code expand
**test macro expand to complete implementation**
test macro 
```c++
TEST(FooTest, Demo)
{
    EXPECT_EQ(1, 1);
}
```


complete implementation
```c++
class FooTest_Demo_Test : public ::testing::Test 
{
public: 
    FooTest_Demo_Test() {}
private: 
    virtual void TestBody();
    static ::testing::TestInfo* const test_info_;
    FooTest_Demo_Test(const FooTest_Demo_Test &);
    void operator=(const FooTest_Demo_Test &);
};

::testing::TestInfo* const FooTest_Demo_Test 
    ::test_info_ = 
        ::testing::internal::MakeAndRegisterTestInfo( 
            "FooTest", "Demo", "", "",
            (::testing::internal::GetTestTypeId()),
            ::testing::Test::SetUpTestCase,
            ::testing::Test::TearDownTestCase,
            new ::testing::internal::TestFactoryImpl< FooTest_Demo_Test>);

void FooTest_Demo_Test::TestBody()
{
    switch (0)
    case 0:
        if (const ::testing::AssertionResultgtest_ar = 
                    (::testing::internal:: EqHelper<(sizeof(::testing::internal::IsNullLiteralHelper(1)) == 1)>::Compare("1", "1", 1, 1)));
        else 
            ::testing::internal::AssertHelper(
                ::testing::TPRT_NONFATAL_FAILURE,
                ".\\gtest_demo.cpp",
                9,
                gtest_ar.failure_message()
                ) = ::testing::Message();
}
```

#### important class
**TEST macro**
> 用户 的调用入口，存放单元测试代码块, 利用用户传入的宏参数，作为类型依据，进行代码生成

**Test**
> 由 TEST 宏进行
> 核心: 用来承接用户通过宏定义传入的Testbody实现，以及宏定义内包含的testinfo_成员

**TestInfo**
> 存储

**TEST宏生成的类在什么时候被实例化呢**
