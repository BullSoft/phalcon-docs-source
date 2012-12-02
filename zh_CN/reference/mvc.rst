MVC架构
====================
Phalcon提供面向对象的类，用于在应用中实现模型、视图和控制器架构（通常被称为 MVC_ ）。这种设计模式被广泛用于WEB框架和桌面程序中。

Phalcon offers the object oriented classes, necessary to implement the Model, View, Controller architecture (often referred to as MVC_) in your application. This design pattern is widely used by other web frameworks and desktop applications. 

MVC带来的收益包括：

* 把业务逻辑从用户界面和数据库层分离出来
* 使代码结构清晰，易于日后维护

MVC benefits include:

* Isolation of business logic from the user interface and the database layer
* Making it clear where different types of code belong for easier maintenance

如果你打算使用MVC架构，每个到你应用资源的请求都会使用 MVC_ 架构调度。Phalcon框架的类是使用C语言开发的，它为PHP项目提供了高性能的MVC模式。

If you decide to use MVC, every request to your application resources will be managed by the MVC_ architecture. Phalcon classes are written in C language, offering a high performance approach of this pattern in a PHP based application. 

模型
------
模型代表应用程序的信息（数据）和操作数据的规则。模型主要用于管理与相关数据库表交互的规则。在大多数情况下，库中的每张表都会关联应用中的一个模型。应用程序的大部分业务逻辑都会由模型来完成。 :doc:`详见 <models>`

A model represents the information (data) of the application and the rules to manipulate that data. Models are primarily used for managing the rules of interaction with a corresponding database table. In most cases, each table in your database will correspond to one model in your application. The bulk of your application's business logic will be concentrated in the models. :doc:`Learn more <models>`

视图
-----
视图代表应用程序的用户界面。视图通过是HTML文件，内嵌了一些PHP代码用于相关数据显示。视图负责向浏览器或其他请求应用的工具传递数据。 :doc:`详见 <views>`

Views represent the user interface of your application. Views are often HTML files with embedded PHP code that perform tasks related solely to the presentation of the data. Views handle the job of providing data to the web browser or other tool that is used to make requests from your application. :doc:`Learn more <views>`

控制器
-----------
控制器提供了模型和视图之前的“流”。控制器负责处理浏览器发起的请求，向模型索要数据，并且把数据传递给视图显示。 :doc:`详见 <controllers>`

The controllers provide the "flow" between models and views. Controllers are responsible for processing the incoming requests from the web browser, interrogating the models for data, and passing that data on to the views for presentation. :doc:`Learn more <controllers>`

.. _MVC: http://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93controller
