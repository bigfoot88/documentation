==================
部署 Odoo
==================

本文档描述了在生产环境或面向互联网的服务器上设置 Odoo 的基本步骤。它遵循 :ref:`installation <setup/install>`，通常不适用于不暴露在互联网上的开发系统。

.. warning:: 如果您正在设置一个公共服务器，请务必查看我们的 :ref:`security` 建议！


.. _db_filter:

数据库过滤器
==============

Odoo 是一个多租户系统：一个 Odoo 系统可以运行和服务多个数据库实例。它也高度可定制，定制化（从加载的模块开始）取决于“当前数据库”。

在作为登录公司用户使用后台（Web 客户端）时，这不是问题：可以在登录时选择数据库，然后加载定制化。

然而，对于非登录用户（门户网站，网站）来说，这是一个问题，因为他们不绑定到数据库：Odoo 需要知道应该使用哪个数据库来加载网页或执行操作。如果不使用多租户，这不是问题，因为只有一个数据库可用，但如果有多个数据库可访问，Odoo 需要一条规则来知道它应该使用哪个。

这就是 :option:`--db-filter <odoo-bin --db-filter>` 的目的之一：它指定了如何根据被请求的主机名（域）选择数据库。该值是一个 `正则表达式`_，可能包括动态注入的主机名（``%h``）或系统访问的第一个子域名（``%d``）。

对于在生产环境中托管多个数据库的服务器，尤其是使用``website``时，必须设置 dbfilter，否则许多功能将无法正常工作。

配置示例
--------

* 仅显示名称以“mycompany”开头的数据库

在 ``/etc/odoo.conf`` 中设置：

.. code-block:: ini

  [options]
  dbfilter = ^mycompany.*$

* 仅显示与``www``后的第一个子域名匹配的数据库：例如，如果传入请求是发送到``www.mycompany.com``或``mycompany.co.uk``，则显示数据库“mycompany”，但不显示``www2.mycompany.com``或``helpdesk.mycompany.com``。

在 ``/etc/odoo.conf`` 中设置：

.. code-block:: ini

  [options]
  dbfilter = ^%d$

.. note::
  设置适当的 :option:`--db-filter <odoo-bin --db-filter>` 是确保部署安全的重要部分。
  一旦它正常工作并且每个主机名仅匹配一个数据库，强烈建议阻止访问数据库管理屏幕，并使用``--no-database-list``启动参数来防止列出您的数据库，并阻止访问数据库管理屏幕。参见 security_。

PostgreSQL
==========

默认情况下，PostgreSQL 仅允许通过 UNIX 套接字和环回连接（来自“localhost”，即安装 PostgreSQL 服务器的同一台机器）进行连接。

