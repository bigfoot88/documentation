==================================
你的第一个模块
==================================

概述
========

本章将指导你创建第一个 Odoo 模块并将其部署到你的 Odoo.sh 项目中。

本教程需要你已经：:ref:`在 Odoo.sh 上创建了一个项目 <odoosh-gettingstarted-create>`，并且你知道你的 Github 仓库的 URL。

基础的 Git 和 Github 使用方法将在下文中解释。

假设如下：

* *~/src* 是存放与 Odoo 项目相关的 Git 仓库的目录，
* *odoo* 是 Github 用户，
* *odoo-addons* 是 Github 仓库，
* *feature-1* 是开发分支的名称，
* *master* 是生产分支的名称，
* *my_module* 是模块的名称。

请将这些替换为你选择的值。

创建开发分支
=============================

从 Odoo.sh
-------------

在分支视图中：

* 点击开发阶段旁边的 :code:`+` 按钮，
* 在 *Fork* 选择中选择分支 *master*，
* 在 *To* 输入框中输入 *feature-1*。

  |pic1|  |pic2|

.. |pic1| image:: ./media/firstmodule-development-+.png
   :width: 45%

.. |pic2| image:: ./media/firstmodule-development-fork.png
   :width: 45%


构建创建后，你可以访问编辑器并浏览到 *~/src/user* 文件夹，访问你开发分支的代码。

.. image:: ./media/firstmodule-development-editor.png
  :align: center

.. image:: ./media/firstmodule-development-editor-interface.png
  :align: center

从你的电脑
------------------


在你的电脑上克隆 Github 仓库：

.. code-block:: bash

  $ mkdir ~/src
  $ cd ~/src
  $ git clone https://github.com/odoo/odoo-addons.git
  $ cd ~/src/odoo-addons

创建一个新分支：

.. code-block:: bash

  $ git checkout -b feature-1 master


创建模块结构
===========================

脚手架模块
----------------------

虽然不是必须的，但脚手架可以避免设置基本 Odoo 模块结构的繁琐操作。你可以使用 *odoo-bin* 可执行文件生成新的模块脚手架。

在 Odoo.sh 编辑器的终端中：

.. code-block:: bash

  $ odoo-bin scaffold my_module ~/src/user/

或者，如果你在电脑上已经 :ref:`安装了 Odoo <setup/install/source>`，可以：

.. code-block:: bash

  $ ./odoo-bin scaffold my_module ~/src/odoo-addons/

如果你不想在电脑上安装 Odoo，你也可以 :download:`下载这个模块结构模板 <media/my_module.zip>`，并在其中将所有 *my_module* 替换为你选择的名称。

将生成以下结构：

::

  my_module
  ├── __init__.py
  ├── __manifest__.py
  ├── controllers
  │   ├── __init__.py
  │   └── controllers.py
  ├── demo
  │   └── demo.xml
  ├── models
  │   ├── __init__.py
  │   └── models.py
  ├── security
  │   └── ir.model.access.csv
  └── views
      ├── templates.xml
      └── views.xml

.. Warning::

  不要在模块名称中使用下划线 ( _ ) 以外的特殊字符，甚至连连字符 ( - ) 也不要使用。
  这个名称将用于模块的 Python 类，而在 Python 中使用下划线以外的特殊字符是不合法的。

取消注释文件的内容：

* *models/models.py*，
  一个包含字段的模型示例，
* *views/views.xml*，
  包含菜单的树形和表单视图，
* *demo/demo.xml*，
  上述示例模型的示例记录，
* *controllers/controllers.py*，
  实现一些路由的控制器示例，
* *views/templates.xml*，
  上述控制器路由使用的两个示例 qweb 视图，
* *__manifest__.py*，
  模块的清单，包括例如其标题、描述和数据文件加载。你只需取消注释访问控制列表数据文件：

  .. code-block:: xml

    # 'security/ir.model.access.csv',

手动创建
--------

如果你想手动创建模块结构，可以参考 :doc:`构建一个 Odoo 模块 </developer/howtos/backend>` 了解模块的结构和每个文件的内容。

推送开发分支
===========================

将更改暂存以便提交

.. code-block:: bash

  $ git add my_module

提交更改

.. code-block:: bash

  $ git commit -m "My first module"

