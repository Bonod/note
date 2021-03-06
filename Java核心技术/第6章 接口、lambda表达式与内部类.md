# 第6章 接口、lambda表达式与内部类
## 6.1 接口
### 6.1.1 接口概念
- **接口的所有方法自动地属于public**（在实现接口时，必须把方法声明为public）
- **接口绝不能含有实例域**（在Java SE 8之前，也不能在接口中实现方法）
### 6.1.2 接口的特性
- 不能使用new运算符实例化一个接口；可以声明接口变量，接口变量必须引用实现了接口的类对象
- 接口也可以被拓展
- 接口不能包含实例域或静态方法，但却可以包含常量
- 接口中的方法被自动设为public
- 接口中的域被自动设为public static final
### 6.1.3 接口与抽象类
### 6.1.4 静态方法
- 在Java SE 8中，允许在接口中增加静态方法
### 6.1.5 默认方法
- 可以为接口方法提供一个默认实现。必须用default修饰符标记这样一个方法
### 6.1.6 解决默认方法冲突
- 解决默认方法冲突
	1. 超类优先
	2. 接口冲突
- 千万不要让一个默认方法重新定义Object类中的某个方法
## 6.2 接口示例
### 6.2.1 接口与回调
### 6.2.2 Comparator接口
- Compare接口与Comparator接口
	1. 排序规则实现的方法不同
		- Comparable接口的方法：`compareTo(Object o)`
		- Comparator接口的方法：`compare(T o1, To2)`
	2. 类设计前后不同
	    - Comparable接口用于在类的设计中使用
	    - Comparator接口用于类设计已经完成，还想排序（Arrays）
### 6.2.3 对象克隆
- 默认的克隆操作是“浅拷贝”
## 6.3 lambda表达式
### 6.3.1 为什么引入lambda表达式
- 使用lambda表达式，会使得设计的代码会更加简洁，且具有可读性
### 6.3.2 lambda表达式的语法
- **即使lambda表达式没有参数，仍然要提供空括号**
- **如果可以推导出一个lambda表达式的参数类型，则可以忽略其类型**
- **如果方法只有一个参数，而且这个参数的类型可以推导得出，那么甚至还可以省略小括号**
- **无需指定lambda表达式的返回类型**。如果一个lambda表达式只在某些分支返回一个值，而在另外一些分支不返回值，这是不合法的
### 6.3.3 函数式接口
- 函数式接口（functional interface）
	- 只有一个抽象方法
- lambda表达式可以转换为接口
### 6.3.4 方法引用
- 方法引用（method reference）
	- `object::instanceMethod`，等价于提供方法参数的lambda表达式
	- `Class::staticMethod`，等价于提供方法参数的lambda表达式
	- `Class::instanceMethod`，第一个参数会成为方法的目标
	- `this::instanceMethod`，可以在方法引用中使用`this`参数
	- `super::instanceMethod`，可以在方法引用中使用`super`参数
``` java
// 对于object::instanceMethod或Class::staticMethod
// 以下两种方式等价
System.out::println // 方法引用
x->System.out.println(x) // lambda表达式
```
``` java
// 对于Class::instanceMethod
// 以下两种方式等价
String::compareToIgnoreCase // 方法引用
(x, y)->x.compareToIgnoreCase(y) // lambda表达式
```
``` java
// 对于this::instanceMethod
// 以下两种方式等价
this::equals // 方法引用
x->this.equals(x) // lambda表达式
```
``` java
// 对于super::instanceMethod
// 以下两种方式等价
super::equals // 方法引用
x->super.equals(x) // lambda表达式
```
### 6.3.5 构造器引用
- 构造器引用
	- `Class::new`
- 可以用数组类型建立构造器引用
``` java
// 以下两种方式等价
int[]::new // 方法引用
x->new int[x] // lambda表达式
```
- Java有一个限制，无法构造泛型类型`T`的数组
### 6.3.6 变量作用域
- **闭包（closure）**
- lambda表达式可以捕获外围作用域中变量的值
- lambda表达式中捕获的变量必须实际上是**最终变量（effectively final）**
- 在lambda表达式中声明与一个局部变量同名的参数或局部变量是不合法的
- **在一个lambda表达式中使用`this`关键字时，是指创建这个lambda表达式的方法的`this`参数**
### 6.3.7 处理lambda表达式
- 延迟执行（deferred execution）
- 可以使用`@FunctionalInterface`注解来标注函数式接口
- Java泛型仅针对引用类型，如果使用Function，会将代码中的int进行装箱，从而在性能上付出代价。java.util.function包针对基本类型的int、double和long提供支持，当输入或/和输出为基本类型时，可以避免自动装箱的操作。
- **大多数标准函数式接口都提供了非抽象方法来生成或合并函数**
### 6.3.8 再谈Comparator
## 6.4 内部类
- **内部类（inner class）**
- 使用内部类的原因
	- 内部类方法可以访问该类定义所在的作用域中的数据，包括私有的数据
	- 内部类可以对同一个包中的其他类隐藏起来
	- 当想要定义一个回调函数而且不想编写大量代码时，使用匿名（anonymous）内部类比较便捷
