.. _odoosh-advanced-submodules:

==================================
子模块
==================================

概述
========

`Git 子模块 <https://git-scm.com/book/en/v2/Git-Tools-Submodules>`_ 允许您将其他 Git 项目集成到您的代码中，而无需复制粘贴所有代码。

确实，您的自定义模块可能依赖于其他存储库中的模块。对于 Odoo，这个功能允许您将其他 Git 存储库中的模块添加到您存储库的分支中。通过子模块将这些依赖项添加到您的分支中，使得代码和服务器的部署更加容易，因为您可以在克隆自己存储库的同时克隆添加为子模块的存储库。

此外，您可以选择作为子模块添加的存储库的分支，并且可以控制您想要的修订版本。您可以决定是否要将子模块固定到特定的修订版本以及何时更新到较新的修订版本。

在 Odoo.sh 上，子模块使您可以使用和依赖其他存储库中的模块。平台会检测到您在分支中通过子模块添加的模块，并自动将它们添加到您的 addons 路径中，以便您可以在数据库中安装它们。

如果您在分支中添加私有存储库作为子模块，您需要在 Odoo.sh 项目设置和您的存储库设置中配置一个部署密钥。否则 Odoo.sh 将无法下载它们。具体步骤详见章节 :ref:`设置 > 子模块 <odoosh-gettingstarted-settings-submodules>`。

添加子模块
==================

通过 Odoo.sh（简单方法）
---------------------

.. warning::
   目前无法通过此方法添加**私有**存储库。您仍可以 :ref:`通过 Git <odoosh-advanced-submodules-withgit>` 添加私有存储库。

在 Odoo.sh 上，在项目的分支视图中，选择您要添加子模块的分支。

在右上角，点击 *Submodule* 按钮，然后点击 *Run*。

.. image:: ./media/advanced-submodules-button.png
   :align: center

会显示一个带有表单的对话框。按如下填写输入框：

* Repository URL: 存储库的 SSH URL。
* Branch: 您要使用的分支。
* Path: 您希望在分支中添加此子模块的文件夹。

.. image:: ./media/advanced-submodules-dialog.png
   :align: center

在 Github 上，您可以通过存储库的 *Clone or download* 按钮获取存储库 URL。确保使用 *SSH*。

.. image:: ./media/advanced-submodules-github-sshurl.png
  :align: center

.. _odoosh-advanced-submodules-withgit:

通过 Git（高级方法）
---------------------

在终端中，在克隆了您的 Git 存储库的文件夹中，检出您要添加子模块的分支：

.. code-block:: bash

  $ git checkout <branch>

然后，使用以下命令添加子模块：

.. code-block:: bash

  $ git submodule add -b <branch> <git@yourprovider.com>:<username/repository.git> <path>

替换

* *<git@yourprovider.com>:<username/repository.git>* 为您要添加为子模块的存储库的 SSH URL，
* *<branch>* 为您要使用的该存储库中的分支，
* *<path>* 为您希望在其中添加此子模块的文件夹。

提交并推送您的更改：

.. code-block:: bash

  $ git commit -a && git push -u <remote> <branch>

替换

* <remote> 为您要推送更改的存储库。对于标准的 Git 设置，这是 *origin*。
* <branch> 为您要推送更改的分支。
  最有可能是您在第一步中使用 :code:`git checkout` 的分支。

您可以阅读 `git-scm.com 文档 <https://git-scm.com/book/en/v2/Git-Tools-Submodules>`_ 以获取有关 Git 子模块的更多详细信息。例如，如果您想更新子模块以获取最新的修订版本，可以参阅章节
`拉取上游更改 <https://git-scm.com/book/en/v2/Git-Tools-Submodules#_pulling_in_upstream_changes>`_。

忽略模块
==============

如果您添加的存储库包含大量模块，您可能希望忽略其中一些模块，以防止它们自动安装。为此，您可以在子模块文件夹前加上 :code:`.` 前缀。平台将忽略此文件夹，您可以通过从其他文件夹创建指向它们的符号链接来手动选择模块。
