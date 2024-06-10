.. _odoosh-gettingstarted-create:

==================================
创建您的项目
==================================

部署您的平台
====================

前往 `Odoo.sh <https://www.odoo.sh/>`_ 并点击 *部署您的平台* 按钮。

.. image:: ./media/deploy.png
   :align: center

使用 Github 登录
===================

使用您的 Github 账户登录。如果您还没有账户，请点击 *创建账户* 链接。

.. image:: ./media/github-signin.png
   :align: center

授权 Odoo.sh
=================

通过点击 *授权* 按钮授予 Odoo.sh 必需的访问权限。

.. image:: ./media/github-authorize.png
   :align: center

Odoo.sh 基本上需要以下权限：

* 知道您的 Github 登录和邮箱，
* 在您决定从头开始时创建一个新仓库，
* 读取您现有的仓库，包括您的组织仓库，以便您选择从现有仓库开始，
* 创建一个 webhook，每次您推送更改时收到通知，
* 提交更改以便于您的部署，例如合并分支或添加新的 `子模块 <https://git-scm.com/book/en/v2/Git-Tools-Submodules>`_。

提交您的项目
===================

选择您是要从头开始创建一个新仓库，还是要使用现有仓库。

然后，选择一个名称或选择您要使用的仓库。

选择您想要使用的 Odoo 版本。如果您计划导入现有数据库或现有应用程序集，则可能需要选择相应的版本。如果您从头开始，使用最新版本。

输入您的 *订阅码*。这也被称为 *订阅推荐码*、*合同号* 或 *激活码*。

它应该是包含 Odoo.sh 的您的企业订阅码。

合作伙伴可以使用他们的合作代码开始试用。如果他们的客户开始一个项目，他们应该获得包含 Odoo.sh 的企业订阅并使用其订阅码。合作伙伴将获得全额回扣佣金。
请联系您的销售代表或客户经理以获取订阅码。

提交表单时，如果您被通知您的订阅无效，这意味着：

* 它不是现有订阅，
* 它不是合作订阅，
* 它是企业订阅，但不包含 Odoo.sh，
* 它既不是合作订阅也不是企业订阅（例如，在线订阅）。

如果对您的订阅有疑问，请联系 `Odoo 支持 <https://www.odoo.com/help>`_。

.. image:: ./media/deploy-form.png
   :align: center

完成！
=============

您可以开始使用 Odoo.sh。您的第一个构建即将创建。您很快就能连接到您的第一个数据库。

.. image:: ./media/deploy-done.png
   :align: center

.. _odoo_sh_import_your_database:

导入您的数据库
====================

只要您的数据库在 Odoo 的 :doc:`支持版本 </services/support/supported_versions>` 内，您就可以将其导入到您的 Odoo.sh 项目中。

将您的模块推送到生产环境
-------------------------------

如果您使用社区或自定义模块，请将它们添加到您的 Github 仓库的一个分支中。
托管在 Odoo.com 在线平台上的数据库没有任何自定义模块。
这些数据库的用户可以跳过此步骤。

您可以按需结构化您的模块，Odoo.sh 会自动检测包含 Odoo 插件的文件夹。
例如，您可以将所有模块文件夹放在仓库的根目录中，或者按您定义的类别（会计、项目等）将模块分组在文件夹中。

对于公共 Git 仓库中可用的社区模块，您也可以考虑使用 :ref:`子模块 <odoosh-advanced-submodules>` 添加它们。

然后，要么 :ref:`将此分支设为生产分支 <odoosh-gettingstarted-branches-stages>`，
要么 :ref:`将其合并到您的生产分支 <odoosh-gettingstarted-branches-mergingbranches>`。

下载备份
-----------------

本地数据库
~~~~~~~~~~~~~~~~~~~~

访问您的本地数据库的 URL :file:`/web/database/manager` 并下载备份。

.. Warning::

  如果您无法访问数据库管理器，可能是被系统管理员禁用的。
  请参阅 :ref:`数据库管理器安全文档 <db_manager_security>`。

您将需要数据库服务器的主密码。如果您没有，请联系系统管理员。

.. image:: ./media/create-import-onpremise-backup.png
   :align: center

选择包含文件存储的 zip 作为备份格式。

.. image:: ./media/create-import-onpremise-backup-dialog.png
  :align: center

Odoo 在线数据库
~~~~~~~~~~~~~~~~~~~~~

`访问您的数据库管理器 <https://accounts.odoo.com/my/databases/manage>`_ 并下载您的数据库备份。

.. image:: ./media/create-import-online-backup.png
  :align: center

.. Warning::

  Saas 版本（例如 *saas-**）在 Odoo.sh 上不支持。

上传备份
-----------------

然后，在您的 Odoo.sh 项目的生产分支的备份选项卡中，导入您刚下载的备份。

.. image:: ./media/create-import-production.png
   :align: center

备份导入后，您可以使用分支历史记录中的 *连接* 按钮访问数据库。

.. image:: ./media/create-import-production-done.png
  :align: center

检查您的外发邮件服务器
---------------------------------

Odoo.sh 提供了一个默认邮件服务器。
要使用它，您的数据库中在 :menuselection:`设置 --> 技术 --> 外发邮件服务器` 中（需启用开发者模式）不能配置启用的外发邮件服务器。

导入数据库后，
所有外发邮件服务器都被禁用，因此您使用的是 Odoo.sh 提供的默认邮件服务器。

.. Warning::

  端口 25 已关闭（且将保持关闭）。如果您想连接到外部 SMTP 服务器，您应该使用端口 465 和 587。

检查您的计划操作
----------------------------

导入后，所有计划操作都被禁用。

这是为了防止新导入的数据库执行可能影响正在运行的生产的操作，
例如发送队列中的剩余邮件、处理大量邮件或第三方服务同步
（日历、文件托管等）。

如果您计划将导入的数据库作为生产数据库，请启用您需要的计划操作。
您可以检查源数据库中启用了哪些操作，并在导入的数据库中启用相同的操作。
计划操作位于 :menuselection:`设置 --> 技术 --> 自动化 --> 计划操作` 下。

注册您的订阅
--------------------------

导入后您的订阅被取消关联。

导入的数据库默认被视为副本，因此企业订阅被删除，
因为每个订阅只能链接一个数据库。

如果您计划将其作为生产数据库，
取消关联前一个数据库并注册新导入的数据库。
阅读 :ref:`数据库注册文档 <db_premise>` 获取说明。
