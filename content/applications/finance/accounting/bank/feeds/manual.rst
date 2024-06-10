=================================
手动登记银行对账单
=================================

概述
========

使用 Odoo，您可以导入银行对账单，与银行同步，但也可以手动登记银行对账单。

配置
=============

登记发票不需要特殊配置。您只需安装会计应用程序即可。

.. image:: media/manual01.png
   :align: center

手动登记银行对账单
=================================

创建银行对账单
---------------------------

在仪表板中，点击与银行日记账相关的 **新对账单** 按钮。如果需要进行一些对账操作，新的对账单链接会显示在下方。

.. image:: media/manual02.png
   :align: center

根据银行对账单上的信息填写字段。参考可以手动填写，也可以留空。我们建议填写合作伙伴，以简化对账过程。

起始余额和期末余额之间的差额应等于计算出的余额。

.. image:: media/manual03.png
   :align: center

完成后，点击 **保存**。

对账银行对账单
------------------------------

您可以选择直接通过点击 |manual04| 按钮进行对账。

.. |manual04| image:: media/manual04.png

您也可以从仪表板开始对账过程，点击 **对账 # 项目**。

.. image:: media/manual05.png
   :align: center

点击 **验证** 对银行对账单进行对账。如果缺少合作伙伴，Odoo 会要求您 **选择合作伙伴**。

.. image:: media/manual06.png
   :align: center

.. tip::

		按 CTRL-Enter 对表格中的所有平衡项进行对账。

从对账中关闭银行对账单
---------------------------------------------

如果余额正确，您可以直接点击 |manual07| 从对账中关闭对账单。

.. |manual07| image:: media/manual07.png

否则，点击 |manual08| 打开对账单并纠正问题。

.. |manual08| image:: media/manual08.png

关闭银行对账单
---------------------

在会计仪表板上，点击您的银行日记账的更多按钮，然后点击银行对账单。

.. image:: media/manual09.png
   :align: center

要关闭银行对账单，只需点击 **验证**。

.. image:: media/manual10.png
   :align: center

.. seealso::

	* :doc:`../reconciliation/use_cases`
	* :doc:`../feeds/synchronize`
