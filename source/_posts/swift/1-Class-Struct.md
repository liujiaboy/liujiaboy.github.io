---
title: Class & Struct
date: 2021-12-28 17:08:10
tags:
    - Swift,
    - Class
    - Struct
categories:
    - Swift
---

# 相同点

1. 定义存储值的属性
2. 定义方法
3. 定义下标以及使用下标语法提供对其值的访问
4. 定义初始化器
5. 使用extension来拓展功能
6. 遵循协议来提供功能

# 不同点

1. 类有继承特性，结构体没有
2. 类型转换，在运行时检查和解释类实例的类型
3. 类有析构函数用来释放器分配的资源
4. 引用计数允许对一个类实例有多个引用
5. 类是引用类型，存储在堆上，也就意味着一个类类型的变量并不是直接存储具体的的实例对象，是对当前存储具体实例内存地址的引用。
6. Struct结构体是值类型，存储在栈上。如果值类型中有引用类型的属性，则该属性的地址在栈上，但是引用的值存储在堆上。
7. 尽可能多的使用值类型，是内存安全的，运行效率也会比值类型高。

## 初始化：

1. 类中添加属性，不会自动提供成员初始化器，必须提供对应的的指定初始化器（或者提供便捷初始化器）。
2. 结构体会提供默认的成员初始化器。

### 便捷初始化器 

```
init(name: String, age: Int) {
    this.name = name
    this.age = age
}

convenience init(){
// 这里需要调用初始化
    self.init(name: "name", age: 18)
}
```

便捷初始化器必须从相同的类里调用另一个初始化器，否则会报错。

#### 重点：
1. 指定初始化器必须在调用父类初始化器之前，对其所有属性完成初始化。（确保成员变量使用安全）
2. 指定初始化器必须在调用父类初始化器之后，才能为继承的属性设置新值，否则指定初始化器中赋的新值会被父类的初始化所覆盖。
3. 便捷初始化器必须先调用同类中的初始化器之后，再为任意属性赋值，否则会报错，（属性的值也会被指定初始化覆盖）。
4. 初始化器在初始化完成之前，不能调用任何实例方法，不能读取任何实例属性的值，也不能引用self作为值。必须保证内存是安全的

#### 可失败初始化：init?

当参数不合法或者条件不满足时存在失败的情况，return nil。


#### 必要初始化：require
在类的初始化之前添加required关键字来修饰。表明该类的子类都必须实现该初始化器。

# 类的生命周期
refCount ：64位的位域信息


