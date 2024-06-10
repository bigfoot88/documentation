.. _setup/install:

===============
安装 Odoo
===============

根据预期用途，有多种安装 Odoo 或完全不安装的方法。

本文档试图描述大多数安装选项。

:ref:`setup/install/online`
    最简单的用于生产或试用 Odoo 的方法。

:ref:`setup/install/packaged`
    适用于测试 Odoo、开发模块，并且可以通过额外的部署和维护工作用于长期生产使用。

:ref:`setup/install/source`
    提供更大的灵活性：例如允许在同一系统上运行多个 Odoo 版本。适合开发模块，也可以作为生产部署的基础。

:ref:`setup/install/docker`
    如果您通常使用 docker_ 进行开发或部署，可以使用官方的 docker_ 基础镜像。


.. _setup/install/editions:

版本
========

Odoo 有两个不同的版本：社区版和企业版。在我们的 SaaS_ 上可以使用企业版，代码访问仅限于企业客户和合作伙伴。社区版对任何人都是免费的。

如果您已经在使用社区版并希望升级到企业版，请参阅 :ref:`setup/enterprise` （除 :ref:`setup/install/source` 外）。

.. _setup/install/online:

在线
======

演示
----

要简单了解 Odoo，可以使用演示_实例。这些是共享实例，只会存在几个小时，可以用来浏览和尝试，无需任何承诺。

演示_实例不需要本地安装，只需要一个网络浏览器。

SaaS
----

开始使用非常简单，由 Odoo S.A. 完全管理和迁移，Odoo 的 SaaS_ 提供了私有实例，并且最初是免费的。可以用来发现和测试 Odoo，并进行非代码定制（即与自定义模块或 Odoo 应用商店不兼容），无需在本地安装。

既可用于测试 Odoo 也可用于长期生产使用。

与演示_实例一样，SaaS_ 实例不需要本地安装，只需一个网络浏览器即可。

.. _setup/install/packaged:

打包安装程序
===================

Odoo 提供适用于 Windows、基于 deb 的发行版（Debian、Ubuntu 等）和基于 RPM 的发行版（Fedora、CentOS、RHEL 等）的打包安装程序，适用于社区版和企业版。

这些软件包会自动设置所有依赖项（社区版），但可能难以保持最新。

官方社区包及其所有相关依赖项需求可在我们的 nightly_ 服务器上找到。社区版和企业版软件包都可以从我们的下载_页面下载（您必须以付费客户或合作伙伴身份登录才能下载企业版软件包）。

Windows
-------

* 从我们的 nightly_ 服务器（仅社区版）或下载_页面（任何版本）的 Windows 安装程序下载安装程序。
* 运行下载的文件。

  .. warning:: 在 Windows 8 上，您可能会看到一个标题为“Windows 保护了您的电脑”的警告。点击 :guilabel:`更多信息` 然后
               :guilabel:`仍然运行`。

* 接受 UAC_ 提示。
* 按照各个安装步骤进行操作。

Odoo 会在安装结束时自动启动。

Linux
-----

Debian/Ubuntu
'''''''''''''

Odoo 12.0 的 'deb' 包当前支持 `Debian Stretch`_，`Ubuntu 18.04`_ 或更高版本。

准备
^^^^^^^

Odoo 需要一个 `PostgreSQL`_ 服务器才能正常运行。Odoo 'deb' 包的默认配置是使用与 Odoo 实例相同主机上的 PostgreSQL 服务器。以 root 身份执行以下命令以安装 PostgreSQL 服务器：

.. code-block:: console

  # apt-get install postgresql -y

为了打印 PDF 报告，您必须自己安装 wkhtmltopdf_：
Debian 仓库中提供的 wkhtmltopdf_ 版本不支持页眉和页脚，因此不作为直接依赖项使用。推荐的版本是 0.12.5，可在 `wkhtmltopdf 下载页面`_ 的存档部分找到。之前推荐的版本 0.12.1 是一个不错的替代选择。
有关各个版本及其各自特点的更多详细信息，请参见我们的 `wiki <https://github.com/odoo/odoo/wiki/Wkhtmltopdf>`_。

仓库
^^^^^^^^^^

Odoo S.A. 提供了一个可以与 Debian 和 Ubuntu 发行版一起使用的仓库。可以通过以下命令以 root 身份安装 Odoo 社区版：

