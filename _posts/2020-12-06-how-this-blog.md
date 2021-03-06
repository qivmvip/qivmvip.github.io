---
layout: post
title: "Hello, C++"
date: 2021-01-28 03:00
description: 这个博客是如何搭建的
tags:
 - github pages
 - gitlab pages
 - jekyll
---

## 首先看一个例子

+ hello
  ```c++
  // +------------------------------------------------------------------------+
  // | Copyright (c) YanJibin <qivmvip AT gmail DOT net> All rights reserved. |
  // | Licensed under the MIT license,  see LICENSE file in the project root. |
  // +------------------------------------------------------------------------+
  // + author : YanJibin <qivmvip AT gmail DOT net>
  // + date   : 2020-03-27
  // + desc   : timer implementation


  #ifndef GYPSY_UTILS_TIMER_IMPL_HXX
  #define GYPSY_UTILS_TIMER_IMPL_HXX


  #include "./base.hxx"
  #include "./service.hxx"
  #include "./ns.hxx"
  #include <chrono>


  GYPSY_DETAIL_TIMER_BEGIN

  template<typename Clock>
  class impl : public base {
  public:
      using self = impl;
      using clock = Clock;
  public:
      impl() {
          service_.initialize();
          service_.activate();
      }
      virtual ~impl() {
          service_.deactivate();
          service_.finalize();
      }
  public:
      virtual int set_timeout(callback const& cb, int delay) override {
          return service_.set_timeout(cb, std::chrono::milliseconds{delay});
      }
      virtual int set_interval(callback const& cb, int interval) override {
          return service_.set_interval(cb, std::chrono::milliseconds{interval});
      }
      virtual bool clear(int id) override {
          return service_.clear(id);
      }
  private:
      bool available_ = false;
      service<clock> service_;
  };

  using steady_clock_impl = impl<std::chrono::steady_clock>;
  using system_clock_impl = impl<std::chrono::system_clock>;

  GYPSY_DETAIL_TIMER_END


  #endif // GYPSY_UTILS_TIMER_BASE_HXX
  ```


## 原理

C++语言发展大概可以分为三个阶段：第一阶段从80年代到1995年。这一阶段C++语言基本上是传统类型上的面向对象语言，并且凭借着接近C语言的效率，在工业界使用的开发语言中占据了相当大份额；第二阶段从1995年到2000年，这一阶段由于标准模板库（STL）和后来的Boost等程序库的出现，泛型程序设计在C++中占据了越来越多的比重。当然，同时由于Java、C#等语言的出现和硬件价格的大规模下降，C++受到了一定的冲击；第三阶段从2000年至今，由于以Loki、MPL(Boost)等程序库为代表的产生式编程和模板元编程的出现，C++出现了发展历史上又一个新的高峰，这些新技术的出现以及和原有技术的融合，使C++已经成为当今主流程序设计语言中最复杂的一员。

比雅尼·斯特劳斯特鲁普（Stroustrup）工作起于1979年的C with Classes。这个构思起源于斯特劳斯特鲁普做博士论文时的一些程序撰写经验。他发现Simula具备很利于大型软件开发的特点，但Simula的运行速度太慢，无法对现实需求发挥功效；BCPL虽快得多，但它过于低端的特性，使其不适于大型软件的开发。当斯特劳斯特鲁普开始在贝尔实验室工作时，他有分析UNIX核心关于分布式计算的问题。回想起他的博士论文经验，斯特劳斯特鲁普开始为C语言增强一些类似Simula的特点[2]。之所以选择C，是因为它适于各种用途、快速和可移植性。除了C和Simula之外，同时也从其它语言中获取灵感，如ALGOL 68、Ada、CLU以及ML。

<!--__jekyll_more_separator__-->

刚开始时，类别、派生类、存储类型检查、内联和缺省参数特性，都是透过Cfront引入C语言之中[3]。

1983年，C with Classes改命名为C++（++是C语言中的增值操作符）。加入了新的特性，其中包括虚函数、函数名和运算符重载、参考、常量、用户可控制的自由空间存储区控制、改良的类型检查，以及新的双斜线（//）单行注解风格。

1985年，发布第一版《C++程序设计语言》，提供一个重点的语言参考，至此还不是官方标准[4]。1985年10月出现了第一个商业化发布。
1989年，发布了Release 2.0。引入了多重继承、抽象类别、静态成员函数、常量成员函数，以及成员保护。1990年，出版了The Annotated C++ Reference Manual。这本书后来成为标准化的基础。稍后还引入了模板、异常处理、名字空间、新的强制类型转换，以及布尔类型。

随着C++语言的演变，也逐渐演化出相应的标准程序库。最先加进C++标准库的是流I/O程序库，其用以取代传统的C函数，如printf和scanf。随后所引入的程序库中最重要的便是标准模板库，简称STL。

多年后，一个联合的ANSI-ISO委员会于1998年对C++标准化（ISO/IEC 14882：1998）。在官方发布1998标准的若干年后，委员会处理缺陷报告，并于2003年发布一个C++标准的修正版本。2005年，一份名为Library Technical Report 1（简称TR1）的技术报告发布。虽然还不是官方标准的一部分，不过它所提供的几个扩展可望成为下一版C++标准的一部分。几乎所有目前仍在维护的C++编译器皆已支持TR1。

目前最新的C++标准是2017年12月发布的ISO/IEC 14882:2017[5]，又称C++17或C++1z。

虽然C++免专利，但标准文件本身并不是免费的，尽管标准文档不是免费的，但是很容易从网络中获取，最简单的就是C++标准文档之前的最后一次草稿版本，它与标准的差别几乎只在于排版上。
