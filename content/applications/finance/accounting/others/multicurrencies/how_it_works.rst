=====================================
Odoo 的多币种功能如何运作？
=====================================

概述
========

选择在 Odoo 中使用多币种选项将允许您发送销售发票、报价单和采购订单或接收账单和付款时使用不同于您自己的货币。通过多币种功能，您还可以设置其他货币的银行账户并运行有关外币活动的报告。

配置
=============

开启多币种功能
----------------------

在会计模块中，前往 :menuselection:`Configuration --> Settings`，勾选 **Allow multi currencies**，然后点击 **Apply**。

.. image:: media/works01.png
   :align: center

汇率差异日记账
---------------------

**Rate Difference Journal** 记录付款登记和预期金额之间的差异。例如，如果付款在发票开具后 1 个月才支付，汇率可能已经发生变化。波动带来的一些损失或收益将由 Odoo 记录。

您可以在设置中进行更改：

.. image:: media/works02.png
   :align: center

查看或编辑使用的汇率
----------------------------

您可以在 :menuselection:`Configuration --> Currencies` 中手动配置货币汇率。打开您想在 Odoo 中使用的货币并进行编辑。确保货币处于激活状态。

.. image:: media/works03.png
   :align: center

点击 **View Rates** 进行编辑并查看历史记录：

.. image:: media/works04.png
   :align: center

点击 **Create** 添加汇率。填写日期和汇率。完成后点击 **Save**。

.. image:: media/works05.png
   :align: center

实时汇率
------------------

默认情况下，汇率需要手动更新。但您可以与 `Yahoo <https://finance.yahoo.com/currency-converter/>`__ 或 `European Central Bank <http://www.ecb.europa.eu>`__ 同步。在 :menuselection:`Configuration --> Settings` 中，前往 **Live Currency Rate** 部分。

选择间隔：手动、每日、每周或每月。您可以随时通过点击 **Update Now** 强制更新。选择供应商，然后设置完成！

.. image:: media/works06.png
   :align: center

.. note::

	只有 **active** 的货币会被更新

配置您的科目表
--------------------------------

在会计应用中，前往 :menuselection:`Adviser --> Charts of Accounts`。在每个账户上，您可以设置一种货币。这将强制该账户的所有交易使用该货币。

如果将其留空，这意味着它可以处理所有已激活的货币。

.. image:: media/works07.png
   :align: center

配置您的日记账
-----------------------

为了在其他货币中登记付款，您必须删除日记账上的货币限制。前往会计应用，点击日记账上的 **More** 和 **Settings**。

.. image:: media/works08.png
   :align: center

检查货币字段是否为空或是您将登记付款的外币。如果填写了某种货币，这意味着您只能在该货币中登记付款。

.. image:: media/works09.png
   :align: center

Odoo 的多币种功能如何运作？
=====================================

现在您正在多币种环境中工作，所有可计账项目将与某种货币（国内或外币）关联。

销售订单和发票
-------------------------

您现在可以在销售订单和发票上设置与公司不同的货币。货币为整份文件设置。

.. image:: media/works10.png
   :align: center

采购订单和供应商账单
---------------------------------

您现在可以在采购订单和供应商账单上设置与公司不同的货币。货币为整份文件设置。

.. image:: media/works11.png
   :align: center

付款登记
---------------------

在会计应用中，前往 **Sales > Payments**。登记付款并设置货币。

.. image:: media/works12.png
   :align: center

银行对账单
---------------

创建或导入银行对账单时，金额以公司货币计。但现在有两个补充字段，即实际支付的金额和支付时使用的货币。

.. image:: media/works13.png
   :align: center

对账时，Odoo 会直接将付款与正确的发票匹配。您将获得发票货币的发票价格和公司货币的金额。

汇率差异日记账
---------------------

前往 :menuselection:`Adviser --> Journal Entries`，查找汇率差异日记账条目。所有汇率差异都记录在其中。

.. image:: media/works14.png
   :align: center

.. include:: full_reconcile_warning.rst

.. seealso::

	* :doc:`invoices_payments`
	* :doc:`exchange`