.. code-block:: console

    # wget -O - https://nightly.odoo.com/odoo.key | apt-key add -
    # echo "deb http://nightly.odoo.com/12.0/nightly/deb/ ./" >> /etc/apt/sources.list.d/odoo.list
    # apt-get update && apt-get install odoo

然后可以使用常规的 ``apt-get upgrade`` 命令保持安装的最新状态。

目前没有企业版的仓库。

Deb 包
^^^^^^^^^^^

除了使用上面描述的仓库之外，还可以在这里下载 'deb' 包：

* 社区版： `nightly`_
* 企业版： `Download`_

然后可以使用 ``gdebi``：

.. code-block:: console

    # gdebi <path_to_installation_package>

或者使用 ``dpkg``：

.. code-block:: console

    # dpkg -i <path_to_installation_package> # 这可能会因缺少依赖项而失败
    # apt-get install -f # 应该会安装缺少的依赖项
    # dpkg -i <path_to_installation_package>

这将把 Odoo 安装为服务，创建必要的 PostgreSQL_ 用户，并自动启动服务器。

.. warning:: 以下 3 个 Python 包仅在 Debian 包中建议使用。Ubuntu Xenial（16.04）中没有这些包。

* python3-vobject：用于日历生成 ical 文件。
* python3-pyldap：用于使用 LDAP 验证用户。
* python3-qrcode：用于 ESC/POS 硬件驱动程序。

如果需要上述警告中提到的一个或多个包，可以手动安装。
一种简单的方法是使用 pip3 进行安装：

.. code-block:: console

    $ sudo pip3 install vobject qrcode
    $ sudo apt install libldap2-dev libsasl2-dev
    $ sudo pip3 install pyldap

.. warning:: Debian 9 和 Ubuntu 不提供 python 模块 num2words 的包。
             Odoo 将不会渲染文本金额，这可能会导致 "l10n_mx_edi" 模块出现问题。

如果需要此功能，可以这样安装 python 模块：

.. code-block:: console

    $ sudo pip3 install num2words

Fedora
''''''

Odoo 12.0 的 'rpm' 包支持 Fedora 26。
截至 2017 年，CentOS 尚不具备 Odoo 12.0 所需的最低 Python 版本（3.5）。

准备
^^^^^^^
Odoo 需要一个 `PostgreSQL`_ 服务器才能正常运行。假设已正确配置了 'sudo' 命令，请运行以下命令：

.. code-block:: console

    $ sudo dnf install -y postgresql-server
    $ sudo postgresql-setup --initdb --unit postgresql
    $ sudo systemctl enable postgresql
    $ sudo systemctl start postgresql

为了打印 PDF 报告，您必须自己安装 wkhtmltopdf_：
Debian 仓库中提供的 wkhtmltopdf_ 版本不支持页眉和页脚，因此不作为直接依赖项使用。推荐的版本是 0.12.5，可在 `wkhtmltopdf 下载页面`_ 的存档部分找到。之前推荐的版本 0.12.1 是一个不错的替代选择。
有关各个版本及其各自特点的更多详细信息，请参见我们的 `wiki <https://github.com/odoo/odoo/wiki/Wkhtmltopdf>`_。

仓库
^^^^^^^^^^

Odoo S.A. 提供了一个可以与 Fedora 发行版一起使用的仓库。
可以通过以下命令安装 Odoo 社区版：

.. code-block:: console

    $ sudo dnf config-manager --add-repo=https://nightly.odoo.com/12.0/nightly/rpm/odoo.repo
    $ sudo dnf install -y odoo
    $ sudo systemctl enable odoo
    $ sudo systemctl start odoo

RPM 包
^^^^^^^^^^^

除了使用上面描述的仓库之外，还可以在这里下载 'rpm' 包：

* 社区版： `nightly`_
* 企业版： `Download`_

下载后，可以使用 'dnf' 包管理器安装软件包：

.. code-block:: console

    $ sudo dnf localinstall odoo_12.0.latest.noarch.rpm
    $ sudo systemctl enable odoo
    $ sudo systemctl start odoo


.. _setup/install/source:

源安装
==============

源“安装”实际上是关于不安装 Odoo，直接从源代码运行它。

