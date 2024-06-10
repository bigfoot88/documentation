========================================
使用内容分发网络进行部署
========================================

.. _reference/cdn/keycdn:

使用 KeyCDN_ 进行部署
======================

.. sectionauthor:: Fabien Meghazi

本文将指导您如何在使用 Odoo 驱动的网站上设置 KeyCDN_ 账户。

步骤 1：在 KeyCDN 仪表板中创建一个拉取区
--------------------------------------------------

.. image:: cdn/keycdn_create_a_pull_zone.png
   :class: img-fluid

在创建拉取区时，请在 :guilabel:`advanced features` 子菜单中启用 CORS 选项。（稍后会详细介绍）

.. image:: cdn/keycdn_enable_CORS.png
   :class: img-fluid

完成后，您需要等待一段时间，KeyCDN_ 会抓取您的网站。

.. image:: cdn/keycdn_progressbar.png
   :class: img-fluid

.. note:: 一个新的 URL 已经为您的拉取区生成，在这个例子中是
          ``http://pulltest-b49.kxcdn.com``

步骤 2：使用您的拉取区配置 Odoo 实例
--------------------------------------------------

在 Odoo 后端，进入 :guilabel:`Website Settings` 菜单，然后启用 CDN 支持并在
:guilabel:`CDN Base URL` 字段中复制/粘贴您的拉取区 URL。此字段仅在开发者模式激活时可见和可配置。

.. image:: cdn/odoo_cdn_base_url.png
   :class: img-fluid

现在，您的网站正在为与 :guilabel:`CDN filters` 正则表达式匹配的资源使用 CDN。

您可以查看网站的 HTML 以检查 CDN 集成是否正常工作。

.. image:: cdn/odoo_check_your_html.png
   :class: img-fluid


为什么我需要启用 CORS？
---------------------------

由于一些浏览器（撰写本文时的 Firefox 和 Chrome）的安全限制，阻止远程链接的 CSS 文件在同一外部服务器上获取相对资源。

如果您不在 CDN 区域中启用 CORS 选项，在默认 Odoo 网站上最明显的问题将是缺少 font-awesome 图标，因为 font-awesome CSS 中声明的字体文件不会在远程服务器上加载。

在这种情况下，您会在主页上看到以下内容：

.. image:: cdn/odoo_font_file_not_loaded.png
   :class: img-fluid

浏览器控制台中也会出现安全错误消息：

.. image:: cdn/odoo_security_message.png
   :class: img-fluid

在 CDN 中启用 CORS 选项可以解决此问题。

.. _KeyCDN: https://www.keycdn.com
