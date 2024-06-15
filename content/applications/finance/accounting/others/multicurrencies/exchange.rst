=================================
记录支付时的汇率
=================================

概述
========

任何从事国际贸易的公司都会遇到支付时使用不同货币的情况。

在收到付款后，可以选择将款项转换为公司的基础货币。多货币支付意味着汇率波动。Odoo 会自动记录汇率差异。

配置
=============

启用多货币
-----------------------

在会计模块中，进入 :menuselection:`Configuration --> Settings`，勾选 **Allow multi currencies**，然后点击 **apply**。

.. image:: media/exchange_rate03.png
   :align: center

在 :menuselection:`Configuration --> Currencies` 中配置汇率。填写汇率并确保货币是激活状态。

.. image:: media/exchange_rate02.png
   :align: center

在本文档中，基础货币是 **欧元**，我们将记录 **美元** 的付款。

.. image:: media/exchange_rate08.png
   :align: center

.. tip:: 
    你可以从 **欧洲中央银行** 或 **Yahoo** 自动获取汇率。请阅读文档 : :doc:`how_it_works`.

配置日记账
----------------------

为了在其他货币中登记付款，你需要**移除日记账上的货币限制**。进入会计应用，点击日记账上的 **More** 和 **Settings**。

.. image:: media/exchange_rate06.png
   :align: center

检查 **Currency** 字段是否为空或是你将要登记付款的外币。如果填写了货币，意味着你只能在此货币中登记付款。

.. image:: media/exchange_rate10.png
   :align: center

记录不同货币的付款
========================================

在 **会计** 应用中，进入 :menuselection:`Sales --> Payments`。登记付款并指明是用外币支付的，然后点击 **confirm**。

.. image:: media/exchange_rate05.png
   :align: center

日记账分录已经发布但未分配。

返回到你的发票 (:menuselection:`Sales --> Customer Invoices`)，点击 **Add** 以分配付款。

.. image:: media/exchange_rate04.png
   :align: center

记录不同货币的银行对账单
===============================================

创建或导入付款的银行对账单。**Amount** 是公司货币的金额。有两个补充字段，**Amount currency** 是实际支付的金额，**Currency** 是支付使用的货币。

.. image:: media/exchange_rate07.png
   :align: center

在对账时，Odoo 会直接匹配付款和正确的 **Invoice**。你会看到发票货币中的发票价格和公司货币中的金额。

.. image:: media/exchange_rate09.png
   :align: center

检查汇率差异
===================================

进入 :menuselection:`Adviser --> Journal Entries` 并查找 **Exchange difference** 日记账分录。所有的汇率差异都记录在其中。

.. image:: media/exchange_rate01.png
   :align: center

.. tip::
    可以在会计设置中更改汇率差异日记账。

.. include:: full_reconcile_warning.rst

.. seealso::
    * :doc:`../../bank/reconciliation/configure`
    * :doc:`../../bank/reconciliation/use_cases`
