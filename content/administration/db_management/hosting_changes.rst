.. _db_management/hosting_changes:

===============
Hosting Changes
===============

你可能需要将你的 Odoo 数据库从一个托管解决方案迁移到另一个托管解决方案。根据不同的平台，你需要自行操作或首先联系我们的支持团队。

从本地部署到 Odoo Online
=======================

.. warning:: Odoo Online 不兼容 **非标准应用**。

1. 创建数据库的 :ref:`副本 <duplicate_premise>`：在这个副本中，卸载所有 **非标准应用**。
2. 使用数据库管理器获取带文件存储的数据库 "dump"。
3. **如果你有时间限制，请提前联系安排迁移。**
4. `创建支持票 <https://www.odoo.com/help>`_ 并附上数据库 dump（如果文件太大，使用任何文件传输服务并将链接附加到你的票中）。同时包括你的订阅号和你想使用的数据库 URL（例如：my-company.odoo.com）。
5. 我们会确保你的数据库兼容并将其上传到我们的云中。如果出现技术问题，我们会联系你。
6. 完成！

从本地部署到 Odoo.sh
===================

1. 按照 :ref:`Odoo.sh 文档中的导入数据库部分 <odoo_sh_import_your_database>` 进行操作。
2. ...就这么简单！

从 Odoo Online 到本地部署
========================

1. 登录 `你的 Odoo Online 用户门户 <https://accounts.odoo.com/my/databases/manage>`_，查看你的数据库版本号。
2. 如果你的数据库不是运行 Odoo 的 :ref:`主要版本 <supported_versions>`，则不能在本地托管，需要先升级到新的主要版本。(*例如：如果你的数据库运行 Odoo 12.3，而这不是主要版本，你需要先将其升级到 Odoo 13.0 或 14.0。*)
3. 点击数据库名称旁边的“齿轮”图标，然后选择 :menuselection:`下载` 以下载数据库备份（如果下载失败，因为备份文件太大，请联系 `我们的支持 <https://www.odoo.com/help>`_）。
4. 在本地服务器上的数据库管理器中恢复它。

从 Odoo Online 到 Odoo.sh
=======================

1. 登录 `你的 Odoo Online 用户门户 <https://accounts.odoo.com/my/databases/manage>`_，查看你的数据库版本号。
2. 如果你的数据库不是运行 Odoo 的 :ref:`主要版本 <supported_versions>`，则不能在 Odoo.sh 上托管，需要先升级到新的主要版本。(*例如：如果你的数据库运行 Odoo 12.3，而这不是主要版本，你需要先将其升级到 Odoo 13.0 或 14.0。*)
3. 点击数据库名称旁边的“齿轮”图标，然后选择 :menuselection:`下载` 以下载数据库备份（如果下载失败，因为备份文件太大，请联系 `我们的支持 <https://www.odoo.com/help>`_）。
4. 按照 :ref:`Odoo.sh 文档中的导入数据库部分 <odoo_sh_import_your_database>` 进行操作。

从 Odoo.sh 到 Odoo Online
========================

.. warning:: Odoo Online 不兼容 **非标准应用**。

1. 卸载所有 **非标准应用**：先在测试环境中进行，然后在生产环境中进行。
2. **如果你有时间限制，请提前联系安排迁移。**
3. `创建支持票 <https://www.odoo.com/help>`_ 并附上数据库 dump（如果文件太大，使用任何文件传输服务并将链接附加到你的票中）。同时包括你的订阅号和你想使用的数据库 URL（例如：my-company.odoo.com）。
4. 我们会确保你的数据库兼容并将其上传到我们的云中。如果出现技术问题，我们会联系你。
5. 完成！

从 Odoo.sh 到本地部署
====================

1. 获取 :ref:`Odoo.sh 生产数据库的备份 <odoo_sh_branches_backups>`。
2. 在本地服务器上的数据库管理器中恢复它。
