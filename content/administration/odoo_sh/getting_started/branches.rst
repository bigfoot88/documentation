==================================
分支
==================================

概述
========

分支视图为您提供了存储库中不同分支的概述。

.. image:: ./media/interface-branches.png
   :align: center

.. _odoosh-gettingstarted-branches-stages:

阶段
===============

Odoo.sh 为您的分支提供了三个不同的阶段：生产、预生产和开发。

您可以通过拖放分支到阶段部分的标题来更改分支的阶段。

.. image:: ./media/interface-branches-stagechange.png
   :align: center

.. _stage_production:

生产阶段
----------
这是运行生产数据库代码的分支。每个项目只能有一个生产分支。

当您在此分支中推送新提交时，生产服务器将更新为新版本的代码并重新启动。

如果您的更改需要更新模块（例如表单视图的更改）并希望自动执行，请增加模块在其 manifest 文件 (*__manifest__.py*) 中的版本号。平台将自动执行更新，期间实例将暂时不可用进行维护。

此方法等同于通过应用菜单进行模块升级，或通过命令行（:code:`-u` 参数）执行升级。

如果提交的更改导致服务器无法重启，或模块更新失败，服务器将自动恢复到上一个成功的代码版本，数据库将回滚到更新前的状态。您仍然可以访问失败更新的日志，以便进行故障排除。

演示数据不会加载，因为它不适用于生产数据库。单元测试不会执行，因为它会增加生产数据库在更新期间的不可用时间。

使用试用项目的合作伙伴应注意，他们的生产分支以及所有预生产分支将在 30 天后自动设置回开发阶段。

预生产阶段
-------
预生产分支旨在使用生产数据测试新功能，而不会因测试记录而影响实际生产数据库。它们将创建生产数据库的中性副本。

中性化包括：

* 禁用计划操作。如果您想测试它们，可以手动触发它们的操作或重新启用它们。请注意，如果没有人在使用数据库，平台将减少触发它们的频率以节省资源。
* 通过邮件捕获器拦截外发电子邮件。提供了一个界面来查看您的数据库发送的电子邮件（:ref:`interface to view <odoosh-gettingstarted-branches-tabs-mails>`）。这样，您就不用担心将测试电子邮件发送给您的联系人。
* 将支付获取器和运输提供商设置为测试模式。
* 禁用 IAP 服务。

最新的数据库将无限期保持活动状态，同一分支的较旧数据库可能会被垃圾回收以腾出空间。它的有效期为 3 个月，之后您需要重新构建该分支。如果您在这些数据库中进行配置或视图更改，请确保记录它们或直接在分支的模块中编写它们，使用 XML 数据文件覆盖默认配置或视图。

单元测试不会执行，因为在 Odoo 中，它们目前依赖于演示数据，而这些数据不会加载到生产数据库中。未来，如果 Odoo 支持在没有演示数据的情况下运行单元测试，Odoo.sh 将考虑在预生产数据库上运行测试。

开发阶段
-----------
开发分支使用演示数据创建新数据库以运行单元测试。安装的模块是您分支中包含的模块。您可以在项目设置（:ref:`project Settings <odoosh-gettingstarted-settings-modules-installation>`）中更改要安装的模块列表。

当您在这些分支之一中推送新提交时，将启动一个新服务器，创建一个从头开始的新数据库并加载分支的新版本。演示数据将被加载，默认情况下将执行单元测试。这验证了您的更改不会破坏它们测试的任何功能。如果愿意，您可以禁用测试或允许在分支设置（:ref:`branch's settings <odoosh-gettingstarted-branches-tabs-settings>`）中使用自定义标签运行特定测试。

与预生产分支类似，电子邮件不会发送，而是通过邮件捕获器拦截，计划操作在数据库未使用时不会频繁触发。

开发分支创建的数据库的预期寿命约为三天。之后，它们可能会被垃圾回收以腾出新数据库的空间。

.. _odoosh-gettingstarted-branches-mergingbranches:

合并分支
---------------------
您可以通过将分支拖放到彼此上轻松合并分支。

.. image:: ./media/interface-branches-merge.png
   :align: center

当您想用生产数据测试开发分支的更改时，您可以：

* 将开发分支合并到预生产分支，通过将其拖放到所需的预生产分支上，
* 将开发分支拖放到预生产部分的标题上，使其成为预生产分支。

当您的最新更改准备好投入生产时，可以将预生产分支拖放到生产分支上，以合并和部署最新功能。

如果您足够大胆，还可以将开发分支合并到生产分支中。这意味着您跳过了通过预生产分支验证更改的过程。

您可以将开发分支彼此合并，将预生产分支彼此合并。

