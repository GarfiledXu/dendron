---
id: fkkijp2w4wzw8p1weadi3ao
title: Refer Assign Pass
desc: ''
updated: 1694093145634
created: 1693989051827
---

#### 场景复现
``` java
nv12ToNv21(){
        public static ByteBuffer nv12ToNv21(ByteBuffer src, ByteBuffer out, int width, int height){
       try{
           long startMs = System.currentTimeMillis();
           if(src == null || out == null || width < 1 || height <1){
               throw new Exception("src == null || out == null || width < 1 || height <1");
           }
           // generic src is read only, as that we need write in out buff and extract from src buff
           Log.d(TAG, String.format("in out capacity:%d, src remain:%d", out.capacity(), src.remaining()));
           if(out.capacity() < src.remaining()){
                //can't dynamic alloc, so new buff to check ref
               //allocate using jvm gc, allocate direct using the memory outside jvm gc, need create cleaner to delete
               out = ByteBuffer.allocate(src.remaining());
               Log.d(TAG, String.format("to allocate buff point, cost time:%d, out capacity:%d, src remain:%d", System.currentTimeMillis() - startMs, out.capacity(), src.remaining()));
           }
           //prepare put the data on beginning of out
           long allocatedMs = System.currentTimeMillis();
           out.clear();
           int ySize = width * height;
           int srcRemain = src.remaining();
           src.position(0);
           src.limit(ySize);
           out.put(src.slice());
           src.position(0);
           src.limit(srcRemain);
           //swap U and V planes
           for (int i = ySize; i < src.remaining(); i += 2) {
               out.put(i, src.get(i+1));
               out.put(i + 1, src.get(i));
           }
           Log.d(TAG, String.format("to out buff point, cost time:%d", System.currentTimeMillis() - allocatedMs));
           out.position(0);
           return out;
       }catch (Exception e){
           Log.e(TAG, "error, nv12ToNv21, "+e.getMessage());
       }
       return null;
    }
}
class{
    private ByteBuffer tmpbuff = ByteBuffer.allocate(100);
    Metiral.nv12ToNv21((ByteBuffer) inVideoFrame.mData, tmpbuff, inVideoFrame.mWidth, inVideoFrame.mHeight);
}
result:  tmpbuff会被反复赋值
```
**本质上Java声明的对象`模型`，`只存储`两种东西，`null引用`和实际被`JVM分配的引用`**, 即对象和引用的关系是存储和被存储的关系，不是等号关系，类似指针变量与内存地址关系，指针变量通过*来进行解引用，而java对象则是直接操作对象符号的方式进行引用

**通过 `=` 符号， 引用以及包含引用的对象可以赋值给另一个对象，即对象存储的引用可以重新赋值修改，右边的引用赋值给左边的对象**

**而函数传参的过程，即把外围对象的引用赋值给了函数的形参，形参本质上是临时变量，`即让临时变量存储了外围引用，即形参拷贝了外围对象存储的引用`，重点！！！在函数内通过将new引用赋值给形参，只能改变形参的引用，不影响外围变量，因为从头到尾都没有修改外围对象所存储的引用**
类比指针操作，只传参外围指针(一级指针)，只能让形参存储外围指针指向的内存，并不能修改外围指针变量的存储的地址，唯二方式只能通过返回函数内新地址给外围变量赋值，或者通过二级指针

**总结，java中引用是属性，java传参让形参对象拷贝存储引用属性，不是让形参变成对象的引用(c++是对象的引用)**
**summary: 如何在函数内部修改外部变量存储的引用？**
1. 不向外传递，把传入的参数在函数外就分配固定好
2. 通过return，在函数外重新赋值给外头对象
3. 作为成员变量**包装入一个class**中，将该类型的非null引用传入，那么**通过一个非null引用的传递**，来访问该引用的成员，对其进行引用修改，即使其存储的是null，即`先传引用，通过引用来持有并修改目标对象`

