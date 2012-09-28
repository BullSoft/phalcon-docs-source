我们的目标
==============

如今有很多 PHP 框架，但没有一个像 Phalcon 一样（真的，请相信我们）。

There are many PHP frameworks nowadays, but none of them is like Phalcon (Really, trust us on this one).

几乎所有的程序员都倾向于使用框架来开发。主要是因为框架提供了很多已经通过测试并可用的功能，因此能保证代码的高效、整洁、重用性。但是，在实际应用中，处理每次请求时，框架本身却需要包含很多文件，解释执行数百行代码。此种情况会影响应用的效率，紧接着便会影响最终用户体验。

Almost all programmers prefer to use a framework. This is primarily because it provides a lot of functionality that is already tested and ready to use, therefore keeping code DRY (Don't Repeat Yourself). However, the framework itself demands a lot of file inclusions and hundreds of lines of code to be interpreted and executed on each request from the actual application. This operation slows the application down and subsequently impacts the end user experience.

问题在哪？
------------

为什么我们不能有一个更完美的框架呢？

Why can't we have a framework with all of its advantages but with none or very few disadvantages?

这就是 Phalcon 诞生的原因！

This is why Phalcon was born!

在最近的几个月里，我们已经广泛地研究了 PHP 的行为，调查了能进行显著优
化（或大或小）的地方。通过对 Zend 引擎的理解，我们设法移除不必要的验证、
压缩代码量并做性能优化，最终得到一个低代价的解决方案 － 高性能的
Phalcon 。

During the last few months, we have extensively researched PHP's behavior, investigating areas for significant optimizations (big or small). Through understanding of the Zend Engine, we managed to remove unnecessary validations, compacted code, performed optimizations and generated low-level solutions so as to achieve maximum performance from Phalcon. 

为什么要用框架？
----

* 在 PHP 专业开发领域，已经强制要求使用框架进行开发了
* The use of frameworks has become mandatory in professional development with PHP
* 框架提供了结构化的理念，能让开发者更轻松维护项目，编写更少的代码，工作也会变得更有趣
* Frameworks offer a structured philosophy to easily maintain projects writing less code and making work more fun

PHP 内部工作原理？
----------------------

* PHP 是动态弱类型语言。每次进行二进制运算时（如： 2 + "2" ），PHP 都会检查操作数类型，并做隐式类型转换
* PHP has dynamic and weak variable types. Every time a binary operation is made (ex. 2 + "2"), PHP checks the operand types to perform potential conversions
* PHP 是解释执行的而不是编译执行的。这种做法最在的缺点就是性能损失
* PHP is interpreted and not compiled. The major disadvantage is performance loss
* 每次请求一个脚本时，首先它都会被解释一次
* Every time a script is requested it must be first interpreted.
* 如果你没有使用字节码缓存（像 APC 这种工具），每次请求都会对每个文件进行语法检查
* If a bytecode cache (like APC) isn't used, syntax checking is performed every time for every file in the request

传统 PHP 框架是如何工作的？
------------------------------------

* 每次请求都会加载很多类和函数文件。而磁盘访问性能代价是极高的，特别是当目录结构较深的时候更是如此
* Many files with classes and functions are read on every request
  made. Disk reading is expensive in terms of performance, especially
  when the file structure includes deep folders
* 现代框架使用延迟加载（自动加载）来提升性能（当需要时才加载和执行文件）
* Modern frameworks use lazy loading (autoload) to increase
  performance (for load and execute only the code needed)
* 持续加载和解释是代价高昂的，很影响性能
* Continuous loading or interpreting is expensive and impacts
  performance
* 框加本身代码一般来说是不需要改动的，但每次请求都需要加载和解释执行一
  次
* The framework code does not change very often, therefore an application needs to load and interpret it every time a request is made

PHP C 扩展如何工作？
--------------------------------

* C 扩展是当 WEB 服务器守护进程启动时随着 PHP 一起加载的
* C extensions are loaded together with PHP one time on the web
  server's daemon start process
* 任何应用都可以使用扩展提供的类和函数
* Classes and functions provided by the extension are ready to use for
  any application
* 代码无需再解释执行，因为它已经在特定平台和处理器上编译过了
* The code isn't interpreted because is already compiled to a specific platform and processor

Phalcon 是如何工作的？
----------------------

* 松耦合架构。使用 Phalcon 开发，没有什么是强加在开发者身上的：你可以
  使用整个框架，也可以选择单独使用其中一部分组件
* Components are loosely coupled. With Phalcon, nothing is imposed on
  you: you're free to use the full framework, or just some parts of it
  as a glue components.
* 底层优化让基于 MVC 的应用开销最低
* Low-level optimizations provides the lowest overhead for MVC-based
  applications
* 使用 C 语言级别的 ORM 访问数据库能得到最大的性能。
* Interact with databases with maximum performance by using a
  C-language ORM for PHP
* Phalcon 通过直接访问内核中 PHP 结构这种方式优化执行过程。
* Phalcon directly accesses internal PHP structures optimizing execution in that way as well

总结
----------

Phalcon 尝试开发最快的 PHP 框架。所以，你现在有一个更简单、健壮的方式开
发应用程序了，完全不用考虑性能问题。 享受编程的快乐吧！

Phalcon is an effort to build the fastest framework for PHP. You now have an even easier and robust way to develop applications without be worrying about performance. Enjoy! 

