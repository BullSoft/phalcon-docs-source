安装
============

PHP 扩展安装过程与传统基于 PHP 代码的库或框架安装过程稍微有点不一样。
你可以下载指定平台的二进制包或从源码重新构建。

PHP extensions require a slightly different installation method to a traditional php-based library or framework. You can either download a binary package for the system of your choice or build it from the sources.

在最近的几个月里，我们已经广泛地研究了 PHP 的行为，调查了能进行显著优
化（或大或小）的地方。通过对 Zend 引擎的理解，我们设法移除不必要的验证、
压缩代码量并做性能优化，最终得到一个低代价的解决方案 － 高性能的
Phalcon 。

During the last few months, we have extensively researched PHP's behavior, investigating areas for significant optimizations (big or small). Through understanding of the Zend Engine, we managed to remove unecessary validations, compacted code, performed optimizations and generated low-level solutions so as to achieve maximum performance from Phalcon.

.. highlights::
   Phalcon 使用 PHP 5.3.1编译，但是因为旧版本 PHP 的 BUG导致内存泄露，
 我们建议您使用 PHP 5.3.11及以后的版本。

.. highlights::
   Phalcon compiles with PHP 5.3.1, but due to old PHP bugs causing memory leaks, we highly recommend you to use at least PHP 5.3.11 or greater.

Windows 系统
-------

要在 Windows 平台使用 Phalcon，你需要下载一个 DLL 库文件。编辑 php.ini
文件，在最后加上这一句：

    extension=php_phalcon.dll
   
To use phalcon on Windows you can download a DLL library. Edit your php.ini file and then append at the end:

    extension=php_phalcon.dll

重启 Web 服务器。
Restart your webserver.

下面将以图文并茂的方式提供更详细的教程：

The following screencast is a step-by-step guide to install Phalcon on Windows:

.. raw:: html

   <div align="center"><iframe src="http://player.vimeo.com/video/40265988" width="500" height="266" frameborder="0" webkitAllowFullScreen mozallowfullscreen allowFullScreen></iframe></div>

相关主题
^^^^^^^^^^^^^^

.. toctree::
   :maxdepth: 1

   xampp
   wamp

Unix/Linux
----------

使用 Unix/Linux 系统，你能很容易从源码编译和安装扩展：

On a Unix/Linux system you can easily compile and install the extension from the source code:

要求
^^^^^^^^^^^^
依赖的软件包如下：

Prerequisite packages are:

* PHP 5.x development resources
* GCC compiler (Linux) or Xcode (Mac)
* Git (if not already installed in your system - unless you download the package from GitHub and upload it on your server via FTP/FTP)

.. code-block:: bash

    #Ubuntu
    sudo apt-get install php5-dev php5-mysql gcc
    sudo apt-get install git-core

    #Suse
    yast2 -i php5-pear php5-dev php5-mysql gcc
    yast2 -i git-core

编译
^^^^^^^^^^^
创建扩展：

Creating the extension:

.. code-block:: bash

    git clone git://github.com/phalcon/cphalcon.git
    cd cphalcon/build
    export CC="gcc"
    export CFLAGS="-O2 -fno-delete-null-pointer-checks"
    phpize
    ./configure --enable-phalcon
    make
    sudo make install

在 php.ini 中加入扩展配置

Add extension to your php.ini

.. code-block:: bash

    extension=phalcon.so

重启Web服务器
Restart the webserver


FreeBSD
^^^^^^^
A port is available for FreeBSD. Just only need these simple line commands to install it:

.. code-block:: bash

    pkg_add -r phalcon

or

.. code-block:: bash

    export CFLAGS="-O2 -fno-delete-null-pointer-checks"
    cd /usr/ports/www/phalcon && make install clean

安装注意事项
^^^^^^^^^^^^^^^^^^

Web服务器安装注意事项：

Installation notes for Web Servers:

.. toctree::
   :maxdepth: 1

   nginx