当然，您也可以在工作站上直接使用 :code:`git merge` 合并分支。当分支中推送了新版本时，Odoo.sh 会收到通知。

将预生产分支合并到生产分支只会合并源代码：您在预生产数据库中进行的任何配置更改不会传递到生产数据库。

如果您在预生产分支中测试配置更改并希望将其应用于生产中，您必须：

* 在分支的模块中使用 XML 数据文件编写配置更改，覆盖默认配置或视图，然后在模块的 manifest 文件 (*__manifest__.py*) 中增加版本号，以在将预生产分支合并到生产分支时触发模块更新。这是更好的扩展开发的最佳实践，因为您将使用 Git 版本控制功能记录所有配置更改，因此可以追溯更改。
* 手动将它们从预生产数据库复制到生产数据库，通过复制粘贴。

.. _odoosh-gettingstarted-branches-tabs:

选项卡
=============

历史记录
-------
分支历史记录概览：

* 提交的消息及其作者，
* 与平台相关的各种事件，例如阶段变更、数据库导入、备份恢复。

.. image:: ./media/interface-branches-history.png
   :align: center

每个事件的状态显示在右上角。它可以提供有关数据库进行的操作（安装、更新、备份导入等）或其结果（测试反馈、成功的备份导入等）的信息。操作成功时，您可以通过 *连接* 按钮访问数据库。

.. _odoosh-gettingstarted-branches-tabs-mails:

邮件
-----
此选项卡包含邮件捕获器。它显示数据库发送的电子邮件概览。邮件捕获器适用于开发和预生产分支，因为生产数据库的电子邮件会实际发送而不是被拦截。

.. image:: ./media/interface-branches-mails.png
   :align: center
   :scale: 50%

Shell
-----
对容器的 shell 访问。您可以执行基本的 Linux 命令（:code:`ls`, :code:`top`），并通过键入 :code:`psql` 打开数据库的 shell。

.. image:: ./media/interface-branches-shell.png
   :align: center

您可以打开多个选项卡并拖放它们以按需排列布局，例如并排显示。

.. Note::
  长时间运行的 shell 实例不受保证。空闲 shell 可能会随时断开连接以释放资源。

编辑器
------
一个在线集成开发环境 (IDE) 用于编辑源代码。您还可以打开终端、Python 控制台，甚至 Odoo Shell 控制台。

.. image:: ./media/interface-branches-editor.png
   :align: center

您可以打开多个选项卡并拖放它们以按需排列布局，例如并排显示。

监控
----------
此链接包含当前构建的各种监控指标。

.. image:: ./media/interface-branches-monitoring.png
   :align: center

您可以缩放、更改时间范围或在每个图表上选择特定指标。在图表上，注释帮助您了解构建的变化（数据库导入、git 推送等）。

日志
----
用于查看服务器日志的查看器。

.. image:: ./media/interface-branches-logs.png
   :align: center

提供不同的日志：

* install.log：数据库安装日志。在开发分支中，包括测试的日志。
* pip.log：Python 依赖项安装日志。
* odoo.log：运行服务器的日志。
* update.log：数据库更新日志。
* pg_long_queries.log：psql 查询花费异常时间的日志。

如果日志中添加了新行，将自动显示。如果滚动到最底部，每次添加新行时浏览器会自动滚动。

您可以通过单击右上角相应的按钮暂停日志获取。获取将在 5 分钟后自动停止。您可以使用播放按钮重新启动它。

.. _odoo_sh_branches_backups:

备份
-------
可下载和恢复的备份列表，手

动备份和导入数据库的能力。

.. image:: ./media/interface-branches-backups.png
   :align: center

Odoo.sh 每天都会对生产数据库进行备份。它会保留 7 天的每日备份，4 周的每周备份和 3 个月的每月备份。每个备份包括数据库转储、文件存储（附件、二进制字段）、日志和会话。

不会备份预生产和开发数据库。尽管如此，您仍然可以将生产数据库的备份恢复到预生产分支，用于测试，或手动恢复从生产数据库中意外删除的数据。

列表包含生产数据库托管服务器上的备份。该服务器只保留一个月的备份：7 天的每日备份和 4 周的每周备份。

专用备份服务器保留相同的备份，以及 3 个额外的每月备份。要恢复或下载这些每月备份，请 `联系我们 <https://www.odoo.com/help>`_。

如果合并一个更新一个或多个模块（在 :file:`__manifest__.py` 中）或其相关 Python 依赖项（在 :file:`requirements.txt` 中）版本的提交，则 Odoo.sh 将自动执行备份（在列表中标记为更新类型），因为要么容器将通过安装新的 pip 包进行更改，要么数据库本身将通过随后触发的模块更新进行更改。在这两种情况下，我们都进行备份，因为它可能会破坏某些内容。

