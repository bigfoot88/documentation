.. _setup/enterprise:

============================
从社区版到企业版
============================

根据您当前的安装情况，有多种方法可以升级您的社区版。
无论哪种情况，基本指南如下：

* 备份您的社区数据库

  .. image:: enterprise/db_manager.png
    :class: img-fluid

* 关闭您的服务器

* 安装 web_enterprise 模块

* 重启服务器

* 输入您的 Odoo Enterprise 订阅代码

.. image:: enterprise/enterprise_code.png
  :class: img-fluid

在 Linux 上使用安装程序
============================

* 备份您的社区数据库

* 停止 odoo 服务

  .. code-block:: console

    $ sudo service odoo stop

* 安装企业版 .deb（它应该覆盖社区版包）

  .. code-block:: console

    $ sudo dpkg -i <path_to_enterprise_deb>
  
* 使用以下命令更新您的数据库到企业版包

  .. code-block:: console

    $ python3 /usr/bin/odoo-bin -d <database_name> -i web_enterprise --stop-after-init

* 您应该能够使用您常用的身份验证方式连接到您的 Odoo 企业版实例。
  然后，您可以通过在表单输入中输入通过电子邮件收到的代码，将数据库与您的 Odoo Enterprise 订阅链接。

在 Linux 上使用源代码
===============================

使用源代码启动服务器的方法有很多，您可能有自己的偏好。您可能需要根据自己的工作流程适当调整部分内容。

* 关闭服务器
* 备份您的社区数据库
* 更新启动命令的 ``--addons-path`` 参数（参见 :ref:`setup/install/source`）
* 使用以下命令安装 web_enterprise 模块

  .. code-block:: console

    $ -d <database_name> -i web_enterprise --stop-after-init

  根据数据库的大小，这可能需要一些时间。

* 使用第 3 步更新的 addons 路径重启服务器。
  您应该能够连接到您的实例。然后，您可以通过在表单输入中输入通过电子邮件收到的代码，将数据库与您的 Odoo Enterprise 订阅链接。

在 Windows 上
==========

* 备份您的社区数据库

* 卸载 Odoo Community（使用安装文件夹中的卸载可执行文件）- PostgreSQL 将保持安装状态

  .. image:: enterprise/windows_uninstall.png
    :class: img-fluid

* 启动 Odoo Enterprise 安装程序并按正常步骤操作。选择安装路径时，您可以设置社区版安装的文件夹
  （此文件夹仍包含 PostgreSQL 安装）。
  在安装结束时取消选中 ``Start Odoo``

  .. image:: enterprise/windows_setup.png
    :class: img-fluid

* 使用命令窗口，使用以下命令更新您的 Odoo 数据库（从 Odoo 安装路径的 server 子文件夹）

  .. code-block:: console

    $ odoo.exe -d <database_name> -i web_enterprise --stop-after-init

* 无需手动启动服务器，服务正在运行。
  您应该能够使用您常用的身份验证方式连接到您的 Odoo 企业版实例。然后，您可以通过在表单输入中输入通过电子邮件收到的代码，将数据库与您的 Odoo Enterprise 订阅链接。
