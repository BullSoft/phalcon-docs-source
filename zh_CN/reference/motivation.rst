我们的目标
==============

如今有很多 PHP 框架，但没有一个像 Phalcon 一样（真的，请相信我们）。

There are many PHP frameworks nowadays, but none of them is like Phalcon (Really, trust us on this one).

几乎所有的程序员都倾向于使用框架来开发。主要是因为框架提供了很多已经通过测试并可用的功能，因此能保证代码的高效、整洁、重用性。但是，在实际应用中，处理每次请求时，框架本身却需要包含很多文件，解释执行数百行代码。此种情况会影响应用的效率，紧接着便会影响最终用户体现。

Almost all programmers prefer to use a framework. This is primarily because it provides a lot of functionality that is already tested and ready to use, therefore keeping code DRY (Don't Repeat Yourself). However, the framework itself demands a lot of file inclusions and hundreds of lines of code to be interpreted and executed on each request from the actual application. This operation slows the application down and subsequently impacts the end user experience.

问题在哪？
------------

为什么我们不能有一个更完美的框架呢？

Why can't we have a framework with all of its advantages but with none or very few disadvantages?

这就是 Phalcon 诞生的原因！

This is why Phalcon was born!

在最近的几个月里，我们已经广泛地研究了 PHP 的行为，调查了能进行显著优化（或大或小）的地方。通过对 Zend 引擎的理解，我们设法移除非必需的验证、压缩代码、性能优化并且生成低代价的解决方案以使 Phalcon 达到最佳性能。

During the last few months, we have extensively researched PHP's behavior, investigating areas for significant optimizations (big or small). Through understanding of the Zend Engine, we managed to remove unnecessary validations, compacted code, performed optimizations and generated low-level solutions so as to achieve maximum performance from Phalcon. 

为什么要用框架？
----

* 在 PHP 专业开发领域，已经强制要求使用框架进行开发了。
* The use of frameworks has become mandatory in professional development with PHP
* 框架提供了结构化的理念，以轻松维护项目，编写更少的代码，使工作变得更有趣
* Frameworks offer a structured philosophy to easily maintain projects writing less code and making work more fun

PHP 内部工作原理？
----------------------

* PHP 是动态弱类型语言。每次进行二进制运算时（如： 2 + "2" ），PHP 都会检查操作数类型，并做隐式类型转换
* PHP has dynamic and weak variable types. Every time a binary operation is made (ex. 2 + "2"), PHP checks the operand types to perform potential conversions
* PHP 是解释执行的而不是编译执行的。最主要的缺点就是性能损失
* PHP is interpreted and not compiled. The major disadvantage is performance loss
* 每次请求一个脚本时，它首先要被解释一次。  
* Every time a script is requested it must be first interpreted.
* 如果你没有使用字节码缓存（像 APC 这种工具），每次请求都会对每个文件进行语法检查。
* If a bytecode cache (like APC) isn't used, syntax checking is performed every time for every file in the request

传统 PHP 框架是如何工作的？
------------------------------------

* Many files with classes and functions are read on every request made. Disk reading is expensive in terms of performance, especially when the file structure includes deep folders
* Modern frameworks use lazy loading (autoload) to increase performance (for load and execute only the code needed)
* Continuous loading or interpreting is expensive and impacts performance
* The framework code does not change very often, therefore an application needs to load and interpret it every time a request is made

How does a PHP C-extension work?
--------------------------------

* C extensions are loaded together with PHP one time on the web server's daemon start process
* Classes and functions provided by the extension are ready to use for any application
* The code isn't interpreted because is already compiled to a specific platform and processor

How does Phalcon work?
----------------------

* Components are loosely coupled. With Phalcon, nothing is imposed on you: you're free to use the full framework, or just some parts of it as a glue components.
* Low-level optimizations provides the lowest overhead for MVC-based applications
* Interact with databases with maximum performance by using a C-language ORM for PHP
* Phalcon directly accesses internal PHP structures optimizing execution in that way as well

Conclusion
----------
Phalcon is an effort to build the fastest framework for PHP. You now have an even easier and robust way to develop applications without be worrying about performance. Enjoy! 

