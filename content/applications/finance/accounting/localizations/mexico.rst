======

墨西哥
======

.. note::
   本文档假设您已了解并熟悉关于发票、销售和会计的官方文档，并且在这些领域有使用 odoo 的经验。我们不打算在这里解释那些文档中已经讲解过的程序，只提供使用国家设置为“Mexico”时在公司中使用 odoo 的必要信息。

指南
~~~~~~~~~~~~

墨西哥本地化是由三个模块组成的：

1. **l10n_mx:** 管理会计、税务和会计科目的所有基础数据，安装的会计科目表是 `SAT`_ 提供的组代码列表的一个拷贝。
2. **l10n_mx_edi**: 关于电子交易的所有内容，CFDI 3.2 和 3.3，付款补充，发票附录。
3. **l10n_mx_reports**: 这里包含所有强制性的电子会计报告（需要会计应用）。

通过在 Odoo 中使用墨西哥本地化，您不仅能够满足墨西哥法律规定的功能要求，还能将其作为您的会计和发票系统使用，满足该市场的所有正常需求，使 Odoo 成为管理墨西哥公司的完美解决方案。

配置
~~~~~~~~~~~~~

.. tip::
   配置完成后，我们会为您提供测试过程，请按照步骤操作，以避免花费时间修复调试问题。您可以在任何步骤中回顾并重新尝试。

1. 安装墨西哥会计本地化
----------------------------------------------

在 Apps 中搜索 Mexico，然后点击 *Install*。

.. image:: media/mexico01.png
   :align: center

.. tip::
   当从 www.odoo.com 创建数据库时，如果您在创建账户时选择了墨西哥作为国家，则会自动安装墨西哥本地化。

2. 电子发票（CDFI 3.2 和 3.3 格式）
------------------------------------------------

要在墨西哥启用此要求，请进入会计配置，在 :menuselection:`Accounting --> Settings` 中启用图中选项，这样您就可以生成签名的发票（CFDI 3.2 和 3.3）并生成签名的付款补充（仅限 3.3），完全集成在 Odoo 的正常发票流程中。

.. image:: media/mexico02.png
   :align: center

.. _mx-legal-info:

3. 在公司中设置您的法律信息
-------------------------------------------

首先，确保您的公司已配置正确的数据。在 :menuselection:`Settings --> Users --> Companies` 中输入公司有效的地址和 VAT，不要忘记在公司的联系信息中定义墨西哥的财政位置。

.. tip::
   如果您想在测试模式下使用墨西哥本地化，可以输入墨西哥境内的任何已知地址，并将 VAT 设置为 **EKU9003173C9**。

.. image:: media/mexico03.png
   :align: center

.. warning::
   从法律角度来看，墨西哥公司必须使用当地货币（MXN）。因此，Odoo 不提供管理替代配置的功能。如果您想使用其他货币，请将 MXN 设置为默认货币，并使用价格表。

4. 在代表公司的合作伙伴上设置适当的“财政位置”
-----------------------------------------------------------------------------

在编辑公司表单的同一页面中保存记录以将此表单设置为只读视图，然后在只读视图中点击合作伙伴链接，编辑它并在 *Invoicing* 标签中设置适当的财政信息（对于 **测试环境**，应为 *601 - General de Ley Personas Morales*，如果看不到选项，只需像普通 Odoo 字段那样搜索它）。

5. 启用 CFDI 版本 3.3
----------------------------

.. warning::
   此步骤仅在启用 CFDI 3.3 时需要（仅适用于 V11.0 及以上版本），如果您的 SaaS 实例中没有 11.0 或以上版本，请通过 https://www.odoo.com/help 向支持部门发送工单请求升级。

启用调试模式：

.. image:: media/mexico10.png
   :align: center

在 :menuselection:`Settings --> Technical --> Parameters --> System Parameters` 中查找并设置名为 *l10n_mx_edi_cfdi_version* 的参数为 3.3（如果此名称的条目不存在，请创建它）。

