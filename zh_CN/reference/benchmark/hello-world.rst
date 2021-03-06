<<<<<<< HEAD:zh_CN/reference/benchmark/hello-world.rst
“你好，世界”基准程序
=====================

基准程序是如何执行的？
----------------------------------
我们创建了一个“你好，世界”的基准程序，来测试各框架的最小开销。很多人不喜欢这类基准程序，因为实际生产环境的情况要复杂得多。但是，这些测试能够让我们知晓各框架执行简单任务的最短时间。这种任务预示着各框架处理单请求的最小开销。

We created a "Hello World" benchmark seeking to identify the smallest load overhead of each framework. Many people don't like this kind of benchmark because real-world applications require more complex features or structures. However, these tests identify the minimum time spent by each framework to perform a simple task. Such a task represents the mimimum requirement for every framework to process a single request.

更具体地说，这个基本准程序仅仅表示框架启动所需要的时间，运行动作，请求结束时释放资源。任何MVC框架都要执行这个过程。这个基准是如此的简单，任何稍复杂的请求所耗费的时间都会比它长。

More specifically, the benchmark only measures the time it takes for a framework to start, run an action and free up resources at the end of the request. Any PHP application based on an MVC architecture will require this time. Due to the simplicity of the benchmark, we ensure that the time needed for a more complex request will be higher.

我们也为每个框架创建了一个控制器和一个视图。控制器"say"和动作"hello"。在动作中只做一件事，向视图发送数据("Hello!")。使用"ab"基准测试工具，我们对每个框架发送5并发、总共1000次请求。

A controller and a view have been created for each framework. The controller "say" and action "hello". The action only sends data to the view which displays it ("Hello!"). Using the "ab" benchmark tool we sent 1000 requests using 5 concurrent connections to each framework.

测试结果如何？
--------------------------------
下面是我们考量的方法，以确保能体现每个框架的整体性能：

These were the measurements we record to identify the overall performance of each framework:

* 每秒请求数
* 所有并发请求的时间
* 单请求包含的PHP文你看数目（使用 get_included_files_ 函数测量）。
* 每次请求的内存使用（使用 memory_get_usage_ 函数测量）。