如果合并的提交仅更改了一些代码而没有上述修改，则 Odoo.sh 不会进行备份，因为既没有更改容器也没有更改数据库，因此平台认为这足够安全。当然，作为额外的预防措施，您可以在生产源代码中进行重大更改之前手动进行备份，以防万一出现问题（这些手动备份大约可用一周）。为了防止滥用，我们每天限制手动备份 5 次。

*导入数据库* 功能接受以下格式的数据库存档：

* 标准 Odoo 数据库管理器提供的格式（适用于本地 Odoo 服务器，位于 :code:`/web/database/manager`）。
* Odoo 在线数据库管理器。
* Odoo.sh 备份选项卡中的 Odoo.sh 备份下载按钮。
* Odoo.sh 在构建视图（:ref:`Builds view <odoosh-gettingstarted-builds>`）中的转储下载按钮。

.. _odoosh-gettingstarted-branches-tabs-settings:

设置
--------
在这里，您可以找到一些仅适用于当前选择的分支的设置。

.. image:: ./media/interface-branches-settings.jpg
   :align: center

**新提交时的行为**

对于开发和预生产分支，您可以更改分支接收新提交时的行为。默认情况下，开发分支会创建新构建，预生产分支会更新先前的构建（见生产阶段 :ref:`Production Stage <stage_production>`）。这对于需要特定设置或配置的功能特别有用，以避免每次提交时手动设置。如果选择为预生产分支创建新构建，每次推送提交时将从生产构建创建新副本。从预生产恢复到开发的分支将自动设置为“无操作”。

**模块安装**

选择要为开发构建自动安装的模块。

.. image:: ./media/interface-settings-modulesinstallation.png
   :align: center

* *仅安装我的模块* 将仅安装分支的模块。这是默认选项。子模块（:ref:`submodules <odoosh-advanced-submodules>`）将被排除。
* *完全安装（所有模块）* 将安装分支的模块、子模块中包含的模块以及 Odoo 的所有标准模块。运行完全安装时，测试套件将被禁用。
* *安装模块列表* 将安装输入框下方指定的模块。名称为模块的技术名称，必须用逗号分隔。

如果启用了测试，标准 Odoo 模块套件可能需要长达 1 小时。此设置仅适用于开发构建。预生产构建复制生产构建，生产构建仅安装基础模块。

**测试套件**

对于开发分支，您可以选择启用或禁用测试套件。默认情况下启用。启用测试套件时，您可以通过指定测试标签（:ref:`test tags <developer/reference/testing/selection>`）来限制它们。

**Odoo 版本**

仅对于开发分支，您可以更改 Odoo 版本，以便在您的生产数据库升级到新版本的过程中测试升级代码或开发功能。

此外，对于每个版本，您有两种代码更新选项。

* 您可以选择自动获取最新的错误、 安全性和性能修复。您的 Odoo 服务器源代码将每周更新一次。这是“最新”选项。
* 您可以选择通过从日期列表中选择特定版本来固定 Odoo 源代码。版本将在 3 个月后过期。到期日期临近时，您将通过邮件收到通知，如果之后没有采取行动，您将自动设置为最新版本。

**自定义域名**

在此处可以为选定的分支配置其他域名。可以添加其他 *<name>.odoo.com* 域名或您自己的自定义域名。对于后者，您必须：

* 拥有或购买域名，
* 在此列表中添加域名，
* 在您的注册商的域名管理器中，配置域名，使用 ``CNAME`` 记录设置为您的生产数据库域名。

例如，要将 *www.mycompany.com* 关联到您的数据库 *mycompany.odoo.com*：

* 在 Odoo.sh 中，在项目设置的自定义域名中添加 *www.mycompany.com*，
* 在您的域名管理器（例如 *godaddy.com*, *gandi.net*, *ovh.com*）中，将 *www.mycompany.com* 配置为值为 *mycompany.odoo.com* 的 ``CNAME`` 记录。

裸域名（例如 *mycompany.com*）不被接受：

* 它们只能使用 ``A`` 记录进行配置，
* ``A`` 记录仅接受 IP 地址作为值，
* 您的数据库的 IP 地址可能会更改，例如升级、硬件故障或希望将数据库托管在其他国家或大陆时。

因此，由于 IP 地址更改，裸域名可能会突然无法使用。

此外，如果希望 *mycompany.com* 和 *www.mycompany.com* 都能与您的数据库一起使用，建议将第一个重定向到第二个，这是 `SEO 最佳实践 <https://support.google.com/webmasters/answer/7451184?hl=en>`_ （参见“为文档提供一个版本的 URL”）以便拥有一个主导 URL。因此，您可以将 *mycompany.com* 配置为重定向到 *www.mycompany.com*。大多数域名管理器都有配置此重定向的功能。这通常称为网页重定向。