将更改推送到远程仓库

在 Odoo.sh 编辑器终端中：

.. code-block:: bash

  $ git push https HEAD:feature-1

上面的命令在 :ref:`提交并推送更改
<odoosh-gettingstarted-online-editor-push>` 部分解释。
它包括关于提示你输入用户名和密码的解释，
以及如果你使用两步验证该怎么办。

或者，在你的电脑终端中：

.. code-block:: bash

  $ git push -u origin feature-1

首次推送需要指定 *-u origin feature-1*。
从此之后，要从电脑推送未来的更改，你只需使用

.. code-block:: bash

  $ git push

测试你的模块
================

你的分支应出现在项目中的开发分支中。

.. image:: ./media/firstmodule-test-branch.png
  :align: center

在项目的分支视图中，
你可以在左侧导航面板中点击分支名称来访问其历史记录。

.. image:: ./media/firstmodule-test-branch-history.png
  :align: center

你可以在这里看到你刚刚推送的更改，包括你设置的评论。
一旦数据库准备好，你可以通过点击 *Connect* 按钮访问它。

.. image:: ./media/firstmodule-test-database.png
  :align: center

如果你的 Odoo.sh 项目配置为自动安装你的模块，
你会直接在数据库应用中看到它。否则，它将在可安装的应用中可见。

然后，你可以试用你的模块，创建新记录并测试功能和按钮。


用生产数据测试
=============================

此步骤需要一个生产数据库。如果你还没有，可以创建一个。

一旦你在带有演示数据的开发构建中测试了你的模块并认为它已经准备好，
你可以使用分支进行阶段测试来用生产数据进行测试。

你可以：

* 将你的开发分支变为阶段分支，通过拖放到 *staging* 区域标题上。

  .. image:: ./media/firstmodule-test-devtostaging.png
    :align: center

* 将其合并到现有的阶段分支，通过拖放到给定的阶段分支上。

  .. image:: ./media/firstmodule-test-devinstaging.png
    :align: center

你也可以使用 :code:`git merge` 命令合并分支。

这将创建一个新的阶段构建，复制生产数据库并使其运行更新后的服务器，
带有你分支的最新更改。

.. image:: ./media/firstmodule-test-mergedinstaging.png
  :align: center

一旦数据库准备好，你可以通过 *Connect* 按钮访问它。

.. _odoosh-gettingstarted-firstmodule-productiondata-install:

安装你的模块
-------------------

你的模块不会自动安装，你需要从应用菜单安装它。
事实上，阶段构建的目的是测试你的更改在生产中的行为，
而在生产中你不希望模块自动安装，而是按需安装。

你的模块也可能不会直接出现在可安装的应用中，你需要先更新应用列表：

* 从设置中激活开发者模式，

  .. image:: ./media/firstmodule-test-developermode.png
    :align: center

* 在应用菜单中，点击 *Update Apps List* 按钮，
* 在出现的对话框中，点击 *Update* 按钮。

  .. image:: ./media/firstmodule-test-updateappslist.png
    :align: center

然后，你的模块将出现在可用应用列表中。

.. image:: ./media/firstmodule-test-mymoduleinapps.png
  :align: center

部署到生产环境
====================

一旦你在带有生产数据的阶段分支中测试了你的模块并认为它已经准备好生产，
你可以将你的分支合并到生产分支中。

将你的阶段分支拖放到生产分支上。

.. image:: ./media/firstmodule-test-mergeinproduction.png
  :align: center

你也可以使用 :code:`git merge` 命令合并分支。

这将合并阶段分支的最新更改到生产分支中，
并用这些最新更改更新生产服务器。

.. image:: ./media/firstmodule-test-mergedinproduction.png
  :align: center

一旦数据库准备好，你可以通过 *Connect*

 按钮访问它。

安装你的模块
-------------------

你的模块不会自动安装，
你需要按照 :ref:`上面关于在阶段数据库中安装模块的部分
<odoosh-gettingstarted-firstmodule-productiondata-install>` 解释手动安装它。

添加更改
============

本节解释如何通过在模型中添加新字段来为你的模块添加更改并部署它。

在 Odoo.sh 编辑器中，
 * 浏览到你的模块文件夹 *~/src/user/my_module*，
 * 然后，打开文件 *models/models.py*。