* Requests per second
* Time across all concurrent requests
* Number of included PHP files on a single request (measured using function get_included_files_.
* Memory Usage per request (measured using function memory_get_usage_.

参与测试的框架
-----------------------
* Yii_ (YII_DEBUG=false) (yii-1.1.12.b600af)
* Symfony_ (2.0.11)
* `Zend Framework`_ (1.11.11)
* Kohana_ (3.2.0)
* FuelPHP_ (1.2.1)
* CakePHP_ (2.1.3)
* Laravel_ 3.2.5
* CodeIgniter_ (2.1.0)
* Nette_ (2.0.4)

结果
-------

Yii (YII_DEBUG=false) Version yii-1.1.12.b600af
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^


.. code-block:: php

	# ab -n 1000 -c 5 http://localhost/bench/yii/index.php?r=say/hello
	This is ApacheBench, Version 2.3 <$Revision: 655654 $>
	Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
	Licensed to The Apache Software Foundation, http://www.apache.org/

	Benchmarking localhost (be patient)


	Server Software:        Apache/2.2.22
	Server Hostname:        localhost
	Server Port:            80

	Document Path:          /bench/yii/index.php?r=say/hello
	Document Length:        61 bytes

	Concurrency Level:      5
	Time taken for tests:   1.174 seconds
	Complete requests:      1000
	Failed requests:        0
	Write errors:           0
	Total transferred:      254000 bytes
	HTML transferred:       61000 bytes
	Requests per second:    851.83 [#/sec] (mean)
	Time per request:       5.870 [ms] (mean)
	Time per request:       1.174 [ms] (mean, across all concurrent requests)
	Transfer rate:          211.29 [Kbytes/sec] received

	Connection Times (ms)
	              min  mean[+/-sd] median   max
	Connect:        0    6   2.5      5      20
	Processing:     0    0   0.4      0       6
	Waiting:        0    0   0.3      0       6
	Total:          2    6   2.5      5      20

	Percentage of the requests served within a certain time (ms)
	  50%      5
	  66%      6
	  75%      7
	  80%      7
	  90%      9
	  95%     11
	  98%     14
	  99%     15
	 100%     20 (longest request)

Symfony Version 2.0.11
^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: php

	# ab -n 1000 -c 5 http://localhost/bench/Symfony/web/app.php/say/hello/
	This is ApacheBench, Version 2.3 <$Revision: 655654 $>
	Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
	Licensed to The Apache Software Foundation, http://www.apache.org/

	Benchmarking localhost (be patient)


	Server Software:        Apache/2.2.22
	Server Hostname:        localhost
	Server Port:            80

	Document Path:          /bench/Symfony/web/app.php/say/hello/
	Document Length:        16 bytes

	Concurrency Level:      5
	Time taken for tests:   1.848 seconds
	Complete requests:      1000
	Failed requests:        0
	Write errors:           0
	Total transferred:      249000 bytes
	HTML transferred:       16000 bytes
	Requests per second:    541.01 [#/sec] (mean)
	Time per request:       9.242 [ms] (mean)
	Time per request:       1.848 [ms] (mean, across all concurrent requests)
	Transfer rate:          131.55 [Kbytes/sec] received

	Connection Times (ms)
	              min  mean[+/-sd] median   max
	Connect:        0    9   4.8      8      61
	Processing:     0    0   0.6      0      15
	Waiting:        0    0   0.6      0      15
	Total:          4    9   4.8      8      61

	Percentage of the requests served within a certain time (ms)
	  50%      8
	  66%      9
	  75%     11
	  80%     12
	  90%     15
	  95%     18
	  98%     22
	  99%     30
	 100%     61 (longest request)

CodeIgniter 2.1.0
^^^^^^^^^^^^^^^^^


.. code-block:: php

	# ab -n 1000 -c 5 http://localhost/bench/codeigniter/index.php/say/hello
	This is ApacheBench, Version 2.3 <$Revision: 655654 $>
	Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
	Licensed to The Apache Software Foundation, http://www.apache.org/

	Benchmarking localhost (be patient)


	Server Software:        Apache/2.2.22
	Server Hostname:        localhost
	Server Port:            80

	Document Path:          /bench/codeigniter/index.php/say/hello
	Document Length:        16 bytes

	Concurrency Level:      5
	Time taken for tests:   1.159 seconds
	Complete requests:      1000
	Failed requests:        0
	Write errors:           0
	Total transferred:      209000 bytes
	HTML transferred:       16000 bytes
	Requests per second:    862.58 [#/sec] (mean)
	Time per request:       5.797 [ms] (mean)
	Time per request:       1.159 [ms] (mean, across all concurrent requests)
	Transfer rate:          176.05 [Kbytes/sec] received

	Connection Times (ms)
	              min  mean[+/-sd] median   max
	Connect:        0    6   3.3      5      34
	Processing:     0    0   1.5      0      34
	Waiting:        0    0   1.5      0      34
	Total:          2    6   3.5      5      35

	Percentage of the requests served within a certain time (ms)
	  50%      5
	  66%      6
	  75%      6
	  80%      7
	  90%      8
	  95%     12
	  98%     17
	  99%     24
	 100%     35 (longest request)


Kohana 3.2.0
^^^^^^^^^^^^

.. code-block:: php

	# ab -n 1000 -c 5 http://localhost/bench/kohana/index.php/say/hello
	This is ApacheBench, Version 2.3 <$Revision: 655654 $>
	Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
	Licensed to The Apache Software Foundation, http://www.apache.org/

	Benchmarking localhost (be patient)


	Server Software:        Apache/2.2.22
	Server Hostname:        localhost
	Server Port:            80

	Document Path:          /bench/kohana/index.php/say/hello
	Document Length:        15 bytes

	Concurrency Level:      5
	Time taken for tests:   1.375 seconds
	Complete requests:      1000
	Failed requests:        0
	Write errors:           0
	Total transferred:      223000 bytes
	HTML transferred:       15000 bytes
	Requests per second:    727.07 [#/sec] (mean)
	Time per request:       6.877 [ms] (mean)
	Time per request:       1.375 [ms] (mean, across all concurrent requests)
	Transfer rate:          158.34 [Kbytes/sec] received

	Connection Times (ms)
	              min  mean[+/-sd] median   max
	Connect:        0    7   3.3      6      37
	Processing:     0    0   0.6      0      10
	Waiting:        0    0   0.4      0       6
	Total:          3    7   3.3      6      37

	Percentage of the requests served within a certain time (ms)
	  50%      6
	  66%      7
	  75%      8
	  80%      8
	  90%     10
	  95%     13
	  98%     16
	  99%     20
	 100%     37 (longest request)


Fuel 1.2.1
^^^^^^^^^^

.. code-block:: php

	# ab -n 1000 -c 5 http://localhost/bench/fuel/say/hello
	This is ApacheBench, Version 2.3 <$Revision: 655654 $>
	Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
	Licensed to The Apache Software Foundation, http://www.apache.org/

	Benchmarking localhost (be patient)


	Server Software:        Apache/2.2.22
	Server Hostname:        localhost
	Server Port:            80

	Document Path:          /bench/fuel/public/say/hello
	Document Length:        16 bytes

	Concurrency Level:      5
	Time taken for tests:   1.759 seconds
	Complete requests:      1000
	Failed requests:        0
	Write errors:           0
	Total transferred:      209000 bytes
	HTML transferred:       16000 bytes
	Requests per second:    568.41 [#/sec] (mean)
	Time per request:       8.796 [ms] (mean)
	Time per request:       1.759 [ms] (mean, across all concurrent requests)
	Transfer rate:          116.01 [Kbytes/sec] received

	Connection Times (ms)
	              min  mean[+/-sd] median   max
	Connect:        0    9   4.3      8      51
	Processing:     0    0   1.3      0      34
	Waiting:        0    0   1.3      0      34
	Total:          4    9   4.4      8      51

	Percentage of the requests served within a certain time (ms)
	  50%      8
	  66%      9
	  75%     10
	  80%     11
	  90%     13
	  95%     17
	  98%     22
	  99%     26
	 100%     51 (longest request)

Cake 2.1.3
^^^^^^^^^^

.. code-block:: php

	# ab -n 10 -c 5 http://localhost/bench/cake/say/hello
	This is ApacheBench, Version 2.3 <$Revision: 655654 $>
	Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
	Licensed to The Apache Software Foundation, http://www.apache.org/

	Benchmarking localhost (be patient).....done


	Server Software:        Apache/2.2.22
	Server Hostname:        localhost
	Server Port:            80

	Document Path:          /bench/cake/say/hello
	Document Length:        16 bytes

	Concurrency Level:      5
	Time taken for tests:   30.051 seconds
	Complete requests:      10
	Failed requests:        0
	Write errors:           0
	Total transferred:      1680 bytes
	HTML transferred:       160 bytes
	Requests per second:    0.33 [#/sec] (mean)
	Time per request:       15025.635 [ms] (mean)
	Time per request:       3005.127 [ms] (mean, across all concurrent requests)
	Transfer rate:          0.05 [Kbytes/sec] received

	Connection Times (ms)
	              min  mean[+/-sd] median   max
	Connect:        0    2   3.6      0      11
	Processing: 15009 15020   9.8  15019   15040
	Waiting:        9   21   7.9     25      33
	Total:      15009 15022   8.9  15021   15040

	Percentage of the requests served within a certain time (ms)
	  50%  15021
	  66%  15024
	  75%  15024
	  80%  15032
	  90%  15040
	  95%  15040
	  98%  15040
	  99%  15040
	 100%  15040 (longest request)

Zend Framework 1.11.11
^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: php

	# ab -n 10 -c 5 http://localhost/bench/zendfw/public/say/hello
	This is ApacheBench, Version 2.3 <$Revision: 655654 $>
	Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
	Licensed to The Apache Software Foundation, http://www.apache.org/

	Benchmarking localhost (be patient)


	Server Software:        Apache/2.2.22
	Server Hostname:        localhost
	Server Port:            80

	Document Path:          /bench/zendfw/public/say/hello
	Document Length:        16 bytes

	Concurrency Level:      5
	Time taken for tests:   3.086 seconds
	Complete requests:      1000
	Failed requests:        0
	Write errors:           0
	Total transferred:      209000 bytes
	HTML transferred:       16000 bytes
	Requests per second:    324.02 [#/sec] (mean)
	Time per request:       15.431 [ms] (mean)
	Time per request:       3.086 [ms] (mean, across all concurrent requests)
	Transfer rate:          66.13 [Kbytes/sec] received

	Connection Times (ms)
	              min  mean[+/-sd] median   max
	Connect:        0   15   6.1     14      61
	Processing:     0    0   1.7      0      37
	Waiting:        0    0   1.7      0      36
	Total:          8   15   6.1     14      61

	Percentage of the requests served within a certain time (ms)
	  50%     14
	  66%     16
	  75%     17
	  80%     18
	  90%     23
	  95%     27
	  98%     33
	  99%     37
	 100%     61 (longest request)

Laravel 3.2.5
^^^^^^^^^^^^^

.. code-block:: php

	This is ApacheBench, Version 2.3 <$Revision: 655654 $>
	Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
	Licensed to The Apache Software Foundation, http://www.apache.org/

	Benchmarking localhost (be patient)


	Server Software:        Apache/2.2.22
	Server Hostname:        localhost
	Server Port:            80

	Document Path:          /bench/laravel/public/say/hello
	Document Length:        15 bytes

	Concurrency Level:      5
	Time taken for tests:   2.353 seconds
	Complete requests:      1000
	Failed requests:        0
	Write errors:           0
	Total transferred:      831190 bytes
	HTML transferred:       15000 bytes
	Requests per second:    424.97 [#/sec] (mean)
	Time per request:       11.765 [ms] (mean)
	Time per request:       2.353 [ms] (mean, across all concurrent requests)
	Transfer rate:          344.96 [Kbytes/sec] received

	Connection Times (ms)
	              min  mean[+/-sd] median   max
	Connect:        0   12   5.6     10      56
	Processing:     0    0   0.6      0      10
	Waiting:        0    0   0.5      0      10
	Total:          5   12   5.6     10      56

	Percentage of the requests served within a certain time (ms)
	  50%     10
	  66%     12
	  75%     13
	  80%     15
	  90%     18
	  95%     22
	  98%     29
	  99%     36
	 100%     56 (longest request)

Nette 2.0.4
^^^^^^^^^^^

.. code-block:: php

	# ab -n 1000 -c 5 http://localhost/bench/nette/www/index.php

	This is ApacheBench, Version 2.3 <$Revision: 655654 $>
	Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
	Licensed to The Apache Software Foundation, http://www.apache.org/

	Benchmarking localhost (be patient)


	Server Software:        Apache/2.2.22
	Server Hostname:        localhost
	Server Port:            80

	Document Path:          /bench/helloworld/nette/www/index.php
	Document Length:        205 bytes

	Concurrency Level:      5
	Time taken for tests:   3.569 seconds
	Complete requests:      1000
	Failed requests:        0
	Write errors:           0
	Total transferred:      448000 bytes
	HTML transferred:       205000 bytes
	Requests per second:    280.18 [#/sec] (mean)
	Time per request:       17.846 [ms] (mean)
	Time per request:       3.569 [ms] (mean, across all concurrent requests)
	Transfer rate:          122.58 [Kbytes/sec] received

	Connection Times (ms)
	              min  mean[+/-sd] median   max
	Connect:        0   18   6.6     16      63
	Processing:     0    0   1.0      0      20
	Waiting:        0    0   1.0      0      20
	Total:          9   18   6.6     16      63

	Percentage of the requests served within a certain time (ms)
	  50%     16
	  66%     19
	  75%     20
	  80%     22
	  90%     27
	  95%     31
	  98%     37
	  99%     39
	 100%     63 (longest request)


Phalcon Version 0.5.0
^^^^^^^^^^^^^^^^^^^^^

.. code-block:: php

	# ab -n 1000 -c 5 http://localhost/bench/phalcon/index.php?_url=/say/hello
	This is ApacheBench, Version 2.3 <$Revision: 655654 $>
	Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
	Licensed to The Apache Software Foundation, http://www.apache.org/

	Benchmarking localhost (be patient)


	Server Software:        Apache/2.2.22
	Server Hostname:        localhost
	Server Port:            80

	Document Path:          /bench/phalcon/index.php?_url=/say/hello
	Document Length:        16 bytes

	Concurrency Level:      5
	Time taken for tests:   0.419 seconds
	Complete requests:      1000
	Failed requests:        0
	Write errors:           0
	Total transferred:      209000 bytes
	HTML transferred:       16000 bytes
	Requests per second:    2386.74 [#/sec] (mean)
	Time per request:       2.095 [ms] (mean)
	Time per request:       0.419 [ms] (mean, across all concurrent requests)
	Transfer rate:          487.14 [Kbytes/sec] received

	Connection Times (ms)
	              min  mean[+/-sd] median   max
	Connect:        0    2   1.1      2      17
	Processing:     0    0   0.1      0       3
	Waiting:        0    0   0.1      0       2
	Total:          1    2   1.1      2      17

	Percentage of the requests served within a certain time (ms)
	  50%      2
	  66%      2
	  75%      2
	  80%      2
	  90%      3
	  95%      4
	  98%      5
	  99%      7
	 100%     17 (longest request)


图表
^^^^^^

第一张图表显示每个框架每秒处理的请求数。每二张图显示处理所有并发请求的平均时间。

The first graph shows how many requests per second each framework was able to accept. The second shows the average time across all concurrent requests.


.. raw:: html

	<script type="text/javascript" src="https://www.google.com/jsapi"></script>
	<script type="text/javascript">
		google.load("visualization", "1", {packages:["corechart"]});
		google.setOnLoadCallback(drawChart);

		function drawChart() {

			var data = new google.visualization.DataTable();
			data.addColumn('string', 'Framework');
			data.addColumn('number', 'Requests per second');
			data.addRows([
				['Nette', 280.18],
				['Zend', 324.02],
				['Laravel', 424.97],
				['Symfony', 541.01],
				['Fuel', 568.41],
				['Kohana', 727.07],
				['Yii', 762.55],
				['CodeIgniter', 862.58],
				['Phalcon', 2386.74]
			]);

			var options = {
				title: 'Framework / Requests per second (#/sec) [more is better]',
				colors: ['#3366CC'],
				animation: {
					duration: 0.5
				},
				fontSize: 12,
				chartArea: {
					width: '600px'
				}
			};

			var chart = new google.visualization.ColumnChart(document.getElementById('rps_div'));
			chart.draw(data, options);

			var data = new google.visualization.DataTable();
			data.addColumn('string', 'Framework');
			data.addColumn('number', 'Time per Request');
			data.addRows([
				['Nette', 3.569],
				['Zend', 3.086],
				['Laravel', 2.353],
				['Symfony', 1.848],
				['Fuel', 1.759],
				['Kohana', 1.375],
				['Yii', 1.174],
				['CodeIgniter', 1.159],
				['Phalcon', 0.419]
			]);

			var options = {
				title: 'Framework / Time per Request (mean, across all concurrent requests) [less is better]',
				colors: ['#3366CC'],
				fontSize: 11
			};

			var chart = new google.visualization.ColumnChart(document.getElementById('tpr_div'));
			chart.draw(data, options);

			var data = new google.visualization.DataTable();
			data.addColumn('string', 'Framework');
			data.addColumn('number', 'Memory Usage (MB)');
			data.addRows([
				['Nette', 3.5],
				['Zend', 1.75],
                ['Symfony', 1.5],
                ['Yii', 1.5],
                ['Laravel', 1.25],
				['Kohana', 1.25],
				['CodeIgniter', 1.1],
				['Fuel', 1.0],
				['Phalcon', 0.75]
			]);

			var options = {
				title: 'Framework / Memory Usage (mean, megabytes per request) [less is better]',
				colors: ['#3366CC'],
				fontSize: 11
			};

			var chart = new google.visualization.ColumnChart(document.getElementById('mpr_div'));
			chart.draw(data, options);

			var data = new google.visualization.DataTable();
			data.addColumn('string', 'Framework');
			data.addColumn('number', 'Number of included PHP files');
			data.addRows([
                ['Zend', 66],
                ['Laravel', 46],
                ['Kohana', 46],
                ['Fuel', 30],
				['Yii', 27],
				['CodeIgniter', 23],
				['Symfony', 18],
				['Nette', 7],
				['Phalcon', 4]
			]);

			var options = {
				title: 'Framework / Number of included PHP files (mean, number on a single request) [less is better]',
				colors: ['#3366CC'],
				fontSize: 11
			};

			var chart = new google.visualization.ColumnChart(document.getElementById('nfi_div'));
			chart.draw(data, options);

		}
	</script>
	<div align="center">
		<div id="rps_div" style="width: 600px; height: 400px; position: relative; "><iframe name="Drawing_Frame_31166" id="Drawing_Frame_31166" width="600" height="400" frameborder="0" scrolling="no" marginheight="0" marginwidth="0"></iframe><div></div></div>
		<div id="tpr_div" style="width: 600px; height: 400px; position: relative; "><iframe name="Drawing_Frame_89467" id="Drawing_Frame_89467" width="600" height="400" frameborder="0" scrolling="no" marginheight="0" marginwidth="0"></iframe><div></div></div>
		<div id="nfi_div" style="width: 600px; height: 400px; position: relative; "><iframe name="Drawing_Frame_49746" id="Drawing_Frame_49746" width="600" height="400" frameborder="0" scrolling="no" marginheight="0" marginwidth="0"></iframe><div></div></div>
		<div id="mpr_div" style="width: 600px; height: 400px; position: relative; "><iframe name="Drawing_Frame_77939" id="Drawing_Frame_77939" width="600" height="400" frameborder="0" scrolling="no" marginheight="0" marginwidth="0"></iframe><div></div></div>
	</div>

总结
----------
Phalcon是编译好的C-扩展，因为这个特性，在这些基准测试中，它的性能表现远优于其他框架。

The compiled nature of Phalcon offers extraordinary performance that outperforms all other frameworks measured in these benchmarks.

.. _get_included_files: http://www.php.net/manual/en/function.get-included-files.php
.. _memory_get_usage: http://php.net/manual/en/function.memory-get-usage.php
.. _Yii: http://www.yiiframework.com/
.. _Symfony: http://symfony.com/
.. _CodeIgniter: http://codeigniter.com/
.. _Kohana: http://kohanaframework.org/index
.. _FuelPHP: http://fuelphp.com/
.. _CakePHP: http://cakephp.org/
.. _Laravel: http://www.laravel.com/
.. _Zend Framework: http://framework.zend.com
.. _Nette: http://nette.org/

=======
Hello World Benchmark
=====================

How the benchmarks were performed?
----------------------------------
We created a "Hello World" benchmark seeking to identify the smallest load overhead of each framework. Many
people don't like this kind of benchmark because real-world applications require more complex features or
structures. However, these tests identify the minimum time spent by each framework to perform a simple task.
Such a task represents the minimum requirement for every framework to process a single request.

More specifically, the benchmark only measures the time it takes for a framework to start, run an action and
free up resources at the end of the request. Any PHP application based on an MVC architecture will require
this time. Due to the simplicity of the benchmark, we ensure that the time needed for a more complex
request will be higher.

A controller and a view have been created for each framework. The controller "say" and action "hello". The
action only sends data to the view which displays it ("Hello!"). Using the "ab" benchmark tool we sent 2000
requests using 10 concurrent connections to each framework.

What measurements were recorded?
--------------------------------
These were the measurements we record to identify the overall performance of each framework:

* Requests per second
* Time across all concurrent requests
* Number of included PHP files on a single request (measured using function get_included_files_.
* Memory Usage per request (measured using function memory_get_usage_.

Participant Frameworks
-----------------------
* Yii_
* Symfony_ 
* `Zend Framework`_ 
* Kohana_
* FuelPHP_ 
* Laravel_
* CodeIgniter_

Results
-------

Yii (YII_DEBUG=false) Version yii-1.1.13
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: php

    # ab -n 2000 -c 10 http://localhost/bench/helloworld/yii/index.php?r=say/hello
    This is ApacheBench, Version 2.3 <$Revision: 655654 $>
    Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
    Licensed to The Apache Software Foundation, http://www.apache.org/

    Benchmarking localhost (be patient)


    Server Software:        Apache/2.2.22
    Server Hostname:        localhost
    Server Port:            80

    Document Path:          /bench/helloworld/yii/index.php?r=say/hello
    Document Length:        61 bytes

    Concurrency Level:      10
    Time taken for tests:   2.081 seconds
    Complete requests:      2000
    Failed requests:        0
    Write errors:           0
    Total transferred:      508000 bytes
    HTML transferred:       122000 bytes
    Requests per second:    961.28 [#/sec] (mean)
    Time per request:       10.403 [ms] (mean)
    Time per request:       1.040 [ms] (mean, across all concurrent requests)
    Transfer rate:          238.44 [Kbytes/sec] received

    Connection Times (ms)
                  min  mean[+/-sd] median   max
    Connect:        0   10   4.3      9      42
    Processing:     0    0   1.0      0      24
    Waiting:        0    0   0.8      0      17
    Total:          3   10   4.3      9      42

    Percentage of the requests served within a certain time (ms)
      50%      9
      66%     11
      75%     13
      80%     14
      90%     15
      95%     17
      98%     21
      99%     26
     100%     42 (longest request)

Symfony Version 2.1.6
^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: php

    # ab -n 2000 -c 10 http://localhost/bench/Symfony/web/app.php/say/hello/
    This is ApacheBench, Version 2.3 <$Revision: 655654 $>
    Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
    Licensed to The Apache Software Foundation, http://www.apache.org/

    Benchmarking localhost (be patient)


    Server Software:        Apache/2.2.22
    Server Hostname:        localhost
    Server Port:            80

    Document Path:          /bench/Symfony/web/app.php/say/hello/
    Document Length:        16 bytes

    Concurrency Level:      5
    Time taken for tests:   1.848 seconds
    Complete requests:      1000
    Failed requests:        0
    Write errors:           0
    Total transferred:      249000 bytes
    HTML transferred:       16000 bytes
    Requests per second:    541.01 [#/sec] (mean)
    Time per request:       9.242 [ms] (mean)
    Time per request:       1.848 [ms] (mean, across all concurrent requests)
    Transfer rate:          131.55 [Kbytes/sec] received

    Connection Times (ms)
                  min  mean[+/-sd] median   max
    Connect:        0    9   4.8      8      61
    Processing:     0    0   0.6      0      15
    Waiting:        0    0   0.6      0      15
    Total:          4    9   4.8      8      61

    Percentage of the requests served within a certain time (ms)
      50%      8
      66%      9
      75%     11
      80%     12
      90%     15
      95%     18
      98%     22
      99%     30
     100%     61 (longest request)

CodeIgniter 2.1.0
^^^^^^^^^^^^^^^^^

.. code-block:: php

    # ab -n 2000 -c 10 http://localhost/bench/codeigniter/index.php/say/hello
    This is ApacheBench, Version 2.3 <$Revision: 655654 $>
    Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
    Licensed to The Apache Software Foundation, http://www.apache.org/

    Benchmarking localhost (be patient)


    Server Software:        Apache/2.2.22
    Server Hostname:        localhost
    Server Port:            80

    Document Path:          /bench/helloworld/codeigniter/index.php/say/hello
    Document Length:        16 bytes

    Concurrency Level:      10
    Time taken for tests:   1.888 seconds
    Complete requests:      2000
    Failed requests:        0
    Write errors:           0
    Total transferred:      418000 bytes
    HTML transferred:       32000 bytes
    Requests per second:    1059.05 [#/sec] (mean)
    Time per request:       9.442 [ms] (mean)
    Time per request:       0.944 [ms] (mean, across all concurrent requests)
    Transfer rate:          216.15 [Kbytes/sec] received

    Connection Times (ms)
                  min  mean[+/-sd] median   max
    Connect:        0    9   4.1      9      33
    Processing:     0    0   0.8      0      19
    Waiting:        0    0   0.7      0      16
    Total:          3    9   4.2      9      33

    Percentage of the requests served within a certain time (ms)
      50%      9
      66%     10
      75%     11
      80%     12
      90%     14
      95%     16
      98%     21
      99%     24
     100%     33 (longest request)

Kohana 3.2.0
^^^^^^^^^^^^

.. code-block:: php

    # ab -n 2000 -c 10 http://localhost/bench/helloworld/kohana/index.php/say/hello
    This is ApacheBench, Version 2.3 <$Revision: 655654 $>
    Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
    Licensed to The Apache Software Foundation, http://www.apache.org/

    Benchmarking localhost (be patient)


    Server Software:        Apache/2.2.22
    Server Hostname:        localhost
    Server Port:            80

    Document Path:          /bench/helloworld/kohana/index.php/say/hello
    Document Length:        15 bytes

    Concurrency Level:      10
    Time taken for tests:   2.324 seconds
    Complete requests:      2000
    Failed requests:        0
    Write errors:           0
    Total transferred:      446446 bytes
    HTML transferred:       30030 bytes
    Requests per second:    860.59 [#/sec] (mean)
    Time per request:       11.620 [ms] (mean)
    Time per request:       1.162 [ms] (mean, across all concurrent requests)
    Transfer rate:          187.60 [Kbytes/sec] received

    Connection Times (ms)
                  min  mean[+/-sd] median   max
    Connect:        0   11   5.1     10      64
    Processing:     0    0   1.9      0      39
    Waiting:        0    0   1.4      0      35
    Total:          3   11   5.3     11      64

    Percentage of the requests served within a certain time (ms)
      50%     11
      66%     13
      75%     15
      80%     15
      90%     17
      95%     18
      98%     24
      99%     31
     100%     64 (longest request)

Fuel 1.2.1
^^^^^^^^^^

.. code-block:: php

    # ab -n 2000 -c 10 http://localhost/bench/helloworld/fuel/public/say/hello
    This is ApacheBench, Version 2.3 <$Revision: 655654 $>
    Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
    Licensed to The Apache Software Foundation, http://www.apache.org/

    Benchmarking localhost (be patient)


    Server Software:        Apache/2.2.22
    Server Hostname:        localhost
    Server Port:            80

    Document Path:          /bench/helloworld/fuel/public/say/hello
    Document Length:        16 bytes

    Concurrency Level:      10
    Time taken for tests:   2.742 seconds
    Complete requests:      2000
    Failed requests:        0
    Write errors:           0
    Total transferred:      418000 bytes
    HTML transferred:       32000 bytes
    Requests per second:    729.42 [#/sec] (mean)
    Time per request:       13.709 [ms] (mean)
    Time per request:       1.371 [ms] (mean, across all concurrent requests)
    Transfer rate:          148.88 [Kbytes/sec] received

    Connection Times (ms)
                  min  mean[+/-sd] median   max
    Connect:        0   13   6.0     12      79
    Processing:     0    0   1.3      0      22
    Waiting:        0    0   0.8      0      21
    Total:          4   14   6.1     13      80

    Percentage of the requests served within a certain time (ms)
      50%     13
      66%     15
      75%     17
      80%     17
      90%     19
      95%     24
      98%     30
      99%     38
     100%     80 (longest request)

Zend Framework 1.11.11
^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: php

    # ab -n 2000 -c 10 http://localhost/bench/helloworld/zendfw/public/index.php
    This is ApacheBench, Version 2.3 <$Revision: 655654 $>
    Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
    Licensed to The Apache Software Foundation, http://www.apache.org/

    Benchmarking localhost (be patient)


    Server Software:        Apache/2.2.22
    Server Hostname:        localhost
    Server Port:            80

    Document Path:          /bench/helloworld/zendfw/public/index.php
    Document Length:        16 bytes

    Concurrency Level:      10
    Time taken for tests:   5.641 seconds
    Complete requests:      2000
    Failed requests:        0
    Write errors:           0
    Total transferred:      418000 bytes
    HTML transferred:       32000 bytes
    Requests per second:    354.55 [#/sec] (mean)
    Time per request:       28.205 [ms] (mean)
    Time per request:       2.820 [ms] (mean, across all concurrent requests)
    Transfer rate:          72.36 [Kbytes/sec] received

    Connection Times (ms)
                  min  mean[+/-sd] median   max
    Connect:        0   27   9.6     25      89
    Processing:     0    1   3.0      0      70
    Waiting:        0    0   2.9      0      70
    Total:          9   28   9.6     26      90

    Percentage of the requests served within a certain time (ms)
      50%     26
      66%     28
      75%     32
      80%     34
      90%     41
      95%     46
      98%     55
      99%     62
     100%     90 (longest request)

Laravel 3.2.5
^^^^^^^^^^^^^

.. code-block:: php

    # ab -n 2000 -c 10 http://localhost/bench/helloworld/laravel/public/say/hello

    This is ApacheBench, Version 2.3 <$Revision: 655654 $>
    Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
    Licensed to The Apache Software Foundation, http://www.apache.org/

    Benchmarking localhost (be patient)


    Server Software:        Apache/2.2.22
    Server Hostname:        localhost
    Server Port:            80

    Document Path:          /bench/helloworld/laravel/public/say/hello
    Document Length:        15 bytes

    Concurrency Level:      10
    Time taken for tests:   4.090 seconds
    Complete requests:      2000
    Failed requests:        0
    Write errors:           0
    Total transferred:      1665162 bytes
    HTML transferred:       30045 bytes
    Requests per second:    489.03 [#/sec] (mean)
    Time per request:       20.449 [ms] (mean)
    Time per request:       2.045 [ms] (mean, across all concurrent requests)
    Transfer rate:          397.61 [Kbytes/sec] received

    Connection Times (ms)
                  min  mean[+/-sd] median   max
    Connect:        0   20   7.6     19      92
    Processing:     0    0   2.5      0      53
    Waiting:        0    0   2.5      0      53
    Total:          6   20   7.6     19      93

    Percentage of the requests served within a certain time (ms)
      50%     19
      66%     21
      75%     23
      80%     24
      90%     29
      95%     34
      98%     42
      99%     48
     100%     93 (longest request)

Phalcon Version 0.8.0
^^^^^^^^^^^^^^^^^^^^^

.. code-block:: php

    # ab -n 2000 -c 10 http://localhost/bench/helloworld/phalcon/index.php?_url=/say/hello
    This is ApacheBench, Version 2.3 <$Revision: 655654 $>
    Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
    Licensed to The Apache Software Foundation, http://www.apache.org/

    Benchmarking localhost (be patient)


    Server Software:        Apache/2.2.22
    Server Hostname:        localhost
    Server Port:            80

    Document Path:          /bench/helloworld/phalcon/index.php?_url=/say/hello
    Document Length:        16 bytes

    Concurrency Level:      10
    Time taken for tests:   0.789 seconds
    Complete requests:      2000
    Failed requests:        0
    Write errors:           0
    Total transferred:      418000 bytes
    HTML transferred:       32000 bytes
    Requests per second:    2535.82 [#/sec] (mean)
    Time per request:       3.943 [ms] (mean)
    Time per request:       0.394 [ms] (mean, across all concurrent requests)
    Transfer rate:          517.56 [Kbytes/sec] received

    Connection Times (ms)
                  min  mean[+/-sd] median   max
    Connect:        0    4   1.7      3      23
    Processing:     0    0   0.2      0       6
    Waiting:        0    0   0.2      0       6
    Total:          2    4   1.7      3      23

    Percentage of the requests served within a certain time (ms)
      50%      3
      66%      4
      75%      4
      80%      4
      90%      5
      95%      6
      98%      8
      99%     14
     100%     23 (longest request)

Graphs
^^^^^^
The first graph shows how many requests per second each framework was able to accept. The second shows the average time across all concurrent requests.

.. raw:: html

    <script type="text/javascript" src="https://www.google.com/jsapi"></script>
    <script type="text/javascript">
        google.load("visualization", "1", {packages:["corechart"]});
        google.setOnLoadCallback(drawChart);

        function drawChart() {

            var data = new google.visualization.DataTable();
            data.addColumn('string', 'Framework');
            data.addColumn('number', 'Requests per second');
            data.addRows([
                ['Zend', 354.55],
                ['Laravel', 489.03],
                ['Symfony', 541.01],
                ['Fuel', 568.41],
                ['Yii', 851.83],
                ['Kohana', 860.59],
                ['CodeIgniter', 1059.05],
                ['Phalcon', 2535.82]
            ]);

            var options = {
                title: 'Framework / Requests per second (#/sec) [more is better]',
                colors: ['#3366CC'],
                animation: {
                    duration: 0.5
                },
                fontSize: 12,
                chartArea: {
                    width: '600px'
                }
            };

            var chart = new google.visualization.ColumnChart(document.getElementById('rps_div'));
            chart.draw(data, options);

            var data = new google.visualization.DataTable();
            data.addColumn('string', 'Framework');
            data.addColumn('number', 'Time per Request');
            data.addRows([
                ['Zend', 2.820],
                ['Laravel', 2.045],
                ['Symfony', 1.848],
                ['Fuel', 1.371],
                ['Yii', 1.174],
                ['Kohana', 1.162],
                ['CodeIgniter', 0.944],
                ['Phalcon', 0.394]
            ]);

            var options = {
                title: 'Framework / Time per Request (mean, across all concurrent requests) [less is better]',
                colors: ['#3366CC'],
                fontSize: 11
            };

            var chart = new google.visualization.ColumnChart(document.getElementById('tpr_div'));
            chart.draw(data, options);

            var data = new google.visualization.DataTable();
            data.addColumn('string', 'Framework');
            data.addColumn('number', 'Memory Usage (MB)');
            data.addRows([
                ['Zend', 1.75],
                ['Symfony', 1.5],
                ['Yii', 1.5],
                ['Laravel', 1.25],
                ['Kohana', 1.25],
                ['CodeIgniter', 1.1],
                ['Fuel', 1.0],
                ['Phalcon', 0.75]
            ]);

            var options = {
                title: 'Framework / Memory Usage (mean, megabytes per request) [less is better]',
                colors: ['#3366CC'],
                fontSize: 11
            };

            var chart = new google.visualization.ColumnChart(document.getElementById('mpr_div'));
            chart.draw(data, options);

            var data = new google.visualization.DataTable();
            data.addColumn('string', 'Framework');
            data.addColumn('number', 'Number of included PHP files');
            data.addRows([
                ['Zend', 66],
                ['Laravel', 46],
                ['Kohana', 46],
                ['Fuel', 30],
                ['Yii', 27],
                ['CodeIgniter', 23],
                ['Symfony', 18],
                ['Phalcon', 4]
            ]);

            var options = {
                title: 'Framework / Number of included PHP files (mean, number on a single request) [less is better]',
                colors: ['#3366CC'],
                fontSize: 11
            };

            var chart = new google.visualization.ColumnChart(document.getElementById('nfi_div'));
            chart.draw(data, options);

        }
    </script>
    <div align="center">
        <div id="rps_div" style="width: 600px; height: 400px; position: relative; "><iframe name="Drawing_Frame_31166" id="Drawing_Frame_31166" width="600" height="400" frameborder="0" scrolling="no" marginheight="0" marginwidth="0"></iframe><div></div></div>
        <div id="tpr_div" style="width: 600px; height: 400px; position: relative; "><iframe name="Drawing_Frame_89467" id="Drawing_Frame_89467" width="600" height="400" frameborder="0" scrolling="no" marginheight="0" marginwidth="0"></iframe><div></div></div>
        <div id="nfi_div" style="width: 600px; height: 400px; position: relative; "><iframe name="Drawing_Frame_49746" id="Drawing_Frame_49746" width="600" height="400" frameborder="0" scrolling="no" marginheight="0" marginwidth="0"></iframe><div></div></div>
        <div id="mpr_div" style="width: 600px; height: 400px; position: relative; "><iframe name="Drawing_Frame_77939" id="Drawing_Frame_77939" width="600" height="400" frameborder="0" scrolling="no" marginheight="0" marginwidth="0"></iframe><div></div></div>
    </div>

Conclusion
----------
The compiled nature of Phalcon offers extraordinary performance that outperforms all other frameworks measured in these benchmarks.

.. _get_included_files: http://www.php.net/manual/en/function.get-included-files.php
.. _memory_get_usage: http://php.net/manual/en/function.memory-get-usage.php
.. _Yii: http://www.yiiframework.com/
.. _Symfony: http://symfony.com/
.. _CodeIgniter: http://codeigniter.com/
.. _Kohana: http://kohanaframework.org/index
.. _FuelPHP: http://fuelphp.com/
.. _Laravel: http://www.laravel.com/
.. _Zend Framework: http://framework.zend.com

>>>>>>> e49c0b4730fea45ee3885b7b7b6cbc894b8de0d4:zh/reference/benchmark/hello-world.rst
