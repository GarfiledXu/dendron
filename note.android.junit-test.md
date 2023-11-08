---
id: 13ko6cnrb0a6kb3lm3wbsw4
title: Junit Test
desc: ''
updated: 1695125404832
created: 1694679808896
---

#### refe
- [offical-AndroidJUnitRunner](https://developer.android.com/training/testing/junit-runner?hl=zh-cn)
- [github-usage](https://github.com/li-xiao-shuang/blog/blob/master/docs/notes/java/JUnit4%E6%95%99%E7%A8%8B+%E5%AE%9E%E8%B7%B5.md)


#### junt4 operaion
------------
- shortcut for create test class
  > move cursor to decalre name of origin class which is the unit test target in the editor interface
  > press down `alt` + `enter` will pop up the selection dialog then select the item `create test`

#### concept
- AndroidJUnitRunner
#### junit4 runtime dependence



#### usage of junit param
> junit的运行规律，首先固定解析的第一个注解RunWith，根据传入参数`运行器`来执行不同的处理流程，依次解析对应的注解
> 总结，junit是由注解驱动的测试框架，不同类型运行器 对应不同的注解处理流程
-----------
1. 使用 `RunWith` 注解，修饰目标测试类，指定使用参数允许器
```java
@RunWith(Parameterized.class)
public class CurTestClass{

};
```

2. 声明成员变量以及定义用来赋值成员变量的 `public` 构造函数
> junit 参数运行器的执行流程：调用 @Parameterized.Parameters 修饰的静态接口，获取测试集，利用测试集来构造n个对象，依次运行所有实例
```java
  private Integer inputNumber;
  private Boolean expectedResult;
  public CurTestClass(Integer inputNumber,
                                Boolean expectedResult) {
      this.inputNumber = inputNumber;
      this.expectedResult = expectedResult;
  }
```

3. 定义一个由 org.junit.runners.Parameterized.Parameters 修饰的静态成员函数，返回值为 java.util.Collection， 在此方法中初始化所有参数对
```java
    @Parameterized.Parameters
    public static Collection primeNumbers() {
        return Arrays.asList(new Object[][]{
                {2, true},
                {6, false},
                {19, true},
                {22, false},
                {23, true}
        });
    }
```

4. 正常编写测试函数，使用成员变量进行设置
```java
    @Test
    public void testPrimeNumberChecker() {
        System.out.println("Parameterized Number is : " + inputNumber);
        Assert.assertEquals(expectedResult,
                primeNumberChecker.validate(inputNumber));
    }
```


#### 静态成员变量与junit测试对象的关系
- 同一个进程内，所有对象