这对于模块开发人员来说可能更方便，因为与使用打包安装相比，Odoo 源代码更容易访问（用于获取信息或构建此文档并离线可用）。

它还

使启动和停止 Odoo 比打包安装程序设置的服务更灵活和明确，并允许使用
:ref:`命令行参数 <reference/cmdline>` 覆盖设置，而无需编辑配置文件。

最后，它提供了对系统设置的更大控制，并允许更轻松地并行保持（和运行）多个 Odoo 版本。

Windows
-------

获取源代码
'''''''''''''''''

有两种方式获取 Odoo 源代码：作为 zip **压缩包** 或通过 **git**。

压缩包
^^^^^^^

社区版：

* `官方下载页面 <download_>`_
* `GitHub 仓库 <community-repository_>`_
* `夜间服务器 <nightly_>`_

企业版：

* `官方下载页面 <download_>`_
* `GitHub 仓库 <enterprise-repository_>`_

Git
^^^

以下操作需要在您的机器上安装 git_ 并且具备基本的 git 命令知识。

社区版：

.. code-block:: doscon

    C:\> git clone https://github.com/odoo/odoo.git


企业版：（请参阅 :ref:`setup/install/editions` 获取访问权限）

.. code-block:: doscon

  C:\> git clone https://github.com/odoo/enterprise.git

.. note:: **企业版 git 仓库不包含完整的 Odoo 源代码**。它仅是一些额外插件的集合。运行企业版实际上意味着运行社区版的服务器，并将 addons-path 选项设置为企业版的文件夹。需要克隆社区版和企业版仓库才能获得可用的 Odoo 企业版安装。

准备
'''''''

Python
^^^^^^

Odoo 需要 Python 3.5 或更高版本才能运行。使用官方 `Python 3 安装程序
<https://www.python.org/downloads/windows/>`_ 下载并安装 Python 3。

安装过程中，勾选 **Add Python 3 to PATH**，然后点击 **Customize Installation** 并确保勾选 **pip**。

.. note:: 如果已经安装了 Python 3，请确保它是 3.5 或以上版本，因为以前的版本与 Odoo 不兼容。

          .. code-block:: doscon

              C:\> python3 --version

          还要验证 pip_ 是否已为该版本安装。

          .. code-block:: doscon

              C:\> pip3 --version

PostgreSQL
^^^^^^^^^^

Odoo 使用 PostgreSQL 作为数据库管理系统。下载并安装 `PostgreSQL 最新版本 <https://www.postgresql.org/download/windows/>`_。

默认情况下，唯一的用户是 `postgres`，但 Odoo 禁止以 `postgres` 身份连接，因此需要创建一个新的 PostgreSQL 用户：

#. 将 PostgreSQL 的 `bin` 目录（默认：`C:\\Program Files\\PostgreSQL\\<version>\\bin`）添加到 `PATH`。
#. 使用 pg admin gui 创建一个 PostgreSQL 用户并设置密码：

   * 打开 **pgAdminIII**。
   * 双击服务器以创建连接。
   * 选择 :menuselection:`编辑 --> 新建对象 --> 新建登录角色`。
   * 在 **角色名称** 字段中输入用户名（例如 `odoo`）。
   * 打开 **定义** 选项卡，输入密码（例如 `odoo`），然后点击 **确定**。

依赖项
^^^^^^^^^^^^

Odoo 依赖项列在 Odoo 社区目录根目录中的 `requirements.txt` 文件中。大多数依赖项可以通过 **pip** 安装。

.. tip:: 可能不希望在 Odoo 的不同实例之间或与系统混合使用 python 模块包。可以使用 virtualenv_ 创建隔离的 Python 环境。

导航到 Odoo 社区安装路径 (`YourOdooCommunityPath`) 并在依赖项文件上运行 **pip**：

.. code-block:: doscon

    C:\> cd \YourOdooCommunityPath
    C:\YourOdooCommunityPath> C:\Python35\Scripts\pip.exe install -r requirements.txt

.. warning:: 某些依赖项无法通过 pip 安装，需要手动安装。
             特别是：

             * `psycopg` 必须通过 `此安装程序 <http://www.stickpeople.com/projects/python/win-psycopg/>`_ 安装。
             * `wkhtmltopdf` 必须安装 `0.12.5 版本 <the wkhtmltopdf download page_>`_ 以支持页眉和页脚。有关各种版本的更多详细信息，请参见我们的 `wiki <https://github.com/odoo/odoo/wiki/Wkhtmltopdf>`_。

对于具有右到左界面的语言（如阿拉伯语或希伯来语），需要安装 `rtlcss` 包：

#. 下载并安装 `nodejs <https://nodejs.org/en/download/>`_。
#. 安装 `rtlcss`：

   .. code-block:: doscon

       C:\> npm install -g rtlcss

#. 编辑系统环境变量 `PATH`，将 `rtlcss.cmd` 所在的文件夹添加进去（通常位于：`C:\\Users\\<user>\\AppData\\Roaming\\npm\\`）。

运行 Odoo
''''''''''''

设置所有依赖项后，可以通过运行 `odoo-bin` 启动 Odoo，这是服务器的命令行界面。它位于 Odoo 社区目录的根目录中。

要配置服务器，可以指定 :ref:`命令行参数 <reference/cmdline/server>` 或 :ref:`配置文件 <reference/cmdline/config>`。

.. tip:: 对于企业版，必须将 `enterprise` 插件的路径添加到 `addons-path` 参数中。注意，它必须位于 `addons-path` 中其他路径之前，才能正确加载插件。

常见的必要配置是：

* PostgreSQL 用户名和密码。
* 默认路径之外的自定义插件路径，以加载自己的模块。

运行服务器的典型方式如下：

.. code-block:: doscon

    C:\YourOdooCommunityPath> python3 odoo-bin -r dbuser -w dbpassword --addons-path=addons -d mydb

其中 `YourOdooCommunityPath` 是 Odoo 社区安装的路径，`dbuser` 是 PostgreSQL 登录名，`dbpassword` 是 PostgreSQL 密码
`mydb` 是在 `localhost:8069` 上提供的默认数据库。可以在 addons-path 选项末尾添加其他目录路径，用逗号分隔。

Linux
-----

获取源代码
'''''''''''''''''

