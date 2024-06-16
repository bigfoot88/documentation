=======================================================
如何生成 Unsplash 访问密钥
=======================================================

.. tip::
  **作为 SaaS 用户**，您已经可以使用 Unsplash。您无需按照本指南设置 Unsplash 信息，因为您将透明地使用我们自己的 Odoo Unsplash 密钥。

为 **非 SaaS** 用户生成 Unsplash 访问密钥
======================================================

- 在 `Unsplash.com <https://unsplash.com/join>`_ 创建一个账户。

- 转到您的 `应用程序控制面板 <https://unsplash.com/oauth/applications>`_，然后点击 **New Application**。

.. image:: media/create_app.png
    :align: center

- 接受条款并点击 **Accept terms**。

.. image:: media/accept_terms.png
    :align: center

- 系统会提示您输入 **Application name** 和 **Description**。请在您的应用程序名称前加上“**Odoo:** ”，以便 Unsplash 识别它为 Odoo 实例。完成后，点击 **Create application**。

.. image:: media/app_infos.png
    :align: center

- 您会被重定向到应用程序详细信息页面。向下滚动即可找到您的 **access key**。

.. image:: media/access_key.png
    :align: center

.. warning::
  **作为非 SaaS 用户**，您将无法注册生产用的 Unsplash 密钥，只能使用限制为每小时 50 次 Unsplash 请求的测试密钥。

.. seealso::
    * :doc:`unsplash_application_id`