或者，在你的电脑上，
 * 使用你选择的文件浏览器浏览到你的模块文件夹 *~/src/odoo-addons/my_module*，
 * 然后，使用你选择的编辑器（如 *Atom*、*Sublime Text*、*PyCharm*、*vim* 等）打开文件 *models/models.py*。

然后，在 description 字段之后

.. code-block:: python

  description = fields.Text()

添加一个 datetime 字段

.. code-block:: python

  start_datetime = fields.Datetime('Start time', default=lambda self: fields.Datetime.now())

然后，打开文件 *views/views.xml*。

在

.. code-block:: xml

    <field name="value2"/>

之后添加

.. code-block:: xml

    <field name="start_datetime"/>

这些更改通过在表中添加列来改变数据库结构，并修改存储在数据库中的视图。

为了在现有数据库（如生产数据库）中应用这些更改，
需要更新模块。

如果你希望在推送更改时由 Odoo.sh 平台自动执行更新，
请在模块清单中增加模块版本。

打开模块清单 *__manifest__.py*。

替换

.. code-block:: python

  'version': '0.1',

为

.. code-block:: python

  'version': '0.2',

平台将检测版本更改并在新版本部署时触发模块更新。

浏览到你的 Git 文件夹。

然后，从 Odoo.sh 终端中：

.. code-block:: bash

  $ cd ~/src/user/

或者，从你的电脑终端中：

.. code-block:: bash

  $ cd ~/src/odoo-addons/

然后，将更改暂存以便提交

.. code-block:: bash

  $ git add my_module

提交更改

.. code-block:: bash

  $ git commit -m "[ADD] my_module: add the start_datetime field to the model my_module.my_module"

推送更改：

在 Odoo.sh 终端中：

.. code-block:: bash

  $ git push https HEAD:feature-1

或者，在你的电脑终端中：

.. code-block:: bash

  $ git push

平台将为 *feature-1* 分支创建一个新构建。

.. image:: ./media/firstmodule-test-addachange-build.png
  :align: center

一旦你测试了更改，你可以将更改合并到生产分支中，例如通过在 Odoo.sh 界面中将分支拖放到生产分支上。由于你在清单中增加了模块版本，
平台将自动更新模块，并且新字段将直接可用。
否则，你可以在应用列表中手动更新模块。

使用外部 Python 库
==============================

如果你想使用未默认安装的外部 Python 库，
你可以定义一个 *requirements.txt* 文件，列出模块依赖的外部库。

平台将使用此文件自动安装项目所需的 Python 库。

此功能通过在模块中使用 `Unidecode library <https://pypi.python.org/pypi/Unidecode>`_ 进行解释。

在你的仓库根目录创建一个 *requirements.txt* 文件

从 Odoo.sh 编辑器中，创建并打开文件 ~/src/user/requirements.txt。

或者，从你的电脑中，创建并打开文件 ~/src/odoo-addons/requirements.txt。

添加

.. code-block:: text

  unidecode

然后在你的模块中使用库，例如在模型的 name 字段中去除字符重音符号。

打开文件 *models/models.py*。

在

.. code-block:: python

  from odoo import models, fields, api

之前添加

.. code-block:: python

  from unidecode import unidecode

在

.. code-block:: python

  start_datetime = fields.Datetime('Start time', default=lambda self: fields.Datetime.now())

之后添加

.. code-block:: python

  @api.model
  def create(self, values):
      if 'name' in values:
          values['name'] = unidecode(values['name'])
      return super(my_module, self).create(values)

  @api.multi
  def write(self, values):
      if 'name' in values:
          values['name'] = unidecode(values['name'])
      return super(my_module, self).write(values)

添加 Python 依赖需要增加模块版本，以便平台安装它。

编辑模块清单 *__manifest__.py*

替换

.. code-block:: python

  'version': '0.2',

为

.. code-block:: python

  'version': '0.3',

暂存并提交更改：

.. code-block:: bash

  $ git add requirements.txt
  $ git add my_module
  $ git commit -m "[IMP] my_module: automatically remove special chars in my_module.my_module name field"

然后，推送更改：

在 Odoo.sh 终端中：

.. code-block:: bash

  $ git push https HEAD:feature-1

在你的电脑终端中：

.. code-block:: bash

  $ git push
