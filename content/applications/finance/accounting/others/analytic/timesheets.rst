======================================================
如何通过工时表追踪人力资源成本？
======================================================

人力资源当然有成本。了解某个特定合同在人工方面对公司成本的影响，并将其与开票金额进行比较，是非常有趣的。

我们将以下面的例子为例：我们的两位员工 **Harry Potter** 和 **Cedric Digory** 都在为客户 **Smith&Co** 工作，进行**咨询服务包**项目。Harry 的工资是 18€ 每小时，Cedric 的工资是 12€ 每小时。我们希望在会计应用中跟踪他们的工时成本，并将其与咨询服务的收入进行比较。

配置
=============

首先，安装使用该功能所需的三个应用，即 **Accounting**，**Sales** 和 **Timesheet**。输入应用模块名称并安装它们。

.. image:: media/timesheets14.png  
   :align: center

.. image:: media/timesheets05.png
   :align: center

.. image:: media/timesheets11.png
   :align: center

接下来，您需要启用分析会计功能。进入 **Accounting** 应用，选择 :menuselection:`Configuration --> Settings` 并勾选 **Analytic accounting** 选项（见下图）。

.. image:: media/timesheets06.png
   :align: center

应用您的更改。

创建员工
------------------

为了检查员工的收入，您需要创建一个员工。进入 **Employee** 应用，选择 **Employees** 并创建一个新员工，填写姓名和基本信息。

在员工信息页面进入 **HR settings** 标签。在这里，您可以指定员工的 **Timesheet Cost**。在这个例子中，Harry 的成本是 18 欧元/小时。因此我们在此字段中填写 18。

.. image:: media/timesheets07.png
   :align: center

.. note:: 
    如果您希望员工能够输入工时表，他需要关联到一个用户。

重复操作创建 Cedric Digory 员工。不要忘记指定其相关用户和 **Timesheet Costs**。

创建销售订单 
--------------------

我们在 **Employee** 应用中创建了两位员工，分别是 Harry Potter 和 Cedric Diggory。他们将在为客户 Smith&Co 提供的咨询合同中记录工时。

因此，我们需要创建一个 **sales order**，其中包含一个 **service** 产品，按照 **time and material** 基础进行开票，并通过工时表以 **hours** 为单位进行跟踪。

.. image:: media/timesheets03.png
   :align: center

有关如何基于时间和材料创建销售订单的更多信息，请参见：*How to invoice based on time and material*（正在进行中）。

.. todo::
    添加链接，该文档在 
    Sales --> Invoicing Methods --> Services --> How to invoices blabla

我们保存包含服务产品 **External Consulting** 的销售订单。一旦确认 **Sales Order**，系统将自动生成一个分析账户。我们的员工必须指向该账户（在此情况下为 **SO002-Smith&Co**）才能记录他们的工时（见下图）。

.. image:: media/timesheets10.png
   :align: center

填写工时表
-----------------

作为关联到用户的员工，Harry 可以进入 **Timesheet** 应用并为合同填写工时表。登录 Harry 的账户，我们进入 **Timesheet** 应用并填写指向前面讨论的 **Analytical Account** 的详细记录。

Harry 为 Smith&Co 的 SWOT 分析工作了三小时。

.. image:: media/timesheets01.png
   :align: center

同时，Cedric 与客户讨论业务需求花费了 1 小时，并在其个人工时表中记录了这些内容，同样指向 **Analytic Account**。

在 **Sales Order** 中，我们注意到已交付的工时数量自动计算出来了（见下图）。

.. image:: media/timesheets02.png
   :align: center

分析会计
-------------------

通过分析账户，我们能够概览人力资源成本和收入。这些交易的所有收入和成本都记录在 **SO002-Smith&Co** 账户中。

我们可以使用两种方法来分析这种情况。

不使用过滤器
~~~~~~~~~~~~~~~

如果我们将项目的所有成本和收入都指向正确的分析账户，我们可以轻松检索与该分析账户相关的成本和收入。进入 *Accounting* 应用，选择 :menuselection:`Adviser --> Analytic Accounts --> Open Charts`。

注意：您可以为 **Analysis** 指定一个时期。如果您希望查看当前情况，则应保持这些字段为空。我们已经可以看到账户的借方和贷方余额。

.. image:: media/timesheets12.png
   :align: center

如果我们点击账户，系统提供一个特殊按钮来查看成本和收入的详细信息（见下图）。

.. image:: media/timesheets13.png
   :align: center

点击 **Cost/Revenue** 按钮以查看带有相应描述的成本和收入概览。

使用过滤器
~~~~~~~~~~~~

我们可以从 **Analytic Entries** 中过滤这些信息。

进入 **Accounting** 应用，然后点击 :menuselection:`Adviser --> Analytic Entries`。
在此菜单中，我们有几种选项可以分析人力资源成本。

1. 我们可以根据 **Analytic account** 过滤，以查看项目的成本和收入。添加一个自定义 **Filter**，其中 **Analytic Account** 包含 **Sales Order** 号码。

   .. image:: media/timesheets04.png
      :align: center

   在结果中，我们可以看到工时表活动和已开票的记录，带有相应的成本和收入。

   .. image:: media/timesheets09.png
     :align: center

2. 我们可以将不同的分析账户组合在一起，检查它们各自的收入。只需按 **Analytic account** 分组，并选择 **Graph view** 以获得清晰概览。

   .. image:: media/timesheets08.png
      :align: center
