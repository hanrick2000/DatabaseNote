# Java Basics

## 1. Introduction

### 输入和输出

主要是理解一下java的pkg应用，和c++的stdin，stdout类似

* System.in : 主要是输入
* System.out：主要是输出
  * print : 单纯打印
  * println ：打印一行，自动换行
  * printf：按照指定格式打印

```java
// java的输入和输出都是由System来控制，输入是in，输出是out
// 这里先初始化一个Scanner，如果是sting就直接用next，不然就用nextInt
// 然后使用 scanner.close来关闭，非常像python读取文本的操作
Scanner scanner = new Scanner(System.in);
String myString = scanner.next();
int myInt = scanner.nextInt();
scanner.close();

// 输出直接调用 System.out.println就行
System.out.println("myString is: " + myString);
System.out.println("myInt is: " + myInt);

// 如何需要设定输出的固定宽度，使用printf
// %s, %d 分别代表String和digit, 这里设定了长度为15，位数为3
System.out.printf("%-15s%03d%n", s1, x);
```

Scanner主要的作用就是获取元素

* scanner.next\(\)
* scanner.nextDouble\(\)
* scanner.nextLine\(\)
* scanner.nextInt\(\)
* scanner.hasNext\(\)

### Loops

java的loops和其他语言一样，也是那几个控制流

* for
* while

```java
// for 循环
for (int i = 1; i <= 10; i++) {
     // command
     }
```

java比较复杂，所有的数学运算还需要加载math的包

{% embed url="https://blog.csdn.net/whq19890827/article/details/51475553" %}

{% embed url="https://www.hackerrank.com/domains/java?filters%5Bsubdomains%5D%5B%5D=java-introduction" %}

### Data Types