**HTTPS/SSL**

如果正确设置了重定向，平台将在一小时内自动生成一个 `Let's Encrypt <https://letsencrypt.org/about/>`_ SSL 证书，您的域名将通过 HTTPS 访问。

目前无法在 Odoo.sh 平台上配置您自己的 SSL 证书，如果需求足够，我们会考虑该功能。

**SPF 和 DKIM 合规性**

如果您的用户电子邮件地址的域名使用 SPF（发送方策略框架）或 DKIM（域名密钥识别邮件），请不要忘记在您的域名设置中授权 Odoo 作为发送主机，以提高外发邮件的送达率。配置步骤在 :ref:`Discuss app documentation <discuss-email_servers-spf-compliant>` 中进行了说明。

.. Warning::
  忘记配置 SPF 或 DKIM 授权 Odoo 作为发送主机，可能导致您的邮件在联系人收件箱中作为垃圾邮件发送。

Shell 命令
==============
在视图的右上角，可以使用不同的 shell 命令。

.. image:: ./media/interface-branches-shellcommands.png
   :align: center

每个命令都可以复制到剪贴板以便在终端中使用，某些命令可以直接在 Odoo.sh 中使用，单击 *运行* 按钮。在这种情况下，将弹出一个窗口提示用户定义可能的占位符（如 ``<URL>``，``<PATH>`` 等）。

克隆
-----
下载 Git 存储库。

.. code-block:: bash

  $ git clone --recurse-submodules --branch master git@github.com:odoo/odoo.git

克隆存储库 *odoo/odoo*。

* :code:`--recurse-submodules`：下载存储库的子模块。包括子模块中的子模块。
* :code:`--branch`：检出存储库的特定分支，在这种情况下为 *master*。

此命令的 *运行* 按钮不可用，因为它旨在在您的机器上使用。

分支
----
基于当前分支创建一个新分支。

.. code-block:: bash

  $ git checkout -b feature-1 master

创建一个名为 *feature-1* 的新分支，基于分

支 *master*，然后检出它。

.. code-block:: bash

  $ git push -u origin feature-1

将新分支 *feature-1* 上传到您的远程存储库。

合并
-----
将当前分支合并到另一个分支。

.. code-block:: bash

  $ git merge staging-1

将分支 *staging-1* 合并到当前分支。

.. code-block:: bash

  $ git push -u origin master

将您刚刚添加到 *master* 分支的更改上传到远程存储库。

SSH
---
设置
~~~~~
要使用 SSH，您必须设置个人资料的 SSH 公钥（如果尚未设置）。要执行此操作，请按以下步骤操作：

#. `生成一个新的 SSH 密钥 <https://help.github.com/en/github/authenticating-to-github/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent#generating-a-new-ssh-key>`_
#. `将 SSH 密钥复制到剪贴板 <https://help.github.com/en/github/authenticating-to-github/adding-a-new-ssh-key-to-your-github-account>`_（仅适用步骤 1）
#. 将复制的内容粘贴到个人资料的 SSH 密钥中并按“添加”

   .. image:: ./media/SSH-key-pasting.png
      :align: center

#. 密钥应显示在下方

   .. image:: ./media/SSH-key-appearing.png
      :align: center

连接
~~~~~~~~~~

要使用 ssh 连接到您的构建，请在终端中使用以下命令：

.. code-block:: bash

  $ ssh <build_id>@<domain>

您将在右上角的 SSH 选项卡中找到此命令的快捷方式。

.. image:: ./media/SSH-panel.png
   :align: center

如果您在项目中拥有正确的访问权限（:ref:`correct access rights <odoosh-gettingstarted-settings-collaborators>`），您将获得构建的 ssh 访问权限。

.. Note::
  长时间运行的 ssh 连接不受保证。空闲连接将被断开以释放资源。

子模块
---------

在当前分支中添加另一个存储库的分支作为 *子模块*。

*子模块* 允许您在项目中使用其他存储库中的模块。

子模块功能在本文档的章节中详细介绍（:ref:`Submodules <odoosh-advanced-submodules>`）。

.. code-block:: bash

  $ git submodule add -b master <URL> <PATH>

将存储库 *<URL>* 的分支 *master* 作为子模块添加到当前分支中的路径 *<PATH>* 下。

.. code-block:: bash

  $ git commit -a

提交您当前的所有更改。

.. code-block:: bash

  $ git push -u origin master

将您刚刚添加到 *master* 分支的更改上传到远程存储库。

删除
------

从存储库中删除分支。

.. code-block:: bash

  $ git push origin :master

删除远程存储库中的分支。

.. code-block:: bash

  $ git branch -D master

删除本地存储库中的分支。
