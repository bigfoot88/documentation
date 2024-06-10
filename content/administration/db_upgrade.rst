.. |assistance-contact| replace::
   如果您在此问题上需要 Odoo 的帮助，请联系您的 Odoo 客户经理或联系
   我们的 `销售部门`_。
.. _Sales department: mailto:sales@odoo.com

.. _db-upgrade:

=======
升级
=======

.. _db-upgrade/overview:

概述
========

.. _db-upgrade/process:

升级流程
-------------------

本文档适用于我们的 *本地部署* (自托管) 和 *Odoo.sh* 客户。如果您使用的是在线托管服务，请查看我们的 :ref:`在线 (SaaS) 客户说明页面 <upgrade_button>`。

.. _db-upgrade/definition:

定义
~~~~~~~~~~

升级是指切换到更新版本的 Odoo (例如，从 Odoo 13.0 升级到 Odoo 14.0)。

升级不包括以下内容：

* 更改 :ref:`版本 <db-upgrade/faq/editions-change>` (即从社区版到企业版)
* 切换 :ref:`托管类型 <db-upgrade/faq/hosting-types-switch>` (即从本地部署到在线或 Odoo.sh)
* 从其他 ERP 迁移到 Odoo

.. note:: |assistance-contact|

.. _db-upgrade/process-workflow:

流程概述
~~~~~~~~~~~~~~~~

升级流程简要概述如下：

#. 创建测试升级请求。
#. | 请求由 Odoo 处理：
   | 这通过自动化过程进行，将数据库通过升级脚本处理，时间在 20 到 120 分钟之间。只有在出现问题时，我们才需要手动干预并针对您的数据库调整脚本，直到升级成功。
#. Odoo 提供测试数据库。
#. 您测试数据库以查找可能的差异 (请参阅 :ref:`db-upgrade/test-guidance`)。
#. 如果有任何差异，请通过 :ref:`帮助门户 <db-upgrade/test-assistance>` 向升级支持团队报告。
#. 我们会修复问题并发送新的测试数据库给您。
#. 一旦您完成测试并对结果满意，您决定何时停止用户访问 Odoo，冻结所有数据输入并创建生产升级请求。
#. Odoo 通过自动化过程交付生产数据库。
#. 几小时后，您将其恢复到您的生产环境中，并继续在新升级的数据库上工作。

.. _db-upgrade/service-level:

服务水平协议
-----------------------

企业许可覆盖哪些内容？
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

在 Odoo 的云平台 (Saas 和 Odoo.sh) 或本地部署 (自托管) 的数据库始终享有以下服务。

升级内容包括：

* 标准应用
* Studio 自定义 (只要 Studio 应用仍然活跃)
* 我们的咨询和开发服务所做的定制 *如果* 它们被“定制维护”订阅覆盖

升级服务仅限于您的数据库 (标准模块和数据) 的技术转换和适配，使其与目标版本兼容。

升级不包括的内容
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

* 升级过程中清理现有数据和配置
* 任何新的开发和/或升级您自己的 :ref:`自定义模块 <db-upgrade/faq/custom-modules>`
* `新版本的培训 <https://www.odoo.com/learn>`_

您可以在我们的 :ref:`Odoo 企业订阅协议 <upgrade>` 页面上获取更多关于您的企业许可的信息。

.. note:: |assistance-contact|

.. _db-upgrade/get-started:

开始
===========

升级过程因数据库托管位置而异。

.. _db-upgrade/online:

在线 (SaaS)
-------------

如果您使用的是在线托管服务，请查看我们的 :ref:`在线 (SaaS) 客户说明页面 <upgrade_button>`。

.. _db-upgrade/odoo-sh:

Odoo.sh
-------

如果您在 Odoo.sh 上托管，请查看我们的 :doc:`升级数据库的具体说明 <odoo_sh/advanced/upgrade_your_database>`。

.. _db-upgrade/on-premise:

本地部署
----------

有两种可能性：

#. 通过我们 `网站表单 <https://upgrade.odoo.com>`_ 界面
#. | 对于技术先进的用户和合作伙伴，通过以下命令行 (在托管数据库的机器上使用)：
   | ``python <(curl -s beta.upgrade.odoo.com/upgrade) test -d <your db name> -t 14.0``

这有什么作用？
~~~~~~~~~~~~~~~~

