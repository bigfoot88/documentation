========
哥伦比亚
========

简介
~~~~

从 Odoo 12 开始，哥伦比亚的电子发票可用，需要以下模块：

#. **l11n_co**：管理会计模块的所有基本数据，包含以下默认设置：会计科目表、税种、扣缴税种、身份证件类型。
#. **l10n_co_edi**：该模块包括与 Carvajal T&S 集成所需的所有额外字段，并根据 DIAN 的法律要求生成电子发票。

工作流程
~~~~~~~~

.. image:: media/colombia01.png
   :align: center

配置
~~~~

1. 安装哥伦比亚本地化模块
--------------------------

在应用程序中搜索哥伦比亚，然后点击安装前两个模块。

.. image:: media/colombia02.png
   :align: center

2. 配置 Carvajal T&S 网络服务的凭证
------------------------------------

模块安装完成后，需要配置用户和凭证信息才能连接到 Carvajal T&S 网络服务，这些信息由 Carvajal T&S 提供。

前往 :menuselection:`会计 --> 配置 --> 设置`，寻找 *哥伦比亚电子发票* 部分。

.. image:: media/colombia03.png
   :align: center

使用测试模式可以连接到 Carvajal T&S 测试环境。这样用户可以测试整个工作流程和与 CEN Financiero 门户的集成，该门户可在此处访问：https://cenflab.cen.biz/site/

当 Odoo 和 Carvajal T&S 完全配置并准备好投产时，可以禁用测试环境。

3. 配置报告数据
----------------

作为 XML 中发送的可配置信息的一部分，您可以在 PDF 中定义财务部分和银行信息的数据。

前往 :menuselection:`会计 --> 配置 --> 设置`，寻找 *哥伦比亚电子发票* 部分。

.. image:: media/colombia04.png
   :align: center

4. 配置 XML 中所需的数据
-----------------------

4.1 合作伙伴
+++++++++++

4.1.1 身份识别
^^^^^^^^^^^^^^

作为哥伦比亚本地化的一部分，DIAN 定义的文件类型现已在合作伙伴表单中可用。哥伦比亚合作伙伴必须设置其身份证号码和文件类型：

.. image:: media/colombia05.png
   :align: center

.. tip:: 当文件类型为 RUT 时，身份证号码需要在 Odoo 中配置，包括校验位，Odoo 会在将数据发送给第三方供应商时分割该号码。

4.1.2 财务结构（RUT）
^^^^^^^^^^^^^^^^^^^^^^

合作伙伴的责任代码（RUT 文件中的第 53 节）包含在电子发票模块中，因为这是 DIAN 要求的信息的一部分。

这些字段可以在 :menuselection:`合作伙伴 --> 销售与采购标签 --> 财务信息` 中找到。

.. image:: media/colombia06.png
   :align: center

此外，还增加了两个布尔字段，以指定合作伙伴的财务制度。

4.2 税
+++++

如果您的销售交易包含税收产品，需要考虑额外字段 *值类型* 需要为每个税种配置。此选项位于高级选项标签中。

.. image:: media/colombia07.png
   :align: center

保留税种（ICA、IVA、Fuente）也包含在配置税收的选项中。此配置用于在发票 PDF 中正确显示税种。

.. image:: media/colombia08.png
   :align: center

4.3 日志
++++++++

DIAN 分配电子发票决议的官方序列和前缀后，需要在 Odoo 中更新与发票文档相关的销售日志。可以使用开发者模式访问该序列： :menuselection:`会计 --> 设置 --> 配置设置 --> 日志`。

.. image:: media/colombia09.png
   :align: center

打开序列后，应配置和同步前缀和下一个编号字段与 CEN Financiero。

.. image:: media/colombia10.png
   :align: center

4.4 用户
++++++++

Odoo 在发票 PDF 中使用的默认模板包括销售人员的职位，因此应配置这些字段：

.. image:: media/colombia11.png
   :align: center

使用与测试
~~~~~~~~~~~

1. 发票
-------

当所有主数据和凭证配置完成后，可以开始测试电子发票工作流程。

1.1 发票创建
+++++++++++++

在发票验证之前进行的功能工作流程不会改变。电子发票引入的主要变化如下字段：

.. image:: media/colombia12.png
   :align: center

有三种类型的文件：

- **Factura Electronica**：这是常规类型的文件，适用于发票、贷记单和借记单。
- **Factura de Importación**：应选择此选项用于进口交易。
- **Factura de contingencia**：这是一个例外类型，用作手动备份，以防公司无法使用 ERP 且需要手动生成发票，当该发票添加到 ERP 时，应选择此发票类型。

1.2 发票验证
+++++++++++++

发票验证后，自动创建并发送 XML 文件到 Carvajal，该文件显示在聊天中。

.. image:: media/colombia13.png
   :align: center

"其他信息" 标签中现在显示一个额外字段，显示 XML 文件的名称。此外，还显示了一个电子发票状态的第二个额外字段，初始值为 "进行中"：

.. image:: media/colombia14.png
   :align: center

1.3 接收法律 XML 和 PDF
++++++++++++++++++++++

电子发票供应商收到 XML 文件后会进行结构和信息的验证，如果一切正确，发票状态会在使用 "检查 Carvajal 状态" 按钮后更改为 "验证"。然后他们会生成包含数字签名和唯一代码 (CUFE) 的法律 XML 文件，还会生成包含 QR 码和 CUFE 的 PDF 发票。

之后：

- 包含法律 XML 和 PDF 的 ZIP 文件会被下载并显示在发票聊天中：

.. image:: media/colombia15.png
   :align: center

.. image:: media/colombia16.png
   :align: center

- 电子发票状态变为 "已接受"。

1.4 常见错误
+++++++++++++

在 XML 验证期间，最常见的错误通常与缺少主数据有关。在这种情况下，错误信息会在更新电子发票状态后显示在聊天中。

.. image:: media/colombia17.png
   :align: center

在更正主数据后，可以使用以下按钮重新处理 XML 并发送更新版本：

.. image:: media/colombia18.png
   :align: center

.. image:: media/colombia19.png
   :align: center

2. 其他使用案例
-----------------

贷记单和借记单的流程与发票完全相同，功能工作流程也保持不变。
