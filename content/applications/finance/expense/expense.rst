========
费用报销
========

如何设置费用类型
================

跟踪费用的第一步是从 *Configuration* 菜单配置公司允许的费用类型（在 Odoo 中管理为产品）。当某一具体费用按固定价格报销时，在产品上设置成本。否则，将成本保持为 0.0，员工将报告每笔费用的实际成本。

.. image:: ./media/expense_product.png
   :align: center

以下是一些配置示例：

* 餐厅：

  * 成本: 0.00 (每次费用将记录票据的成本)
* 私家车旅行：

  * 成本: 0.30 (公司固定报销的每英里价格)
* 酒店：

  * 成本: 0.00 (每次费用将记录票据的成本)

* 其他：

  * 成本: 0.0

不要忘记在每种费用类型上设置费用税（如果您使用 Odoo 会计，还要设置一个账户）。通常，使用配置为 *Tax Included in Price* 的税种是一个好习惯（参见: :doc:`../accounting/others/taxes/tax_included`）。这样，员工报告的费用将包括税款，这是通常的期望行为。

.. tip:: 
    *Sales* 应用允许您为费用类型指定计量单位（单位、英里、夜晚等）。前往 :menuselection:`Sales --> Configuration --> Settings` 并勾选 *Some products may be sold/purchased in different units of measure (advanced)*。

如何记录费用
============

手动记录
--------

作为员工（拥有员工用户访问权限），您可以从 :menuselection:`My Expenses --> Expenses to Submit` 记录费用。

.. image:: ./media/expense_submit_01.png
   :align: center

1. 选择相关产品并输入您支付的总金额（数量 = 1）或如果数量可计数（例如酒店晚数），则输入单价。
2. 输入费用日期。
3. 选择您是否自行支付账单（并期望报销）或公司直接支付（例如，使用公司的信用卡）。
4. 设置账单参考，添加一些备注（如有要求）并在讨论线程中附上一张收据的照片/扫描件。这将有助于经理和会计师验证它。

.. image:: ./media/expense_submit_02.png
   :align: center

通过电子邮件一键记录
--------------------
让您的员工通过简单的电子邮件记录他们的费用。拍摄收据快照并通过电子邮件发送，或简单地转发账单！

唯一需要做的是在 :menuselection:`Expenses --> Configuration --> Settings` 设置一个电子邮件别名（例如 *expenses* @mycompany.odoo.com）。出于安全目的，仅接受经过身份验证的员工电子邮件（参见员工详细表单中的 *Work Email*）。

.. tip::
    如果邮件主题包含产品的内部参考（例如 [Food]），则费用产品会自动设置。将费用金额输入邮件主题也可将其设置在费用上。

如何提交费用给经理
====================

当您准备好将费用提交给经理时（例如在商务旅行结束时，或每月一次），前往 :menuselection:`My Expenses --> Expenses to Submit` 菜单。从列表视图中选择所有费用，然后点击 :menuselection:`Action > Submit to Manager`。保存新创建的费用报告（即费用集），并等待经理批准。

.. image:: ./media/expense_submit_03.png
   :align: center

您也可以从费用表单视图中的 *Submit to Manager* 按钮逐个提交费用。

您提交的所有费用报告可以在 :menuselection:`Expenses --> My Expenses --> Expense Reports` 中找到。

如何批准费用
============

HR 和团队经理可以从顶部菜单 :menuselection:`To Approve --> Expense Reports to Approve` 查看所有待验证的费用报告。这些用户必须至少拥有 *Expenses* 的 *Officers* 访问权限。

.. image:: ./media/expense_approval_01.png
   :align: center

他们可以审查费用报告，批准或拒绝它们，并通过集成的通信工具提供反馈。

.. image:: ./media/expense_approval_02.png
   :align: center

作为团队经理，您可以轻松找到团队成员的费用报告。您需要在这些员工的详细表单中设置为经理。

.. image:: ./media/expense_approval_03.png
   :align: center

如何在会计中记录费用
======================

经理批准费用报告后，会计部门前往 :menuselection:`Expenses --> Accountant --> Expense Reports To Post` 检查账户、产品和税金。他们可以点击 *Post Journal Entries* 将相关的日记帐分录记录到您的账簿中。为此，用户必须拥有以下访问权限：

* 会计: Accountant 或 Adviser
* 费用: Manager

.. note::
    要记录费用，必须在员工的 *Home Address* 上设置地址。如果在记录时收到相关的阻止消息，请点击员工，进入 *Personal Information* 选项卡并在地址簿中选择/创建该员工的联系人。如果此人正在使用 Odoo，则会自动创建一个联系人。

如何报销员工
==============

您现在可以在 :menuselection:`Expenses --> Accountant --> Expense Reports To Pay` 中看到所有待报销的费用报告。要记录付款或通过支票支付，请点击 *Register a Payment*。

参见如何在 Odoo 中轻松管理付款流程：

* :doc:`../accounting/payables/pay/check`
* :doc:`../accounting/payables/pay/sepa`

如何向客户重新开票费用
==========================

如果您跟踪客户项目上的费用，您可以自动将其回收费给客户。

设置
----

- 在费用设置中启用 **Customer Billing**

- 前往产品配置菜单并在所有费用类型上设置开票方式：

   -  订单数量: 将根据订购数量开票费用

   -  交付数量: 将根据费用数量开票

   -  成本价: 将按实际成本开票费用。

   -  销售价格: 将根据销售订单中设置的固定销售价格开票。

.. image:: media/expense_invoicing_01.png
  :align: center

创建订单
--------

- 作为销售人员，为提供给客户的服务创建并确认销售订单。如果订单中没有任何费用，一旦会计发布，它将自动添加。

- 将费用链接到销售订单。

.. image:: media/expense_invoicing_02.png
  :align: center

提交、验证和记录费用
----------------------

- 作为经理，在批准费用报告时确保每行费用都设置了分析账户。如果缺少，请点击该行添加。员工在提交时已经可以设置。

.. image:: media/expense_invoicing_03.png
  :align: center

- 作为会计，记录日记帐分录。

开票费用
--------

现在您可以开票订单。它显示在 :menuselection:`Sales --> Invoicing --> Sales to Invoice` 中。费用已自动添加到订单行中。此类项目显示为蓝色（即待开票）。

.. image:: media/expense_invoicing_04.png
  :align: center
