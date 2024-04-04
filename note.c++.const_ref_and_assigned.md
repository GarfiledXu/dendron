---
id: kdajvbn510qwkp6rot5gbrg
title: Const_ref_and_assigned
desc: ''
updated: 1707879504455
created: 1707879459930
---

##### test
```c++
#include <stdio.h>

const int a_const = 0;
int a_non_const = 0;
const int& return_const(){
    static int a = 0;
    return a;
}
int& return_non_const(){
    static int b = 0;
    return b;
}
int main(){
    //non -> const : copy
    const int const_out = a_non_const;
    //const -> const: copy
    const int const_out2 = a_const;
    //const -> non: copy
    int non_1 = a_const;
    
    //bind ref int& to const int var, fail  error: binding reference of type ‘int&’ to ‘const int’ discards qualifiers
    //int& ref_non_const_1 = a_const;
    //bind ref int& to non const int var, pass
    int& ref_non_const_2 = a_non_const;
    //bind const int& ref to const int var, pass
    const int& ref_const_1 = a_const;
    //bind const int& ref to non const int var, pass
    const int& ref_const_2 = a_non_const;
    
    //summay:
    //first: there are two types of operations, the one is assignment, the other is binding reference;
    //assignment: is copy directly, there is no need to consider the attribute of both parties; just copy and assigned the new variable property,such as const qualify;
    //binding reference: reference is based on the origin variable, so, need to keep the property of source variable
    //if src is const, the ref variable  can't be non-const, that will break the modifiers of the original variables;
    //is src is non-const, the ref varible can be add const qualify for user, beacuse it's not break original property
    return 0;
}
```