### 6.4.1 使用内部类访问对象状态
- 内部类既可以访问自身的数据域，也可以访问创建它的外围类对象的数据域
- 外围类的引用在内部类构造器中设置，这个引用在内部类的定义中是不可见的
- 只有内部类可以是私有类，而常规类只可以具有包可见性，或共有可见性
### 6.4.2 内部类的特殊语法规则
- 使用外围类引用的正规语法
``` java
OuterClass.this
```
- 可以采用下列语法格式更加明确地编写内部对象的构造器
``` java
outerObject.new InnerClass(construction parameters)
```
- 内部类中声明的所有静态域都必须是`final`
- 内部类不能有`static`方法
- 可以通过显式地命名将外部类引用设置为其他的对象
### 6.4.3 内部类是否有用、必要和安全
- 编译器将会把内部类翻译成用$（美元符号）分隔外部类名与内部类名的常规类文件，而虚拟机则对此一无所知
- 如果内部类访问了私有数据域，就有可能通过附加在外围类所在包中的其他类访问它们
### 6.4.4 局部内部类
- 局部类不能用`public`或`private`访问说明符进行声明。它的作用域被限定在声明这个局部类的块中
### 6.4.5 由外部方法访问变量
- 局部内部类可以访问局部变量。不过这些局部变量必须**事实上**为`final`
> 可以使用长度为1的数组或者对象来绕过此限制。在多线程中执行内部类中的代码时，这做法可能会导致竞态条件
### 6.4.6 匿名内部类
- 匿名内部类（anonymous inner class）
- 通常的语法格式
	- 其中`SuperType`可以是一个接口，也可以是一个类
``` java
new SuperType(construction parameter) {
	inner class methods and data
}
```
- 由于构造器的名字必须与类名相同，而匿名类没有类名，所以，匿名类不能有构造器。取而代之的是，将构造器参数传递给超类（superclass）构造器
- 建议使用lambda表达式替代匿名内部类实现事件监视器和其他回调
- 双括号初始化（double brace initialization）
``` java
new ArrayList<Class>(){{add(object0);add(object1);}} // 构造匿名列表并添加元素
```
- 在静态方法中获取当前类名
``` java
new Object(){}.getClass().getEnclosingClass()
```
> 静态方法没有`this`，故不能使用`this.getClass()获取当前类名`
### 6.4.6 静态内部类
- 只有内部类可以声明为`static`
- 静态内部类的对象除了没有对生成它的外围类对象的引用特权外，与其他所有内部类完全一样
## 6.5 代理
- **代理（proxy）**
- 利用代理可以在运行时创建一个实现了一组给定接口的新类
### 6.5.1 何时使用代理
### 6.5.2 创建代理对象
- `Proxy`类的`newProxyInstance`方法的三个参数
	- 一个类加载器（class loader）。用`null`表示使用默认的类加载器
	- 一个`Class`对象数组，每个元素都是需要实现的接口
	- 一个调用处理器   
### 6.5.3 代理类的特性
- 代理类是在程序运行过程中创建的。然而，一旦被创建，就变成了**常规类**，与虚拟机中的任何其他类没有什么区别
- 所有代理类都扩展于`Proxy`类
- 一个代理类只有一个实例域——调用处理器
- 所有的代理类都覆盖了`Object`类中的方法`toString`、`equals`和`hashCode`
- 没有定义代理类的名字，`Sun`虚拟机中的`Proxy`类将生成一个以字符串`$Proxy`开头的类名
- 对于特定的类加载器和预设的一组接口来说，只能用一个代理类
- 代理类一定是`public`和`final`
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE1OTc5MTAwMjYsMzQyMzc5NTM5LC04Mj
Y3NDE3NTksLTE4NjgyNTY4OTEsMTE5NDYzMzE2NywyMDI5MjI2
NTAxLDEwOTAwMTM2NjksNzUyNTYwMjU1LC0xMDk3MjI3NzE1LD
E4OTM4NTE5MjIsLTEwMTYxNzY0MjcsLTI2NzE5NTYwNiw4NzQ5
Njg1NywtMzkzNTI1NjgzLC05OTUxMDEwMzIsNzcwMDc0MTYsLT
UyNjA2MTkyMCwtNjkzMzQyOTcxLDEzMjUwMzc3MTQsNDI4NDE2
NDMwXX0=
-->