上述命令会将您的数据库转储到一个文件，然后将其发送到升级平台进行升级，显示实时日志，然后将升级后的数据库恢复到您的服务器上，作为一个重复的测试数据库。

.. _db-upgrade/steps:

步骤
=====

.. _db-upgrade/steps-test:

测试阶段
-----------------

.. _db-upgrade/test-process:

测试过程
~~~~~~~~~~~~

测试阶段也称为预生产阶段，允许您在不影响生产数据库的情况下，审查升级后的数据库版本。

我们建议您至少运行一次测试升级过程，但您可以根据需要多次运行 (一次一次地进行)。

一旦您收到升级后的测试数据库，您应该检查所有数据、流程和功能是否仍然正确并按预期工作。

如果发现差异，您可以：

* | :ref:`报告您的问题 <db-upgrade/test-assistance>`
  | 和/或
* 在升级脚本中的问题修复后，申请新的 :ref:`测试请求 <db-upgrade/test-db-request>`。

当您未发现任何差异时，您可以：

* 继续升级您的 :ref:`生产数据库 <db-upgrade/production-live>`。

.. _db-upgrade/test-db-request:

请求测试数据库
~~~~~~~~~~~~~~~~~~~~~~~

填写 `网站表单 <https://upgrade.odoo.com>`_ 时，选择 *测试* 目的。

.. image:: media/db-upgrade-test-purpose.png
   :align: center
   :alt: 在 Odoo 升级表单中选择“测试”目的

.. _db-upgrade/test-guidance:

测试指南
~~~~~~~~~~~~~

每个企业和组织都有自己的运营需求，需要分别测试其特定的 Odoo 实例。然而，我们建议您查看我们创建的 `测试场景 <https://drive.google.com/open?id=1Lm4JqbsHBirB1wMi14UChoz_YHLjx5ec>`_，了解您应该测试和注意的高层次内容。

.. todo:: 文档发布后更改“测试场景”的链接

.. _db-upgrade/test-assistance:

协助
~~~~~~~~~~

如果您在 **测试数据库** 中遇到问题，请联系 Odoo 升级支持：

#. 访问我们的 `Odoo 支持页面 <https://www.odoo.com/help>`_。
#. 在 *工单描述* 部分，选择 *与我的升级相关的问题* 工单类型。

   .. image:: media/db-upgrade-test-assistance.png
      :align: center
      :alt: 在 Odoo 支持表单中选择“与我的升级相关的问题”作为工单类型

   .. warning::
      如果您选择了其他 *工单描述* 类型，请求将被重定向到其他团队，从而减慢处理和响应时间。

#. 请尽可能提供详细信息。在适用的情况下，使用视频和/或截图说明当前和以前的流程。这将避免澄清问题并显著加快解决过程。

   .. image:: media/db-upgrade-test-assistance-details.png
      :align: center
      :alt: Odoo 支持表单中的“详细描述”字段

.. note::
   * 测试阶段的目的不是纠正数据库中的现有数据或配置。
   * |assistance-contact|

.. _db-upgrade/steps-production:

生产启动
---------------------

.. _db-upgrade/production-live:

生产上线
~~~~~~~~~~~~~~~~~~~~

生产升级请求是指您决定将当前数据库（包括所有生产数据，如发票、增值税申报、库存、当前订单）升级到您选择的新版本。

在您对测试结果满意后，通过我们的 `网站表单 <https://upgrade.odoo.com>`_ 提交升级生产数据库的请求。选择 *生产* 目的。

.. image:: media/db-upgrade-production-purpose.png
   :align: center
   :alt: 在 Odoo 升级表单中选择“生产”目的

.. danger::
   未经测试就进入生产可能导致：

   - 业务中断 (例如，不再有可能验证某个操作)
   - 糟糕的客户体验 (例如，电子商务网站无法正常工作)

.. _db-upgrade/production-assistance:

协助
~~~~~~~~~~

如果您在 **生产数据库** 中遇到问题，请联系 **Odoo 支持**：

#. 访问我们的 `Odoo 支持页面 <https://www.odoo.com/help>`_。
#. 在 *工单描述* 部分，选择与您的问题相关的适当类型，但 **不要选择** 选项 *与我的升级相关的问题*。

   .. note::
      升级到生产后，将由支持团队而不是升级团队提供支持。

