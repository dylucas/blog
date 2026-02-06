---
title: == 和 equals 的区别
description: 
date: 2024-08-01
image: "https://www.bing.com/th?id=OHR.MorroJable_ZH-CN7382027689_1920x1080.jpg&rf=LaDigue_1920x1080.jpg&pid=hp"
draft: false
tags: 
categories:
  - Java
---
## == 运算符

当应用于基本数据类型（如 int, double, char 等）时，它比较的是值是否相等。 当用于引用类型（比如对象、数组）时，== 比较的是两个引用是否指向堆内存中的同一个对象，即它们的内存地址是否相同。

## equals() 方法

equals() 是从 Object 类继承而来的方法，默认实现实际上也是比较对象的引用是否相同，和 == 的行为一致。 然而，许多类（尤其是像 String, Integer 这样的包装类）重写了 equals() 方法，使其比较的是对象的实际内容是否相等，而不是引用。

**String类的equals方法：**

```Java
public boolean equals(Object anObject) {
    // 判断两个对象是否为同一个引用
    if (this == anObject) {
        return true;
    }
    // 检查参数是否为字符串实例
    if (anObject instanceof String) {
        String anotherString = (String)anObject;
        int n = value.length;
        // 比较两个字符串的长度
        if (n == anotherString.value.length) {
            char v1[] = value;
            char v2[] = anotherString.value;
            int i = 0;
            // 循环比较两个字符串的字符内容
            while (n-- != 0) {
                if (v1[i] != v2[i])
                    // 如果发现字符不同，则字符串不相等
                    return false;
                i++;
            }
            return true;
        }
    }
    return false;
}
```

**Integer类的equals方法：**

```Java
public boolean equals(Object obj) {
    if (obj instanceof Integer) {
        // 与传入的实际值进行比较
        // .intValue()将Integer对象包装的整数值提取出来，返回一个基本类型的int值
        return value == ((Integer)obj).intValue();
    }
    return false;
}
```

**示例代码：**

下面通过具体的Java代码示例来阐述这两个操作符的区别：

```Java
public class EqualsDemo {
    public static void main(String[] args) {
        // 基本类型比较
        int num1 = 10;
        int num2 = 10;
        System.out.println(num1 == num2);  // 输出 true，因为数值相等

        // 引用类型的 == 比较
        String str1 = new String("Hello");
        String str2 = new String("Hello");
        System.out.println(str1 == str2);  // 输出 false，虽然内容相同，但不是同一个对象引用

        // 使用 equals 比较
        System.out.println(str1.equals(str2));  // 输出 true，因为 String 类重写了 equals 方法，比较的是内容   
    }
}
```

**实操：bean equals方法重写进行比较**

```Java
public class EqualsDemo {
    public static void main(String[] args) {
        // 自定义类的示例
        Person person1 = new Person("Alice", 30);
        Person person2 = new Person("Alice", 30);
        System.out.println(person1 == person2);  // 输出 false，因为是两个不同对象
        System.out.println(person1.equals(person2));  // 默认输出 false，除非重写 equals 方法
    }
    
    static class Person {
        String name;
        int age;
    
        Person(String name, int age) {
            this.name = name;
            this.age = age;
        }
    
        @Override
        public boolean equals(Object obj) {
            if (this == obj) {
                return true;
            }
            if (obj == null || getClass() != obj.getClass()) {
                return false;
            }
            Person person = (Person) obj;
            return age == person.age && name.equals(person.name);
        }
    }
}
```

## 总结

1. == 主要来判断基本类型变量的值是否相等，或判断两个引用是否指向同一个对象。
    
2. equals() 默认行为为比较引用与 == 相同，当使用String、Integer等重写了equals方法包装类时会比较对象的实际属性。