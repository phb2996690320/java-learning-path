> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/D_boj/article/details/132257791)

**Java 语言入门到精通章节**

1.  [学习笔记：Java - 基础篇（第一部分）_ljtxy.love 的博客 - CSDN 博客](https://blog.csdn.net/D_boj/article/details/132192272)
2.  [学习笔记：Java - 中级篇（第二部分）_ljtxy.love 的博客 - CSDN 博客](https://blog.csdn.net/D_boj/article/details/132224349)
3.  [学习笔记：Java - 高级篇（第三部分）_ljtxy.love 的博客 - CSDN 博客](https://blog.csdn.net/D_boj/article/details/132241235)
4.  [学习笔记：Java - 进阶篇（一）（第四部分）_ljtxy.love 的博客 - CSDN 博客](https://blog.csdn.net/D_boj/article/details/132257791)
5.  [学习笔记：Java - 进阶篇（二）（第五部分）_ljtxy.love 的博客 - CSDN 博客](https://blog.csdn.net/D_boj/article/details/132257812)
6.  [学习笔记：Java8 新特性_ljtxy.love 的博客 - CSDN 博客](https://blog.csdn.net/D_boj/article/details/132257818)

#### 文章目录

*   [20.Stream 流](#20Stream_11)
*   *   [20.1 概述](#201_136)
    *   *   [20.1.1 定义](#2011_138)
        *   [20.1.2 思想](#2012_146)
        *   [20.1.3 使用步骤](#2013_154)
    *   [20.2 作用](#202_165)
    *   [20.3 获取流](#203_169)
    *   [20.4 中间方法](#204_222)
    *   *   [20.4.1filter（）](#2041filter_226)
        *   [20.4.2limit（）](#2042limit_250)
        *   [20.4.3skip（）](#2043skip_258)
        *   [20.4.4distinct（）](#2044distinct_266)
        *   [20.4.5concat（）](#2045concat_286)
        *   [20.4.5map（）](#2045map_297)
    *   [20.5 终结方法](#205_331)
    *   *   [20.5.1forEach（）](#2051forEach_335)
        *   [20.5.2count（）](#2052count_361)
        *   [20.5.3toArray（）](#2053toArray_374)
        *   [20.5.4collect（）](#2054collect_414)
*   [21. 方法引用](#21_450)
*   *   [21.1 概述](#211_536)
    *   *   [21.1.1 定义](#2111_538)
        *   [21.1.2 规则](#2112_542)
        *   [21.1.3 方法引用符](#2113_549)
        *   [21.1.4 分类](#2114_604)
    *   [21.2 作用](#212_612)
    *   [21.3 引用静态方法](#213_624)
    *   [21.4 引用成员方法](#214_663)
    *   [21.5 引用构造方法](#215_753)
    *   [21.6 类名引用成员方法](#216_806)
    *   [21.7 引用数组的构造方法](#217_857)
*   [22. 异常处理](#22_887)
*   *   [22.1 概述](#221_964)
    *   *   [22.1.1 定义](#2211_966)
        *   [22.1.2 异常体系](#2212_970)
        *   [22.1.3 异常分类](#2213_981)
        *   [22.1.5 异常处理机制](#2215_988)
    *   [22.2 作用](#222_1006)
    *   [22.3 异常捕获](#223_1022)
    *   [22.4 异常处理常见问题](#224_1063)
    *   [22.5 异常常用方法](#225_1149)
    *   *   [22.5.1getMessage（）](#2251getMessage_1153)
        *   [22.5.2toString（）](#2252toString_1166)
        *   [22.5.3printStackTrace（）](#2253printStackTrace_1179)
    *   [22.6 异常抛出处理](#226_1193)
    *   *   [22.6.1 声明异常 throws 关键字](#2261throws_1200)
        *   [22.6.2 抛出异常 throw 关键字](#2262throw_1214)
    *   [22.7 自定义异常](#227_1234)
    *   *   [22.7.1 概述](#2271_1236)
        *   [22.7.2 使用步骤](#2272_1240)
    *   [22.8 最终执行](#228_1307)
*   [23.File 类](#23File_1341)
*   *   [23.1 概述](#231_1362)
    *   *   [23.1.1 定义](#2311_1364)
        *   [23.1.2 路径](#2312_1368)
    *   [23.2 作用](#232_1372)
    *   [23.3 构造方法](#233_1380)
    *   [23.4 常用成员方法](#234_1411)
    *   *   [23.4.1 判断、获取](#2341_1413)
        *   *   [23.4.1.1isDirectory()、isFIle()、exists()](#23411isDirectoryisFIleexists_1419)
            *   [23.4.1.2length()](#23412length_1441)
            *   [23.4.1.3getAbsolutePath()](#23413getAbsolutePath_1458)
            *   [23.4.1.4getPath()](#23414getPath_1470)
            *   [23.4.1.5getName()](#23415getName_1482)
            *   [23.4.1.6lastModified()](#23416lastModified_1499)
        *   [23.4.2 创建、删除](#2342_1507)
        *   *   [23.4.2.1createNewFile()](#23421createNewFile_1515)
            *   [23.4.2.2mkdir()](#23422mkdir_1530)
            *   [23.4.2.3mkdirs()](#23423mkdirs_1543)
            *   [23.4.2.4delete()](#23424delete_1555)
        *   [23.4.3 获取并遍历](#2343_1572)
        *   *   [23.4.3.1listRoots()](#23431listRoots_1576)
            *   [23.4.3.2list()](#23432list_1583)
            *   [23.4.3.3list(FilenameFilter filter)](#23433listFilenameFilter_filter_1593)
            *   [23.4.3.4listFiles()（掌握）](#23434listFiles_1615)
            *   [23.4.3.6listFiles(FileFilter filter)](#23436listFilesFileFilter_filter_1638)
            *   [23.4.3.5listFiles(FilenameFilter filter)](#23435listFilesFilenameFilter_filter_1652)
*   [24.IO 流](#24IO_1666)
*   *   [24.1 概述](#241_1670)
    *   *   [24.1.1 定义](#2411_1689)
        *   [24.1.2 分类](#2412_1695)
        *   [24.1.3 输入流与输出流](#2413_1699)
        *   [24.1.4 字节流与字符流](#2414_1711)
        *   [24.1.5 流体系](#2415_1725)
    *   [24.2 字节基本流](#242_1729)
    *   *   *   *   [write(byte[] b)](#writebyte_b_1762)
        *   [24.2.1 输入流 - FileIntputStream](#2421FileIntputStream_1789)
        *   *   [24.2.1.1 概述](#24211_1791)
            *   [24.2.1.2 基本用例](#24212_1795)
            *   [24.2.1.3 构造方法](#24213_1821)
            *   *   [24.2.1.3.1FileInputStream(File file)](#242131FileInputStreamFile_file_1826)
                *   [24.2.1.3.2FileInputStream(String name)](#242132FileInputStreamString_name_1840)
            *   [24.2.1.4 常用成员方法](#24214_1853)
            *   *   [24.2.1.4.1read()](#242141read_1857)
                *   [24.2.1.4.2read(byte[] b)](#242142readbyte_b_1871)
                *   [24.2.1.4.3close()](#242143close_1885)
            *   [24.2.1.5 一次读取多个字节](#24215_1898)
            *   [24.2.1.6 循环读取](#24216_1935)
            *   [24.2.1.7 原理](#24217_1949)
        *   [24.3.2 输出流 - FileOutputStream](#2432FileOutputStream_1977)
        *   *   [24.3.2.1 概述](#24321_1979)
            *   [24.3.2.2 基本用例](#24322_1983)
            *   [24.3.2.3 构造方法](#24323_1996)
            *   *   [24.3.2.3.1FileOutputStream(File file)](#243231FileOutputStreamFile_file_2001)
                *   [24.3.2.3.2FileOutputStream(String name)](#243232FileOutputStreamString_name_2016)
            *   [24.3.1.4 常用成员方法](#24314_2025)
            *   *   [24.3.1.4.1write(int b)](#243141writeint_b_2029)
                *   [24.3.1.4.2write(byte[] b)](#243142writebyte_b_2042)
                *   [24.3.1.4.3write(byte[] b, int off, int len)](#243143writebyte_b_int_off_int_len_2056)
                *   [24.3.1.4.4close()](#243144close_2067)
            *   [24.3.1.5 换行和续写](#24315_2080)
            *   [24.3.1.3 原理](#24313_2120)
        *   [24.3.3 案例 -- 文件拷贝](#2433_2142)
    *   [24.3 字符基本流](#243_2191)
    *   *   *   *   [FileReader(File file)](#FileReaderFile_file_2207)
                *   [FileReader(String fileName)](#FileReaderString_fileName_2209)
                *   [FileWriter(File file)](#FileWriterFile_file_2213)
                *   [FileWriter(String pathName)](#FileWriterString_pathName_2215)
                *   [read（）](#read_2221)
                *   [read(char[] cbuf)](#readchar_cbuf_2223)
                *   [write(int/string c)](#writeintstring_c_2227)
                *   [write(char[] cbuf)](#writechar_cbuf_2229)
                *   [write(char[] cbuf, int off, int len)](#writechar_cbuf_int_off_int_len_2231)
                *   [flush()](#flush_2233)
        *   [24.3.1 输入流 - FileReader](#2431FileReader_2244)
        *   *   [24.3.1.1 概述](#24311_2246)
            *   [24.3.1.2 基本用例](#24312_2250)
            *   [24.3.1.3 构造方法](#24313_2280)
            *   *   [24.3.1.3.1FileReader(File file)](#243131FileReaderFile_file_2285)
                *   [24.3.1.3.2FileReader(String fileName)](#243132FileReaderString_fileName_2295)
            *   [24.3.1.4 常用成员方法](#24314_2304)
            *   *   [24.3.1.4.1read（）](#243141read_2309)
                *   [24.3.1.4.2read(char[] cbuf)](#243142readchar_cbuf_2322)
                *   [24.3.1.4.4close()](#243144close_2338)
            *   [24.3.1.5 循环读取](#24315_2351)
            *   [24.3.1.6 原理](#24316_2385)
        *   [24.3.2 输出流 - FileWriter](#2432FileWriter_2427)
        *   *   [24.3.2.1 概述](#24321_2429)
            *   [24.3.2.2 基本用例](#24322_2433)
            *   [24.3.2.3 构造方法](#24323_2444)
            *   *   [24.3.2.3.1FileWriter(File file)](#243231FileWriterFile_file_2448)
                *   [24.3.2.3.2FileWriter(String pathName)](#243232FileWriterString_pathName_2463)
            *   [24.3.2.4 常用成员方法](#24324_2477)
            *   *   [24.3.2.3.3write(int/string c)](#243233writeintstring_c_2483)
                *   [24.3.2.3.4write(char[] cbuf)](#243234writechar_cbuf_2498)
                *   [24.3.2.3.5write(char[] cbuf, int off, int len)](#243235writechar_cbuf_int_off_int_len_2508)
                *   [24.3.2.3.6flush()](#243236flush_2518)
                *   [24.3.2.3.7close()](#243237close_2547)
            *   [24.3.2.5 续写和换行](#24325_2560)
            *   [24.3.2.6 原理](#24326_2586)
    *   [24.4 字符集](#244_2598)
    *   *   [24.4.1 定义](#2441_2608)
        *   [24.4.2 乱码产生原因](#2442_2616)
        *   [24.4.3Java 中编码与解码](#2443Java_2624)
    *   [24.4 缓冲流](#244_2653)
    *   *   [24.4.1 概述](#2441_2715)
        *   *   [24.4.1.1 定义](#24411_2717)
            *   [24.4.4.2 体系结构](#24442_2726)
        *   [24.4.2 字节缓冲流](#2442_2730)
        *   *   [24.4.2.1 输入流 - BufferedInputStream](#24421BufferedInputStream_2732)
            *   *   [24.4.2.1.1 概述](#244211_2734)
                *   [24.4.2.1.2 基本用例](#244212_2738)
                *   [24.4.2.1.3 构造方法](#244213_2749)
                *   [24.4.2.1.4 常用成员方法](#244214_2767)
            *   [24.4.2.2 输出流 - BufferedOutputStream](#24422BufferedOutputStream_2771)
            *   *   [24.4.2.2.1 概述](#244221_2773)
                *   [24.4.2.2.2 基本用例](#244222_2775)
                *   [24.4.2.2.3 构造方法](#244223_2788)
                *   [24.4.2.2.4 常用成员方法](#244224_2803)
            *   [24.4.2.3 案例 -- 文件拷贝](#24423_2807)
            *   [24.4.4.4 原理](#24444_2846)
        *   [24.4.3 字符缓冲流](#2443_2862)
        *   *   [24.4.3.1 输入流 - BufferedReader](#24431BufferedReader_2864)
            *   *   [24.4.3.1.1 概述](#244311_2866)
                *   [24.4.3.1.2 基本用例](#244312_2870)
                *   [24.4.3.1.3 构造方法](#244313_2881)
                *   [24.4.3.1.4 常用成员方法](#244314_2891)
                *   *   [24.4.3.1.4.1readLine()](#2443141readLine_2896)
            *   [24.4.3.2 输出流 - BufferedWriter](#24432BufferedWriter_2905)
            *   *   [24.4.3.2.1 概述](#244321_2907)
                *   [24.4.3.2.2 基本用例](#244322_2911)
                *   [24.4.3.2.3 构造方法](#244323_2924)
                *   [24.4.3.2.4 常用成员方法](#244324_2933)
            *   [24.4.3.3 成员方法](#24433_2945)
            *   [24.4.4.4 原理](#24444_2951)
    *   [24.5 转换流](#245_2957)
    *   *   [24.5.1 概述](#2451_2965)
        *   *   [24.5.1.1 定义](#24511_2967)
            *   [24.5.1.1 体系结构](#24511_2973)
        *   [24.5.2 输入流 - InputStreamReader](#2452InputStreamReader_2977)
        *   *   [24.5.2.1 构造方法](#24521_2979)
            *   [24.5.2.2 常用成员方法](#24522_2991)
        *   [24.5.3 输出流 - OutputStreamWriter](#2453OutputStreamWriter_3038)
        *   *   [24.5.3.1 构造方法](#24531_3040)
            *   [24.5.3.2 常用成员方法](#24532_3052)
        *   [24.5.4 案例 - 文件编码转换](#2454_3090)
    *   [24.6 序列化流](#246_3116)
    *   *   [24.6.1 概述](#2461_3124)
        *   *   [24.6.1.1 定义](#24611_3126)
            *   [24.6.1.1 体系结构](#24611_3130)
        *   [24.6.2 序列化流 - ObjectOutputStream](#2462ObjectOutputStream_3134)
        *   *   [24.6.2.1 概述](#24621_3136)
            *   [24.6.2.2 构造方法](#24622_3140)
            *   [24.6.2.3 成员方法](#24623_3172)
        *   [24.6.3 反序列化流 - ObjectInputStream](#2463ObjectInputStream_3183)
        *   *   [24.6.2.1 概述](#24621_3185)
            *   [24.6.3.2 构造方法](#24632_3189)
            *   [24.6.3.2 成员方法](#24632_3229)
        *   [24.6.4 案例 - 多个文件序列化与反序列化](#2464_3242)
    *   [24.7 打印流](#247_3278)
    *   *   [24.7.1 概述](#2471_3286)
        *   *   [24.7.1.1 定义](#24711_3288)
            *   [24.7.1.2 体系结构](#24712_3296)
        *   [24.7.2 字符打印流 PrintWriter](#2472PrintWriter_3300)
        *   *   [24.7.2.1 概述](#24721_3302)
            *   [24.7.2.2 构造方法](#24722_3304)
            *   [24.7.2.3 成员方法](#24723_3319)
        *   [24.7.3 字节打印流 PrintStream](#2473PrintStream_3330)
        *   *   [24.7.3.1 概述](#24731_3332)
            *   [24.7.3.2 构造方法](#24732_3334)
            *   [24.7.3.3 成员方法](#24733_3349)
        *   [24.7.4 案例 - 改变打印流](#2474_3368)
    *   [24.8 压缩流和解压缩流（了解）](#248_3391)
    *   *   [24.8.1 概述](#2481_3397)
        *   *   [24.8.1.1 含义](#24811_3399)
            *   [24.8.1.2 体系结构](#24812_3403)
        *   [24.8.2 解压缩流](#2482_3407)
        *   [24.8.3 压缩流](#2483_3459)
    *   [24.9 工具类](#249_3557)
    *   *   [24.9.1Commos-io](#2491Commosio_3568)
        *   *   [24.9.1.1 概述](#24911_3570)
            *   [24.9.1.2FileUtils 类常用方法](#24912FileUtils_3578)
            *   [24.9.1.3IOUtils 类常用方法](#24913IOUtils_3582)
            *   [24.9.1.4 案例 - 基本使用](#24914_3586)
        *   [24.9.2Hutool（重点）](#2492Hutool_3625)
        *   *   [24.9.2.1 概述](#24921_3629)
            *   [14.9.2.2 工具类](#14922_3633)
            *   [14.9.2.3 案例 - 基本使用](#14923_3637)
*   [知识加油站](#_3685)
*   *   [1.IDEA 中查看此方法的源码](#1IDEA_3687)
    *   [2.IDEA 中包裹代码快捷键](#2IDEA_3697)
    *   [3.IDEA 中提取代码快捷键](#3IDEA_3703)
    *   [4.Java 中异常处理的扩展方式](#4Java_3709)
    *   [5.StringBuider 和 StringBuffer 区别](#5StringBuiderStringBuffer_3713)
    *   [6.Arrays 数组工具类](#6Arrays_3727)

20.Stream 流
-----------

笔记小结：

> 1.  概述：在 [Java 8](https://so.csdn.net/so/search?q=Java%208&spm=1001.2101.3001.7020) 中，Stream 是一种**处理集合**的机制，它可以对集合进行各种操作（**过滤**、**映射**、**排序**等）并生成新的集合，同时支持[并行处理](https://so.csdn.net/so/search?q=%E5%B9%B6%E8%A1%8C%E5%A4%84%E7%90%86&spm=1001.2101.3001.7020)
>     
> 2.  作用：结合了 Lambda 表达式，**简化**集合、数组的**操作**
>     
> 3.  使用步骤：
>     
>     1.  **获取** Stream **流对象**
>         
>         *   Collection 体系集合：使用默认方法 stream() 生成流， default Stream stream()
>             
>         *   Map 体系集合：把 Map 转成 Set 集合，间接的生成流
>             
>         *   数组：通过 Arrays 工具类中的静态方法 stream 生成流
>             
>         *   同种数据类型的多个零散数据：通过 Stream 接口的静态方法 of(T… values) 生成流
>             
>     2.  使用**中间方法处理**数据
>         
>         *   **filter**：**过滤**
>             
>             ```
>             list.stream().filter(new Predicate<String>() {
>                 @Override
>                 public boolean test(String s) {
>                     return s.startsWith("张");
>                 }
>             })
>             
>             ```
>             
>         *   **limit**：**获取前几**个元素
>             
>             ```
>             list.stream().limit(3)
>             
>             ```
>             
>         *   **skip**：**跳过**前几个元素
>             
>             ```
>             list.stream().skip(2)
>             
>             ```
>             
>         *   **distinct**：元素**去重**，（注意，当使用自定义对象进行去重时，依赖 **hashCode** 和 **equals** 方法)
>             
>             ```
>             list1.stream().distinct()
>             
>             ```
>             
>         *   **concat**：**合并** a 和 b 两个流为一个**流**
>             
>             ```
>             Stream.concat(list1.stream(),list2.stream())
>             //注意：在使用时，此处使用Stream对象中的concat静态方法
>             
>             ```
>             
>         *   **map**：**转换**流中的**数据类型**
>             
>             ```
>             // Function<原本数据类型，需要转换的目标数据类型>
>             list.stream().map(new Function<String, Integer>() {
>                 @Override
>                 public Integer apply(String s) {
>                     String[] arr = s.split("-");
>                     String ageString = arr[1];
>                     int age = Integer.parseInt(ageString);
>                     return age;
>                 }
>             })
>             
>             ```
>             
>     3.  使用**终结方法处理**数据
>         
>         *   **forEach**：遍历
>             
>             ```
>             list.stream().forEach(new Consumer<String>() {
>                         @Override
>                         public void accept(String s) {
>                             System.out.println(s);
>                         }
>                     })
>             
>             ```
>             
>         *   **count**：统计
>             
>             ```
>             list.stream().count();
>             
>             ```
>             
>         *   **toArray**：收集流中的数据，放到数组中
>             
>             ```
>             list.stream().toArray(new IntFunction<String[]>() {
>                 @Override
>                 public String[] apply(int value) {
>                     return new String[value];
>                 }
>             });
>             
>             System.out.println(Arrays.toString(arr));
>             
>             ```
>             
>         *   **collect**：收集流中的数据，放到集合中
>             
>             ```
>             Map<String, Integer> map = list.stream()
>                 .filter(s -> "男".equals(s.split("-")[1]))
>                 .collect(Collectors.toMap(
>                     new Function<String, String>() {
>                         @Override
>                         public String apply(String s) {
>                             return s.split("-")[0];
>                         }
>                     },
>                     new Function<String, Integer>() {
>                         @Override
>                         public Integer apply(String s) {
>                             return Integer.valueOf(s.split("-")[2]);
>                         }
>                     }
>                 ));
>             
>             ```
>             

### 20.1 概述

#### 20.1.1 定义

​ 在 Java 8 中，Stream 是一种处理集合的机制，它可以对集合进行各种操作（过滤、映射、排序等）并生成新的集合，同时支持并行处理。

​ 简单来说，Stream 流就是一个可以被多次处理的容器，可以通过一些链式操作来实现对元素的转换和处理。Stream 流的操作分为中间操作和结束操作，中间操作返回的是另一个流，结束操作返回的不是流，而是一个计算结果。

​ 使用 Stream 流可以简化代码实现，并且由于其并行处理能力，可以大大提升代码运行效率

#### 20.1.2 思想

![](https://i-blog.csdnimg.cn/blog_migrate/a89d68c2eb72980c571aa5f4c8ded27e.png)

说明：

> ​ 一层一层过滤，最终获取想要的结果

#### 20.1.3 使用步骤

*   获取 Stream 流
    *   创建一条流水线, 并把数据放到流水线上准备进行操作
*   中间方法
    *   流水线上的操作
    *   一次操作完毕之后, 还可以继续进行其他操作
*   终结方法
    *   一个 Stream 流只能有一个终结方法
    *   是流水线上的最后一个操作

### 20.2 作用

​ Stream 流的主要作用是对数据进行集合操作和函数式编程，可以实现高效的数据筛选、排序、过滤、分组、统计等操作

### 20.3 获取流

![](https://i-blog.csdnimg.cn/blog_migrate/0eda2e59aa34cd1f54c89afc9b8b1ae1.png)

获取方式：

*   Collection 体系集合：使用默认方法 stream() 生成流， default Stream stream()
    
*   Map 体系集合：把 Map 转成 Set 集合，间接的生成流
    
*   数组：通过 Arrays 中的静态方法 stream 生成流
    
*   同种数据类型的多个数据：通过 Stream 接口的静态方法 of(T… values) 生成流
    

示例代码：

```
public class StreamDemo {
    public static void main(String[] args) {
        //Collection体系的集合可以使用默认方法stream()生成流
        List<String> list = new ArrayList<String>();
        Stream<String> listStream = list.stream();

        Set<String> set = new HashSet<String>();
        Stream<String> setStream = set.stream();

        //Map体系的集合间接的生成流
        Map<String,Integer> map = new HashMap<String, Integer>();
        Stream<String> keyStream = map.keySet().stream();
        Stream<Integer> valueStream = map.values().stream();
        Stream<Map.Entry<String, Integer>> entryStream = map.entrySet().stream();

        //数组可以通过Arrays中的静态方法stream生成流
        String[] strArray = {"hello","world","java"};
        Stream<String> strArrayStream = Arrays.stream(strArray);
      
      	//同种数据类型的多个数据可以通过Stream接口的静态方法of(T... values)生成流
        Stream<String> strArrayStream2 = Stream.of("hello", "world", "java");
        Stream<Integer> intStream = Stream.of(10, 20, 30);
    }
}

```

说明：

> 在**集合**中，单列集合可以通过. stream（）方法获取 Stream 流。双列集合可以先转换为单列集合，再进行获取
> 
> 在数组中，可以通过 Arrays 工具类的. stream（）方法获取 Stream 流
> 
> 在**零散数据**中，可以通过 Stream.of（）方法获取 Stream 流。
> 
> 注意，零散数据需要是同种数据类型、Stream.of() 中的参数类型，一定是是引用数据类型的，若是基本数据类型则不会完成自动装箱的操作，而是会将整个基本数据类型当成一个元素看待

### 20.4 中间方法

![](https://i-blog.csdnimg.cn/blog_migrate/7132445816f12cecfca744339b228d03.png)

#### 20.4.1filter（）

```
ArrayList<String> list = new ArrayList<>();
Collections.addAll(list, "张无忌", "周芷若", "赵敏", "张强", "张三丰", "张翠山", "张良", "王二麻子", "谢广坤");
// filter   过滤  把张开头的留下，其余数据过滤不要
list.stream().filter(new Predicate<String>() {
    @Override
    public boolean test(String s) {
        //如果返回值为true，表示当前数据留下
        //如果返回值为false，表示当前数据舍弃不要
        return s.startsWith("张");
    }
}).forEach(s -> System.out.println(s));

/*	因为filter中的参数类型为函数式接口，
	而Predicate接口中又只有一个抽象方法test，
	因此可以通过Lambda方式进行简化	*/
list.stream()
    .filter(s -> s.startsWith("张"))
    .filter(s -> s.length() == 3)
    .forEach(s -> System.out.println(s)

```

#### 20.4.2limit（）

```
ArrayList<String> list = new ArrayList<>();
Collections.addAll(list, "张无忌", "周芷若", "赵敏", "张强", "张三丰", "张翠山", "张良", "王二麻子", "谢广坤");
list.stream().limit(3).forEach(s -> System.out.println(s)); // "张无忌", "周芷若", "赵敏"

```

#### 20.4.3skip（）

```
ArrayList<String> list = new ArrayList<>();
Collections.addAll(list, "张无忌", "周芷若", "赵敏", "张强");
list.stream().skip(2).forEach(s -> System.out.println(s)); // "赵敏", "张强"

```

#### 20.4.4distinct（）

```
ArrayList<String> list1 = new ArrayList<>();
Collections.addAll(list1, "张无忌","张无忌","张无忌", "张强", "张三丰", "张翠山", "张良", "王二麻子", "谢广坤");

// 此处并没有重写equals和hashCode方法，因为list的类型为String，Java已经完成了这两个方法的重写
list1.stream().distinct().forEach(s -> System.out.println(s));

```

说明：

> ​ distinct() 方法底层是 new 了 HashSet, 而 HashSet 进行去重需要依赖 equals 和 hashCode 方法。
> 
> ![](https://i-blog.csdnimg.cn/blog_migrate/ca3fd952e3c14e4b24cf6a4af3f1613b.png)

注意：

> 若需要将自定义对象进行去重，需要重写 equals 和 hashCode 方法

#### 20.4.5concat（）

```
ArrayList<String> list1 = new ArrayList<>();
Collections.addAll(list1, "张无忌","张无忌","张无忌", "张强", "张三丰", "张翠山", "张良", "王二麻子", "谢广坤");

ArrayList<String> list2 = new ArrayList<>();
Collections.addAll(list2, "周芷若", "赵敏");
Stream.concat(list1.stream(),list2.stream()).forEach(s -> System.out.println(s));

```

#### 20.4.5map（）

```
ArrayList<String> list = new ArrayList<>();
Collections.addAll(list, "张无忌-15", "周芷若-14", "赵敏-13", "张强-20", "张三丰-100", "张翠山-40", "张良-35", "王二麻子-37", "谢广坤-41");

// new Function<原本数据类型，需要转换到的数据类型>
list.stream().map(new Function<String, Integer>() {
    @Override
    public Integer apply(String s) {
        String[] arr = s.split("-");
        String ageString = arr[1];
        int age = Integer.parseInt(ageString);
        return age;
    }
}).forEach(s-> System.out.println(s));

System.out.println("------------------------");

// 同样，因为map中的参数为函数式接口，并且函数中只有一个抽象类方法，因此可以使用Lambda表达式进行简化
list.stream()
    .map(s-> Integer.parseInt(s.split("-")[1]))
    .forEach(s-> System.out.println(s));

```

说明：底层源码

> ![](https://i-blog.csdnimg.cn/blog_migrate/f98b0bf5cb0b7608d70ecc9465d1f0ef.png)

注意：

> *   中间方法，返回新的 Stream 流，原来的 Stream 流只能使用一次（流在使用一次后会进行自动关闭），建议使用链式编程
> *   修改 Stream 流中的数据，不会影响原来集合或者数组中的数据

### 20.5 终结方法

![](https://i-blog.csdnimg.cn/blog_migrate/95610cd8f15d147da028b0eda32a1992.png)

#### 20.5.1forEach（）

```
ArrayList<String> list = new ArrayList<>();
Collections.addAll(list, "张无忌", "周芷若", "赵敏", "张强", "张三丰", "张翠山", "张良", "王二麻子", "谢广坤");


//void forEach(Consumer action)           遍历

//Consumer的泛型：表示流中数据的类型
//accept方法的形参s：依次表示流里面的每一个数据
//方法体：对每一个数据的处理操作（打印）
/*list.stream().forEach(new Consumer<String>() {
            @Override
            public void accept(String s) {
                System.out.println(s);
            }
        });*/

list.stream().forEach(s -> System.out.println(s));

```

说明：

> ![](https://i-blog.csdnimg.cn/blog_migrate/b28e00ee7e58716340b3d59c13a0c0a0.png)

#### 20.5.2count（）

```
ArrayList<String> list = new ArrayList<>();
Collections.addAll(list, "张无忌", "周芷若", "赵敏", "张强", "张三丰", "张翠山", "张良", "王二麻子", "谢广坤");
long count = list.stream().count();
System.out.println(count);

```

说明：

> .count() 方法的返回值为 int 类型，因此调用完毕之后，Stream 流就会终止

#### 20.5.3toArray（）

```
Object[] arr1 = list.stream().toArray();
System.out.println(Arrays.toString(arr1));

//IntFunction的泛型：具体类型的数组
//apply的形参:流中数据的个数，要跟数组的长度保持一致
//apply的返回值：具体类型的数组
//方法体：就是创建数组
// toArray()                               收集流中的数据，放到数组中
Object[] arr1 = list.stream().toArray();
System.out.println(Arrays.toString(arr1));

	/* String[] arr = list.stream().toArray(new IntFunction<String[]>() {
            @Override
            public String[] apply(int value) {
                return new String[value];
            }
        });

        System.out.println(Arrays.toString(arr));*/

String[] arr2 = list.stream().toArray(value -> new String[value]);
System.out.println(Arrays.toString(arr2));

```

说明：

> *   toArray 方法的参数的作用：负责创建一个指定类型的数组
> *   toArray 方法的底层，会依次得到流里面的每一个数据，并把数据放到数组当中
> *   toArray 方法的返回值：是一个装着流里面所有数据的数组

细节：

> *   问题：String[] arr2 ，字符串数组，打印出来的为地址值。
> *   原因：在 Java 中打印引用类型的对象是，默认会使用 Java 中的会调 toString() 方法，toString（）方法，打印为地址值。
> *   解决：此时可以重写 toString() 方法，也可以使用 Arrays 工具类中的 toString 方法
> *   补充：Arrays.toString 方法，会帮我们重写 java 中的 toString 方法

#### 20.5.4collect（）

```
ArrayList<String> list = new ArrayList<>();
Collections.addAll(list, "张无忌-男-15", "周芷若-女-14", "赵敏-女-13", "张强-男-20",
                   "张三丰-男-100", "张翠山-男-40", "张良-男-35", "王二麻子-男-37", "谢广坤-男-41");

Map<String, Integer> map = list.stream()
    .filter(s -> "男".equals(s.split("-")[1]))
    .collect(Collectors.toMap(
        new Function<String, String>() {
            @Override
            public String apply(String s) {
                return s.split("-")[0];
            }
        },
        new Function<String, Integer>() {
            @Override
            public Integer apply(String s) {
                return Integer.valueOf(s.split("-")[2]);
            }
        }
    ));
// 或者
Map<String, Integer> map2 = list.stream()
    .filter(s -> "男".equals(s.split("-")[1]))
    .collect(Collectors.toMap(
        s -> s.split("-")[0],
        s -> Integer.parseInt(s.split("-")[2])));
System.out.println(map);// {张强=20, 张良=35, 张翠山=40, 王二麻子=37, 张三丰=100, 张无忌=15, 谢广坤=41}

```

说明：

> ​ collect 方法配合工具类 Collectors.toList 或者 Collectors.toSet 或者 Collectors.toMap 方法，可完成 Stream 流的数据获取

21. 方法引用
--------

笔记小结：

> 1.  概述：
>     
>     1.  含义：把已经有的方法拿过来用，当**做函数式接口中抽象方法的方法体**
>         
>     2.  规则：
>         
>         *   **引用处**必须是**函数式接口**
>         *   **被引用**的**方法**必须**已经存在**
>         *   **被引用**方法的**形参**和**返回值**需要跟**抽象**方法保持**一致**
>         *   被引用方法的**功能**要**满足当前需求**
>     3.  引用符： `：：`（两个连续的冒号）
>         
> 2.  作用：方法引用可以使代码更加简洁易懂，提高代码的可读性和可维护性。**避免了 Lambda 表达式**中的不必要的**重复调用**。
>     
> 3.  分类：
>     
>     *   引用**静态方法**：
>         
>         ```
>         /*格式：
>         	类::方法名*/
>         list.stream().map(Integer::parseInt).forEach(s-> System.out.println(s));
>         
>         ```
>         
>     *   引用**成员方法**：
>         
>         *   **其他**类
>             
>             ```
>             /* 格式：
>             	new对象::方法名*/
>             list.stream().filter(new stringOpration()::stringJudge)
>             
>             ```
>             
>         *   **本**类
>             
>             ```
>             /* 格式：
>             	本类:this ::方法名 */
>             list.stream().filter(this::stringJudge)
>             
>             ```
>             
>             > 细节：若该方法用于静态方法中，则需要 new 对象，因为静态方法中无 this
>             
>         *   **父**类
>             
>             ```
>             /* 格式：
>             	父类:super::方法名*/
>             list.stream().filter(super::stringJudge)
>             
>             ```
>             
>             > 细节：方法引用中没有 super 关键字
>             
>     *   引用**构造方法**：
>         
>         ```
>         /*格式:
>         	类名::new */
>         List<Student> newList2 = list.stream().map(Student::new)
>         
>         ```
>         
>     *   **类名引用成员方法**:
>         
>         ```
>         /*格式:
>         	类名::成员方法 */
>         list.stream().map(String::toUpperCase)
>         
>         ```
>         
>         > 注意：与引用成员方法区别
>         > 
>         > *   一个是**对象**进行**引用**，可以使用类中**所有方法**；
>         > *   这个则是**类名**进行**引用**，它的**参数需要与抽象方法中的第一个参数类型对应**
>         
>     *   引用**数组的构造方法**:
>         
>         ```
>         /*格式:
>         	数据类型[]::new */
>         list.stream().toArray(Integer[]::new);
>         
>         ```
>         

### 21.1 概述

#### 21.1.1 定义

​ 在 Java 中，方法引用是一种特殊的 Lambda 表达式，它是用来简化 Lambda 表达式的写法，特别是当 Lambda 表达式中只调用了一个已经存在的方法时。

#### 21.1.2 规则

*   引用处必须是函数式接口
*   彼引用的方法必须已经存在
*   被引用方法的形参和返回值需要跟抽象方法保持一致
*   被引用方法的功能要满足当前需求

#### 21.1.3 方法引用符

: : 该符号为引用运算符，而它所在的表达式被称为方法引用

: 的含义：

*   方法引用符

示例代码：

```
class JavaMainApplicationTests {
    public static int subtraction(int num1, int num2) {
        return num2 - num1;
    }

    @Test
    void contextLoads() {
        //需求：创建一个数组，进行倒序排列
        Integer[] arr = {3, 5, 4, 1, 6, 2};
        //方式一：匿名内部类

        /* Arrays.sort(arr, new Comparator<Integer>() {
            @Override
            public int compare(Integer o1, Integer o2) {
                return o2 - o1;
            }
        });*/


        //方式二：lambda表达式
        //因为第二个参数的类型Comparator是一个函数式接口
        /* Arrays.sort(arr, (Integer o1, Integer o2)->{
            return o2 - o1;
        });*/

        //方式三：lambda表达式简化格式
        //Arrays.sort(arr, (o1, o2)->o2 - o1 );


        //方式四：方法引用
        //1.引用处需要是函数式接口
        //2.被引用的方法需要已经存在
        //3.被引用方法的形参和返回值需要跟抽象方法的形参和返回值保持一致
        //4.被引用方法的功能需要满足当前的要求

        //表示引用FunctionDemo1类里面的subtraction方法
        //把这个方法当做抽象方法的方法体
        Arrays.sort(arr, JavaMainApplicationTests::subtraction);

        System.out.println(Arrays.toString(arr));
    }
}

```

#### 21.1.4 分类

1.  引用静态方法
2.  引用成员方法
3.  引用构造方法
4.  类名引用成员方法
5.  引用数组的构造方法

### 21.2 作用

​ 它是将方法作为一个对象来使用，而不是像 Lambda 表达式那样直接定义一个匿名方法。

​ 方法引用可以使代码更加简洁易懂，提高代码的可读性和可维护性。另外，方法引用还可以用于提高代码的性能，因为它**避免了 Lambda 表达式**中的不必要的**重复调用**。

![](https://i-blog.csdnimg.cn/blog_migrate/200824d871ced0734129e1c37cb73e12.png)

说明：

> 此处展示方法引用的作用区别

### 21.3 引用静态方法

格式：

```
类::方法名

```

示例：

```
//1.创建集合并添加元素
ArrayList<String> list = new ArrayList<>();
Collections.addAll(list,"1","2","3","4","5");

//2.把他们都变成int类型
/* list.stream().map(new Function<String, Integer>() {
            @Override
            public Integer apply(String s) {
                int i = Integer.parseInt(s);
                return i;
            }
        }).forEach(s -> System.out.println(s));*/



//1.方法需要已经存在
//2.方法的形参和返回值需要跟抽象方法的形参和返回值保持一致
//3.方法的功能需要把形参的字符串转换成整数

list.stream()
    .map(Integer::parseInt)
    .forEach(s-> System.out.println(s));

```

说明：

> 当需要的方法的参数已存在时，可以根据” 类名：：方法名 “，进行调用

### 21.4 引用成员方法

格式：

```
对象::成员方法
1.其他类:
	对象::方法名
2.本类:
	this ::方法名 
3.父类:
	super::方法名

```

示例（其他类）：

```
/* 格式：
	其他类:其他类对象::方法名*/
public static void main(String[] args) {
    //1.创建集合
    ArrayList<String> list = new ArrayList<>();
    //2.添加数据
    Collections.addAll(list,"张无忌","周芷若","赵敏","张强","张三丰");
    //3.其他类：
    list.stream().filter(new stringOpration()::stringJudge)
        .forEach(s-> System.out.println(s));
}

```

```
class stringOpration {
public boolean stringJudge(String s){
        return s.startsWith("张") && s.length() == 3;
    }
}

```

说明：

> 通过调用其他类对象的方法名进行调用

示例（本类）：

```
/* 格式：
	本类:this ::方法名*/
public class FunctionDemo3  {
    public static void main(String[] args) {
        //1.创建集合
        ArrayList<String> list = new ArrayList<>();
        //2.添加数据
        Collections.addAll(list,"张无忌","周芷若","赵敏","张强","张三丰");
        //3.本类，静态方法中是没有this的
        list.stream().filter(new FunctionDemo3()::stringJudge)
                .forEach(s-> System.out.println(s));
    }

    public boolean stringJudge(String s){
        return s.startsWith("张") && s.length() == 3;
    }
}

```

细节：

> 在静态方法中，是没有 this 的，因此需要调用需要 new 本类对象

示例（父类）：

```
/* 格式：
	父类:super::方法名*/
public class FunctionDemo3  {
    public static void main(String[] args) {
        //1.创建集合
        ArrayList<String> list = new ArrayList<>();
        //2.添加数据
        Collections.addAll(list,"张无忌","周芷若","赵敏","张强","张三丰");
        //3.本类，静态方法中是没有this的
        list.stream().filter(super::stringJudge)
                .forEach(s-> System.out.println(s));
    }
}

```

说明：

> 若本类 FunctionDemo3 有父类，则可以通过 super 的方法名进行调用

### 21.5 引用构造方法

```
/*格式:
	类名::new */
//1.创建集合对象
ArrayList<String> list = new ArrayList<>();
//2.添加数据
Collections.addAll(list, "张无忌,15", "周芷若,14", "赵敏,13", "张强,20", "张三丰,100", "张翠山,40", "张良,35", "王二麻子,37", "谢广坤,41");
//3.封装成Student对象并收集到List集合中
//String --> Student
/*  List<Student> newList = list.stream().map(new Function<String, Student>() {
            @Override
            public Student apply(String s) {
                String[] arr = s.split(",");
                String name = arr[0];
                int age = Integer.parseInt(arr[1]);
                return new Student(name, age);
            }
        }).collect(Collectors.toList());
        System.out.println(newList);*/


List<Student> newList2 = list.stream().map(Student::new).collect(Collectors.toList());
System.out.println(newList2);

```

说明：

> 此时，通过 Student 类名进行 new，就可以调用 Student 的构造方法。

```
public class Student {
    private String name;
    private int age;


    public Student(String str) {
        String[] arr = str.split(",");
        this.name = arr[0];
        this.age = Integer.parseInt(arr[1]);
    }
}

```

说明：

> 此时 Student 类对象中的形参和返回值需要跟抽象方法保持一致

小细节：

> 此时构造方法的返回值默认就是返回 Student 整个类对象

### 21.6 类名引用成员方法

```
/*格式:
	类名::成员方法 */
public static void main(String[] args) {
    /*

        方法引用的规则：
        1.需要有函数式接口
        2.被引用的方法必须已经存在
        3.被引用方法的形参，需要跟抽象方法的第二个形参到最后一个形参保持一致，返回值需要保持一致。
        4.被引用方法的功能需要满足当前的需求

        抽象方法形参的详解：
        第一个参数：表示被引用方法的调用者，决定了可以引用哪些类中的方法
                    在Stream流当中，第一个参数一般都表示流里面的每一个数据。
                    假设流里面的数据是字符串，那么使用这种方式进行方法引用，只能引用String这个类中的方法

        第二个参数到最后一个参数：跟被引用方法的形参保持一致，如果没有第二个参数，说明被引用的方法需要是无参的成员方法

        局限性：
            不能引用所有类中的成员方法。
            是跟抽象方法的第一个参数有关，这个参数是什么类型的，那么就只能引用这个类中的方法。

       */

    //1.创建集合对象
    ArrayList<String> list = new ArrayList<>();
    //2.添加数据
    Collections.addAll(list, "aaa", "bbb", "ccc", "ddd");
    //3.变成大写后进行输出
    //map(String::toUpperCase)
    //拿着流里面的每一个数据，去调用String类中的toUpperCase方法，方法的返回值就是转换之后的结果。
    list.stream().map(String::toUpperCase).forEach(s -> System.out.println(s));


    //String --> String
    /* list.stream().map(new Function<String, String>() {
            @Override
            public String apply(String s) {
                return s.toUpperCase();
            }
        }).forEach(s -> System.out.println(s));*/
}

```

注意：与引用成员方法做区别

> ​ **不能**引用类中的**所有成员方法**, 如果抽象方法的第一个参数是 A 类型的只能引用 A 类中的方法

### 21.7 引用数组的构造方法

```
/*格式:
	数据类型[]::new */
 public static void main(String[] args) {
        /*
        细节：
            数组的类型，需要跟流中数据的类型保持一致。
       */

        //1.创建集合并添加元素
        ArrayList<Integer> list = new ArrayList<>();
        Collections.addAll(list, 1, 2, 3, 4, 5);
        //2.收集到数组当中

        Integer[] arr2 = list.stream().toArray(Integer[]::new);
        System.out.println(Arrays.toString(arr2));

        /*Integer[] arr = list.stream().toArray(new IntFunction<Integer[]>() {
            @Override
            public Integer[] apply(int value) {
                return new Integer[value];
            }
        });*/
     
        //3.打印
    }

```

22. 异常处理
--------

笔记小结：

> 1.  概述
>     
>     *   异常：指的是程序在执行过程中，出现的**非正常**的情况，最终会**导致 JVM 的非正常停止**。
>     *   异常的体系结构
>     
>     ![](https://i-blog.csdnimg.cn/blog_migrate/fed535618f73245ef9ffea3a9190b547.png)
>     
>     *   异常分类
>         
>         1.  **编译时异常**: 除了 RuntimeExcpetion 和他的子类，其他都是编译时异常。编译阶段需要进行处理，作用在于提醒程序员。
>         2.  **运行时异常**:RuntimeException 本身和所有子类，都是运行时异常。编译阶段不报错，是程序运行时出现的。
>     *   异常的机制
>         
>         *   把异常的名称，**异常原因及异常**出现的**位置**等信息**输出在**了**控制台**
>         *   程序停止执行、**异常**下面的**代码不会**再**执行**了
> 2.  异常的作用
>     
>     *   异常是用来**查询 bug 的**关键**参考信息**
>     *   异常可以作为方法内部的一种**特殊返回值**，以便通知调用者**底层**的执行情况
> 3.  捕获异常
>     
>     *   格式
>         
>         ```
>         try{
>              编写可能会出现异常的代码
>         }catch(异常类型  e){
>              处理异常的代码
>              //记录日志/打印异常信息/继续抛出异常
>         }
>         
>         ```
>         
> 4.  灵魂四问
>     
>     ![](https://i-blog.csdnimg.cn/blog_migrate/650258e12984c4d3ba4614e14876b4c5.png)
>     
> 5.  异常方法
>     
>     ![](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAADIAAAAyCAYAAAAeP4ixAAACbklEQVRoQ+2aMU4dMRCGZw6RC1CSSyQdLZJtKQ2REgoiRIpQkCYClCYpkgIESQFIpIlkW+IIcIC0gUNwiEFGz+hlmbG9b1nesvGW++zxfP7H4/H6IYzkwZFwQAUZmpJVkSeniFJKA8ASIi7MyfkrRPxjrT1JjZ8MLaXUDiJuzwngn2GJaNd7vyP5IoIYY94Q0fEQIKIPRGS8947zSQTRWh8CwLuBgZx479+2BTkHgBdDAgGAC+fcywoyIFWqInWN9BSONbTmFVp/AeA5o+rjKRJ2XwBYRsRXM4ZXgAg2LAPzOCDTJYQx5pSIVlrC3EI45y611osMTHuQUPUiYpiVooerg7TWRwDAlhSM0TuI+BsD0x4kGCuFSRVzSqkfiLiWmY17EALMbCAlMCmI6IwxZo+INgQYEYKBuW5da00PKikjhNNiiPGm01rrbwDwofGehQjjNcv1SZgddALhlJEgwgJFxDNr7acmjFLqCyJuTd6LEGFttpmkYC91Hrk3s1GZFERMmUT01Xv/sQljjPlMRMsxO6WULwnb2D8FEs4j680wScjO5f3vzrlNJszESWq2LYXJgTzjZm56MCHf3zVBxH1r7ftU1splxxKYHEgoUUpTo+grEf303rPH5hxENJqDKQEJtko2q9zGeeycWy3JhpKhWT8+NM/sufIhBwKI+Mta+7pkfxKMtd8Qtdbcx4dUQZcFCQ2I6DcAnLUpf6YMPxhIDDOuxC4C6djoQUE6+tKpewWZ1wlRkq0qUhXptKTlzv93aI3jWmE0Fz2TeujpX73F9TaKy9CeMk8vZusfBnqZ1g5GqyIdJq+XrqNR5AahKr9CCcxGSwAAAABJRU5ErkJggg==)
> 6.  抛出异常
>     
>     *   **声明**异常 **throws 关键字**
>     *   **抛出**异常 **throw 关键字**
> 7.  自定义异常：
>     
>     *   定义异常类
>         
>         ```
>         // 业务逻辑异常
>         public class LoginException extends Exception {
>             /**
>              *
>              * @param message 表示异常提示
>              */
>             public LoginException(String message) {
>                 super(message);
>             }
>         }
>         //
>         //
>         
>         ```
>         
>         说明：
>         
>         > 1.  异常名以 Exception 结尾
>         > 2.  异常类继承 Exception
>         > 3.  异常类可生成无参，带参，空参构造方法进行详细说明
>         
> 8.  最终执行：finally 关键字
>     

### 22.1 概述

#### 22.1.1 定义

异常含义指的是程序在执行过程中，出现的非正常的情况，最终会导致 JVM 的非正常停止。

#### 22.1.2 异常体系

​ 异常机制其实是帮助我们**找到**程序中的问题，异常的根类是`java.lang.Throwable`，其下有两个子类：`java.lang.Error`与`java.lang.Exception`，平常所说的异常指`java.lang.Exception`

![](https://i-blog.csdnimg.cn/blog_migrate/0703dad6cee3bd14cfc9a8faf2eca8af.png)

*   Error：代表的系统级别错误（属于严重问题)，系统一旦出现问题，sun 公司会把这些错误封装成 Error 对象。Error 是给 sun 公司自己用的，不是给我们程序员用的。因此我们开发人员不用管它。
*   Exception：叫做异常，代表程序可能出现的问题。我们通常会用 Exception 以及他的子类来封装程序出现的问题。
*   运行时异常: RuntimeException 及其子类，编译阶段不会出现异常提醒。运行时出现的异常（如: 数组索引越界异常)
*   编译时异常: 编译阶段就会出现异常提醒的。(如︰日期解析异常)

#### 22.1.3 异常分类

![](https://i-blog.csdnimg.cn/blog_migrate/c3f488f5eeca260b99ff85f871da0f17.png)

*   **编译时期异常**:checked 异常。在编译时期, 就会检查, 如果没有处理异常, 则编译失败。(如日期格式化异常)
*   **运行时期异常**:runtime 异常。在运行时期, 检查异常. 在编译时期, 运行异常不会编译器检测 (不报错)。(如数学异常)

#### 22.1.5 异常处理机制

JVM 默认处理异常的方式

1.  把异常的名称，异常原因及异常出现的位置等信息**输出在**了**控制台**
2.  程序停止执行、**异常**下面的**代码不会**再**执行**了

示例：

```
System.out.println("狂踹瘸子那条好腿");
System.out.println(2/0);//算术异常 ArithmeticException
System.out.println("是秃子终会发光");
System.out.println("火鸡味锅巴");

```

打印效果：![](https://i-blog.csdnimg.cn/blog_migrate/dd7ccfbd0409b3d7f05e395a3da37f17.png)

### 22.2 作用

作用：

*   异常是用来查询 bug 的关键参考信息
    
    ![](https://i-blog.csdnimg.cn/blog_migrate/aaa695fdaa83bdbbabc7c4b1756d431e.png)
    
*   异常可以作为方法内部的一种特殊返回值，以便通知调用者底层的执行情况
    
    ![](https://i-blog.csdnimg.cn/blog_migrate/f81d5d885f4cdcbb4b74e6cc4fd1b8ef.png)
    
    说明：
    

> ​ 简单来说，就是抛出异常

### 22.3 异常捕获

**try-catch** 的方式就是捕获异常。

说明：

> *   **try：** 该代码块中编写可能产生异常的代码。
> *   **catch：** 用来进行某种异常的捕获，实现对捕获到的异常进行处理。
> *   **捕获异常**：Java 中对异常有针对性的语句进行捕获，可以对出现的异常进行指定方式的处理。

注意：

> ​ try 和 catch 都不能单独使用, 必须连用。

基本用例

```
/* 格式：
try{
     编写可能会出现异常的代码
}catch(异常类型  e){
     处理异常的代码
     //记录日志/打印异常信息/继续抛出异常
} */

int[] arr = {1, 2, 3, 4, 5, 6};
try{
    //可能出现异常的代码;
    System.out.println(arr[10]);//此处出现了异常，程序就会在这里创建一个ArrayIndexOutOfBoundsException对象
    //new ArrayIndexOutOfBoundsException();
    //拿着这个对象到catch的小括号中对比，看括号中的变量是否可以接收这个对象
    //如果能被接收，就表示该异常就被捕获（抓住），执行catch里面对应的代码
    //当catch里面所有的代码执行完毕，继续执行try...catch体系下面的其他代码
}catch(ArrayIndexOutOfBoundsException e){
    //如果出现了ArrayIndexOutOfBoundsException异常，我该如何处理
    System.out.println("索引越界了");
}

System.out.println("看看我执行了吗？");

```

### 22.4 异常处理常见问题

![](https://i-blog.csdnimg.cn/blog_migrate/650258e12984c4d3ba4614e14876b4c5.png)

1.  如果 try 中没有遇到问题，怎么执行？
    
    *   会把 try 里面所有的代码全部执行完毕，不会执行 catch 里面的代码
        
        ```
         int[] arr = {1, 2, 3, 4, 5, 6};
        
                try{
                    System.out.println(arr[0]);//1
                }catch(ArrayIndexOutOfBoundsException e){
                    System.out.println("索引越界了");
                }
        
                System.out.println("看看我执行了吗？");//看看我执行了吗？
        
        ```
        
2.  如果 try 中可能会遇到多个问题，怎么执行？
    
    *   会写多个 catch 与之对应
        
        ```
        //JDK7
        int[] arr = {1, 2, 3, 4, 5, 6};
        
        try{
            System.out.println(arr[10]);//ArrayIndexOutOfBoundsException
            System.out.println(2/0);//ArithmeticException，报错则跳过此句
            String s = null;
            System.out.println(s.equals("abc"));
        }catch(ArrayIndexOutOfBoundsException | ArithmeticException e){
            System.out.println("索引越界了");
        }catch(NullPointerException e){
            System.out.println("空指针异常");
        }catch (Exception e){
            System.out.println("Exception");
        }
        
        System.out.println("看看我执行了吗？");
        
        ```
        
        细节：
        
        > 如果我们要捕获多个异常，这些异常中如果存在父子关系的话，那么父类一定要写在下面
        
        补充：
        
        > 在 JDK7 之后，我们可以在 catch 中同时捕获多个异常，中间用 | 进行隔开
        
3.  如果 try 中遇到的问题没有被捕获，怎么执行？
    
    *   相当于 try…catch 的代码白写了，最终还是会交给虚拟机进行处理。
        
        ```
        int[] arr = {1, 2, 3, 4, 5, 6};
        
        try{
            System.out.println(arr[10]);//new ArrayIndexOutOfBoundsException();
        }catch(NullPointerException e){
            System.out.println("空指针异常");
        }
        
        System.out.println("看看我执行了吗？");
        
        
        ```
        
4.  如果 try 中遇到了问题，那么 try 下面的其他代码还会执行吗？
    
    *   下面的代码就不会执行了，直接跳转到对应的 catch 当中，执行 catch 里面的语句体。但是如果没有对应 catch 与之匹配，那么还是会交给虚拟机进行处理
        
        ```
        int[] arr = {1, 2, 3, 4, 5, 6};
        
        try{
            System.out.println(arr[10]);
            System.out.println("看看我执行了吗？... try");
        }catch(ArrayIndexOutOfBoundsException e){
            System.out.println("索引越界了");
        }
        
        System.out.println("看看我执行了吗？... 其他代码");
        
        ```
        

### 22.5 异常常用方法

![](https://i-blog.csdnimg.cn/blog_migrate/5c10166cf21a4f29ea675c48b8ba224f.png)

#### 22.5.1getMessage（）

```
// 返回此throwable的详细消息字符串
int[] arr = {1, 2, 3, 4, 5, 6};
try {
    System.out.println(arr[10]);
} catch (ArrayIndexOutOfBoundsException e) {
    String message = e.getMessage();
    System.out.println(message);//Index 10 out of bounds for length 6 
}

```

#### 22.5.2toString（）

```
// 返回此可抛出的简短描述
int[] arr = {1, 2, 3, 4, 5, 6};
try {
    System.out.println(arr[10]);
} catch (ArrayIndexOutOfBoundsException e) {
    String str = e.toString();
    System.out.println(str);//java.lang.ArrayIndexOutOfBoundsException: Index 10 out of bounds for length 6
}

```

#### 22.5.3printStackTrace（）

```
// 把异常的错误信息输出在控制台
int[] arr = {1, 2, 3, 4, 5, 6};
try {
    System.out.println(arr[10]);
} catch (ArrayIndexOutOfBoundsException e) {
    e.printStackTrace();
}

```

![](https://i-blog.csdnimg.cn/blog_migrate/a49f968f03d7e90ccbce0b4979bf5a89.png)

### 22.6 异常抛出处理

异常处理：

*   编译时异常 throws
*   运行时异常 throw

#### 22.6.1 声明异常 throws 关键字

注意：

> ​ 写在方法定义处，表示声明一个异常，告诉调用者，使用本方法可能会有哪些异常

格式：

```
public void 方法() throws 异常类名1,异常类名2...{
	……
}

```

#### 22.6.2 抛出异常 throw 关键字

注意：

> ​ 写在方法内，结束方法，手动抛出异常对象，交给调用者方法中下面的代码不再执行了

格式：

```
public void方法(){
    throw new Nul1PointerException();
}
// 例如
if(arr == null){
    //手动创建一个异常对象，并把这个异常交给方法的调用者处理
    //此时方法就会结束，下面的代码不会再执行了
    throw new NullPointerException();
}

```

### 22.7 自定义异常

#### 22.7.1 概述

含义：在开发中根据自己业务的异常情况来定义异常类

#### 22.7.2 使用步骤

**1. 定义异常类**

```
// 业务逻辑异常
public class LoginException  {
 
}

```

说明：

> 通常将类名，命名为 xxxException，以 Exception 结尾，同时类名见名知意最好

**2. 写继承关系**

```
// 业务逻辑异常
public class LoginException extends Exception {
 
}

```

说明：

> 若此自定义异常用于编译时期异常，则继承 Exception
> 
> 若此自定义异常用于运行时期异常，则继承 RuntimeException

**3. 空参构造**

```
// 业务逻辑异常
public class LoginException extends Exception {
    /**
     * 空参构造
     */
    public LoginException() {
    }
}

```

说明：

> 通常实现空参构造和带参构造方法

**4. 带参构造**

```
// 业务逻辑异常
public class LoginException extends Exception {
    /**
     *
     * @param message 表示异常提示
     */
    public LoginException(String message) {
        super(message);
    }
}
}

```

说明：

> 带参构造的目的是为了更好的描述错误信息

### 22.8 最终执行

​ 在 Java 中，finally 是一个关键字，用于定义在 try-catch 代码块中必须执行的代码。无论 try 代码块中是否抛出异常，finally 代码块都会被执行。

格式：

```
try{
     编写可能会出现异常的代码
}catch(异常类型  e){
     处理异常的代码
     //记录日志/打印异常信息/继续抛出异常
}finally{
	无论如何都会执行的代码
}

```

细节

> 当只有在 try 或者 catch 中调用退出 JVM 的相关方法, 此时 finally 才不会执行, 否则 finally 永远会执行。

示例

```
try {
    read("a.txt");
} catch (FileNotFoundException e) {
    //抓取到的是编译期异常  抛出去的是运行期 
    throw new RuntimeException(e);
} finally {
    System.out.println("不管程序怎样，这里都将会被执行。");
}

```

23.File 类
---------

笔记小结：

> 1.  概述：
>     
>     1.  含义：`java.io.File` 类是**文件和目录路径**名的抽象表示
>     2.  路径：**绝对**路径和**相对**路径
> 2.  作用：用于操作文件和目录，例如**创建**、**删除**，获取文件的信息。注意，**File 类不能获取文件内的详细内容**！
>     
> 3.  构造方法：
>     
>     ![](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAADIAAAAyCAYAAAAeP4ixAAACbklEQVRoQ+2aMU4dMRCGZw6RC1CSSyQdLZJtKQ2REgoiRIpQkCYClCYpkgIESQFIpIlkW+IIcIC0gUNwiEFGz+hlmbG9b1nesvGW++zxfP7H4/H6IYzkwZFwQAUZmpJVkSeniFJKA8ASIi7MyfkrRPxjrT1JjZ8MLaXUDiJuzwngn2GJaNd7vyP5IoIYY94Q0fEQIKIPRGS8947zSQTRWh8CwLuBgZx479+2BTkHgBdDAgGAC+fcywoyIFWqInWN9BSONbTmFVp/AeA5o+rjKRJ2XwBYRsRXM4ZXgAg2LAPzOCDTJYQx5pSIVlrC3EI45y611osMTHuQUPUiYpiVooerg7TWRwDAlhSM0TuI+BsD0x4kGCuFSRVzSqkfiLiWmY17EALMbCAlMCmI6IwxZo+INgQYEYKBuW5da00PKikjhNNiiPGm01rrbwDwofGehQjjNcv1SZgddALhlJEgwgJFxDNr7acmjFLqCyJuTd6LEGFttpmkYC91Hrk3s1GZFERMmUT01Xv/sQljjPlMRMsxO6WULwnb2D8FEs4j680wScjO5f3vzrlNJszESWq2LYXJgTzjZm56MCHf3zVBxH1r7ftU1splxxKYHEgoUUpTo+grEf303rPH5hxENJqDKQEJtko2q9zGeeycWy3JhpKhWT8+NM/sufIhBwKI+Mta+7pkfxKMtd8Qtdbcx4dUQZcFCQ2I6DcAnLUpf6YMPxhIDDOuxC4C6djoQUE6+tKpewWZ1wlRkq0qUhXptKTlzv93aI3jWmE0Fz2TeujpX73F9TaKy9CeMk8vZusfBnqZ1g5GqyIdJq+XrqNR5AahKr9CCcxGSwAAAABJRU5ErkJggg==)
> 4.  常用成员方法
>     
>     1.  判断、获取：**isDirectory**()、**isFIle**()、**exists**()、**length**()、**getAbsolutePath**()、**getPath**()、**getName**()、**lastModified**()
>     2.  创建、删除：**createNewFile**()、**mkdir**()、**mkdirs**()、**delete**()
>     3.  获取并遍历：**ListFiles**()（重点）

### 23.1 概述

#### 23.1.1 定义

​ `java.io.File` 类是文件和目录路径名的抽象表示，主要用于文件和目录的创建、查找和删除等操作

#### 23.1.2 路径

![](https://i-blog.csdnimg.cn/blog_migrate/2386b5667cf2e76dff3b2e9d41ccc6a0.png)

### 23.2 作用

​ Java 中的 File 类是一个用于表示**文件或目录**的类。它可以用来**操作文件或目录**，如创建、读取、写入、删除等操作。

​ File 类是 Java I/O 库中的一部分，它提供了多种方法来获取有关文件和目录的信息，例如文件名、路径、大小、修改时间等等。

​ File 类还可以用于文件和目录的遍历，以及文件和目录的过滤。它是 Java 程序中常用的一个类之一

### 23.3 构造方法

![](https://i-blog.csdnimg.cn/blog_migrate/c1ecb4aa78bb4bb836d3d90d013f883d.png)

示例：

```
// 文件路径名
String pathname = "D:\\aaa.txt";
File file1 = new File(pathname); 

// 文件路径名
String pathname2 = "D:\\aaa\\bbb.txt";
File file2 = new File(pathname2); 

// 通过父路径和子路径字符串
 String parent = "d:\\aaa";
 String child = "bbb.txt";
 File file3 = new File(parent, child);

// 通过父级File对象和子路径字符串
File parentDir = new File("d:\\aaa");
String child = "bbb.txt";
File file4 = new File(parentDir, child);

```

小知识：

> 1.  一个 File 对象代表硬盘中实际存在的一个文件或者目录。
> 2.  无论该路径下是否存在文件或者目录，都不影响 File 对象的创建。

### 23.4 常用成员方法

#### 23.4.1 判断、获取

![](https://i-blog.csdnimg.cn/blog_migrate/8ec6b068d6d30d7573aae90d44f9256c.png)

示例：

##### 23.4.1.1isDirectory()、isFIle()、exists()

```
//1.对一个文件的路径进行判断
File f1 = new File("D:\\aaa\\a.txt");
System.out.println(f1.isDirectory());//false
System.out.println(f1.isFile());//true
System.out.println(f1.exists());//true
System.out.println("--------------------------------------");
//2.对一个文件夹的路径进行判断
File f2 = new File("D:\\aaa\\bbb");
System.out.println(f2.isDirectory());//true
System.out.println(f2.isFile());//false
System.out.println(f2.exists());//true
System.out.println("--------------------------------------");
//3.对一个不存在的路径进行判断
File f3 = new File("D:\\aaa\\c.txt");
System.out.println(f3.isDirectory());//false
System.out.println(f3.isFile());//false
System.out.println(f3.exists());//false

```

##### 23.4.1.2length()

```
File f1 = new File("D:\\aaa\\a.txt");
long len = f1.length();
System.out.println(len);

File f2 = new File("D:\\aaa\\bbb");
long len2 = f2.length();
System.out.println(len2);

```

细节：

> *   该方法只能获取**文件的大小**，单位是字节。如果单位我们要是 M，G，可以不断的除以 1024
> *   该方法无法获取文件夹的大小。如果我们要获取一个文件夹的大小，需要把这个文件夹里面所有的文件大小都累加在一起

##### 23.4.1.3getAbsolutePath()

```
File f3 = new File("D:\\aaa\\a.txt");
String path1 = f3.getAbsolutePath();
System.out.println(path1);

File f4 = new File("myFile\\a.txt");
String path2 = f4.getAbsolutePath();
System.out.println(path2);

```

##### 23.4.1.4getPath()

```
File f5 = new File("D:\\aaa\\a.txt");
String path3 = f5.getPath();
System.out.println(path3);//D:\aaa\a.txt

File f6 = new File("myFile\\a.txt");
String path4 = f6.getPath();
System.out.println(path4);//myFile\a.txt

```

##### 23.4.1.5getName()

```
File f7 = new File("D:\\aaa\\a.txt");
String name1 = f7.getName();
System.out.println(name1);//a/txt

File f8 = new File("D:\\aaa\\bbb");
String name2 = f8.getName();
System.out.println(name2);//bbb

```

细节：

> *   该方法获取的是文件的名字时
> *   该方法获取的是文件夹的名字时

##### 23.4.1.6lastModified()

```
File f9 = new File("D:\\aaa\\a.txt");
long time = f9.lastModified();
System.out.println(time); //1667380952425

```

#### 23.4.2 创建、删除

![](https://i-blog.csdnimg.cn/blog_migrate/9a6ff4e847fe5e8f5591f3d3a49ab3b1.png)

注意：

> delete 方法默认只能删除文件和空文件夹，delete 方法直接删除不走回收站

##### 23.4.2.1createNewFile()

```
boolean b = f1.createNewFile();
System.out.println(b);//true

```

细节：

> *   创建文件
>     *   如果当前路径表示的文件是不存在的，则创建成功，方法返回 true
>     *   如果当前路径表示的文件是存在的，则创建失败，方法返回 false
> *   如果父级路径是不存在的，那么方法会有异常 IOException
> *   createNewFile 方法创建的一定是文件，如果路径中不包含后缀名，则创建一个没有后缀的文件

##### 23.4.2.2mkdir()

```
File f2 = new File("D:\\aaa\\aaa\\bbb\\ccc");
boolean b = f2.mkdir();
System.out.println(b);

```

细节：

> *   windows 当中路径是唯一的，如果当前路径已经存在，则创建失败，返回 false
> *   mkdir 方法只能创建单级文件夹，无法创建多级文件夹。

##### 23.4.2.3mkdirs()

```
File f3 = new File("D:\\aaa\\ggg");
boolean b = f3.mkdirs();
System.out.println(b);//true

```

细节：

> *   既可以创建单级的，又可以创建多级的文件夹

##### 23.4.2.4delete()

```
//1.创建File对象
File f1 = new File("D:\\aaa\\eee");
//2.删除
boolean b = f1.delete();
System.out.println(b);

```

细节：

> *   如果删除的是文件，则直接删除，不走回收站。
> *   如果删除的是文件夹
>     1.  如果文件夹为空，则直接删除，不走回收站
>     2.  如果文件夹有内容，则删除失败

#### 23.4.3 获取并遍历

![](https://i-blog.csdnimg.cn/blog_migrate/a8bf3381b5679707566daaa4f3a346c3.png)

##### 23.4.3.1listRoots()

```
File[] arr = File.listRoots();
System.out.println(Arrays.toString(arr));

```

##### 23.4.3.2list()

```
File f1 = new File("D:\\aaa");
String[] arr2 = f1.list();
for (String s : arr2) {
    System.out.println(s);
}

```

##### 23.4.3.3list(FilenameFilter filter)

```
//3.list(FilenameFilter filter)  利用文件名过滤器获取当前该路径下所有内容
//需求：我现在要获取D：\\aaa文件夹里面所有的txt文件
File f2 = new File("D:\\aaa");
//accept方法的形参，依次表示aaa文件夹里面每一个文件或者文件夹的路径
//参数一：父级路径
//参数二：子级路径
//返回值：如果返回值为true，就表示当前路径保留
//        如果返回值为false，就表示当前路径舍弃不要
String[] arr3 = f2.list(new FilenameFilter() {
    @Override
    public boolean accept(File dir, String name) {
        File src = new File(dir,name);
        return src.isFile() && name.endsWith(".txt");
    }
});

System.out.println(Arrays.toString(arr3));

```

##### 23.4.3.4listFiles()（掌握）

```
//1.创建File对象
File f = new File("D:\\aaa");
//2.需求：打印里面所有的txt文件
File[] arr = f.listFiles();
for (File file : arr) {
    //file依次表示aaa文件夹里面每一个文件或者文件夹的路径
    if(file.isFile() && file.getName().endsWith(".txt")){
        System.out.println(file);
    }
}

```

注意:

> *   当调用者 File 表示的路径不存在时，返回 null
> *   当调用者 File 表示的路径是文件时，返回 null
> *   当调用者 File 表示的路径是一个空文件夹时，返回一个长度为 0 的数组
> *   当调用者 File 表示的路径是一个有内容的文件夹时，将里面所有文件和文件夹的路径放在 File 数组中返回
> *   当调用者 File 表示的路径是一个有隐藏文件的文件夹时，将里面所有文件和文件夹的路径放在 File 数组中返回，包含隐藏文件当调用者 File 表示的路径是需要权限才能访问的文件夹时，返回 null

##### 23.4.3.6listFiles(FileFilter filter)

```
//创建File对象
File f = new File("D:\\aaa");
//调用listFiles(FileFilter filter)
File[] arr1 = f.listFiles(new FileFilter() {
    @Override
    public boolean accept(File pathname) {
        return pathname.isFile() && pathname.getName().endsWith(".txt");
    }
});

```

##### 23.4.3.5listFiles(FilenameFilter filter)

```
//调用listFiles(FilenameFilter filter)
File[] arr2 = f.listFiles(new FilenameFilter() {
    @Override
    public boolean accept(File dir, String name) {
        File src = new File(dir, name);
        return src.isFile() && name.endsWith(".txt");
    }
});
System.out.println(Arrays.toString(arr2));

```

24.IO 流
-------

笔记小结：见各个小节

### 24.1 概述

笔记小结：

> 1.  定义：IO 指**对文件进行输入输出操作**
>     
> 2.  分类：
>     
>     *   输入流、输出流：**读取数据**和**写出数据**
>     *   字节流、字符流：任何**二进制文**件和处理**字符**。底层传输的始终为**二进制数据**
>     
>     ![](https://i-blog.csdnimg.cn/blog_migrate/978e4afb9be6fdde902e75b7c49f204a.png)
>     
> 3.  流体系结构：
>     
>     ![](https://i-blog.csdnimg.cn/blog_migrate/728760991a7f262e7986df9480f48760.png)
>     
> 4.  使用 IO 流原则：随用随创建
>     

#### 24.1.1 定义

​ Java 中 I/O 操作主要是指使用`java.io`包下的内容，进行输入、输出操作。**输入**也叫做**读取**数据，**输出**也叫做作**写出**数据。

​ Java 中的 IO 流指的是一组用于处理输入输出操作的类和接口，可以让程序读取和写入数据，实现与文件、网络和其他设备的交互。

#### 24.1.2 分类

![](https://i-blog.csdnimg.cn/blog_migrate/67762e6fb5a5a4df37460a5fe1c3d36f.png)

#### 24.1.3 输入流与输出流

*   输入流：
    
    ​ 在 Java 中的 IO 流中，输入流是用于**读**取**数据**的流。输入流可以从文件、网络连接、标准输入等各种数据源中读取数据
    
    ![](https://i-blog.csdnimg.cn/blog_migrate/dcbfaf8c1d321bd8851308a2e23ad767.png)
    
*   输出流：
    
    ​ 在 Java 的 IO 流中，输出流是指从程序中向外部**写数据**的流。输出流通常用于将程序中的数据写入到文件、网络连接、管道等地方![](https://i-blog.csdnimg.cn/blog_migrate/45e05c70a7b0265ced33c0c10e6cb7dc.png)
    

#### 24.1.4 字节流与字符流

*   字节流
    
    ​ 一切文件数据 (文本、图片、视频等) 在存储时，都是以**二进制数字**的形式保存，都一个一个的字节，那么传输时一样如此。所以，**字节流可以传输任意文件数据**。在操作流的时候，我们要时刻明确，无论使用什么样的流对象，**底层传输的始终为二进制数据**。
    
    ![](https://i-blog.csdnimg.cn/blog_migrate/09023b6f0c61bfff8fa872276f1dfc57.png)
    
*   字符流
    
    ​ 在 Java 中的 IO 流中，字符流是一种**处理字符**数据的 IO 流，即按字符读取或写入数据的流。和字节流不同，字符流是按照字符（Unicode 码）来处理的
    
    ![](https://i-blog.csdnimg.cn/blog_migrate/9087281fdc0db8fe8c364842ac7d0401.png)
    

#### 24.1.5 流体系

![](https://i-blog.csdnimg.cn/blog_migrate/79aa1343b8d73705a47e6676727baaf7.png)

### 24.2 字节基本流

笔记小结：

> 1.  概述：
>     
>     *   定义：文件中**读取字节流**数据
>         
>     *   体系结构：
>         
>         ![](https://i-blog.csdnimg.cn/blog_migrate/e9c177a786fd9d9b80e35382ef78c21d.png)
>         
> 2.  构造方法：
>     
>     1.  输入流：
>         *   **FileInputStream**(File file)
>         *   **FileInputStream**(String name)
>     2.  输出流：
>         *   **FileOutputStream**(File file)
>         *   **FileOutputStream**(String name)
> 3.  常用成员方法：
>     
>     1.  输入流：
>         
>         *   **read**()
>         *   **read**(byte[] b)
>         *   **close**()
>     2.  输出流：
>         
>         *   **write**(int b)
>             
>         *   ###### write(byte[] b)
>             
>         *   **write**(byte[] b, int off, int len)
>             
> 4.  原理：
>     
>     1.  **创建通道**
>         
>         ```
>         FileInputStream fis = new FileInputStream();
>         FileOutputStream fos = new FileOutputStream("myio\\a.txt");
>         
>         ```
>         
>     2.  **处理数据**
>         
>         ```
>         int b1 = fis.read(); // 读数据
>         fos.write(97); // 写数据
>         
>         ```
>         
>     3.  **释放资源**
>         
>         ```
>         fis.close();
>         fos.close();
>         
>         ```
>         

#### 24.2.1 输入流 - FileIntputStream

##### 24.2.1.1 概述

​ `FileInputStream` 是 Java IO 库中用于读取文件数据的类，用于从文件中读取字节流数据。

##### 24.2.1.2 基本用例

```
//1.创建对象
FileInputStream fis = new FileInputStream("myio\\a.txt");
//2.读取数据
int b1 = fis.read();
System.out.println((char)b1);
int b2 = fis.read();
System.out.println((char)b2);
int b3 = fis.read();
System.out.println((char)b3);
int b4 = fis.read();
System.out.println((char)b4);
int b5 = fis.read();
System.out.println((char)b5);
int b6 = fis.read();
System.out.println(b6);//-1
//3.释放资源
fis.close();

```

小知识：

> read() 方法，每读取一个数据，就会移动一次指针

##### 24.2.1.3 构造方法

*   `FileInputStream(File file)`： 通过打开与实际文件的连接来创建一个 FileInputStream ，该文件由文件系统中的 File 对象 file 命名。
*   `FileInputStream(String name)`： 通过打开与实际文件的连接来创建一个 FileInputStream ，该文件由文件系统中的路径名 name 命名。

###### 24.2.1.3.1FileInputStream(File file)

```
/* 格式：
	public FileInputStream(File file) */
// 使用File对象创建流对象
File file = new File("a.txt");
FileInputStream fos = new FileInputStream(file);

```

细节：

> ​ 如果文件不存在，就直接报错。因为创建出来的文件是没有数据的，没有任何意义。所以 Java 就没有设计这种无意义的逻辑，文件不存在直接报错

###### 24.2.1.3.2FileInputStream(String name)

```
/* 格式：
	FileInputStream(String name) */
// 使用文件名称创建流对象
FileInputStream fos = new FileInputStream("b.txt");

```

细节：

> 同上

##### 24.2.1.4 常用成员方法

![](https://i-blog.csdnimg.cn/blog_migrate/b5a63acfe5ac2a96a3790a794f61c325.png)

###### 24.2.1.4.1read()

```
/* 格式：
	FileInputStream对象.raad() 
   作用：一次读取一个字节数据*/
int b1 = fis.read();

```

细节：

> *   一次读一个字节，读出来的是数据在 ASClI 上对应的数字
> *   读到文件末尾了, read 方法返回 - 1。

###### 24.2.1.4.2read(byte[] b)

```
/* 格式：
	FileInputStream对象.raad(byte[] b)
   作用：一次读取一个字节数组数据 */
byte[] b = new byte[2];
int len= fis.read(b)

```

小知识：

> 使用数组读取，每次读取多个字节，减少了系统间的 IO 操作次数，从而提高了读写的效率

###### 24.2.1.4.3close()

```
/* 格式：
	FileInputStream对象.close()
   作用：释放资源 */
fis.close();

```

说明：

> 解除资源占用

##### 24.2.1.5 一次读取多个字节

```
//1.创建对象
FileInputStream fis = new FileInputStream("myio\\a.txt");
//2.读取数据
byte[] bytes = new byte[2];
//一次读取多个字节数据，具体读多少，跟数组的长度有关
//返回值：本次读取到了多少个字节数据
int len1 = fis.read(bytes);
System.out.println(len1);//2
String str1 = new String(bytes,0,len1);
System.out.println(str1);


int len2 = fis.read(bytes);
System.out.println(len2);//2
String str2 = new String(bytes,0,len2);
System.out.println(str2);

int len3 = fis.read(bytes);
System.out.println(len3);// 1
String str3 = new String(bytes,0,len3);
System.out.println(str3);// ed 

//3.释放资源
fis.close();

```

说明：

> ![](https://i-blog.csdnimg.cn/blog_migrate/d4a4b037a045b4c491ac92682c59c3fd.png)
> 
> *   此时每次读取都会将从文件内读取到的字节放入字节数组中
> *   当读取到最后一个字节时，此时只会覆盖数组中的第一个数，因此数据中会残留上一次读取的数据
> *   现在通过 new String(字节流数组，索引，长度)，这个 String 的构造方法就可以合理避免此读取问题

##### 24.2.1.6 循环读取

```
//1.创建对象
FileInputStream fis = new FileInputStream("myio\\a.txt");
//2.循环读取
int b;
while ((b = fis.read()) != -1) {
    System.out.println((char) b);
}
//3.释放资源
fis.close();

```

##### 24.2.1.7 原理

**输入流工作原理**

![](https://i-blog.csdnimg.cn/blog_migrate/0a6bfb41294abffeecbcd8dca81d1c93.png)

说明：

> ```
> FileInputStream fis = new FileInputStream();
> 
> ```

![](https://i-blog.csdnimg.cn/blog_migrate/6c00f0a98f37c93565873aa0634f1f90.png)

说明：

> ```
> int b1 = fis.read();
> 
> ```

![](https://i-blog.csdnimg.cn/blog_migrate/fac9ce0452052894a1bb2c40094e2eb7.png)

说明：

> ```
> fis.close();
> 
> ```

#### 24.3.2 输出流 - FileOutputStream

##### 24.3.2.1 概述

​ `FileOutputStream` 是 Java I/O 包中的一个类，又叫字节输出流。用于操作本地文件的字节输出流，可以把程序中的数据写到本地文件中

##### 24.3.2.2 基本用例

```
//1.创建对象
//写出 输出流 OutputStream
//本地文件    File
FileOutputStream fos = new FileOutputStream("myio\\a.txt");
//2.写出数据
fos.write(97);
//3.释放资源
fos.close();

```

##### 24.3.2.3 构造方法

*   `public FileOutputStream(File file)`：创建文件输出流以写入由指定的 File 对象表示的文件。
*   `public FileOutputStream(String name)`： 创建文件输出流以指定的名称写入文件。

###### 24.3.2.3.1FileOutputStream(File file)

```
/* 格式：
	public FileOutputStream(File file) */
// 使用File对象创建流对象
File file = new File("a.txt");
FileOutputStream fos = new FileOutputStream(file);

```

细节：

> *   如果文件不存在会创建一个新的文件，但是要保证父级路径是存在的
> *   如果文件已经存在，则会清空文件

###### 24.3.2.3.2FileOutputStream(String name)

```
/* 格式：
	public FileOutputStream(File file) 
	作用：使用文件名称创建流对象*/
FileOutputStream fos = new FileOutputStream("b.txt");

```

##### 24.3.1.4 常用成员方法

![](https://i-blog.csdnimg.cn/blog_migrate/e68386399935e61b86feb19a7079775c.png)

###### 24.3.1.4.1write(int b)

```
/* 格式：
	FileOutputStream对象.write(int b)
	作用：一次写一个字节数据*/
fos.write(97); 

```

细节：

> write 方法的参数是整数，但是实际上写到本地文件中的是整数在 ASCII 上对应的字符

###### 24.3.1.4.2write(byte[] b)

```
/* 格式：
	FileOutputStream对象.write(int b)
	作用：一次写一个字节数组数据*/
byte[] bytes = {97, 98, 99, 100, 101};
fos.write(bytes);

```

说明：

> 使用数组写入，每次写入多个字节，减少了系统间的 IO 操作次数，从而提高了读写的效率

###### 24.3.1.4.3write(byte[] b, int off, int len)

```
/* 格式：
	FileOutputStream对象.write(byte[] b, int off, int len)
	作用：一次写一个字节数组的部分数据*/
byte[] bytes = {97, 98, 99, 100, 101};
// 从索引1位开始，写入2为字节数
fos.write(bytes,1,2);// b c

```

###### 24.3.1.4.4close()

```
/* 格式：
	FileOutputStream对象.close()
   作用：释放资源 */
fis.close();

```

说明：

> 每次使用完流之后都要释放资源

##### 24.3.1.5 换行和续写

```
//1.创建对象
FileOutputStream fos = new FileOutputStream("myio\\a.txt",true);  //第二个参数为续写开关
//2.写出数据
String str = "kankelaoyezuishuai";
byte[] bytes1 = str.getBytes();
fos.write(bytes1);

//再次写出一个换行符就可以了
String wrap = "\r\n";
byte[] bytes2 = wrap.getBytes();
fos.write(bytes2);

String str2 = "666";
byte[] bytes3 = str2.getBytes();
fos.write(bytes3);

//3.释放资源
fos.close();

```

小知识：

> 1.  换行写：
>     *   再次写出一个换行符就可以了
>     *   windows： \r\n
>     *   Linux: \n
>     *   Mac: \r
> 2.  细节：
>     *   在 windows 操作系统当中，java 对回车换行进行了优化。
>     *   虽然完整的是 \ r\n，但是我们写其中一个 \ r 或者 \ n，
>     *   java 也可以实现换行，因为 java 在底层会补全。
> 3.  续写：
>     *   如果想要续写，打开续写开关即可
>     *   开关位置：创建对象的第二个参数
>     *   默认 false：表示关闭续写，此时创建对象会清空文件
>     *   手动传递 true：表示打开续写，此时创建对象不会清空文件

##### 24.3.1.3 原理

**输出流工作原理**

![](https://i-blog.csdnimg.cn/blog_migrate/9dfbc59c57ca2d0b77982ee453e22649.png)

说明：

> 通过 new 方法产生数据传输的通道

![](https://i-blog.csdnimg.cn/blog_migrate/4990fbb1e477887eed1ed896a638bf49.png)

说明：

> 通过 write 方法进行数据的传输

![](https://i-blog.csdnimg.cn/blog_migrate/fd7a963e3dfec101d1a9931f22af6816.png)

说明：

> 通过 close 方法将数据传输的通道销毁掉

#### 24.3.3 案例–文件拷贝

```
//1.创建对象
FileInputStream fis = new FileInputStream("D:\\itheima\\movie.mp4");
FileOutputStream fos = new FileOutputStream("myio\\copy.mp4");
//2.拷贝
//核心思想：边读边写
int b;
while((b = fis.read()) != -1){
    fos.write(b);
}
//3.释放资源
//规则：先开的最后关闭
fos.close();
fis.close();

```

小知识：

> 流的关闭原则：先开后关，后开先关。

弊端：

![](https://i-blog.csdnimg.cn/blog_migrate/4a9705a8e7d085e10f3d74cd6af4284d.png)

说明：

> 若是大文件读取，则会非常的缓慢

```
//1.创建对象
FileInputStream fis = new FileInputStream("D:\\itheima\\movie.mp4");
FileOutputStream fos = new FileOutputStream("myio\\copy.mp4");
//2.拷贝
int len;
byte[] bytes = new byte[1024 * 1024 * 5];
while((len = fis.read(bytes)) != -1){
    fos.write(bytes,0,len);
}
//3.释放资源
fos.close();
fis.close();

```

说明：

> 此时使用数组进行读取与写入，可以减少系统间的 IO 操作次数，从而提高了读写的效率

### 24.3 字符基本流

笔记小结：

> 1.  概述：
>     
>     *   含义：从文件中**读取字符**数据
>         
>     *   体系结构：
>         
>         ![](https://i-blog.csdnimg.cn/blog_migrate/76fef2505cd61c00b062a6dbf2bf8cdf.png)
>         
> 2.  构造方法：
>     
>     1.  输入流：
>         
>         *   ###### FileReader(File file)
>             
>         *   ###### FileReader(String fileName)
>             
>     2.  输出流：
>         
>         *   ###### FileWriter(File file)
>             
>         *   ###### FileWriter(String pathName)
>             
> 3.  成员方法：
>     
>     1.  输入流：
>         
>         *   ###### read（）
>             
>         *   ###### read(char[] cbuf)
>             
>     2.  输出流：
>         
>         *   ###### write(int/string c)
>             
>         *   ###### write(char[] cbuf)
>             
>         *   ###### write(char[] cbuf, int off, int len)
>             
>         *   ###### flush()
>             
> 4.  原理：底层读取数据后，会将部分数据保存 4 字节在**缓冲区**，便于读取加快读写速度
>     

特点：

*   输入流: 一次读一个字节，遇到中文时，一次读多个字节
*   输出流: 底层会把数据按照指定的编码方式进行编码，变成字节再写到文件中

字符编码：字节与字符的对应规则。Windows 系统的中文编码默认是 GBK 编码表。IDEA 中的编码默认是 **UTF-8** 编码表

#### 24.3.1 输入流 - FileReader

##### 24.3.1.1 概述

​ `FileReader` 是 Java I/O 包中的一个类，用于从文件中读取字符数据。它的作用是把字符输入流转换成字节输入流，读取文件中的字符数据，常用于读取文本文件。

##### 24.3.1.2 基本用例

```
//1.创建对象并关联本地文件
FileReader fr = new FileReader("myio\\a.txt");
//2.读取数据 read()
//字符流的底层也是字节流，默认也是一个字节一个字节的读取的。
//如果遇到中文就会一次读取多个，GBK一次读两个字节，UTF-8一次读三个字节

//read（）细节：
//1.read():默认也是一个字节一个字节的读取的,如果遇到中文就会一次读取多个
//2.在读取之后，方法的底层还会进行解码并转成十进制。
//  最终把这个十进制作为返回值
//  这个十进制的数据也表示在字符集上的数字
//  英文：文件里面二进制数据 0110 0001
//          read方法进行读取，解码并转成十进制97
//  中文：文件里面的二进制数据 11100110 10110001 10001001
//          read方法进行读取，解码并转成十进制27721

// 我想看到中文汉字，就是把这些十进制数据，再进行强转就可以了

int ch;
while((ch = fr.read()) != -1){
    System.out.print((char)ch);
}

//3.释放资源
fr.close();

```

##### 24.3.1.3 构造方法

*   `FileReader(File file)`： 创建一个新的 FileReader ，给定要读取的 File 对象。
*   `FileReader(String fileName)`： 创建一个新的 FileReader ，给定要读取的文件的名称。

###### 24.3.1.3.1FileReader(File file)

```
/* 格式：
	public FileReader(File file) 
	作用：使用File对象创建流对象*/
File file = new File("a.txt");
FileReader fr = new FileReader(file);

```

###### 24.3.1.3.2FileReader(String fileName)

```
/* 格式：
	public FileReader(File file) 
	作用：使用文件名称创建流对象*/
FileReader fr = new FileReader("b.txt");

```

##### 24.3.1.4 常用成员方法

*   `read`方法，每次可以读取一个字符的数据，提升为 int 类型，读取到文件末尾，返回`-1`
*   `read(char[] cbuf)`，每次读取 b 的长度个字符到数组中，返回读取到的有效字符个数，读取到末尾时，返回`-1`

###### 24.3.1.4.1read（）

```
/* 格式：
	FileReader对象.read()
	作用：每次可以读取一个字符的数据*/
int b = fr.read())

```

说明：

> 每次读取一个字符，都会自动提升为 int 类型，因此需要强制转换才可得到数据

###### 24.3.1.4.2read(char[] cbuf)

```
/* 格式：
	FileReader对象.(char[] cbuf)
	作用：每次读取b的长度个字符到数组中*/

char[] cbuf = new char[2];
// 循环读取
int  len = fr.read(cbuf)

```

说明：

> 读取数据，解码，强转三步合并了，把强转之后的字符放到数组当中

###### 24.3.1.4.4close()

```
/* 格式：
	FileReader对象.close()
   作用：释放资源 */
fis.close();

```

说明：

> 解除资源占用

##### 24.3.1.5 循环读取

循环–单字符读取

```
// 使用文件名称创建流对象
FileReader fr = new FileReader("read.txt");
// 定义变量，保存数据
int b ;
// 循环读取
while ((b = fr.read())!=-1) {
    System.out.println((char)b); //黑马程序员
}
// 关闭资源
fr.close();

```

循环–字符数组读取

```
// 使用文件名称创建流对象
FileReader fr = new FileReader("read.txt");
// 定义变量，保存有效字符个数
int len;
// 定义字符数组，作为装字符数据的容器
char[] cbuf = new char[2];
// 循环读取
while ((len = fr.read(cbuf))!=-1) {
    System.out.println(new String(cbuf));
}
// 关闭资源
fr.close();

```

##### 24.3.1.6 原理

字符输入流底层原理

![](https://i-blog.csdnimg.cn/blog_migrate/85b4ced15c2558d66d995b57031f6be1.png)

说明：

> ![](https://i-blog.csdnimg.cn/blog_migrate/96170161ef40069850aac3fcd6fceba6.png)

![](https://i-blog.csdnimg.cn/blog_migrate/995aff9b0931979c82cc12ec4e3a5fb6.png)

说明：

> 当代码执行时，打断点调试即可

示例

```
 FileReader fr = new FileReader("myio\\b.txt");
        fr.read();//会把文件中的数据放到缓冲区当中

        //清空文件
        FileWriter fw = new FileWriter("myio\\b.txt");

        //请问，如果我再次使用fr进行读取
        //会读取到数据吗？

        //会把缓冲区中的数据全部读取完毕

        //正确答案：
        //但是只能读取缓冲区中的数据，文件中剩余的数据无法再次读取
        int ch;
        while((ch = fr.read()) != -1){
            System.out.println((char)ch);
        }


        fw.close();
        fr.close();

```

#### 24.3.2 输出流 - FileWriter

##### 24.3.2.1 概述

​ `java.io.Writer` 抽象类是表示用于写出字符流的所有类的超类，将指定的字符信息写出到目的地。它定义了字节输出流的基本共性功能方法

##### 24.3.2.2 基本用例

```
//1.创建对象并关联本地文件
FileWriter fw = new FileWriter("myio\\a.txt",true);
//2.写出数据 write()
fw.write(25105);
//3.释放资源
fw.close();

```

##### 24.3.2.3 构造方法

![](https://i-blog.csdnimg.cn/blog_migrate/95981068bf40d42e577e1c3de0c0e537.png)

###### 24.3.2.3.1FileWriter(File file)

```
/* 格式：
	public FileWriter(File file)
	作用：使用File对象创建流对象*/
File file = new File("a.txt");
FileWriter fw = new FileWriter(file);

```

细节：

> *   如果文件不存在会创建一个新的文件，但是要保证父级路径是存在的
> *   如果文件已经存在，则会清空文件，如果不想清空可以打开续写开关

###### 24.3.2.3.2FileWriter(String pathName)

```
/* 格式：
	public FileWriter(File file)
	作用：使用文件名称创建流对象*/

 FileWriter fw = new FileWriter("b.txt");

```

细节：

> 同上

##### 24.3.2.4 常用成员方法

![](https://i-blog.csdnimg.cn/blog_migrate/77cd668a9910672801dde3454d754541.png)

![](https://i-blog.csdnimg.cn/blog_migrate/1c644c35d20d66c718ce92318949e145.png)

###### 24.3.2.3.3write(int/string c)

```
/* 格式：
	FileWriter对象.write(int/string c)
	作用：写出一个字符*/
fw.write(97);
fw.write('C');

```

注意：

> *   未调用 close 方法，数据只是保存到了缓冲区，并未写出到文件中
> *   如果 write 方法的参数是整数，但是实际上写到本地文件中的是整数在字符集上对应的字符

###### 24.3.2.3.4write(char[] cbuf)

```
/* 格式：
	FileWriter对象.write(char[] cbuf)
	作用：写出一个字符数组*/
char[] chars = "黑马程序员".toCharArray();
fw.write(chars); // 黑马程序员

```

###### 24.3.2.3.5write(char[] cbuf, int off, int len)

```
/* 格式：
	FileWriter对象.write(char[] cbuf, int off, int len)
	作用：写出字符数组的一部分*/
char[] chars = "黑马程序员".toCharArray();
fw.write(b,2,2); // 程序

```

###### 24.3.2.3.6flush()

```
/* 格式：
	FileWriter对象.flush()
	作用：将缓冲区中的数据，刷新到本地文件中*/
public class FWWrite {
    public static void main(String[] args) throws IOException {
        // 使用文件名称创建流对象
        FileWriter fw = new FileWriter("fw.txt");
        // 写出数据，通过flush
        fw.write('刷'); // 写出第1个字符
        fw.flush();
        fw.write('新'); // 继续写出第2个字符，写出成功
        fw.flush();
      
      	// 写出数据，通过close
        fw.write('关'); // 写出第1个字符
        fw.close();
        fw.write('闭'); // 继续写出第2个字符,【报错】java.io.IOException: Stream closed
        fw.close();
    }
}

```

注意：

> 即便是 flush 方法写出了数据，操作的最后还是要调用 close 方法，释放系统资源

###### 24.3.2.3.7close()

```
/* 格式：
	FileWriter对象.close()
   作用：释放资源 */
fis.close();

```

说明：

> 解除资源占用

##### 24.3.2.5 续写和换行

```
public class FWWrite {
    public static void main(String[] args) throws IOException {
        // 使用文件名称创建流对象，可以续写数据
        FileWriter fw = new FileWriter("fw.txt"，true);     
      	// 写出字符串
        fw.write("黑马");
      	// 写出换行
      	fw.write("\r\n");
      	// 写出字符串
  		fw.write("程序员");
      	// 关闭资源
        fw.close();
    }
}
输出结果:
黑马
程序员

```

说明：

> 在程序中手动写入换行符

##### 24.3.2.6 原理

字符基础流的输出流原理

![](https://i-blog.csdnimg.cn/blog_migrate/8b32a22b39cea9a34ca9e0e382500e23.png)

说明：

> *   当写入数据时，Java 会先将数据写入内存缓存区。
> *   当缓存区长度满了 8192 个字节，则会自动写入文件
> *   当 IO 流关闭时，也会将缓存区内的内容，自动写入文件

### 24.4 字符集

笔记小结：

> 1.  定义：是将一个字符集中的**字符映射**为一个或多个数字的方法
> 2.  乱码产生原因：读取**数据**，**未**读**完整**个数据、**解码和编码**时的**字符集方式不统一**
> 3.  Java 中编码和解码：
>     *   **GBK 编码**使用**两个字节**来存储一个中文字符
>     *   **UTF-8 编码**使用**三个字节**来存储一个中文字符

#### 24.4.1 定义

字符集（Character set）是将一个字符集中的字符映射为一个或多个数字的方法。

![](https://i-blog.csdnimg.cn/blog_migrate/4f8ee70a856d91161bd751044f76150a.png)

![](https://i-blog.csdnimg.cn/blog_migrate/c8b7cf3aaeb886982a98dca37c848c8f.png)

#### 24.4.2 乱码产生原因

![](https://i-blog.csdnimg.cn/blog_migrate/3ae7a83f2335f9dcd151a50e9fdbac14.png)

![](https://i-blog.csdnimg.cn/blog_migrate/b5f10337669cecbff17481e5f72606e3.png)

![](https://i-blog.csdnimg.cn/blog_migrate/aa47553a1af7b226c8a94a1896d6ad44.png)

#### 24.4.3Java 中编码与解码

![](https://i-blog.csdnimg.cn/blog_migrate/df7d1f48c18f024c759554cb1e5cc2b5.png)

示例：

```
//1.编码
String str = "ai你哟";
byte[] bytes1 = str.getBytes();
System.out.println(Arrays.toString(bytes1));

byte[] bytes2 = str.getBytes("GBK");
System.out.println(Arrays.toString(bytes2));


//2.解码
String str2 = new String(bytes1);
System.out.println(str2);

String str3 = new String(bytes1,"GBK");
System.out.println(str3);

```

说明：

> *   GBK 编码使用两个字节来存储一个中文字符
> *   UTF-8 编码使用三个字节来存储一个中文字符

### 24.4 缓冲流

笔记小结：

> 1.  概述：缓冲流, 也叫**高效流**，是对 4 个基本的`FileXxx` **流的增强**，把基本流包装成高级流，提高**读取 / 写出**数据的性能
>     
> 2.  字节缓存流：
>     
>     *   输入流 **BufferedInputStream**：
>         
>         ```
>         // 1.创建流对象
>         BufferedInputStream bis = new BufferedInputStream(new FileInputStream("jdk9.exe"));
>         // 2.读取数据
>         int b = bis.read();
>         // 3.关闭资源
>         bis.close();
>         
>         ```
>         
>     *   输出流 **BufferedOutputStream**
>         
>         ```
>         // 1.创建流对象
>         BufferedOutputStream bos = new BufferedOutputStream(new FileOutputStream("copy.exe"));
>         // 2.写出数据
>         int len;
>         byte[] bytes = new byte[8*1024];
>         bos.write(bytes, 0 , len);
>         // 3.关闭资源
>         bis.close();
>         
>         ```
>         
> 3.  字符缓冲流
>     
>     *   输入流 **BufferedReader**
>         
>         ```
>         // 1.创建流对象
>         BufferedReader br = new BufferedReader(new FileReader("in.txt"));
>         // 2.读取数据
>         String line = bis.readLine();
>         // 3.关闭资源
>         br.close();
>         
>         ```
>         
>     *   输出流 **BufferedWriter**
>         
>         ```
>         // 1.创建流对象
>         BufferedWriter bw = new BufferedWriter(new FileWriter("out.txt"));
>         // 2.写出数据
>         bw.write("黑马");
>         // 3.写出换行
>         bw.newLine();
>         // 4.关闭资源
>         bw.close();
>         
>         ```
>         
> 4.  底层原理：缓冲区调高效率，是在内存中开辟了一块区域，命名为**缓冲区**。缓冲区的作用，**减少了内存与硬盘的频繁读写**，从而提高了硬盘区域的读写效率
>     
>     ![](https://i-blog.csdnimg.cn/blog_migrate/8be606d1a0ee34b128499340db420628.png)
>     

#### 24.4.1 概述

##### 24.4.1.1 定义

​ 缓冲流, 也叫高效流，是对 4 个基本的`FileXxx` 流的增强，把基本流包装成高级流，提高**读取 / 写出**数据的性能。

按照数据类型分类：

*   **字节缓冲流**：`BufferedInputStream`，`BufferedOutputStream`
*   **字符缓冲流**：`BufferedReader`，`BufferedWriter`

##### 24.4.4.2 体系结构

![](https://i-blog.csdnimg.cn/blog_migrate/3e4509325a7302593ddb835a068b5e61.png)

#### 24.4.2 字节缓冲流

##### 24.4.2.1 输入流 - BufferedInputStream

###### 24.4.2.1.1 概述

`BufferedInputStream` 是 Java I/O 库提供的一个输入流类，它使用了内部缓冲区的方式提高了读取文件的效率

###### 24.4.2.1.2 基本用例

```
// 1.创建流对象
BufferedInputStream bis = new BufferedInputStream(new FileInputStream("jdk9.exe"));
// 2.读取数据
int b = bis.read();
// 3.关闭资源
bis.close();

```

###### 24.4.2.1.3 构造方法

*   `public BufferedInputStream(InputStream in)` ：创建一个 新的缓冲输入流。

```
/* 格式：
	public BufferedInputStream(InputStream in)
    作用：创建字节缓冲输入流*/
// 创建字节缓冲输入流
BufferedInputStream bis = new BufferedInputStream(new FileInputStream("bis.txt"));

```

说明：

> ![](https://i-blog.csdnimg.cn/blog_migrate/cb0aa396a8d5772e70a29c29088d7bc9.png)
> 
> *   当使用此构造方法时，Java 会在底层进行 new 长度为 8192 字节的数组

###### 24.4.2.1.4 常用成员方法

请参考字节基本流的输入流

##### 24.4.2.2 输出流 - BufferedOutputStream

###### 24.4.2.2.1 概述

###### 24.4.2.2.2 基本用例

```
// 1.创建流对象
BufferedOutputStream bos = new BufferedOutputStream(new FileOutputStream("copy.exe"));
// 2.写出数据
int len;
byte[] bytes = new byte[8*1024];
bos.write(bytes, 0 , len);
// 3.关闭资源
bis.close();

```

###### 24.4.2.2.3 构造方法

```
/* 格式：
	public BufferedOutputStream(OutputStream os)
    作用：创建字节缓冲输出流对象*/
 BufferedOutputStream bos = new BufferedOutputStream(new FileOutputStream("myio\\a.txt"));

```

说明：

> ![](https://i-blog.csdnimg.cn/blog_migrate/4b9d1af913414218615f05396845e146.png)
> 
> *   当使用此构造方法时，Java 会在底层进行 new 长度为 8192 字节的数组

###### 24.4.2.2.4 常用成员方法

请参考字节基本流的输出流

##### 24.4.2.3 案例–文件拷贝

```
// 方式一：
//1.创建缓冲流的对象
BufferedInputStream bis = new BufferedInputStream(new FileInputStream("myio\\a.txt"));
BufferedOutputStream bos = new BufferedOutputStream(new FileOutputStream("myio\\a.txt"));
//2.循环读取并写到目的地
int b;
while ((b = bis.read()) != -1) {
    bos.write(b);
}
//3.释放资源
bos.close();
bis.close();

// -----------------------

// 方式二：
//1.创建缓冲流的对象
BufferedInputStream bis = new BufferedInputStream(new FileInputStream("myio\\a.txt"));
BufferedOutputStream bos = new BufferedOutputStream(new FileOutputStream("myio\\copy2.txt"));
//2.拷贝（一次读写多个字节）
byte[] bytes = new byte[1024];
int len;
while((len = bis.read(bytes)) != -1){
    bos.write(bytes,0,len);
}
//3.释放资源
bos.close();
bis.close();

```

小知识：

> ​ 当使用缓冲流来创建对象时，只需要释放缓冲流对象的资源，不需要释放文件输入和输出流对象的资源。因为，Java 会在底层进行自动的关闭操作
> 
> ![](https://i-blog.csdnimg.cn/blog_migrate/0efc5ca43944c75da5db9648adccaeef.png)

##### 24.4.4.4 原理

字节缓冲流高效率原理

![](https://i-blog.csdnimg.cn/blog_migrate/e7c4aea11cc01313bff1aba191eab5fd.png)

说明：

> ​ 缓冲区调高效率，是在内存中开辟了一块区域，命名为缓冲区。缓冲区的作用，减少了内存与硬盘的频繁读写，从而提高了硬盘区域的读写效率

![](https://i-blog.csdnimg.cn/blog_migrate/2479f2487550bf07916350392a669562.png)

说明：

> ​ 当通过定义数组的方式来进行读写数据时，实际上是加快了数据在内存中的频繁读写，从而提高了内存区域的读写效率

#### 24.4.3 字符缓冲流

##### 24.4.3.1 输入流 - BufferedReader

###### 24.4.3.1.1 概述

​ `BufferedReader` 是 Java 中的一个字符缓冲输入流，可以提供一次读取一行或多行文本数据的方法，并且能够保证输入流的顺序读取

###### 24.4.3.1.2 基本用例

```
// 1.创建流对象
BufferedReader br = new BufferedReader(new FileReader("in.txt"));
// 2.读取数据
String line = bis.readLine();
// 3.关闭资源
br.close();

```

###### 24.4.3.1.3 构造方法

```
/* 格式：
	public BufferedReader(Reader in) 
	作用：创建字符缓冲输入流对象*/
// 示例
BufferedReader br = new BufferedReader(new FileReader("br.txt"));

```

###### 24.4.3.1.4 常用成员方法

*   `public String readLine()`: 读一行文字。
*   其余常用方法，参见字符基本输入流

###### 24.4.3.1.4.1readLine()

```
/* 格式：
	BufferedReader对象.readLine()
	作用：每次可以读取一个字符的数据*/
 String line = br.readLine()

```

##### 24.4.3.2 输出流 - BufferedWriter

###### 24.4.3.2.1 概述

​ `BufferedWriter` 是 Java IO 中的一种字符输出流，用于将文本数据写入到字符输出流中，可以提供缓冲机制，提高写入的效率

###### 24.4.3.2.2 基本用例

```
// 1.创建流对象
BufferedWriter bw = new BufferedWriter(new FileWriter("out.txt"));
// 2.写出数据
bw.write("黑马");
// 3.写出换行
bw.newLine();
// 4.关闭资源
bw.close();

```

###### 24.4.3.2.3 构造方法

```
/* 格式：
	public BufferedReader(Reader in) 
	作用：创建字符缓冲输出流对象*/
BufferedWriter bw = new BufferedWriter(new FileWriter("bw.txt"))

```

###### 24.4.3.2.4 常用成员方法

*   `public void newLine()`: 写一行行分隔符, 由系统属性定义符号。
*   其余常用方法，参见字符基本输出流

```
/* 格式：
	BufferedWriter对象.newLine()
	作用：换行*/
bw.newLine();

```

##### 24.4.3.3 成员方法

说明：

> 特有的输出方法， 会根据当前的操作系统进行判断使用哪种换行符

##### 24.4.4.4 原理

​ 缓冲流的基本原理，是在创建流对象时，会创建一个内置的默认大小（长度为 8192）的缓冲区数组，通过缓冲区读写，减少系统 IO 次数，从而提高读写的效率。

​ 参考上一小节`字节缓冲流的原理`

### 24.5 转换流

笔记小结：

> 1.  概述：读取**字节**，并使用**指定的字符集**将其**解码为字符**。也可以实现读取时的**字符集编码转换**
> 2.  输入流 **InputStreamReader**
> 3.  输出流 **OutputStreamWriter**

#### 24.5.1 概述

##### 24.5.1.1 定义

​ 转换流`java.io.InputStreamReader`，是 Reader 的子类，是从字节流到字符流的桥梁。它读取字节，并使用指定的字符集将其解码为字符。它的字符集可以由名称指定，也可以接受平台的默认字符集

![](https://i-blog.csdnimg.cn/blog_migrate/0b640a8508baf021eca6077c0df56cc4.png)

##### 24.5.1.1 体系结构

![](https://i-blog.csdnimg.cn/blog_migrate/6b76648b82773718d42cafe08bb7e4ac.png)

#### 24.5.2 输入流 - InputStreamReader

##### 24.5.2.1 构造方法

*   `InputStreamReader(InputStream in)`: 创建一个使用默认字符集的字符流。
*   `InputStreamReader(InputStream in, String charsetName)`: 创建一个指定字符集的字符流。

示例：

```
InputStreamReader isr = new InputStreamReader(new FileInputStream("in.txt"));
InputStreamReader isr2 = new InputStreamReader(new FileInputStream("in.txt") , "GBK");

```

##### 24.5.2.2 常用成员方法

*   常用方法，参见字符基本输入流

示例：

```
public class ReaderDemo2 {
    public static void main(String[] args) throws IOException {
      	// 定义文件路径,文件为gbk编码
        String FileName = "E:\\file_gbk.txt";
      	// 创建流对象,默认UTF8编码
        InputStreamReader isr = new InputStreamReader(new FileInputStream(FileName));
      	// 创建流对象,指定GBK编码
        InputStreamReader isr2 = new InputStreamReader(new FileInputStream(FileName) , "GBK");
		// 定义变量,保存字符
        int read;
      	// 使用默认编码字符流读取,乱码
        while ((read = isr.read()) != -1) {
            System.out.print((char)read); // ��Һ�
        }
        isr.close();
      
      	// 使用指定编码字符流读取,正常解析
        while ((read = isr2.read()) != -1) {
            System.out.print((char)read);// 大家好
        }
        isr2.close();
    }
}

```

说明：

> 以上代码在 JDK11 被字符基本输入流给替代

```
FileReader fr = new FileReader("myio\\gbkfile.txt", Charset.forName("GBK"));
//2.读取数据
int ch;
while ((ch = fr.read()) != -1){
    System.out.print((char)ch);
}
//3.释放资源
fr.close();

```

#### 24.5.3 输出流 - OutputStreamWriter

##### 24.5.3.1 构造方法

*   `OutputStreamWriter(OutputStream in)`: 创建一个使用默认字符集的字符流。
*   `OutputStreamWriter(OutputStream in, String charsetName)`: 创建一个指定字符集的字符流。

示例：

```
OutputStreamWriter isr = new OutputStreamWriter(new FileOutputStream("out.txt"));
OutputStreamWriter isr2 = new OutputStreamWriter(new FileOutputStream("out.txt") , "GBK");

```

##### 24.5.3.2 常用成员方法

*   常用方法，参见字符基本输出流

示例：

```
public class OutputDemo {
    public static void main(String[] args) throws IOException {
      	// 定义文件路径
        String FileName = "E:\\out.txt";
      	// 创建流对象,默认UTF8编码
        OutputStreamWriter osw = new OutputStreamWriter(new FileOutputStream(FileName));
        // 写出数据
      	osw.write("你好"); // 保存为6个字节
        osw.close();
      	
		// 定义文件路径
		String FileName2 = "E:\\out2.txt";
     	// 创建流对象,指定GBK编码
        OutputStreamWriter osw2 = new OutputStreamWriter(new FileOutputStream(FileName2),"GBK");
        // 写出数据
      	osw2.write("你好");// 保存为4个字节
        osw2.close();
    }
}

```

说明：

> 以上代码在 JDK11 被字符基本输出流给替代

```
FileWriter fw = new FileWriter("myio\\c.txt", Charset.forName("GBK"));
fw.write("你好你好");
fw.close();

```

#### 24.5.4 案例 - 文件编码转换

```
//1.JDK11以前的方案
/* InputStreamReader isr = new InputStreamReader(new FileInputStream("myio\\b.txt"),"GBK");
        OutputStreamWriter osw = new OutputStreamWriter(new FileOutputStream("myio\\d.txt"),"UTF-8");

        int b;
        while((b = isr.read()) != -1){
            osw.write(b);
        }

        osw.close();
        isr.close();*/

//2.替代方案
FileReader fr = new FileReader("myio\\b.txt", Charset.forName("GBK"));
FileWriter fw = new FileWriter("myio\\e.txt",Charset.forName("UTF-8"));
int b;
while ((b = fr.read()) != -1){
    fw.write(b);
}
fw.close();
fr.close();

```

### 24.6 序列化流

笔记小结：

> 1.  概述：用一个字节序列可以表示一个对象，该字节序列包含该`对象的数据`、`对象的类型`和`对象中存储的属性`等信息。**持久保存**了一个对象的信息
> 2.  序列化流 **ObjectOutputStream**：将 Java 对象的原始数据类型写出到文件, 实现对象的持久存储
> 3.  反序列化流 **ObjectInputStream**：将之前使用 ObjectOutputStream 序列化的原始数据恢复为对象。

#### 24.6.1 概述

##### 24.6.1.1 定义

​ Java 提供了一种对象**序列化**的机制。用一个字节序列可以表示一个对象，该字节序列包含该`对象的数据`、`对象的类型`和`对象中存储的属性`等信息。字节序列写出到文件之后，相当于文件中**持久保存**了一个对象的信息。

##### 24.6.1.1 体系结构

![](https://i-blog.csdnimg.cn/blog_migrate/69c4870864241e2766aa0258b4542f45.png)

#### 24.6.2 序列化流 - ObjectOutputStream

##### 24.6.2.1 概述

​ `java.io.ObjectOutputStream` 类，将 Java 对象的原始数据类型写出到文件, 实现对象的持久存储。

##### 24.6.2.2 构造方法

*   `public ObjectOutputStream(OutputStream out)` ： 创建一个指定 OutputStream 的 ObjectOutputStream

**ObjectOutputStream(OutputStream out)**

```
/* 格式：
	public ObjectOutputStream(OutputStream out)
    作用：创建序列化流*/
ObjectOutputStream out = new ObjectOutputStream(new FileOutputStream("employee.txt"));

```

注意：

> *   该类必须实现`java.io.Serializable` 接口，`Serializable` 是一个标记接口，不实现此接口的类将不会使任何状态序列化或反序列化，会抛出`NotSerializableException` 。
>     
> *   该类的所有属性必须是可序列化的。如果有一个属性不需要可序列化的，则该属性必须注明是瞬态的，使用`transient` 关键字修饰。
>     
> *   示例：
>     
>     ```
>     public class Employee implements java.io.Serializable {
>         public String name;
>         public String address;
>         public transient int age; // transient瞬态修饰成员,不会被序列化
>         public void addressCheck() {
>           	System.out.println("Address  check : " + name + " -- " + address);
>         }
>     }
>     
>     ```
>     

##### 24.6.2.3 成员方法

*   `public final void writeObject (Object obj)` : 将指定的对象写出

```
/* 格式：
	ObjectOutputStream对象.writeObject(Object obj)
    作用：将指定的对象写出*/
out.writeObject(e);

```

#### 24.6.3 反序列化流 - ObjectInputStream

##### 24.6.2.1 概述

`ObjectInputStream`反序列化流，将之前使用 ObjectOutputStream 序列化的原始数据恢复为对象。

##### 24.6.3.2 构造方法

*   `public ObjectInputStream(InputStream in)` ： 创建一个指定 InputStream 的 ObjectInputStream。

**ObjectOutputStream(OutputStream out)**

```
/* 格式：
	public ObjectInputStream(InputStream in) 
    作用：创建反序列化流*/
ObjectInputStream in = new ObjectInputStream(new FileInputStream("employee.txt"));

```

注意：

> 1.  对于 JVM 可以反序列化对象，它必须是能够找到 class 文件的类。如果找不到该类的 class 文件，则抛出一个 `ClassNotFoundException` 异常。
> 2.  当 JVM 反序列化对象时，能找到 class 文件，但是 class 文件在序列化对象之后发生了修改，那么反序列化操作也会失败，抛出一个`InvalidClassException`异常。发生这个异常的原因如下：
>     *   该类的序列版本号与从流中读取的类描述符的版本号不匹配
>     *   该类包含未知数据类型
>     *   该类没有可访问的无参数构造方法
> 
> 示例：
> 
> 在创建类时，添加版本号
> 
> ```
> public class Employee implements java.io.Serializable {
>      // 加入序列版本号
>      private static final long serialVersionUID = 1L;
>      public String name;
>      public String address;
>      // 添加新的属性 ,重新编译, 可以反序列化,该属性赋为默认值.
>      public int eid; 
> 
>      public void addressCheck() {
>          System.out.println("Address  check : " + name + " -- " + address);
>      }
> }
> 
> ```

##### 24.6.3.2 成员方法

*   `public final Object readObject ()` : 读取一个对象。

**readObject ()**

```
/* 格式：
	ObjectInputStream对象.readObject ()
    作用：读取一个对象*/
e = (Employee) in.readObject();

```

#### 24.6.4 案例 - 多个文件序列化与反序列化

示例：

```
Student s1 = new Student("zhangsan",23,"南京");
Student s2 = new Student("lisi",24,"重庆");
Student s3 = new Student("wangwu",25,"北京");

ArrayList<Student> list = new ArrayList<>();

list.add(s1);
list.add(s2);
list.add(s3);

ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("myio\\a.txt"));
oos.writeObject(list);

oos.close();

```

```
//1.创建反序列化流的对象
ObjectInputStream ois = new ObjectInputStream(new FileInputStream("myio\\a.txt"));

//2.读取数据
ArrayList<Student> list = (ArrayList<Student>) ois.readObject();

for (Student student : list) {
    System.out.println(student);
}

//3.释放资源
ois.close();

```

### 24.7 打印流

笔记小结：

> 1.  概述：`java.io.PrintStream`类，该类能够方便地**打印各种数据类型的值**，是一种便捷的**输出方式**
> 2.  字符打印流 **PrintWriter**：需要**输出文本数据到文件**等地方，并需要更加**丰富的字符编码和异常处理**
> 3.  字节打印流 **PrintStream**：需要简单地**输出文本**数据到控制台或文件

#### 24.7.1 概述

##### 24.7.1.1 定义

​ 平时我们在控制台打印输出，是调用`print`方法和`println`方法完成的，这两个方法都来自于`java.io.PrintStream`类，该类能够方便地打印各种数据类型的值，是一种便捷的输出方式。

注意：

> ​ 打印流，只能读，不能写

##### 24.7.1.2 体系结构

![](https://i-blog.csdnimg.cn/blog_migrate/a5309a31d09cc6674d8ae672cf6db6b1.png)

#### 24.7.2 字符打印流 PrintWriter

##### 24.7.2.1 概述

##### 24.7.2.2 构造方法

![](https://i-blog.csdnimg.cn/blog_migrate/2e41199b96169417f3e4f759caf37b12.png)

注意：

> 字符流底层有缓冲区，想要自动刷新需要开启

```
/* 格式：
	public PrintWriter(OutpusStream out, boolean autoFlush);
    作用：创建打印流*/
PrintStream ps = new PrintStream(new FileOutputStream("myio\\a.txt"), true);

```

##### 24.7.2.3 成员方法

![](https://i-blog.csdnimg.cn/blog_migrate/41116da0f01e344499aaf481ea01bcb3.png)

```
/* 格式：
	PrintWriter对象.println(xxx b )
    作用：将指定的对象写出*/
 ps.println(97);

```

#### 24.7.3 字节打印流 PrintStream

##### 24.7.3.1 概述

##### 24.7.3.2 构造方法

![](https://i-blog.csdnimg.cn/blog_migrate/843ab5af4fafba936d406c93cbf21ecf.png)

注意：

> ​ 字节流底层没有缓冲区，开不开自动刷新都一样

```
/* 格式：
	public PrintStream(OutpusStream out, boolean autoFlush, String encoding);
    作用：创建打印流*/
PrintStream ps = new PrintStream(new FileOutputStream("myio\\a.txt"), true, Charset.forName("UTF-8"));

```

##### 24.7.3.3 成员方法

![](https://i-blog.csdnimg.cn/blog_migrate/fedc4bcef05dc5e7b8279b61742d44bc.png)

**println（）**

```
/* 格式：
	PrintStream对象.println(xxx b )
    作用：将指定的对象写出*/
 ps.println(97);//写出 + 自动刷新 + 自动换行

```

细节：

> *   此处，在文件中为原样输出
>     
>     ![](https://i-blog.csdnimg.cn/blog_migrate/893c307fecaa7bbb64a8bcf46f1d8ac7.png)
>     

#### 24.7.4 案例 - 改变打印流

```
public class PrintDemo {
    public static void main(String[] args) throws IOException {
		// 调用系统的打印流,控制台直接输出97
        System.out.println(97);
      
		// 创建打印流,指定文件的名称
        PrintStream ps = new PrintStream("ps.txt");
      	
      	// 设置系统的打印流流向,输出到ps.txt
        System.setOut(ps);
      	// 调用系统的打印流,ps.txt中输出97
        System.out.println(97);
    }
}

```

说明：

> 我们平时常用的 System.out.println() 方法就是高级打印流的链式编程法

### 24.8 压缩流和解压缩流（了解）

笔记小结：

> ​ 概述：压缩文件或者文件夹

#### 24.8.1 概述

##### 24.8.1.1 含义

​ 负责压缩文件或者文件夹

##### 24.8.1.2 体系结构

![](https://i-blog.csdnimg.cn/blog_migrate/1b5520a818774f4b705904722c9b34e6.png)

#### 24.8.2 解压缩流

```
/*
*   解压缩流
*
* */
public class ZipStreamDemo1 {
    public static void main(String[] args) throws IOException {

        //1.创建一个File表示要解压的压缩包
        File src = new File("D:\\aaa.zip");
        //2.创建一个File表示解压的目的地
        File dest = new File("D:\\");

        //调用方法
        unzip(src,dest);

    }

    //定义一个方法用来解压
    public static void unzip(File src,File dest) throws IOException {
        //解压的本质：把压缩包里面的每一个文件或者文件夹读取出来，按照层级拷贝到目的地当中
        //创建一个解压缩流用来读取压缩包中的数据
        ZipInputStream zip = new ZipInputStream(new FileInputStream(src));
        //要先获取到压缩包里面的每一个zipentry对象
        //表示当前在压缩包中获取到的文件或者文件夹
        ZipEntry entry;
        while((entry = zip.getNextEntry()) != null){
            System.out.println(entry);
            if(entry.isDirectory()){
                //文件夹：需要在目的地dest处创建一个同样的文件夹
                File file = new File(dest,entry.toString());
                file.mkdirs();
            }else{
                //文件：需要读取到压缩包中的文件，并把他存放到目的地dest文件夹中（按照层级目录进行存放）
                FileOutputStream fos = new FileOutputStream(new File(dest,entry.toString()));
                int b;
                while((b = zip.read()) != -1){
                    //写到目的地
                    fos.write(b);
                }
                fos.close();
                //表示在压缩包中的一个文件处理完毕了。
                zip.closeEntry();
            }
        }
        zip.close();
    }
}

```

#### 24.8.3 压缩流

```
public class ZipStreamDemo2 {
    public static void main(String[] args) throws IOException {
        /*
         *   压缩流
         *      需求：
         *          把D:\\a.txt打包成一个压缩包
         * */
        //1.创建File对象表示要压缩的文件
        File src = new File("D:\\a.txt");
        //2.创建File对象表示压缩包的位置
        File dest = new File("D:\\");
        //3.调用方法用来压缩
        toZip(src,dest);
    }

    /*
    *   作用：压缩
    *   参数一：表示要压缩的文件
    *   参数二：表示压缩包的位置
    * */
    public static void toZip(File src,File dest) throws IOException {
        //1.创建压缩流关联压缩包
        ZipOutputStream zos = new ZipOutputStream(new FileOutputStream(new File(dest,"a.zip")));
        //2.创建ZipEntry对象，表示压缩包里面的每一个文件和文件夹
        //参数：压缩包里面的路径
        ZipEntry entry = new ZipEntry("aaa\\bbb\\a.txt");
        //3.把ZipEntry对象放到压缩包当中
        zos.putNextEntry(entry);
        //4.把src文件中的数据写到压缩包当中
        FileInputStream fis = new FileInputStream(src);
        int b;
        while((b = fis.read()) != -1){
            zos.write(b);
        }
        zos.closeEntry();
        zos.close();
    }
}

```

```
public class ZipStreamDemo3 {
    public static void main(String[] args) throws IOException {
        /*
         *   压缩流
         *      需求：
         *          把D:\\aaa文件夹压缩成一个压缩包
         * */
        //1.创建File对象表示要压缩的文件夹
        File src = new File("D:\\aaa");
        //2.创建File对象表示压缩包放在哪里（压缩包的父级路径）
        File destParent = src.getParentFile();//D:\\
        //3.创建File对象表示压缩包的路径
        File dest = new File(destParent,src.getName() + ".zip");
        //4.创建压缩流关联压缩包
        ZipOutputStream zos = new ZipOutputStream(new FileOutputStream(dest));
        //5.获取src里面的每一个文件，变成ZipEntry对象，放入到压缩包当中
        toZip(src,zos,src.getName());//aaa
        //6.释放资源
        zos.close();
    }

    /*
    *   作用：获取src里面的每一个文件，变成ZipEntry对象，放入到压缩包当中
    *   参数一：数据源
    *   参数二：压缩流
    *   参数三：压缩包内部的路径
    * */
    public static void toZip(File src,ZipOutputStream zos,String name) throws IOException {
        //1.进入src文件夹
        File[] files = src.listFiles();
        //2.遍历数组
        for (File file : files) {
            if(file.isFile()){
                //3.判断-文件，变成ZipEntry对象，放入到压缩包当中
                ZipEntry entry = new ZipEntry(name + "\\" + file.getName());//aaa\\no1\\a.txt
                zos.putNextEntry(entry);
                //读取文件中的数据，写到压缩包
                FileInputStream fis = new FileInputStream(file);
                int b;
                while((b = fis.read()) != -1){
                    zos.write(b);
                }
                fis.close();
                zos.closeEntry();
            }else{
                //4.判断-文件夹，递归
                toZip(file,zos,name + "\\" + file.getName());
                //     no1            aaa   \\   no1
            }
        }
    }
}

```

### 24.9 工具类

笔记小结：

> *   Commons-io 工具类是 apache 开源基金组织提供的一组有关 IO 操作的开源工具包（了解）
> *   Hutool 工具类（重点）：
>     *   `Hutool`是一个功能丰富且易用的 **Java 工具库**，通过诸多实用工具类的使用，旨在帮助开发者快速、便捷地完成各类开发任务。 这些封装的工具涵盖了字**符串、数字、集合、编码、日期、文件、IO、加密、数据库 JDBC、JSON、HTTP 客户端**等一系列操作， 可以满足各种不同的开发需求
>     *   官方网址：[入门和安装 (oschina.io)](https://loolly_admin.oschina.io/hutool-site/docs/#/)
> 
> ![](https://i-blog.csdnimg.cn/blog_migrate/a2309f00de3a4031246f218998416be7.png)

#### 24.9.1Commos-io

##### 24.9.1.1 概述

Commons-io 是 apache 开源基金组织提供的一组有关 IO 操作的开源工具包。

作用: 提高 I0 流的开发效率。

![](https://i-blog.csdnimg.cn/blog_migrate/7829bbd2548fe0eb4aa0969dbfb22652.png)

##### 24.9.1.2FileUtils 类常用方法

![](https://i-blog.csdnimg.cn/blog_migrate/53380129c12ab60b4f09c2eaa201ef9f.png)

##### 24.9.1.3IOUtils 类常用方法

![](https://i-blog.csdnimg.cn/blog_migrate/2f0bf73d6ecb2811266a5300f9c5b9d8.png)

##### 24.9.1.4 案例 - 基本使用

```
public class CommonsIODemo1 {
    public static void main(String[] args) throws IOException {
        /*
          FileUtils类
                static void copyFile(File srcFile, File destFile)                   复制文件
                static void copyDirectory(File srcDir, File destDir)                复制文件夹
                static void copyDirectoryToDirectory(File srcDir, File destDir)     复制文件夹
                static void deleteDirectory(File directory)                         删除文件夹
                static void cleanDirectory(File directory)                          清空文件夹
                static String readFileToString(File file, Charset encoding)         读取文件中的数据变成成字符串
                static void write(File file, CharSequence data, String encoding)    写出数据

            IOUtils类
                public static int copy(InputStream input, OutputStream output)      复制文件
                public static int copyLarge(Reader input, Writer output)            复制大文件
                public static String readLines(Reader input)                        读取数据
                public static void write(String data, OutputStream output)          写出数据
         */


        /* File src = new File("myio\\a.txt");
        File dest = new File("myio\\copy.txt");
        FileUtils.copyFile(src,dest);*/


        /*File src = new File("D:\\aaa");
        File dest = new File("D:\\bbb");
        FileUtils.copyDirectoryToDirectory(src,dest);*/

        /*File src = new File("D:\\bbb");
        FileUtils.cleanDirectory(src);*/
    }
}


```

#### 24.9.2Hutool（重点）

![](https://i-blog.csdnimg.cn/blog_migrate/ce157838940b83b34a1fa8feefbe5bbe.png)

##### 24.9.2.1 概述

​ Commons 是国人开发的开源工具包，里面有很多帮助我们提高开发效率的 API

##### 14.9.2.2 工具类

![](https://i-blog.csdnimg.cn/blog_migrate/4ecfa752565a97a98af9f3e029368f97.png)

##### 14.9.2.3 案例 - 基本使用

```
public class Test1 {
    public static void main(String[] args) {
    /*
        FileUtil类:
                file：根据参数创建一个file对象
                touch：根据参数创建文件

                writeLines：把集合中的数据写出到文件中，覆盖模式。
                appendLines：把集合中的数据写出到文件中，续写模式。
                readLines：指定字符编码，把文件中的数据，读到集合中。
                readUtf8Lines：按照UTF-8的形式，把文件中的数据，读到集合中

                copy：拷贝文件或者文件夹
    */


       /* File file1 = FileUtil.file("D:\\", "aaa", "bbb", "a.txt");
        System.out.println(file1);//D:\aaa\bbb\a.txt

        File touch = FileUtil.touch(file1);
        System.out.println(touch);


        ArrayList<String> list = new ArrayList<>();
        list.add("aaa");
        list.add("aaa");
        list.add("aaa");

        File file2 = FileUtil.writeLines(list, "D:\\a.txt", "UTF-8");
        System.out.println(file2);*/

      /*  ArrayList<String> list = new ArrayList<>();
        list.add("aaa");
        list.add("aaa");
        list.add("aaa");
        File file3 = FileUtil.appendLines(list, "D:\\a.txt", "UTF-8");
        System.out.println(file3);*/
        List<String> list = FileUtil.readLines("D:\\a.txt", "UTF-8");
        System.out.println(list);
    }
}

```

知识加油站
-----

### 1.IDEA 中查看此方法的源码

若 IDEA 中 进行 CTRL+B 查看的是此方法的接口

![](https://i-blog.csdnimg.cn/blog_migrate/cc4b484215cd20dae8843729ec7ba566.png)

那么我们可以选中方法，进行实现方法的查看

![](https://i-blog.csdnimg.cn/blog_migrate/35ae3b4e8209a15f80d95b6d61d4aef6.png)

### 2.IDEA 中包裹代码快捷键

若 IDEA 中 使用 CTRL+ALT+T 快捷键进行包裹代码

![](https://i-blog.csdnimg.cn/blog_migrate/b74e9783789e791d1e53427af4484d26.png)

### 3.IDEA 中提取代码快捷键

**Ctrl+Alt+M**

![](https://i-blog.csdnimg.cn/blog_migrate/bcd10db61407fad87d8dc504a554a6e8.png)

### 4.Java 中异常处理的扩展方式

![](https://i-blog.csdnimg.cn/blog_migrate/636d51bee52d7eb3aa6c6486e635cf48.png)

### 5.StringBuider 和 StringBuffer 区别

![](https://i-blog.csdnimg.cn/blog_migrate/0ac0272a50f87b2d888625c1f9ed03eb.png)

![](https://i-blog.csdnimg.cn/blog_migrate/210148aa135b1cbf048fe8dd268aa4d3.png)

说明：

> ​ StringBuider 和 StringBuffer 在 java 中的方法都是一样的，区别就在于，StringBuffer 为每个方法添加了同步方法，这样在操作方法时，保证只有一个方法来操作，从而提升了代码安全性

补充：

> ​ 若只是单线程，则使用 StringBuider 即可，若是在多线程环境下，需要考虑线程安全，则使用 StringBuffer8

### 6.Arrays 数组工具类

![](https://i-blog.csdnimg.cn/blog_migrate/65219dac1689c75cfa5836d54a396c56.png)

在 Java 中，`Arrays`是一个提供了许多有用的静态方法的类，可以用于操作和处理数组。

以下是`Arrays`类的一些常见用法：

1.  `Arrays.toString()`方法：用于将数组转换为一个字符串。
2.  `Arrays.sort()`方法：用于将数组按升序排序。
3.  `Arrays.copyOf()`方法：用于复制一个数组。
4.  `Arrays.binarySearch()`方法：用于在已排序的数组中搜索指定的元素。
5.  `Arrays.fill()`方法：用于将数组的所有元素设置为指定的值。
6.  `Arrays.equals()`方法：用于比较两个数组是否相等。
7.  `Arrays.asList()`方法：用于将数组转换为列表。

需要注意的是，`Arrays`类的大多数方法都是静态方法，因此不需要创建`Arrays`对象就可以调用它们。另外，`Arrays`类中的方法适用于所有基本数据类型的数组，以及对象数组。