.. warning::
   CFDI 3.2 将在 2017 年 11 月 30 日之前合法有效，启用 3.3 版本是遵守新 `SAT 决议`_ 的强制步骤，自 v11.0 版本发布以来创建的所有新数据库，CFDI 3.3 是默认行为。

.. image:: media/mexico11.png
   :align: center

启用 CFDI 3.3 时的重要考虑事项
====================================================

代表 16% 和 0% 增值税的税务其“因素类型”字段必须设置为“Tasa”。

.. image:: media/mexico12.png
   :align: center
.. image:: media/mexico13.png
   :align: center

您必须进入财政位置配置并设置适当的代码（即名称中的前三个数字），例如对于测试，您应设置 601，如图所示。

.. image:: media/mexico14.png
   :align: center

所有产品必须为 CFDI 3.3 设置正确的“SAT 代码”和“参考”字段，您可以导出并重新导入它们以更快地完成。

.. image:: media/mexico15.png
   :align: center

6. 配置 PAC 以正确签署发票
-----------------------------------------------------------

要使用 **PACs** 配置 EDI，请进入 :menuselection:`Accounting --> Settings --> Electronic Invoicing (MX)`。您可以在 *PAC 字段* 中选择 **支持的 PAC 列表** 中的一个 PAC，然后输入您的 PAC 用户名和 PAC 密码。

.. warning::
   请记住，您必须事先在相关 PAC 中注册，该过程可以通过 PAC 本身完成，在这种情况下我们将有两个（2）可用的 `Finkok`_ 和 `Solución Factible`_。

   在遵循这些步骤之前，您必须通过 SAT 机构处理您的 **私钥（CSD）**，如果您没有这些信息，请尝试所有“测试步骤”，并在完成 SAT 提议的流程后返回此过程，为生产环境设置真实交易信息。

.. image:: media/mexico04.png
   :align: center

.. tip::
   如果勾选了 *MX PAC 测试环境*，则无需输入 PAC 用户名或密码。

.. image:: media/mexico05.png
   :align: center

.. tip::
   这里有一个 SAT 证书，如果您想在墨西哥会计本地化中使用 *测试环境* 可以使用。

   :download:` Certificate <files/certificate.cer>`
   :download:` Certificate Key <files/certificate.key>`
   - **Password:** 12345678a

7. 配置销售税的标签
-----------------------------------

此标签用于设置 CFDI 中概念的税务类型代码，转移或扣留。如果税是销售税，“标签”字段应为“IVA”、“ISR”或“IEPS”。

.. image:: media/mexico33.png
   :align: center

请注意，默认税项已经分配了标签，但当您创建新税时，应该选择一个标签。

用法与测试
~~~~~~~~~~~~~~~~~

发票
---------

要使用墨西哥发票，只需按照 Odoo 的正常行为进行正常的发票处理。

验证第一张发票后，正确签署的发票应如下所示：

.. image:: media/mexico07.png
   :align: center

您可以通过点击发票上的打印按钮生成 PDF，或通过电子邮件发送发票，按照 odoo 的正常流程发送电子发票。

.. image:: media/mexico08.png
   :align: center

一旦您通过电子邮件发送了电子发票，它应如下所示：

.. image:: media/mexico09.png
   :align: center

取消发票
-------------------

取消过程完全链接到 Odoo 的正常取消过程。

如果发票未支付：

- 前往属于该发票的客户发票日记账

.. image:: media/mexico28.png
   :align: center

.. image:: media/mexico29.png
   :align: center

- 检查“允许取消条目”字段

.. image:: media/mexico29.png
   :align: center

- 返回您的发票并点击“取消发票”按钮

.. image:: media/mexico30.png
   :align: center

- 出于安全原因，建议将允许取消的选项重新设置为 false，然后前往日记账并取消选中此字段。

**法律考虑**

- 取消的发票将在 SAT 上自动取消。
- 如果您尝试取消后再次使用同一发票，您将有多次取消的 CFDI，因此需要维护这些 xml 的良好控制。
- 在取消发票之前，您必须取消与发票相关的所有付款，按照相同的方法取消这些付款，但在付款中设置“允许取消条目”。

