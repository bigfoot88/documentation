==========================
导入 OFX 对账单文件
==========================

开放财务交换（OFX）是一种统一规范，用于在金融机构、企业和消费者之间通过互联网电子交换财务数据。

通过 Odoo，您可以从银行或会计软件中下载 OFX 文件，并直接导入您的 Odoo 实例。这将创建所有的银行对账单。

.. tip::

    现在就用 `这个示例 OFX 文件 <https://drive.google.com/file/d/0B5BDHVRYo-q5Mmg4T3oxTWszeEk/view>`__ 来测试该功能。

配置
=============

为了导入 OFX 对账单，您需要在 Odoo 中激活此功能。在会计应用中，进入菜单 :menuselection:`Configuration --> Settings`。在会计设置中，勾选 **Import in .OFX Format** 银行对账单选项并应用。

.. image:: media/ofx01.png
   :align: center

安装此功能后，您可以设置您的银行账户以允许导入银行对账单文件。为此，请转到会计仪表板，点击银行账户的 **More** 按钮。然后，点击 **Import Statement** 以加载您的第一个 OFX 文件。

.. image:: media/ofx02.png
   :align: center

在以下屏幕中加载您的 OFX 文件并点击 **Import** 以创建所有的银行对账单。

.. image:: media/ofx03.png
   :align: center

如果文件成功加载，您将被重定向到银行对账屏幕，所有需要对账的交易都会显示在此。

导入 OFX 文件
===================

在导入第一个文件后，Odoo 会计仪表板会自动建议您为银行导入更多文件。对于下次导入，您不再需要进入 **More** 菜单，可以直接点击 **Import Statement** 链接。

.. image:: media/ofx04.png
   :align: center

每次您获得与新客户或供应商相关的对账单时，Odoo 会要求您选择正确的联系人以对账该交易。Odoo 会从这个操作中学习，并在您下次收到或进行这些联系人的付款时自动完成。这将大大加快对账过程。

.. seealso::

    * :doc:`qif`
    * :doc:`coda`
    * :doc:`synchronize`
    * :doc:`manual`