有两种方式获取 Odoo 源代码：作为 zip **压缩包** 或通过 **git**。

压缩包
^^^^^^^

社区版：

* `官方下载页面 <download_>`_
* `GitHub 仓库 <community-repository_>`_
* `夜间服务器 <nightly_>`_

企业版：

* `官方下载页面 <download_>`_
* `GitHub 仓库 <enterprise-repository_>`_

Git
^^^

以下操作需要在您的机器上安装 git_ 并且具备基本的 git 命令知识。

社区版：

.. code-block:: console

    $ git clone https://github.com/odoo/odoo.git


企业版：（请参阅 :ref:`setup/install/editions` 获取访问权限）

.. code-block:: console

  $ git clone https://github.com/odoo/enterprise.git

.. note:: **企业版 git 仓库不包含完整的 Odoo 源代码**。它仅是一些额外插件的集合。运行企业版实际上意味着运行社区版的服务器，并将 addons-path 选项设置为企业版的文件夹。需要克隆社区版和企业版仓库才能获得可用的 Odoo 企业版安装。

准备
'''''''

Python
^^^^^^

Odoo 需要 Python 3.5 或更高版本才能运行。使用包管理器下载并安装 Python 3（如果尚未安装）。

.. note:: 如果已经安装了 Python 3，请确保它是 3.5 或以上版本，因为以前的版本与 Odoo 不兼容。

          .. code-block:: console

              $ python3 --version

          还要验证 pip_ 是否已为该版本安装。

          .. code-block:: console

              $ pip3 --version

PostgreSQL
^^^^^^^^^^

Odoo 使用 PostgreSQL 作为数据库管理系统。使用包管理器下载并安装最新版本的 PostgreSQL。

默认情况下，唯一的用户是 `postgres`，但 Odoo 禁止以 `postgres` 身份连接，因此需要创建一个新的 PostgreSQL 用户：

.. code-block:: console

  $ sudo -u postgres createuser -s $USER
  $ createdb $USER

.. note:: 由于 PostgreSQL 用户与 Unix 登录名相同，因此可以无需密码连接到数据库。

依赖项
^^^^^^^^^^^^

Odoo 依赖项列在 Odoo 社区目录根目录中的 `requirements.txt` 文件中。大多数依赖项可以通过 **pip** 安装。

