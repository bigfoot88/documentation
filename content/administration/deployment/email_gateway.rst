==================
Odoo 邮件网关
==================

Odoo 邮件网关允许您将所有收到的邮件直接注入 Odoo。

其原理很简单：您的 SMTP 服务器为每封新收到的电子邮件执行 "mailgate" 脚本。

该脚本负责通过 XML-RPC 连接到您的 Odoo 数据库，并通过 `MailThread.message_process()` 功能发送电子邮件。

先决条件
-------------
- Odoo 数据库的管理员访问权限。
- 您自己的邮件服务器，例如 Postfix 或 Exim。
- 配置邮件服务器的技术知识。

对于 Postfix
-----------
在您的别名配置文件（:file:`/etc/aliases`）中：

.. code-block:: text

   email@address: "|/odoo-directory/addons/mail/static/scripts/odoo-mailgate.py -d <database-name> -u <userid> -p <password>"

.. note::
   资源

   - `Postfix <http://www.postfix.org/documentation.html>`_
   - `Postfix 别名 <http://www.postfix.org/aliases.5.html>`_
   - `Postfix 虚拟 <http://www.postfix.org/virtual.8.html>`_


对于 Exim
--------
.. code-block:: text

   *: |/odoo-directory/addons/mail/static/scripts/odoo-mailgate.py -d <database-name> -u <userid> -p <password>

.. note::
   资源

   - `Exim <https://www.exim.org/docs.html>`_

.. tip::
   如果您没有访问/管理您的邮件服务器，请使用 :ref:`入站消息
   <discuss/email_servers/inbound_messages>`。