支付 (仅限CFDI 3.3)
--------------------------------------

要生成付款补充，只需按照 Odoo 中的正常付款流程进行操作，理解这些行为的考虑事项非常重要。

1. 要生成付款补充，发票中的付款条款必须为 PPD，因为这是法律要求的“现金付款”行为。

   **1.1. 如何生成带有付款条款 `PUE` 的发票？**

   `根据 SAT 文档`_，如果发票约定在下一个日历月的 17 日之前全额支付，则付款分类为 ``PUE``，其他条件将生成 ``PPD`` 发票。

   **1.2. 如何在 Odoo 中实现？**

   为了设置适当的 CFDI 付款条款（PPD 或 PUE），可以通过发票中定义的 ``Payment Terms`` 轻松设置。

   - 如果发票生成时没有 ``Payment Term``，属性 ``MetodoPago`` 将为 ``PUE``。

   - 今天是本月第一天，如果生成的发票 ``Payment Term`` 为 ``30 Net Days``，计算的 ``Due Date`` 将是下个月的第一天，这意味着它在下个月 17 日之前，因此属性 ``MetodoPago`` 将为 ``PUE``。

   - 今天如果生成的发票 ``Payment Term`` 为 ``30 Net Days``，且 ``Due Date`` 超过下个月 17 日，则 ``MetodoPago`` 将为 ``PPD``。

   - 如果有 ``Payment Term`` 具有 2 行或更多行，例如 ``30% Advance End of Following Month``，这是一个分期付款条款，那么属性 ``MetodoPago`` 将为 ``PPD``。

2. 要测试正常签署的付款，只需创建一张付款条款为 ``30% Advance End of Following Month`` 的发票，然后为其登记付款。
3. 必须打印付款才能正确获取 PDF。
4. 关于“预付款”，必须创建一张包含预付款的发票，将预付款作为产品行设置，并按照 SAT 官方文档 `给出的流程`_ 在 **Apéndice 2 Procedimiento para la emisión de los CFDI en el caso de anticipos recibidos** 部分中设置适当的 SAT 代码。
5. 关于第 4 条，阻止在没有适当发票的情况下创建客户付款的可能性。

会计
----------
Odoo 中的墨西哥会计由三个报告组成：

1. 会计科目表（显示为 COA）。
2. 电子试算平衡表。
3. DIOT 报告。

1 和 2 被认为是电子会计，而 DIOT 是仅在会计上下文中可用的报告。

可以在会计应用的原始报告菜单中找到所有这些报告。

.. image:: media/mexico16.png
   :align: center

电子会计（需要会计应用）
===============================================

电子会计科目表 CoA
-------------------------------

电子会计从未如此简单，只需前往 :menuselection:`Accounting --> Reporting --> Mexico --> COA` 并点击 **Export for SAT (XML)** 按钮

.. image:: media/mexico19.png
   :align: center

**如何添加新账户？**

如果您添加的账户编码格式为 NNN.YY.ZZ，其中 NNN.YY 是 SAT 编码组，则您的账户将自动配置。

例如，添加一个新银行账户，请前往 :menuselection:`Accounting --> Settings --> Chart of Account`，然后点击“创建”按钮创建一个新账户，尝试使用号码 102.01.99 创建账户，一旦您更改名称，您将看到一个自动设置的标签，这些标签是在 xml 中用于 COA 的选定标签。

.. image:: media/mexico20.png
   :align: center

**标签的含义是什么？**

要了解所有可能的标签，您可以阅读 SAT 网站上的 `Anexo 24`_ 在 **Código agrupador de cuentas del SAT** 部分。

.. tip::
   安装 l10n_mx 模块时，您的会计科目依赖于它（当您在数据库中设置墨西哥为国家时会自动发生这种情况），那么您将拥有更常见的标签，如果需要的标签未创建，可以随时创建。

电子试算平衡表
------------------------

与 COA 完全相同，但包含初始借贷余额，一旦您的会计科目表正确设置，可以前往 :menuselection:`Accounting --> Reports --> Mexico --> Trial Balance`，该报告会自动生成，并可以使用顶部的 **Export for SAT (XML)** 按钮导出为 XML，之前需要选择您要导出的期间。

