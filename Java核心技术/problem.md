1. 程序清单5-15未按原文编写
> P199，”**ObjectAnalyzer** 将记录已经被访问过的对象“
2.  Java有一个限制，无法构造泛型类型`T`的数组
> P237， 6.3.5

如果允许创建泛型数组，就绕过了泛型的编译时的类型检查
3.  
> P241

Java泛型仅针对引用类型，如果使用Function，会将代码中的int进行装箱，从而在性能上付出代价。java.util.function包针对基本类型的int、double和long提供支持，当输入或/和输出为基本类型时，可以避免自动装箱的操作。
4. **闭包（closure）**
5. 内部类不能有`static`方法
java类加载顺序，首先加载类，执行static变量初始化，接下来执行对象的创建，如果我们要执行代码中的变量int a 初始化，那么必须先执行加载外部类，再加载内部类，最后初始化静态变量 a ,问题就出在加载内部类上面，我们可以把内部类看成外部类的非静态成员，它的初始化必须在外部类对象创建后以后进行，要加载内部类必须在实例化外部类之后完成 ，java虚拟机要求所有的静态变量必须在对象创建之前完成，这样便产生了矛盾。  
(有点绕，呵呵)  
而java常量放在内存中常量池，它的机制与变量是不同的，编译时，加载常量是不需要加载类的，所以就没有上面那种矛盾。
6.  可以通过显式地命名将外部类引用设置为其他的对象
> P247
7. java编译 Error: Could not find or load main class java执行包main方法
在java源文件开头有包声明语句，编译的时候需要指定生成的class文件路径.
``` java
javac -d your_path your_class.java
```
8.  注释
> P250
> 
翻译有问题，关于**私有内部类**
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTczMjEzNzkyMiwyNTYxMjcwODMsMjA3OT
Q0NTkwMSwtNjIzNTQ1ODI0LDE3MjE2MzI3MTcsNDU5MTYzMTc4
LC05Njc3MDcxOTVdfQ==
-->