---
id: i5js70terlshyftmshgp5ge
title: Factory
desc: ''
updated: 1703492324709
created: 1703490286166
---

#### sample code
```c++
// gtest-internal.h
// Defines the abstract factory interface that creates instances
// of a Test object.
class TestFactoryBase {
 public:
  virtual ~TestFactoryBase() = default;

  // Creates a test instance to run. The instance is both created and destroyed
  // within TestInfoImpl::Run()
  virtual Test* CreateTest() = 0;

 protected:
  TestFactoryBase() {}

 private:
  TestFactoryBase(const TestFactoryBase&) = delete;
  TestFactoryBase& operator=(const TestFactoryBase&) = delete;
};

// This class provides implementation of TestFactoryBase interface.
// It is used in TEST and TEST_F macros.
template <class TestClass>
class TestFactoryImpl : public TestFactoryBase {
 public:
  Test* CreateTest() override { return new TestClass; }
};

//gtest.h
// Dynamically registers a test with the framework.
//
// This is an advanced API only to be used when the `TEST` macros are
// insufficient. The macros should be preferred when possible, as they avoid
// most of the complexity of calling this function.
//
// The `factory` argument is a factory callable (move-constructible) object or
// function pointer that creates a new instance of the Test object. It
// handles ownership to the caller. The signature of the callable is
// `Fixture*()`, where `Fixture` is the test fixture class for the test. All
// tests registered with the same `test_suite_name` must return the same
// fixture type. This is checked at runtime.
//
// The framework will infer the fixture class from the factory and will call
// the `SetUpTestSuite` and `TearDownTestSuite` for it.
//
// Must be called before `RUN_ALL_TESTS()` is invoked, otherwise behavior is
// undefined.
//
// Use case example:
//
// class MyFixture : public ::testing::Test {
//  public:
//   // All of these optional, just like in regular macro usage.
//   static void SetUpTestSuite() { ... }
//   static void TearDownTestSuite() { ... }
//   void SetUp() override { ... }
//   void TearDown() override { ... }
// };
//
// class MyTest : public MyFixture {
//  public:
//   explicit MyTest(int data) : data_(data) {}
//   void TestBody() override { ... }
//
//  private:
//   int data_;
// };
//
// void RegisterMyTests(const std::vector<int>& values) {
//   for (int v : values) {
//     ::testing::RegisterTest(
//         "MyFixture", ("Test" + std::to_string(v)).c_str(), nullptr,
//         std::to_string(v).c_str(),
//         __FILE__, __LINE__,
//         // Important to use the fixture type as the return type here.
//         [=]() -> MyFixture* { return new MyTest(v); });
//   }
// }
// ...
// int main(int argc, char** argv) {
//   ::testing::InitGoogleTest(&argc, argv);
//   std::vector<int> values_to_test = LoadValuesFromConfig();
//   RegisterMyTests(values_to_test);
//   ...
//   return RUN_ALL_TESTS();
// }
//
template <int&... ExplicitParameterBarrier, typename Factory>
TestInfo* RegisterTest(const char* test_suite_name, const char* test_name,
                       const char* type_param, const char* value_param,
                       const char* file, int line, Factory factory) {
  using TestT = typename std::remove_pointer<decltype(factory())>::type;

  class FactoryImpl : public internal::TestFactoryBase {
   public:
    explicit FactoryImpl(Factory f) : factory_(std::move(f)) {}
    Test* CreateTest() override { return factory_(); }

   private:
    Factory factory_;
  };

  return internal::MakeAndRegisterTestInfo(
      test_suite_name, test_name, type_param, value_param,
      internal::CodeLocation(file, line), internal::GetTypeId<TestT>(),
      internal::SuiteApiResolver<TestT>::GetSetUpCaseOrSuite(file, line),
      internal::SuiteApiResolver<TestT>::GetTearDownCaseOrSuite(file, line),
      new FactoryImpl{std::move(factory)});
}


//using
// Expands to the name of the class that implements the given test.
#define GTEST_TEST_CLASS_NAME_(test_suite_name, test_name) \
  test_suite_name##_##test_name##_Test

// Helper macro for defining tests.
#define GTEST_TEST_(test_suite_name, test_name, parent_class, parent_id)       \
  static_assert(sizeof(GTEST_STRINGIFY_(test_suite_name)) > 1,                 \
                "test_suite_name must not be empty");                          \
  static_assert(sizeof(GTEST_STRINGIFY_(test_name)) > 1,                       \
                "test_name must not be empty");                                \
  class GTEST_TEST_CLASS_NAME_(test_suite_name, test_name)                     \
      : public parent_class {                                                  \
   public:                                                                     \
    GTEST_TEST_CLASS_NAME_(test_suite_name, test_name)() = default;            \
    ~GTEST_TEST_CLASS_NAME_(test_suite_name, test_name)() override = default;  \
    GTEST_TEST_CLASS_NAME_(test_suite_name, test_name)                         \
    (const GTEST_TEST_CLASS_NAME_(test_suite_name, test_name) &) = delete;     \
    GTEST_TEST_CLASS_NAME_(test_suite_name, test_name) & operator=(            \
        const GTEST_TEST_CLASS_NAME_(test_suite_name,                          \
                                     test_name) &) = delete; /* NOLINT */      \
    GTEST_TEST_CLASS_NAME_(test_suite_name, test_name)                         \
    (GTEST_TEST_CLASS_NAME_(test_suite_name, test_name) &&) noexcept = delete; \
    GTEST_TEST_CLASS_NAME_(test_suite_name, test_name) & operator=(            \
        GTEST_TEST_CLASS_NAME_(test_suite_name,                                \
                               test_name) &&) noexcept = delete; /* NOLINT */  \
                                                                               \
   private:                                                                    \
    void TestBody() override;                                                  \
    static ::testing::TestInfo* const test_info_ GTEST_ATTRIBUTE_UNUSED_;      \
  };                                                                           \
                                                                               \
  ::testing::TestInfo* const GTEST_TEST_CLASS_NAME_(test_suite_name,           \
                                                    test_name)::test_info_ =   \
      ::testing::internal::MakeAndRegisterTestInfo(                            \
          #test_suite_name, #test_name, nullptr, nullptr,                      \
          ::testing::internal::CodeLocation(__FILE__, __LINE__), (parent_id),  \
          ::testing::internal::SuiteApiResolver<                               \
              parent_class>::GetSetUpCaseOrSuite(__FILE__, __LINE__),          \
          ::testing::internal::SuiteApiResolver<                               \
              parent_class>::GetTearDownCaseOrSuite(__FILE__, __LINE__),       \
          new ::testing::internal::TestFactoryImpl<GTEST_TEST_CLASS_NAME_(     \
              test_suite_name, test_name)>);                                   \
  void GTEST_TEST_CLASS_NAME_(test_suite_name, test_name)::TestBody()

//!!!! TEST
//gtest.h
#define GTEST_TEST(test_suite_name, test_name)             \
  GTEST_TEST_(test_suite_name, test_name, ::testing::Test, \
              ::testing::internal::GetTestTypeId())

// Define this macro to 1 to omit the definition of TEST(), which
// is a generic name and clashes with some other libraries.
#if !(defined(GTEST_DONT_DEFINE_TEST) && GTEST_DONT_DEFINE_TEST)
#define TEST(test_suite_name, test_name) GTEST_TEST(test_suite_name, test_name)
#endif


//!!!! TEST_P
//gtest-param-test.h
#define TEST_P(test_suite_name, test_name)                                     \
  class GTEST_TEST_CLASS_NAME_(test_suite_name, test_name)                     \
      : public test_suite_name,                                                \
        private ::testing::internal::GTestNonCopyable {                        \
   public:                                                                     \
    GTEST_TEST_CLASS_NAME_(test_suite_name, test_name)() {}                    \
    void TestBody() override;                                                  \
                                                                               \
   private:                                                                    \
    static int AddToRegistry() {                                               \
      ::testing::UnitTest::GetInstance()                                       \
          ->parameterized_test_registry()                                      \
          .GetTestSuitePatternHolder<test_suite_name>(                         \
              GTEST_STRINGIFY_(test_suite_name),                               \
              ::testing::internal::CodeLocation(__FILE__, __LINE__))           \
          ->AddTestPattern(                                                    \
              GTEST_STRINGIFY_(test_suite_name), GTEST_STRINGIFY_(test_name),  \
              new ::testing::internal::TestMetaFactory<GTEST_TEST_CLASS_NAME_( \
                  test_suite_name, test_name)>(),                              \
              ::testing::internal::CodeLocation(__FILE__, __LINE__));          \
      return 0;                                                                \
    }                                                                          \
    static int gtest_registering_dummy_ GTEST_ATTRIBUTE_UNUSED_;               \
  };                                                                           \
  int GTEST_TEST_CLASS_NAME_(test_suite_name,                                  \
                             test_name)::gtest_registering_dummy_ =            \
      GTEST_TEST_CLASS_NAME_(test_suite_name, test_name)::AddToRegistry();     \
  void GTEST_TEST_CLASS_NAME_(test_suite_name, test_name)::TestBody()

```