.. image:: media/mexico21.png
   :align: center

所有常规的审计和分析功能也都可在这里使用，像任何常规的 Odoo 报告一样。

DIOT 报告（需要会计应用）
=====================================

**什么是 DIOT 及其向 SAT 报告的重要性**

在处理 SAT 税务管理服务的程序时，我们知道我们不应忽视所提交的内容。在 Odoo 中也是如此。

DIOT 是与第三方操作的信息声明（DIOT），这是增值税的附加义务，我们必须提供与第三方操作的状态，或所谓的供应商操作状态。

这适用于个人和法人，因此如果我们需要向 SAT 提交增值税并与供应商打交道，就有必要提交 DIOT：

**何时提交 DIOT 以及以何种格式提交？**

提交 DIOT 很简单，因为与所有格式一样，您可以在 SAT 网站上获取电子格式 A-29。

每个月如果与第三方有交易，就需要提交 DIOT，就像我们提交增值税一样，因此如果一月份与供应商有交易，二月份必须提交相关信息。

**DIOT 在哪里提交？**

您可以选择不同的方式提交 DIOT，取决于哪种方式对您更方便。

A-29 格式是电子格式，可以在 SAT 网站上提交，但前提是已经输入了最多 500 条记录。

输入这些记录后，您必须将其提交到与您的税务地址对应的地方税务服务管理局（ALSC），这些记录可以以数字存储介质如 CD 或 USB 的形式提交，一旦验证完毕将归还，因此您仍将拥有这些记录及您的 CD 或 USB。

**另一需要了解的事实：批量上传？**

在查看 DIOT 的 SAT 官方文档时，您会发现批量上传，根据 SAT 网站的信息：

“批量上传”是通过将纳税人对供应商交易的记录数据库转换为文本文件（.txt）。这些文件具有必要的结构，用于应用和导入系统的第三方操作信息声明，避免直接输入，从而优化集成所需的时间，以便按时向 SAT 提交。

您可以使用它来提交 DIOT，因为这是允许的，这将使该操作变得更容易，因此没有理由不与 SAT 保持一致。

可以在 `此处查看官方信息`_。

**如何在 Odoo 中生成此报告？**

- 前往 :menuselection:`Accounting --> Reports --> Mexico --> Transactions with third parties (DIOT)`。

.. image:: media/mexico23.png
   :align: center

- 显示报告视图，选择上个月以报告当前月份之前的月份，或保留当前月份以适合您。

.. image:: media/mexico25.png
   :align: center

- 点击“导出（TXT）”。

.. image:: media/mexico24.png
   :align: center

- 将下载的文件保存在安全的地方，并前往 SAT 网站按照必要步骤进行申报。

DIOT 的供应商和发票数据的重要考虑事项
======================================================================

- 所有供应商必须在会计选项卡中设置“DIOT 信息”字段，*L10N Mx Nationality* 字段只需在地址中选择正确的国家，其他无须再做任何操作，但 *L10N Mx Type Of Operation* 字段必须由您在所有供应商中填写。

.. image:: media/mexico22.png
   :align: center

- 该报告有三种增值税选项，16%、0% 和免税，如果发票行中没有税，Odoo 会认为其免税，其他两个税项已正确配置。
- 记得支付代表预付款的发票之前，必须先要求发票，然后支付并按照标准 Odoo 程序正确对账。
- 生成供应商发票时无需在合作伙伴中填写所有数据，可以在生成报告时修正这些信息。
- 该报告仅显示实际支付的供应商发票。

如果不考虑这些事项，当生成 DIOT 的 TXT 时，会出现这样的消息，列出需要检查的所有合作伙伴，建议不仅导出您的法定义务，还在月底前生成报告，并作为审计过程，确保所有合作伙伴都正确设置。

.. image:: media/mexico26.png
   :align: center

额外推荐功能
~~~~~~~~~~~~~~~~~~~~~~~~~~

