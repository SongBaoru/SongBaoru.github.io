---
title: Java基础
date: 2023-05-02 21:23:04
tags: java
---

# JAVA中Scanner的使用及nextLine和nextInt等混用报错问题解决

> 参考出处：https://blog.csdn.net/qq_38367575/article/details/120420633

Scanner类用于扫描从控制台输入的数据，可以接收字符串和基本数据类型的数据。位于java.util.Scanner包中。

## Scanner常用方法

1. String  next(); 

   作用：接收控制台输入的一个字符串。

   注意：以空格和回车为结束符 

2. String  nextLine(); 

   作用：接收控制台输入的一个字符串。 

   注意：以回车为结束符

3. int  nextInt(); 

   作用：接收控制台输入的一个int类型的数据。

   注意：以空格和回车为结束符 

4. double  nextDouble(); 

   作用：接收控制台输入的一个double类型的数据。

   注意：以空格和回车为结束符  

5. boolean  nextBoolean(); 

   作用：接收控制台输入的一个boolean类型的数据。

   注意：以空格和回车为结束符 

示例：

```java
//步骤1、创建Scanner类的一个对象。 
Scanner scanner = new Scanner(System.in); 	 
//步骤2、通过scanner调用next等方法，接收控制台输入的数据。 
System.out.println(“姓名：”);
String name = scanner.next();
```

## 常见问题

<img src="./Java基础/image-20230517204729870.png" alt="image-20230517204729870" style="zoom:80%;" />

原因分析：原因是nextDouble()、nextInt()等方法，能够读出空格或者回车前的字符串或者数字，并且下次读时不会切换到空格或者回车之后（也就是还在18之后继续读，不会切换到下一行）。而nextLine则会读取本行且下次自动切换到下一行开头读。

## 解决方法

在nextDouble()、nextInt()等方法后面都要加上一句不需要赋值的nextLine()，把读的位置切换到下一行

示例：

```Java
import java.util.Scanner;

public class Tess {
	public static void main(String[] args) {
		Scanner scanner = new Scanner(System.in);
		int age = scanner.nextInt();
		scanner.nextLine();//把读的位置切换到下一行
		String name = scanner.nextLine();
		double salary = scanner.nextDouble();
		scanner.nextLine();//把读的位置切换到下一行
		System.out.println("age:" + age);
		System.out.println("name:" + name);
		System.out.println("salary:" + salary);
	}
}
```

# IO流

## 字节流和字符流

> 根据数据流向的方向不同，分为输入流和输出流

IO流的输入\出源有控制台、文件、网络、数据库...

常用的IO流：分字节流（以字节，8bit为单位对数据进行读写操作）和字符流（以字符为单位，一个字符占2个字节）



输入字节流包含：文件（File）、对象（Object）、字节数组（ByteArray）、管道（Pipe）、过滤器（Filter）、缓冲字符串、顺序输入流（InputStream），都是InputStream（抽象类）的子类

输出字节流包含：文件（File）、对象（Object）、字节数组（ByteArray）、管道（Pipe）、过滤器（Filter）输出流（OutputStream），都是OutputStream（抽象类）的子类



输入字符流包含：字符数组（CharArray）、字符串（String）、其他输入流的缓冲区（Buffered）、管道（Piped）、过滤器（Filter）、将字节输入流转换为字符输入流（InputStream）Reader，都是Reader的子类（Reader是所有字符输入流的父类）

输出字符流包含：字符数组（CharArray）、字符串（String）、其他输出流的缓冲区（Buffered）、管道（Piped）、过滤器（Filter）、将字节输入流转换为字符输入流（InputStream）Writer，都是Writer的子类（Writer是所有字符输出流的父类）

## 节点流和处理流

节点流是低级流，直接与数据源相连，并且进行读写；处理流是高级流，不直接和数据源相连（采用装饰器模式对节点流进行封装），主要用于消除不同节点实现上的差异。

节点流包含：FileInputStream、FileOutputStream、FileReader、FileWriter

处理流包含：BufferInputStream（[FilterInputStream](https://docs.oracle.com/javase/7/docs/api/java/io/FilterInputStream.html)的实现类）、BufferOutputStream、BufferReader、BufferWriter

处理流相比节点流的优点：通过增加缓存提高了数据输入和输出的效率、封装了一系列高级方法来完成一次性大批量数据的输入和输出

## 内存映射文件技术

含义：操作系统利用虚拟内存将文件映射到内存中，然后，这个文件就可以被当作内存数据来访问

关键技术优势：

- 让操作系统负责文件的读写，应用程序只需要处理内存数据，就可以实现IO操作；
- 可以实现共享内存，内存映射文件可以被多个进程同时访问；内存映射文件技术涉及的内存在Java的堆空间之外；
- 大幅提升文件数据的输入输出速度

Java的NIO包支持内存映射技术，实现方式是通过**MapperdBytyBuffer**读写内存