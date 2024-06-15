================================================================
如何使用 TaxCloud 获取美国的正确税率
================================================================

**TaxCloud** 集成允许您为美国的每个地址正确计算销售税，并跟踪哪些产品免于销售税，以及每个州的免税适用范围。TaxCloud 实时计算美国每个州、城市和特殊司法管辖区的销售税。

配置
=============

在 TaxCloud 上
-----------
* 在 `TaxCloud <https://taxcloud.com/#register>`__ 网站上创建一个免费账户。
* 在 TaxCloud 上注册您的 Odoo 网站以获取 *API ID* 和 *API Key*。

.. image:: media/taxcloud01.png
  :align: center

* 在 TaxCloud 的设置中，点击 *Locations* 输入您的办公室和仓库的位置。
* 在 TaxCloud 的设置中，点击 *Manage Tax States* 验证您在哪些州收取销售税。

在 Odoo 上
-------
* 进入 :menuselection:`Invoicing / Accounting --> Configuration --> Settings` 并勾选 *TaxCloud - Compute tax rates based on U.S. ZIP codes*。
* 输入您的 TaxCloud 凭证。
* 点击保存以存储您的凭证。

.. image:: media/taxcloud02.png
  :align: center

* 点击 *Default Category* 旁边的刷新图标从 TaxCloud 导入 TIC 产品类别（税收信息代码）。某些类别可能意味着特定的税率。
* 选择您的默认 *TIC Code*。这将适用于任何新创建的产品。
* 在产品的 *General Information* 选项卡或产品类别上设置特定的 TaxCloud 类别。
* 确保您的公司地址完整（包括州和邮政编码）。进入 :menuselection:`Settings --> Users & Companies --> Companies` 打开并编辑您的公司记录。

工作原理
============

在 Odoo 中，销售税基于财务位置计算（参见 :doc:`application`）。安装 *TaxCloud* 时会为美国创建一个财务位置。一切开箱即用。

您可以配置 Odoo 自动检测哪些客户应使用此财务位置。进入 :menuselection:`Accounting --> Configuration --> Fiscal Positions` 打开并编辑记录。

.. image:: media/taxcloud03.png
  :align: center

现在，当客户国家为 *United States* 时，此财务位置会自动设置在任何销售订单、网络订单或发票上。这将触发自动税额计算。

.. image:: media/taxcloud04.png
  :align: center

添加您的产品。您有两种方式在订单上获取销售税。您可以确认它，或者您可以保存它，然后从 *Action* 菜单选择 **Update Taxes with TaxCloud**。

.. seealso::
  * :doc:`application`