联系模块（免费）
---------------------

如果您想正确管理客户、供应商和地址，即使这不是技术需要，建议安装该模块。

多货币（需要会计应用）
----------------------------------------

在墨西哥，几乎所有公司都以不同货币发送和接收付款，如果想管理此功能，应启用多货币功能，并启用与 **Banxico** 的同步，此

功能允许您自动从 SAT 检索正确的汇率，而无需每天手动输入这些信息。

前往设置并启用多货币功能。

.. image:: media/mexico17.png
   :align: center

使用 XSD 本地验证器启用 CFDI 显示显式错误（CFDI 3.3）
-----------------------------------------------------------------------------

通常希望从 xml 中不正确设置的字段收到显式错误，这些错误在启用检查时对用户更明确。启用 xsd 检查功能，请按照以下步骤操作（启用调试模式）。

- 前往 :menuselection:`Settings --> Technical --> Actions --> Server Actions`
- 查找名为“下载 XSD 文件到 CFDI”的操作
- 点击“创建上下文操作”按钮
- 前往公司表单 :menuselection:`Settings --> Users&Companies --> Companies`
- 打开任意公司。
- 点击“操作”，然后点击“下载 XSD 文件到 CFDI”。

.. image:: media/mexico18.png
   :align: center

现在可以制作一张包含任何错误的发票（例如产品没有代码，这是非常常见的），将显示显式错误，而不是没有解释的通用错误。

