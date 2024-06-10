.. _db_premise:

===============================
本地数据库管理
===============================

注册数据库
===================

要注册数据库，只需在应用程序切换器中的横幅中输入订阅代码。确保不要在订阅代码前后添加多余的空格。如果注册成功，横幅将变为绿色，并显示刚注册数据库的到期日期。你可以在 About 菜单（Odoo 9）或设置仪表板（Odoo 10）中查看此到期日期。

注册错误信息
--------------------------

如果你无法注册数据库，你可能会遇到以下信息：

.. image:: media/error_message_sub_code.png
    :align: center
    :alt: 注册数据库时出现问题，你可以重试或联系 Odoo 帮助

解决方案
'''''''''

* 你有有效的企业订阅吗？

  * 检查你的订阅详情是否在 `Odoo 账户 <https://accounts.odoo.com/my/subscription>`__ 上显示为“进行中”或联系你的账户经理

* 你是否已将数据库链接到你的订阅参考？

  * 每个订阅只能链接一个数据库。
    （需要测试或开发数据库？ `寻找合作伙伴 <https://www.odoo.com/partners>`__）

  * 你可以在你的 `Odoo 合同 <https://accounts.odoo.com/my/subscription>`__ 中使用“解除链接数据库”按钮自行解除旧数据库链接

    .. image:: media/unlink_single_db.png
        :align: center


    将出现确认消息；请确保这是正确的数据库，因为它将很快被停用：

    .. image:: media/unlink_confirm_enterprise_edition.png
        :align: center


* 你有更新版本的 Odoo 9 吗？

  * 自 2016 年 7 月起，Odoo 9 现在会自动更改复制数据库的 uuid；不再需要手动操作。

  * 如果不是这种情况，你可能有多个数据库共享相同的 UUID。请在你的 `Odoo 合同 <https://accounts.odoo.com/my/subscription>`__ 中查看，会出现一条简短的信息，说明哪个数据库有问题：

    .. image:: media/unlink_db_name_collision.png
        :align: center


    在这种情况下，你需要更改测试数据库上的 UUID 来解决此问题。你可以在 :ref:`此部分 <duplicate_premise>` 中找到更多信息。

    供你参考，我们用 UUID 来识别数据库。因此，每个数据库应有一个不同的 UUID，以确保注册和开票过程顺利进行。

* 检查你的网络和防火墙设置

  * 更新通知必须能够访问 Odoo 的订阅验证服务器。换句话说，请确保 Odoo 服务器能够打开到以下地址的出站连接：

    * services.odoo.com 端口 443（或 80）
    * services.openerp.com 端口 443（或 80）用于旧部署

  * 一旦激活数据库，你必须保持这些端口开放，因为更新通知每周运行一次。



用户过多导致的错误信息
-----------------------------------

如果本地数据库中的用户数量超过了 Odoo 企业订阅中提供的用户数量，你可能会遇到以下信息：

.. image:: media/add_more_users.png
    :align: center
    :alt: 此数据库将在 X 天后到期，你的用户数量超过了订阅允许的数量


出现此消息时，你还有 30 天的时间处理问题。倒计时每天更新。

解决方案
'''''''''

* **在订阅中添加更多用户**：点击链接，验证并支付额外用户的费用。

或者

* **停用用户**，如本 `文档 <https://www.odoo.com/documentation/user/12.0/db_management/documentation.html#deactivating-users>`__ 所述，并**拒绝**增购报价。

一旦数据库中的用户数量正确，过期消息将在几天后自动消失，当下一次验证发生时。我们理解看到倒计时可能会有些紧张，所以你可以 :ref:`强制更新通知 <force_ping>` 让消息立即消失。

数据库到期错误信息
------------------------------

如果数据库在你续订订阅之前达到到期日期，你会遇到以下信息：

.. image:: media/database_expired.png
    :align: center
    :alt: 该数据库已到期。


此**阻止**消息在持续 30 天的非阻止消息之后出现。如果在倒计时结束前未采取措施，数据库将到期。

解决方案
'''''''''

* 续订你的订阅：点击链接并续订订阅 - 请注意，如果你希望通过电汇支付，订阅只有在付款到达时才会实际续订，这可能需要几天时间。信用卡支付会立即处理。
* 联系我们的 `支持 <https://www.odoo.com/help>`__

以上解决方案都不起作用？请联系我们的 `支持 <https://www.odoo.com/help>`__


.. _force_ping:

.. _duplicate_premise:

复制数据库
====================

你可以通过访问服务器上的数据库管理器（<odoo-server>/web/database/manager）来复制数据库。在此页面，你可以轻松复制数据库（以及其他操作）。

.. image:: media/db_manager.gif
    :align: center


当你复制本地数据库时，**强烈**建议更改复制数据库的 uuid（全局唯一标识符），因为此 uuid 是数据库向我们的服务器识别自己的方式。两个数据库共享相同的 uuid 可能会导致将来出现开票或注册问题。

.. note:: 自 2016 年 7 月起，Odoo 9 现在会自动更改复制数据库的 uuid；不再需要手动操作。

数据库 uuid 目前可从菜单 **设置 > 技术 > 系统参数** 访问，我们建议你使用 `uuid 生成器 <https://www.uuidgenerator.net>`__ 或使用 unix 命令 ``uuidgen`` 来生成新的 uuid。然后你可以像更改任何其他记录一样简单地点击并使用编辑按钮进行替换。

.. image:: media/db_uuid.png
    :align: center
