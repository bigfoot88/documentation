=========================================
如何管理大量相同产品？
=========================================

概述
========

批次管理对于接收大量产品并且批号可以帮助报告、质量控制或任何其他信息时非常有用。批次管理可以帮助识别存在生产缺陷的多个产品。这在衣物或食品的批量生产中非常有用。

Odoo 具有管理批次的能力，确保符合大多数行业要求的可追溯性要求。

Odoo 的双重条目管理使您能够运行非常高级的可追溯性。

设置
==========

应用配置
-------------------------

您需要在设置中激活批次跟踪。在 **Inventory** 应用中，前往 :menuselection:`Configuration --> Settings`，选择 **Track lots or serial numbers**。

.. image:: media/lots01.png
    :align: center

为了对批次进行高级管理，您还应选择 **Manage several locations per warehouse**。

.. image:: media/lots02.png
    :align: center

然后点击 **Apply**。

操作类型配置
-----------------------------

您还需要设置如何为每个操作管理批次。在 **Inventory** 应用中，前往 :menuselection:`Configuration --> Operation Types`。

对于每种类型（接收、内部转移、交付等），您可以设置是否可以创建新批次号或仅使用现有批次号。

.. image:: media/lots03.png
    :align: center

产品配置
---------------------

最后，您必须配置要在批次中跟踪的产品。

进入 :menuselection:`Inventory Control --> Products`，打开您选择的产品。点击 **Edit**，在 **Inventory** 选项卡中选择 **Tracking by Lots**，然后点击 **Save**。

.. image:: media/lots04.png
    :align: center

管理批次
===========

转移
---------

为了处理跟踪批次的产品转移，您必须输入批次号。

点击批次图标：

.. image:: media/lots05.png
    :align: center

将弹出一个窗口。点击 **Add an item** 并填写批次号和数量。

.. image:: media/lots06.png
    :align: center

根据您的操作类型配置，您将能够填写新批次号或仅使用现有批次号。

.. note::
    在扫描器界面中，您只需扫描批次号。

库存调整
--------------------

可以通过两种方式进行跟踪批次产品的库存：

-  按产品进行经典库存

-  按批次进行库存

进行经典库存时，有一个 **Serial Number** 列。如果产品已经分配了编号，则该列已预填充。

如果产品尚未入库，点击 **Add an item**。您可以轻松创建批次，只需在列中输入一个新批次号。

.. image:: media/lots07.png
    :align: center

您也可以仅对批次进行库存。在这种情况下，您需要填写 **Lot number**。您还可以在这里创建一个新批次。只需输入编号，一个窗口将弹出以将编号链接到产品。

.. image:: media/lots08.png
    :align: center

批次可追溯性
=================

您可以从 :menuselection:`Inventory --> Inventory Control --> Serial Numbers/Lots` 检查批次可追溯性。

.. image:: media/lots09.png
    :align: center

您可以通过点击 **Traceability** 按钮获取更多详细信息：

.. image:: media/lots10.png
    :align: center

.. seealso::
    * :doc:`differences`
    * :doc:`serial_numbers`
