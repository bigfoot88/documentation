==================================
容器
==================================

概述
========

每个构建都在其自己的容器中隔离（Linux 命名空间容器）。

基础系统是 Ubuntu，其中安装了 Odoo 所需的所有依赖项以及常用的实用程序包。

如果您的项目需要额外的 Python 依赖项或更近期的版本，可以在分支根目录中定义一个 :file:`requirements.txt` 文件来列出它们。平台将负责在您的容器中安装这些依赖项。 `pip 依赖项说明 <https://pip.pypa.io/en/stable/reference/pip_install/#requirement-specifiers>`_ 文档可以帮助您编写 :file:`requirements.txt` 文件。要获得一个具体的例子，请查看 `Odoo 的 requirements.txt 文件 <https://github.com/odoo/odoo/blob/12.0/requirements.txt>`_。

子模块的 :file:`requirements.txt` 文件也会被考虑在内。平台会在每个包含 Odoo 模块的文件夹中查找 :file:`requirements.txt` 文件：而不是在模块文件夹本身，而是在其父文件夹中。

目录结构
===================

由于容器基于 Ubuntu，其目录结构遵循 Linux 文件系统层次标准。 `Ubuntu 的文件系统树概述 <https://help.ubuntu.com/community/LinuxFilesystemTreeOverview#Main_directories>`_ 解释了主要目录。

以下是 Odoo.sh 的相关目录：

::

  .
  ├── home
  │    └── odoo
  │         ├── src
  │         │    ├── odoo                Odoo Community 源代码
  │         │    │    └── odoo-bin       Odoo 服务器可执行文件
  │         │    ├── enterprise          Odoo Enterprise 源代码
  │         │    ├── themes              Odoo Themes 源代码
  │         │    └── user                您的存储库分支源代码
  │         ├── data
  │         │    ├── filestore           数据库附件以及二进制字段的文件
  │         │    └── sessions            访客和用户会话
  │         └── logs
  │              ├── install.log         数据库安装日志
  │              ├── odoo.log            运行服务器日志
  │              ├── update.log          数据库更新日志
  │              └── pip.log             Python 包安装日志
  └── usr
       ├── lib
       │    ├── python2.7
       │         └── dist-packages       Python 2.7 标准库
       │    ├── python3
       │         └── dist-packages       Python 3 标准库
       │    └── python3.5
       │         └── dist-packages       Python 3.5 标准库
       ├── local
       │    └── lib
       │         ├── python2.7
       │         │    └── dist-packages  Python 2.7 第三方库
       │         └── python3.5
       │              └── dist-packages  Python 3.5 第三方库
       └── usr
            └── bin
                 ├── python2.7           Python 2.7 可执行文件
                 └── python3.5           Python 3.5 可执行文件

容器中安装了 Python 2.7 和 3.5。然而：

* 如果您的项目配置为使用 Odoo 10.0，Odoo 服务器运行的是 Python 2.7。
* 如果您的项目配置为使用 Odoo 11.0 或更高版本，Odoo 服务器运行的是 Python 3.5。

数据库 shell
==============

在使用 shell 访问容器时，您可以使用 *psql* 访问数据库。

.. code-block:: bash

  odoo@odoo-addons-master-1.odoo.sh:~$ psql
  psql (9.5.2, server 9.5.11)
  SSL 连接 (协议: TLSv1.2, 密码: ECDHE-RSA-AES256-GCM-SHA384, 位数: 256, 压缩: 关闭)
  输入 "help" 获取帮助。

  odoo-addons-master-1=>

**请注意！**
`使用事务 <https://www.postgresql.org/docs/current/static/sql-begin.html>`_ (*BEGIN...COMMIT/ROLLBACK*) 进行所有导致更改的 *sql* 语句
(*UPDATE*, *DELETE*, *ALTER*, ...)，尤其是针对生产数据库。

事务机制是您在出现错误时的安全保障。
只需回滚更改，即可将数据库恢复到之前的状态。