.. tip:: 可能不希望在 Odoo 的不同实例之间或与系统混合使用 python 模块包。可以使用 virtualenv_ 创建隔离的 Python 环境。

导航到 Odoo 社区安装路径 (`YourOdooCommunityPath`) 并在依赖项文件上运行 **pip**：

.. code-block:: console

    $ cd /YourOdooCommunityPath
    /YourOdooCommunityPath$ pip3 install -

r requirements.txt

.. warning:: 对于使用本地代码的库（Pillow、lxml、greenlet、gevent、psycopg2、ldap），可能需要在 pip 安装依赖项之前安装开发工具和本地依赖项。这些工具和依赖项可在 Python、PostgreSQL、libxml2、libxslt、libevent、libsasl2 和 libldap2 的 `-dev` 或 `-devel` 包中找到。

.. warning:: 某些依赖项无法通过 pip 安装，需要手动安装。
             特别是：

             * `wkhtmltopdf` 必须安装 `0.12.5 版本 <the wkhtmltopdf download page_>`_ 以支持页眉和页脚。有关各种版本的更多详细信息，请参见我们的 `wiki <https://github.com/odoo/odoo/wiki/Wkhtmltopdf>`_。

对于具有右到左界面的语言（如阿拉伯语或希伯来语），需要安装 `rtlcss` 包：

#. 使用包管理器下载并安装 **nodejs** 和 **npm**。
#. 安装 `rtlcss`：

   .. code-block:: console

       $ sudo npm install -g rtlcss

运行 Odoo
''''''''''''

设置所有依赖项后，可以通过运行 `odoo-bin` 启动 Odoo，这是服务器的命令行界面。它位于 Odoo 社区目录的根目录中。

要配置服务器，可以指定 :ref:`命令行参数 <reference/cmdline/server>` 或 :ref:`配置文件 <reference/cmdline/config>`。

.. tip:: 对于企业版，必须将 `enterprise` 插件的路径添加到 `addons-path` 参数中。注意，它必须位于 `addons-path` 中其他路径之前，才能正确加载插件。

常见的必要配置是：

* PostgreSQL 用户名和密码。Odoo 没有默认设置，除了
  `psycopg2 的默认值 <http://initd.org/psycopg/docs/module.html>`_：通过端口 `5432` 的 UNIX 套接字连接，使用当前用户且无密码。
* 默认路径之外的自定义插件路径，以加载自己的模块。

运行服务器的典型方式如下：

.. code-block:: console

    /YourOdooCommunityPath$ python3 odoo-bin --addons-path=addons -d mydb

其中 `YourOdooCommunityPath` 是 Odoo 社区安装的路径，
`mydb` 是在 `localhost:8069` 上提供的默认数据库。可以在 addons-path 选项末尾添加其他目录路径，用逗号分隔。

Mac OS
------