如果您希望 Odoo 和 PostgreSQL 在同一台机器上执行，UNIX 套接字是可以的，并且在未提供主机时是默认设置，但如果您希望 Odoo 和 PostgreSQL 在不同的机器上执行 [#different-machines]_，它将需要 `监听网络接口`_ [#remote-socket]_，要么：

* 仅接受环回连接并 `使用 SSH 隧道`_ 在运行 Odoo 的机器和运行 PostgreSQL 的机器之间，然后配置 Odoo 连接到隧道的终端
* 接受对安装了 Odoo 的机器的连接，可能通过 ssl（有关详细信息，请参阅 `PostgreSQL 连接设置`_），然后配置 Odoo 通过网络连接

配置示例
--------

* 允许在本地主机上进行 tcp 连接
* 允许从 192.168.1.x 网络进行 tcp 连接

在 ``/etc/postgresql/9.5/main/pg_hba.conf`` 中设置：

.. code-block:: text

  # IPv4 本地连接：
  host    all             all             127.0.0.1/32            md5
  host    all             all             192.168.1.0/24          md5

在 ``/etc/postgresql/9.5/main/postgresql.conf`` 中设置：

.. code-block:: text

  listen_addresses = 'localhost,192.168.1.2'
  port = 5432
  max_connections = 80

.. _setup/deploy/odoo:

配置 Odoo
--------

开箱即用，Odoo 通过端口 5432 连接到本地 postgres。这可以在 Postgres 部署不是本地且/或不使用安装默认值时使用 :ref:`数据库选项 <reference/cmdline/server/database>` 覆盖。

:ref:`打包的安装程序 <setup/install/packaged>` 将自动创建一个新用户（``odoo``）并将其设置为数据库用户。

* 数据库管理屏幕受``admin_passwd``设置保护。此设置只能使用配置文件设置，并且仅在执行数据库更改之前检查。应将其设置为随机生成的值，以确保第三方无法使用此界面。
* 所有数据库操作都使用 :ref:`数据库选项 <reference/cmdline/server/database>`，包括数据库管理屏幕。要使数据库管理屏幕工作，PostgreSQL 用户需要具有``createdb``权限。
* 用户可以随时删除他们拥有的数据库。要使数据库管理屏幕完全不可用，需要创建一个没有``createdb``权限的 PostgreSQL 用户，并且数据库必须由另一个 PostgreSQL 用户拥有。

  .. warning:: PostgreSQL 用户 *不能* 是超级用户

配置示例
~~~~~~~~~~~~~~~~~~~~

* 连接到 192.168.1.2 上的 PostgreSQL 服务器
* 端口 5432
* 使用 'odoo' 用户账户，
* 密码为 'pwd'
* 仅过滤名称以“mycompany”开头的数据库

在 ``/etc/odoo.conf`` 中设置：

.. code-block:: ini

  [options]
  admin_passwd = mysupersecretpassword
  db_host = 192.168.1.2
  db_port = 5432
  db_user = odoo
  db_password = pwd
  dbfilter = ^mycompany.*$

.. _postgresql_ssl_connect:

Odoo 和 PostgreSQL 之间的 SSL
-----------------------------

从 Odoo 11.0 开始，您可以在 Odoo 和 PostgreSQL 之间强制执行 ssl 连接。
在 Odoo 中，db_sslmode 控制连接的 ssl 安全性，值可以选择 'disable'，'allow'，'prefer'，'require'，'verify-ca' 或 'verify-full'

`PostgreSQL 文档 <https://www.postgresql.org/docs/current/static/libpq-ssl.html>`_

.. _builtin_server:

内置服务器
============

Odoo 包括内置的 HTTP 服务器，可以使用多线程或多进程。

对于生产环境，建议使用多进程服务器，因为它提高了稳定性，更好地利用计算资源，并且可以更好地监控和限制资源。

* 可以通过配置 :option:`非零数量的 worker 进程 <odoo-bin --workers>` 启用多进程，worker 的数量应基于机器中的核心数量（可能留出一些空间用于 cron 工作，具体取决于预测的 cron 工作量）
* 可以根据硬件配置配置 worker 限制，以避免资源耗尽

.. warning:: 多进程模式目前在 Windows 上不可用


Worker 数量计算
----------------

* 经验法则 : (#CPU * 2) + 1
* Cron worker 需要 CPU
* 1 worker ~= 6 个并发用户

内存大小计算
----------------

* 我们考虑 20% 的请求是重请求，而 80% 是较轻的请求
* 当所有计算字段设计良好，SQL 请求设计良好时，重 worker 预计消耗约 1GB 内存
* 在相同场景下，轻 worker 预计消耗约 150MB 内存

所需内存 = #worker * ( (light_worker_ratio * light_worker_ram_estimation) + (heavy_worker_ratio * heavy_worker_ram_estimation) )

实时聊天
--------

在多进程中，将自动启动一个专用的实时聊天 worker，并监听 :option:`长轮询端口 <odoo-bin --longpolling-port>`，但客户端不会连接到它。

相反，您必须有一个代理将 URL 以``/longpolling/``开头的请求重定向到长轮询端口。其他请求应代理到 :option:`正常的 HTTP 端口 <odoo-bin --http-port>`

要实现这一点，您需要在 Odoo 前部署一个反向代理，如 nginx 或 apache。这样做时，您需要将更多的 http 头转发给 Odoo，并在 Odoo 配置中启用代理模式，以便 Odoo 读取这些头。

配置示例
--------------------

* 具有 4 个 CPU，8 个线程的服务器
* 60 个并发用户

* 60 用户 / 6 = 10 <- 所需 worker 理论数量
* (4 * 2) + 1 = 9 <- 最大 worker 理论数量
* 我们将使用 8 个 worker + 1 个用于 cron。我们还将使用一个监控系统来测量 CPU 

负载，并检查是否在 7 和 7.5 之间。
* 内存 = 9 * ((0.8*150) + (0.2*1024)) ~= 3GB 内存用于 Odoo

在 ``/etc/odoo.conf`` 中：

.. code-block:: ini

  [options]
  limit_memory_hard = 1677721600
  limit_memory_soft = 629145600
  limit_request = 8192
  limit_time_cpu = 600
  limit_time_real = 1200
  max_cron_threads = 1
  workers = 8

.. _https_proxy:

HTTPS
=====

无论是通过网站/网络客户端还是 Web 服务访问，Odoo 都会以明文传输认证信息。这意味着 Odoo 的安全部署必须使用 HTTPS\ [#switching]_。SSL 终止可以通过几乎任何 SSL 终止代理实现，但需要以下设置：

* 启用 Odoo 的 :option:`代理模式 <odoo-bin --proxy-mode>`。这应仅在 Odoo 位于反向代理之后时启用
* 设置 SSL 终止代理 (`Nginx 终止示例`_)
* 设置代理 (`Nginx 代理示例`_)
* 您的 SSL 终止代理还应自动将非安全连接重定向到安全端口

.. warning::

  如果您将 POS 模块与 `POSBox`_ 一起使用，您必须禁用 HTTPS 配置以避免``/pos/web``路径上的混合内容错误。

配置示例
--------------------

* 将 http 请求重定向到 https
* 代理请求到 odoo

在 ``/etc/odoo.conf`` 中设置：

.. code-block:: ini

  proxy_mode = True

在 ``/etc/nginx/sites-enabled/odoo.conf`` 中设置：

.. code-block:: nginx

  #odoo 服务器
  upstream odoo {
   server 127.0.0.1:8069;
  }
  upstream odoochat {
   server 127.0.0.1:8072;
  }

  # http -> https
  server {
     listen 80;
     server_name odoo.mycompany.com;
     rewrite ^(.*) https://$host$1 permanent;
  }

  server {
   listen 443;
   server_name odoo.mycompany.com;
   proxy_read_timeout 720s;
   proxy_connect_timeout 720s;
   proxy_send_timeout 720s;

   # 为 odoo 代理模式添加头
   proxy_set_header X-Forwarded-Host $host;
   proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
   proxy_set_header X-Forwarded-Proto $scheme;
   proxy_set_header X-Real-IP $remote_addr;

   # SSL 参数
   ssl on;
   ssl_certificate /etc/ssl/nginx/server.crt;
   ssl_certificate_key /etc/ssl/nginx/server.key;
   ssl_session_timeout 30m;
   ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
   ssl_ciphers 'ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES256-GCM-SHA384:DHE-RSA-AES128-GCM-SHA256:DHE-DSS-AES128-GCM-SHA256:kEDH+AESGCM:ECDHE-RSA-AES128-SHA256:ECDHE-ECDSA-AES128-SHA256:ECDHE-RSA-AES128-SHA:ECDHE-ECDSA-AES128-SHA:ECDHE-RSA-AES256-SHA384:ECDHE-ECDSA-AES256-SHA384:ECDHE-RSA-AES256-SHA:ECDHE-ECDSA-AES256-SHA:DHE-RSA-AES128-SHA256:DHE-RSA-AES128-SHA:DHE-DSS-AES128-SHA256:DHE-RSA-AES256-SHA256:DHE-DSS-AES256-SHA:DHE-RSA-AES256-SHA:AES128-GCM-SHA256:AES256-GCM-SHA384:AES128-SHA256:AES256-SHA256:AES128-SHA:AES256-SHA:AES:CAMELLIA:DES-CBC3-SHA:!aNULL:!eNULL:!EXPORT:!DES:!RC4:!MD5:!PSK:!aECDH:!EDH-DSS-DES-CBC3-SHA:!EDH-RSA-DES-CBC3-SHA:!KRB5-DES-CBC3-SHA';
   ssl_prefer_server_ciphers on;

   # 日志
   access_log /var/log/nginx/odoo.access.log;
   error_log /var/log/nginx/odoo.error.log;

   # 将长轮询请求重定向到 odoo 长轮询端口
   location /longpolling {
   proxy_pass http://odoochat;
   }

   # 将请求重定向到 odoo 后端服务器
   location / {
     proxy_redirect off;
     proxy_pass http://odoo;
   }

   # 通用 gzip
   gzip_types text/css text/scss text/plain text/xml application/xml application/json application/javascript;
   gzip on;
  }

Odoo 作为 WSGI 应用
==================

Odoo 也可以作为标准 WSGI_ 应用挂载。Odoo 提供了一个名为``odoo-wsgi.example.py``的 WSGI 启动脚本的基础。该脚本应定制（可能在从设置目录复制后）以直接在 :mod:`odoo.tools.config` 中正确设置配置，而不是通过命令行或配置文件。

然而，WSGI 服务器只会暴露用于 Web 客户端、网站和 Web 服务 API 的主要 HTTP 端点。因为 Odoo 不再控制 worker 的创建，所以不能设置 cron 或 livechat worker。

Cron Workers
------------

要运行作为 WSGI 应用的 Odoo 部署的 cron 作业，需要

* 一个经典的 Odoo（通过``odoo-bin``运行）
* 连接到必须运行 cron 作业的数据库（通过 :option:`odoo-bin -d`）
* 不应暴露于网络。为了确保 cron 运行器不可网络访问，可以完全禁用内置的 HTTP 服务器，使用 :option:`odoo-bin --no-http` 或在配置文件中设置``http_enable = False``。

实时聊天
--------

对于 WSGI 部署，第二个有问题的子系统是实时聊天：大多数 HTTP 连接相对较短，并且很快释放其 worker 进程以供下一个请求使用，而实时聊天需要每个客户端的长连接以实现近实时通知。

这与基于进程的 worker 模型冲突，因为它将占用 worker 进程，并防止新用户访问系统。然而，这些长连接做的事情很少，主要是等待通知。

支持 WSGI 应用中实时聊天/通知的解决方案是：

* 部署一个线程版的 Odoo（而不是基于进程的预分叉版），并将仅以``/longpolling/``开头的请求重定向到该 Odoo，这是最简单的方法，长轮询 URL 可以作为 cron 实例。
* 通过``odoo-gevent``部署一个事件驱动的 Odoo，并将以``/longpolling/``开头的请求代理到 :option:`长轮询端口 <odoo-bin --longpolling-port>`。

服务静态文件
============

为了开发的便利性，Odoo 直接在其模块中服务所有静态文件。这在性能方面可能不是最理想的，静态文件通常应该由静态 HTTP 服务器服务。

Odoo 静态文件位于每个模块的``static/``文件夹中，因此可以通过拦截所有请求 :samp:`/{MODULE}/static/{FILE}`，并在各个插件路径中查找正确的模块（和文件）来服务静态文件。

.. todo:: 测试是否有必要通过这种方式服务文件存储的附件，以及如何（例如是否可以将 ir.attachment id 映射到数据库中的文件存储哈希？）

.. _security:

安全性
======

首先，请记住，保护信息系统是一个持续的过程，而不是一次性操作。在任何时候，您的安全性都只和环境中最薄弱的环节一样强。

所以请不要将本节视为防止所有安全问题的最终措施清单。它仅旨在总结您应确保包含在安全行动计划中的一些重要事项。其余部分将来自您的操作系统和发行版的最佳安全实践、用户、密码和访问控制管理方面的最佳实践等。

在部署面向互联网的服务器时，请务必考虑以下与安全相关的主题：

- 始终设置一个强大的超级管理员密码，并在系统设置好后限制对数据库管理页面的访问。参见 :ref:`db_manager_security`。

- 为所有数据库上的所有管理员帐户选择唯一的登录名和强密码。不要使用“admin”作为登录名。不要将这些登录名用于日常操作，仅用于控制/管理安装。
  *永远不要* 使用任何默认密码，如 admin/admin，即使是测试/开发数据库。

- **不要** 在面向互联网的服务器上

安装演示数据。包含演示数据的数据库包含可用于进入您的系统并造成重大问题的默认登录名和密码，即使在测试/开发系统上也是如此。

- 使用适当的数据库过滤器（ :option:`--db-filter <odoo-bin --db-filter>`）根据主机名限制数据库的可见性。参见 :ref:`db_filter`。
  您也可以使用 :option:`-d <odoo-bin -d>` 提供自己的（逗号分隔的）可用数据库列表，而不是让系统从数据库后端获取所有数据库。

- 一旦配置好``db_name``和``db_filter``并且每个主机名仅匹配一个数据库，您应该将``list_db``配置选项设置为``False``，以完全防止列出数据库，并阻止访问数据库管理屏幕（这也作为 :option:`--no-database-list <odoo-bin --no-database-list>` 命令行选项暴露）

- 确保 PostgreSQL 用户（:option:`--db_user <odoo-bin --db_user>`) *不是* 超级用户，并且您的数据库由不同的用户拥有。例如，如果您使用专用的非特权``db_user``，它们可以由``postgres``超级用户拥有。参见 :ref:`setup/deploy/odoo`。

- 通过定期安装最新版本来保持安装更新，可以通过 GitHub 或从 https://www.odoo.com/page/download 或 http://nightly.odoo.com 下载最新版本

- 使用适当的限制将您的服务器配置为多进程模式，以匹配您的典型使用情况（内存/CPU/超时）。参见 :ref:`builtin_server`。

- 在提供有效 SSL 证书的 Web 服务器后面运行 Odoo，以防止窃听明文通信。SSL 证书很便宜，许多免费选项也存在。
  配置 Web 代理以限制请求的大小，设置适当的超时，然后启用 :option:`代理模式 <odoo-bin --proxy-mode>` 选项。参见 :ref:`https_proxy`。

- 如果您需要允许远程 SSH 访问服务器，请确保为**所有**帐户设置强密码，而不仅仅是 `root`。强烈建议完全禁用基于密码的身份验证，仅允许公钥身份验证。还可以考虑通过 VPN 限制访问，仅允许防火墙中的受信任 IP，并/或运行如 `fail2ban` 或等效的暴力破解检测系统。

- 考虑在您的代理或防火墙上安装适当的速率限制，以防止暴力破解攻击和拒绝服务攻击。参见 :ref:`login_brute_force` 以获取具体措施。

  许多网络提供商提供分布式拒绝服务攻击（DDOS）的自动缓解措施，但这通常是可选服务，因此您应该咨询他们。

- 只要可能，将面向公众的演示/测试/开发实例托管在与生产实例不同的机器上。并采取与生产相同的安全预防措施。

- 如果您托管多个客户，请使用容器或适当的“监狱”技术将客户数据和文件彼此隔离。

- 设置数据库和文件存储数据的每日备份，并将其复制到无法从服务器本身访问的远程存档服务器。

.. _login_brute_force:

阻止暴力破解攻击
-------------------
对于面向互联网的部署，用户密码的暴力破解攻击非常常见，对于 Odoo 服务器，这种威胁不容忽视。Odoo 每次进行登录尝试时都会发出一个日志条目，并报告结果：成功或失败，以及目标登录名和来源 IP。

日志条目将具有以下形式。

登录失败::

      2018-07-05 14:56:31,506 24849 INFO db_name odoo.addons.base.res.res_users: Login failed for db:db_name login:admin from 127.0.0.1

登录成功::

      2018-07-05 14:56:31,506 24849 INFO db_name odoo.addons.base.res.res_users: Login successful for db:db_name login:admin from 127.0.0.1

这些日志可以很容易地被入侵防御系统如 `fail2ban` 分析。

例如，以下 fail2ban 过滤器定义应该匹配一个
登录失败::

    [Definition]
    failregex = ^ \d+ INFO \S+ \S+ Login failed for db:\S+ login:\S+ from <HOST>
    ignoreregex =

这可以与监狱定义一起使用，以阻止 HTTP(S) 上的攻击 IP。

以下是检测到同一 IP 在 1 分钟内有 10 次登录失败时阻止 IP 15 分钟的示例::

    [odoo-login]
    enabled = true
    port = http,https
    bantime = 900  ; 15 分钟禁令
    maxretry = 10  ; 如果 10 次尝试
    findtime = 60  ; 在 1 分钟内 /!\ 应该根据时区偏移进行调整
    logpath = /var/log/odoo.log  ; 设置实际的 odoo 日志路径


.. _db_manager_security:

数据库管理器安全性
---------------------

:ref:`setup/deploy/odoo` 提到了``admin_passwd``。

此设置用于所有数据库管理屏幕（创建、删除、转储或还原数据库）。

如果管理屏幕根本不应该访问，您应该将``list_db``配置选项设置为``False``，以阻止所有数据库选择和管理屏幕的访问。

.. warning::

  强烈建议禁用任何面向互联网的系统的数据库管理器！它旨在作为一个开发/演示工具，以便快速轻松地创建和管理数据库。它不是为生产使用设计的，可能甚至向攻击者暴露危险功能。它也不是为处理大型数据库而设计的，可能会触发内存限制。

  在生产系统中，数据库管理操作应始终由系统管理员执行，包括新数据库的配置和自动备份。

确保设置适当的``db_name``参数（和可选的``db_filter``）以便系统可以为每个请求确定目标数据库，否则用户将被阻止，因为他们将不被允许选择数据库。

如果管理屏幕只能从选定的一组机器访问，请使用代理服务器的功能阻止对所有以``/web/database``开头的路由的访问，除了（也许）``/web/database/selector``显示数据库选择屏幕。

如果数据库管理屏幕应保持可访问状态，则
``admin_passwd``设置必须从其``admin``默认值更改：在允许数据库更改操作之前会检查此密码。

它应该被安全存储，并且应该随机生成，例如。

.. code-block:: console

    $ python3 -c 'import base64, os; print(base64.b64encode(os.urandom(24)))'

这将生成一个 32 个字符的伪随机可打印字符串。

支持的浏览器
==============

Odoo 支持市场上所有主要的桌面和移动浏览器，只要它们受到其发布者的支持。

支持的浏览器有：

- Google Chrome
- Mozilla Firefox
- Microsoft Edge
- Apple Safari

.. warning:: 在提交错误报告之前，请确保您的浏览器是最新的，并且仍然受到其发布者的支持。



.. [#different-machines]
    让多个 Odoo 安装使用同一个 PostgreSQL 数据库，或者为两种软件提供更多计算资源。
.. [#remote-socket]
    从技术上讲，像 socat_ 这样的工具可以用来代理 UNIX 套接字跨网络，但这主要是针对只能通过 UNIX 套接字使用的软件。
.. [#switching]
    或者仅通过内部分组交换网络访问，但这需要安全的交换机，防止 `ARP 欺骗`_，并且排除使用 WiFi。即使在安全的分组交换网络上，也推荐通过 HTTPS 部署，并且由于“自签名”证书在受控环境中比在互联网上更容易部署，因此可能会降低成本。

.. _regular expression: https://docs.python.org/3/library/re.html
.. _ARP 欺骗: https://en.wikipedia.org/wiki/ARP_spoofing
.. _Nginx 终止示例:
    https://nginx.com/resources/admin-guide/nginx-ssl-termination/
.. _Nginx 代理示例:
    https://nginx.com/resources/admin-guide/reverse-proxy/
.. _socat: http://www.dest-unreach.org/socat/
.. _PostgreSQL 连接设置:
.. _监听网络接口:
    https://www.postgresql.org/docs/9.6/static/runtime-config-connection.html
.. _使用 SSH 隧道:
    https://www.postgresql.org/docs/9.6/static/ssh-tunnels.html
.. _WSGI: https://wsgi.readthedocs.org/
.. _POSBox: https://www.odoo.com/page/point-of-sale-hardware#part_2
