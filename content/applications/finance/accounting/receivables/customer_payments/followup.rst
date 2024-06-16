=========================================
催收发票并加快收款速度
=========================================

当您的业务账款逾期时，收款是至关重要的。Odoo 可以帮助您识别逾期的款项，并允许您发送适当的提醒。

管理您的催收
======================

.. tip::
    我们建议在启动催收流程之前先对您的银行对账单进行核对。这样可以避免向已经付款的客户发送催收通知。

默认情况下，您需要跟进的逾期发票可在 :menuselection:`Accounting --> Sales --> Follow-up Reports` 中找到。在这里，您可以轻松地通过电子邮件发送提醒或打印为信件。然后，您可以点击 *Done* 按钮查看下一个需要您关注的催收。

如果现在不是发送提醒的合适时间，点击 *Remind me later*。您将在声明中设置的 *Next Reminder Date* 后收到下一次报告。

.. tip::
    为避免在短时间内发送过多的提醒，您可以通过 :menuselection:`Accounting --> Configuration --> Settings --> Payment Follow-up` 调整每次报告之间的天数。

您还可以通过在催收报告中将客户标记为差、普通或良好债务人来设置客户的信任级别。

.. image:: media/followup01.png
    :align: center

批量发送提醒
=======================

为了简化催收流程，您可以从催收报告页面批量发送提醒电子邮件。选择所有您想处理的报告，点击 *Action*，然后点击 *Process Follow-ups*。系统还会自动生成包含所有催收信件的 PDF 文件，供您打印。

.. image:: media/followup02.png
    :align: center

计划催收流程
========================

要计划催收流程，请前往 :menuselection:`Accounting --> Configuration --> Settings` 并在 *Customer Payments* 部分激活 *Follow-up Levels* 功能。然后，点击设置页面上出现的新 *Follow-up Levels* 按钮。

Odoo 默认提供一个包含多个动作的催收计划，您可以根据需要进行自定义。根据特定的逾期天数，计划发送电子邮件、信件或进行手动操作。您还可以根据流程阶段编辑声明使用的模板。

.. image:: media/followup03.png
    :align: center

.. tip::
    如果您希望在实际到期日前收到提醒，请设置一个负数的逾期天数。
