.. _odoosh-gettingstarted-online-editor:

==================================
在线编辑器
==================================

概述
========

在线编辑器允许你通过网页浏览器编辑构建的源代码。它还提供了打开终端、Python 控制台、Odoo Shell 控制台和
`Notebooks <https://jupyterlab.readthedocs.io/en/stable/user/notebook.html>`_ 的功能。

.. image:: ./media/interface-editor.png
   :align: center

你可以通过以下方式访问构建的编辑器：
:ref:`分支选项卡 <odoosh-gettingstarted-branches-tabs>`,
:ref:`构建下拉菜单 <odoosh-gettingstarted-builds-dropdown-menu>`
或在构建域名后添加 */odoo-sh/editor*
（例如 *https://odoo-addons-master-1.dev.odoo.com/odoo-sh/editor*）。

编辑源代码
====================

工作目录包含以下文件夹：

::

  .
  ├── home
  │    └── odoo
  │         ├── src
  │         │    ├── odoo                Odoo 社区版源代码
  │         │    │    └── odoo-bin       Odoo 服务器可执行文件
  │         │    ├── enterprise          Odoo 企业版源代码
  │         │    ├── themes              Odoo 主题源代码
  │         │    └── user                你的仓库分支源代码
  │         ├── repositories             项目使用的 Git 仓库
  │         ├── data
  │         │    ├── filestore           数据库附件及二进制字段文件
  │         │    └── sessions            访客和用户会话
  │         └── logs
  │              ├── install.log         数据库安装日志
  │              ├── odoo.log            运行服务器日志
  │              ├── update.log          数据库更新日志
  │              └── pip.log             Python 包安装日志

你可以在开发和预发布构建中编辑源代码（位于 */src* 下的文件）。

.. note::
  你的更改不会传播到新的构建中，如果你想使更改持久化，必须将其提交到源代码中。

对于生产构建，源代码是只读的，因为在生产服务器上应用本地更改不是一个好做法。

* 你的 Github 仓库的源代码位于 */src/user* 下，
* Odoo 的源代码位于

  * */src/odoo* (`odoo/odoo <https://github.com/odoo/odoo>`_),
  * */src/enterprise* (`odoo/enterprise <https://github.com/odoo/enterprise>`_),
  * */src/themes* (`odoo/design-themes <https://github.com/odoo/design-themes>`_)。

要在编辑器中打开文件，只需在左侧文件浏览器面板中双击它。

.. image:: ./media/interface-editor-open-file.png
   :align: center

然后你可以开始进行更改。你可以通过菜单 :menuselection:`File --> Save .. File` 保存更改，或使用 :kbd:`Ctrl+S` 快捷键。

.. image:: ./media/interface-editor-save-file.png
   :align: center

如果你保存的 Python 文件位于 Odoo 服务器插件路径下，
Odoo 会检测到它并自动重新加载，使更改立即生效，而无需手动重启服务器。

.. image:: ./media/interface-editor-automaticreload.gif
   :align: center

但是，如果更改的是存储在数据库中的数据（例如字段标签或视图），你必须更新相应的模块以应用更改。
你可以使用菜单 :menuselection:`Odoo --> Update current module` 更新当前打开的文件模块。请注意，当前打开的文件是文本编辑器中聚焦的文件，而不是文件浏览器中突出显示的文件。

.. image:: ./media/interface-editor-update-current-module.png
   :align: center

你也可以打开终端并执行以下命令：

.. code-block:: bash

  $ odoo-bin -u <逗号分隔的模块名称> --stop-after-init

.. _odoosh-gettingstarted-online-editor-push:

提交并推送更改
==========================

你可以将更改提交并推送到你的 Github 仓库。

* 打开终端 (:menuselection:`File --> New --> Terminal`),
* 使用 :code:`cd ~/src/user` 切换到 *~/src/user* 目录，
* 使用 :code:`git add` 暂存更改，
* 使用 :code:`git commit` 提交更改，
* 使用 :code:`git push https HEAD:<branch>` 推送更改。

在上述命令中，

* *https* 是你的 *HTTPS* Github 远程仓库的名称
  （例如 https://github.com/username/repository.git），
* HEAD 是你提交的最新修订的引用，
* <branch> 必须替换为你想推送更改的分支名称，
  如果你在开发构建中工作，通常是当前分支。

.. image:: ./media/interface-editor-commit-push.png
   :align: center

.. Note::
  SSH Github 远程仓库没有使用，因为你的 SSH 私钥没有托管在构建容器中（出于显而易见的安全考虑），
  也没有通过 SSH 代理转发（因为你是通过网页浏览器访问这个编辑器），
  因此你无法使用 SSH 验证 Github 身份。
  你必须使用 Github 仓库的 HTTPS 远程仓库推送更改，它会自动添加并命名为 *https* 到你的 Git 远程仓库中。
  系统会提示你输入 Github 用户名和密码。
  如果你在 Github 上启用了两步验证，
  你可以创建一个
  `个人访问令牌 <https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/>`_
  并将其用作密码。授予 ``repo`` 权限即可。

.. Note::
  Git 源文件夹 *~/src/user* 没有检出在分支上，而是检出在分离的修订版上：
  这是因为构建工作在特定修订版而不是分支上。
  换句话说，这意味着你可以在同一个分支上有多个构建，但在不同的修订版上。

一旦你的更改被推送，
根据你的 :ref:`分支推送行为 <odoosh-gettingstarted-branches-tabs-settings>`，
可能会创建一个新的构建。你可以继续在你推送的编辑器中工作，
因为它会具有与创建的新构建相同的修订版，但始终确保在使用分支的最新修订版的构建的编辑器中工作。

控制台
========

你可以打开 Python 控制台，它们是
`IPython 交互式 shell <https://ipython.readthedocs.io/en/stable/interactive/tutorial.html>`_。
使用 Python 控制台而不是终端中的 IPython shell 的一个最有趣的附加功能是
`富显示 <https://ipython.readthedocs.io/en/stable/config/integrating.html#rich-display>`_
功能。
借助这一点，你可以以 HTML 方式显示对象。

例如，你可以使用
`pandas <https://pandas.pydata.org/pandas-docs/stable/tutorials.html>`_
显示 CSV 文件的单元格。

.. image:: ./media/interface-editor-console-python-read-csv.png
   :align: center

你还可以打开 Odoo Shell 控制台来操作
你的数据库的 Odoo 注册表和模型方法。你还可以直接读取或写入你的记录。

.. Warning::
  在 Odoo 控制台中，事务是自动提交的。
  例如，这意味着记录中的更改会有效地应用到数据库中。
  如果你更改了用户的名称，该用户的名称也会在数据库中更改。
  因此，你在生产数据库上使用 Odoo 控制台时应小心。

你可以使用 *env* 调用数据库注册表的模型，例如 :code:`env['res.users']`。

.. code-block:: python

  env['res.users'].search_read([], ['name', 'email', 'login'])
  [{'id': 2,
  'login': 'admin',
  'name': 'Administrator',
  'email': 'admin@example.com'}]

类 :code:`Pretty` 提供了
轻松以漂亮方式显示列表和字典的可能性，使用上述提到的
`富显示 <https://ipython.readthedocs.io/en/stable/config/integrating.html#rich-display>`_。

.. image:: ./media/interface-editor-console-odoo-pretty.png
   :align: center

你还可以使用
`pandas <https://pandas.pydata.org/pandas-docs/stable/tutorials.html>`_
显示图表。

.. image:: ./media/interface-editor-console-odoo-graph.png
  :align: center
