==========================================
导入 CODA 对账单文件（仅限比利时）
==========================================

CODA 是一种比利时银行对账单的文件格式。大多数比利时银行以及 Isabel 软件允许下载包含所有银行对账单的 CODA 文件。

在 Odoo 中，你可以从银行或会计软件中下载 CODA 文件，并直接导入到 Odoo 中。这将创建所有的银行对账单。

.. tip:: 
    现在就通过 `这个示例 CODA 文件 <https://drive.google.com/file/d/0B5BDHVRYo-q5UVVMbGRxUmtpVDg/view?usp=sharing>`__ 测试该功能。

配置
=============

安装 CODA 功能
------------------------

如果你已经安装了 Odoo 提供的比利时会计科目表，CODA 导入功能已默认安装。在这种情况下，你可以直接跳到下一节 `导入你的第一个 CODA 文件 <InstallCoda_>`_

如果 CODA 功能尚未激活，你需要先进行激活。在会计应用中，进入菜单 :menuselection:`Configuration --> Settings`。在会计设置中，勾选 **Import of Bank Statements in .CODA Format** 选项并应用。

导入你的第一个 CODA 文件
---------------------------

一旦安装了此功能，你可以设置你的银行账户以允许导入银行对账单文件。为此，请进入会计 **Dashboard**，点击银行账户卡上的 **More** 按钮。然后，点击 **Import Statement** 加载你的第一个 CODA 文件。

.. image:: media/coda01.png
   :align: center

在以下屏幕中加载你的 CODA 文件并点击 **Import** 以创建所有的银行对账单。

.. image:: media/coda02.png
   :align: center

如果文件成功加载，你将被重定向到银行对账页面，其中包含所有待对账的交易。

.. _InstallCoda:

导入 CODA 文件
====================

在导入你的第一个文件后，Odoo 会计仪表板将自动建议你为你的银行导入更多文件。对于下一次导入，你不再需要点击 **More** 按钮，可以直接点击 **Import Statement** 链接。

.. image:: media/coda03.png
   :align: center

每次你收到与新客户/供应商相关的对账单时，Odoo 会要求你选择正确的联系人以对账。Odoo 会从该操作中学习，并在下次收到或付款给这些联系人时自动完成。这将大大加快对账过程。

.. note::
    Odoo 能够自动检测某些文件或交易是否已被导入。因此，你无需担心避免重复导入同一文件：Odoo 会在创建新的银行对账单之前为你检查一切。

.. seealso::
    * :doc:`ofx`
    * :doc:`qif`
    * :doc:`synchronize`
    * :doc:`manual`
