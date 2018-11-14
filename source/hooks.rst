.. _hooks:

********
钩子系统
********

钩子概览
    - `beforeRequest`_
    - `beforeExit`_

beforeRequest
=============

在爬虫程序发起一个请求前执行。钩子接受两个参数：
    - $spider，当前的爬虫程序。
    - $request，当前的Request请求对象。

beforeExit
==========

在爬虫程序终止前执行，在 `on_exit` 回调之前执行。回调接受两个参数：
    - $spider，当前的爬虫程序。
    - $exit_code，退出状态码，正常退出时为0。