例如，可能会发生忘记设置 *WHERE* 条件的情况。

.. code-block:: sql

  odoo-addons-master-1=> BEGIN;
  BEGIN
  odoo-addons-master-1=> UPDATE res_users SET password = '***';
  更新 457
  odoo-addons-master-1=> ROLLBACK;
  回滚

在这种情况下，您可以回滚以恢复您刚才意外进行的不必要更改，然后重写语句：

.. code-block:: sql

  odoo-addons-master-1=> BEGIN;
  BEGIN
  odoo-addons-master-1=> UPDATE res_users SET password = '***' WHERE id = 1;
  更新 1
  odoo-addons-master-1=> COMMIT;
  提交

但是，请不要忘记在完成后提交或回滚您的事务。
打开的事务可能会锁定表中的记录，并且运行中的数据库可能会等待它们被释放。 这可能导致服务器无限期挂起。

此外，在可能的情况下，请首先使用您的暂存数据库测试您的语句。 这为您提供了额外的安全保障。

运行 Odoo 服务器
==================

您可以从容器 shell 启动 Odoo 服务器实例。 您将无法从外部世界通过浏览器访问它，但您可以例如：

* 使用 Odoo shell,

.. code-block:: bash

  $  odoo-bin shell
  >>> partner = env['res.partner'].search([('email', '=', 'asusteK@yourcompany.example.com')], limit=1)
  >>> partner.name
  'ASUSTeK'
  >>> partner.name = 'Odoo'
  >>> env['res.partner'].search([('email', '=', 'asusteK@yourcompany.example.com')], limit=1).name
  'Odoo'

* 安装模块,

.. code-block:: bash

  $  odoo-bin -i sale --without-demo=all --stop-after-init

* 更新模块,

.. code-block:: bash

  $  odoo-bin -u sale --stop-after-init

* 运行模块测试,

.. code-block:: bash

  $  odoo-bin -i sale --test-enable --log-level=test --stop-after-init

在上述命令中，参数：

* ``--without-demo=all`` 防止加载所有模块的演示数据
* ``--stop-after-init`` 将在完成您要求的操作后立即关闭服务器实例。

更多选项可以在 :doc:`CLI 文档 </developer/reference/cmdline>` 中找到详细说明。

您可以在日志 (*~/logs/odoo.log*) 中找到 Odoo.sh 用于运行服务器的 addons 路径。
查找 "*odoo: addons paths*":

::

  2018-02-19 10:51:39,267 4 INFO ? odoo: Odoo 版本 12.0
  2018-02-19 10:51:39,268 4 INFO ? odoo: 使用配置文件位于 /home/odoo/.config/odoo/odoo.conf
  2018-02-19 10:51:39,268 4 INFO ? odoo: addons 路径: ['/home/odoo/data/addons/12.0', '/home/odoo/src/user', '/home/odoo/src/enterprise', '/home/odoo/src/themes', '/home/odoo/src/odoo/addons', '/home/odoo/src/odoo/odoo/addons']

**请注意**，尤其是对于您的生产数据库。
运行此 Odoo 服务器实例执行的操作不会被隔离：
更改将在数据库中生效。始终在您的暂存数据库中进行测试。

在 Odoo.sh 中调试
====================

调试 Odoo.sh 构建与调试其他 Python 应用并无太大不同。本文仅解释 Odoo.sh 平台的特殊性和限制，假设您已经知道如何使用调试器。

.. note:: 如果您还不知道如何调试 Python 应用程序，可以在互联网上轻松找到多种入门课程。

您可以使用 ``pdb``、``pudb`` 或 ``ipdb`` 在 Odoo.sh 上调试代码。
由于服务器是在 shell 外运行的，您无法直接从 Odoo 实例后端启动调试器，因为调试器需要 shell 才能运行。

- 每个容器中默认安装了 `pdb <https://docs.python.org/3/library/pdb.html>`_。

- 如果您想使用