获取源代码
'''''''''''''''''

有两种方式获取 Odoo 源代码：作为 zip **压缩包** 或通过 **git**。

压缩包
^^^^^^^

社区版：

* `官方下载页面 <download_>`_
* `GitHub 仓库 <community-repository_>`_
* `夜间服务器 <nightly_>`_

企业版：

* `官方下载页面 <download_>`_
* `GitHub 仓库 <enterprise-repository_>`_

Git
^^^

以下操作需要在您的机器上安装 git_ 并且具备基本的 git 命令知识。

社区版：

.. code-block:: console

    $ git clone https://github.com/odoo/odoo.git


企业版：（请参阅 :ref:`setup/install/editions` 获取访问权限）

.. code-block:: console

  $ git clone https://github.com/odoo/enterprise.git

.. note:: **企业版 git 仓库不包含完整的 Odoo 源代码**。它仅是一些额外插件的集合。运行企业版实际上意味着运行社区版的服务器，并将 addons-path 选项设置为企业版的文件夹。需要克隆社区版和企业版仓库才能获得可用的 Odoo 企业版安装。

准备
'''''''

Python
^^^^^^

Odoo 需要 Python 3.5 或更高版本才能运行。使用首选的包管理器（homebrew_, macports_）下载并安装 Python 3（如果尚未安装）。

.. note:: 如果已经安装了 Python 3，请确保它是 3.5 或以上版本，因为以前的版本与 Odoo 不兼容。

          .. code-block:: console

              $ python3 --version

          还要验证 pip_ 是否已为该版本安装。

          .. code-block:: console

              $ pip3 --version

PostgreSQL
^^^^^^^^^^

Odoo 使用 PostgreSQL 作为数据库管理系统。使用 `postgres.app <https://postgresapp.com>`_
下载并安装最新版本的 PostgreSQL。

默认情况下，唯一的用户是 `postgres`，但 Odoo 禁止以 `postgres` 身份连接，因此需要创建一个新的 PostgreSQL 用户：

.. code-block:: console

  $ sudo -u postgres createuser -s $USER
  $ createdb $USER

.. note:: 由于 PostgreSQL 用户与 Unix 登录名相同，因此可以无需密码连接到数据库。

依赖项
^^^^^^^^^^^^

Odoo 依赖项列在 Odoo 社区目录根目录中的 `requirements.txt` 文件中。大多数依赖项可以通过 **pip** 安装。

.. tip:: 可能不希望在 Odoo 的不同实例之间或与系统混合使用 python 模块包。可以使用 virtualenv_ 创建隔离的 Python 环境。

导航到 Odoo 社区安装路径 (`YourOdooCommunityPath`) 并在依赖项文件上运行 **pip**：

.. code-block:: console

   $ cd /YourOdooCommunityPath
   /YourOdooCommunityPath$ pip3 install -r requirements.txt

.. warning:: 非 Python 依赖项需要通过包管理器安装：

             #. 下载并安装 **命令行工具**：

                .. code-block:: console

                   $ xcode-select --install

             #. 下载并安装首选的包管理器（homebrew_, macports_）。
             #. 安装非 Python 依赖项。

.. warning:: 某些依赖项无法通过 pip 安装，需要手动安装。
             特别是：

             * `wkhtmltopdf` 必须安装 `0.12.5 版本 <the wkhtmltopdf download page_>`_ 以支持页眉和页脚。有关各种版本的更多详细信息，请参见我们的 `wiki <https://github.com/odoo/odoo/wiki/Wkhtmltopdf>`_。

对于具有右到左界面的语言（如阿拉伯语或希伯来语），需要安装 `rtlcss` 包：

#. 使用首选的包管理器（homebrew_, macports_）下载并安装 **nodejs**。
#. 安装 `rtlcss`：

   .. code-block:: console

       $ sudo npm install -g rtlcss


.. _setup/install/docker:

Docker
======

有关如何使用 Docker 使用 Odoo 的完整文档，请参见官方 Odoo `docker 镜像 <https://registry.hub.docker.com/_/odoo/>`_ 页面。

.. _Debian Stretch: https://www.debian.org/releases/stretch/
.. _demo: https://demo.odoo.com
.. _docker: https://www.docker.com
.. _download: https://www.odoo.com/page/download
.. _Ubuntu 18.04: http://releases.ubuntu.com/18.04/
.. _EPEL: https://fedoraproject.org/wiki/EPEL
.. _PostgreSQL: http://www.postgresql.org
.. _the official installer:
.. _install pip:
    https://pip.pypa.io/en/latest/installing.html#install-pip
.. _Quilt: http://en.wikipedia.org/wiki/Quilt_(software)
.. _saas: https://www.odoo.com/page/start
.. _the wkhtmltopdf download page: https://github.com/wkhtmltopdf/wkhtmltopdf/releases/tag/0.12.5
.. _UAC: http://en.wikipedia.org/wiki/User_Account_Control
.. _wkhtmltopdf: http://wkhtmltopdf.org
.. _pip: https://pip.pypa.io
.. _macports: https://www.macports.org
.. _homebrew: http://brew.sh
.. _wheels: https://wheel.readthedocs.org/en/latest/
.. _virtualenv: https://pypi.python.org/pypi/virtualenv
.. _virtualenvwrapper: https://virtualenvwrapper.readthedocs.io/en/latest/
.. _pywin32: http://sourceforge.net/projects/pywin32/files/pywin32/
.. _community-repository: https://github.com/odoo/odoo
.. _enterprise-repository: https://github.com/odoo/enterprise
.. _git: https://git-scm.com/
.. _Editions: https://www.odoo.com/pricing#pricing_table_features
.. _nightly: https://nightly.odoo.com/
.. _extra: https://nightly.odoo.com/extra/