#. 请尽可能提供详细信息。在适用的情况下，使用视频和/或截图说明当前和以前的流程。这将避免澄清问题并显著加快解决过程。

   .. image:: media/db-upgrade-production-assistance-details.png
      :align: center
      :alt: Odoo 支持表单中的“详细描述”字段

   .. warning::
      如果您

选择 *与我的升级相关的问题* 作为工单类型，请求将被重定向到其他团队，从而减慢处理和响应时间。

.. _db-upgrade/faq:

常见问题
===

.. _db-upgrade/faq/why:

为什么升级？
------------

* 您可以享受 :ref:`新主要版本 <db-upgrade/faq/release-notes>` 中的最新功能。
* 如果您使用的是 :ref:`不受支持的版本 <db-upgrade/supported-versions>`，您将获得具有支持的新版本。

.. _db-upgrade/faq/when:

何时升级？
----------------

任何时候都可以。我们一旦在我们的 `网站表单 <https://upgrade.odoo.com>`_ 上发布新版本，您就可以提出升级请求。

.. _db-upgrade/faq/availability:

新版本的可用性
-------------------------------

请注意，一旦我们宣布发布新主要版本 (通常在年底)，升级服务团队需要调整升级脚本，因此新版本不会立即对现有数据库开放。

.. _db-upgrade/faq/finalization:

升级的最终完成 (预计到达时间)
---------------------------------------------------------------------

不幸的是，无法为每个升级请求提供时间估计。Odoo 提供了如此多的可能性 (例如品牌、工作流程、自定义等)，使得升级和转换为新结构变得复杂。如果您使用多个管理敏感数据的应用程序 (例如，会计、库存等)，某些情况下可能仍需要人工干预，从而使过程变慢。

在新主要版本发布的最初几个月内尤其如此，这可能显著延长升级延迟时间。

通常，数据库越小，升级速度越快。一个仅使用 CRM 的单用户数据库处理速度会比使用会计、销售、采购和制造的多公司、多用户数据库快得多。

那么，总的来说，哪些因素会影响您的升级时间？

* 源版本和目标版本
* 已安装的应用
* 数据量
* 自定义程度 (模型、字段、方法、工作流程、报告、网站等)
* 测试阶段开始后的新应用安装或配置更改

通常，第一次请求过程中遇到的延迟 (您提交首次测试升级请求后的等待时间) 可以大致反映生产请求的等待时间。

.. _db-upgrade/faq/custom-modules:

自定义模块的升级
-----------------------------

如我们在 :doc:`../legal/terms/enterprise` 中所述，:ref:`标准费用 <charges_standard>` 部分，此可选服务需支付额外费用。

如果您有自定义代码，您可以选择由我们的服务、我们的 `合作伙伴 <https://www.odoo.com/partners>`_ 升级，或自行升级。

.. note:: |assistance-contact|

.. _db-upgrade/faq/editions-change:

版本变更 (从社区版到企业版)
----------------------------------------------

升级不包括更改 `版本 <https://www.odoo.com/page/editions>`_

.. note:: |assistance-contact|

.. _db-upgrade/faq/hosting-types-switch:

切换托管类型 (自托管与在线与 Odoo.sh)
--------------------------------------------------------------

升级不包括更改 `托管类型 <https://www.odoo.com/page/hosting-types>`_。

请打开以下链接，获取 :doc:`有关如何更改托管类型的更多信息 <db_management/hosting_changes>`。

.. note:: |assistance-contact|

.. _db-upgrade/faq/release-notes:

按版本的发行说明
------------------------

打开我们的 `发行说明 <https://www.odoo.com/page/release-notes>`_ 页面，获取每个版本新增功能和改进的摘要。

.. _db-upgrade/assistance:

协助
==========

.. _db-upgrade/contact:

联系我们的升级服务支持
-----------------------------------

如果您有任何关于升级的更多问题，请随时发送消息至 `Odoo 升级团队 <mailto:upgrade@odoo.com>`_。我们将非常乐意尽快回答您的问题。

.. _db-upgrade/supported-versions:

支持的版本
------------------

请注意，Odoo 仅为最后三个主要版本提供支持和错误修复。

这是升级前需要考虑的一个因素。如果您使用的是较旧的版本，我们建议您选择最新版本，以便享受更长时间的支持 (在再次升级之前)。

您可以在我们的 :doc:`支持的版本 <../services/support/supported_versions>` 页面上获取更多信息。