.. note::
   如果看到类似这样的错误：

     生成的 cfdi 无效

     属性声明“TipoRelacion”，属性“type”：QName 值“{http://www.sat.gob.mx/sitio_internet/cfd/catalogos}c_TipoRelacion”不能解析为（简单类型定义），第 36 行

   这可能是由于数据库备份恢复到另一个服务器，或者 XSD 文件未正确下载。请按照上述步骤操作，但：

   - 前往发生错误的公司。
   - 点击“操作”，然后点击“下载 XSD 文件到 CFDI”。

常见问题
~~~

- **错误消息**（仅适用于 CFDI 3.3）：

:9:0:ERROR:SCHEMASV:SCHEMAV_CVC_MINLENGTH_VALID: Element '{http://www.sat.gob.mx/cfd/3}Concepto', attribute 'NoIdentificacion': [facet 'minLength'] The value '' has a length of '0'; this underruns the allowed minimum length of '1'.

:9:0:ERROR:SCHEMASV:SCHEMAV_CVC_PATTERN_VALID: Element '{http://www.sat.gob.mx/cfd/3}Concepto', attribute 'NoIdentificacion': [facet 'pattern'] The value '' is not accepted by the pattern '[^|]{1,100}'.

.. tip::
   **解决方法：** 忘记在产品中设置适当的“参考”字段，请前往产品表单并正确设置内部参考。

- **错误消息**：

:6:0:ERROR:SCHEMASV:SCHEMAV_CVC_COMPLEX_TYPE_4: Element '{http://www.sat.gob.mx/cfd/3}RegimenFiscal': The attribute 'Regimen' is required but missing.

:5:0:ERROR:SCHEMASV:SCHEMAV_CVC_COMPLEX_TYPE_4: Element '{http://www.sat.gob.mx/cfd/3}Emisor': The attribute 'RegimenFiscal' is required but missing.

.. tip::
   **解决方法：** 忘记在公司合作伙伴上设置适当的“财政位置”，前往客户，移除客户过滤器，查找与公司同名的合作伙伴，设置适当的财政位置，这是公司与 SAT 业务相关的类型，另一个选项是忘记了财政位置的考虑。

   您必须前往财政位置配置并设置适当的代码（即名称中的前三个数字），例如对于测试，您应设置 601，如图所示。

.. image:: media/mexico27.png
   :align: center

.. tip::
   出于测试目的，此值应为 *601 - General de Ley Personas Morales*，这是演示增值税所需的值。

- **错误消息**：

:2:0:ERROR:SCHEMASV:SCHEMAV_CVC_ENUMERATION_VALID: Element '{http://www.sat.gob.mx/cfd/3}Comprobante', attribute 'FormaPago': [facet 'enumeration'] The value '' is not an element of the set {'01', '02', '03', '04', '05', '06', '08', '12', '13', '14', '15', '17', '23', '24', '25', '26', '27', '28', '29', '30', '99'}

.. tip::
   **解决方法：** 发票上需要付款方式。

.. image:: media/mexico31.png
   :align: center

- **错误消息**：

:2:0:ERROR:SCHEMASV:SCHEMAV_CVC_ENUMERATION_VALID: Element '{http://www.sat.gob.mx/cfd/3}Comprobante', attribute 'LugarExpedicion': [facet 'enumeration'] The value '' is not an element of the set {'00
:2:0:ERROR:SCHEMASV:SCHEMAV_CVC_DATATYPE_VALID_1_2_1: Element '{http://www.sat.gob.mx/cfd/3}Comprobante', attribute 'LugarExpedicion': '' is not a valid value of the atomic type '{http://www.sat.gob.mx/sitio_internet/cfd/catalogos}c_CodigoPostal'.
:5:0:ERROR:SCHEMASV:SCHEMAV_CVC_COMPLEX_TYPE_4: Element '{http://www.sat.gob.mx/cfd/3}Emisor': The attribute 'Rfc' is required but missing.

.. tip::
   **解决方法：** 必须正确设置公司地址，这是必填字段组，可以前往 :menuselection:`Settings --> Users & Companies --> Companies` 中正确填写地址的所有必填字段，按照步骤 :ref:`mx-legal-info`。

- **错误消息**：

:2:0:ERROR:SCHEMASV:SCHEMAV_CVC_DATATYPE_VALID_1_2_1: Element '{http://www.sat.gob.mx/cfd/3}Comprobante', attribute 'LugarExpedicion': '' is not a valid value of the atomic type '{http://www.sat.gob.mx/sitio_internet/cfd/catalogos}c_CodigoPostal'.

.. tip::
   **解决方法：** 公司地址上的邮政编码不是墨西哥的有效邮编，修正它。

.. image:: media/mexico32.png
   :align: center

- **错误消息**：

:18:0:ERROR:SCHEMASV:SCHEMAV_CVC_COMPLEX_TYPE_4: Element '{http://www.sat.gob.mx/cfd/3}Traslado': The attribute 'TipoFactor' is required but missing.
:34:0:ERROR:SCHEMASV:SCHEMAV_CVC_COMPLEX_TYPE_4: Element '{http://www.sat.gob.mx/cfd/3}Traslado': The attribute 'TipoFactor' is required but missing.", '')

.. tip::
   **解决方法：** 在系统中为 0% 和 16% 的税项设置墨西哥名称，并在发票上使用。

   代表 16% 和 0% 增值税的税务其“因素类型”字段必须设置为“Tasa”。

.. image:: media/mexico12.png
   :align: center
.. image:: media/mexico13.png
   :align: center

.. _SAT: http://www.sat.gob.mx/fichas_tematicas/buzon_tributario/Documents/Anexo24_05012015.pdf
.. _Finkok: https://www.finkok.com/contacto.html
.. _`Solución Factible`: https://solucionfactible.com/sf/v3/timbrado.jsp
.. _`SAT 决议`: http://sat.gob.mx/informacion_fiscal/factura_electronica/Paginas/Anexo_20_version3.3.aspx
.. _`根据 SAT 文档`: https://www.sat.gob.mx/cs/Satellite?blobcol=urldata&blobkey=id&blobtable=MungoBlobs&blobwhere=1461173400586&ssbinary=true
.. _`给出的流程`: http://sat.gob.mx/informacion_fiscal/factura_electronica/Documents/GuiaAnexo20DPA.pdf
.. _`Anexo 24`: http://www.sat.gob.mx/fichas_tematicas/buzon_tributario/Documents/Anexo24_05012015.pdf
.. _`此处查看官方信息`: http://www.sat.gob.mx/fichas_tematicas/declaraciones_informativas/Paginas/declaracion_informativa_terceros.aspx
