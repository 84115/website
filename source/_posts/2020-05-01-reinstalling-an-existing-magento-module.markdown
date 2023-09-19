---
layout: post
title: "Reinstalling Existing Magento 1 Modules"
date: 2015-09-17 20:33
comments: false
categories: php magento
---

Despite the fact that *Magento* has the `<version>` inside of it's config files (with the exception of a newer mysql install file e.g. mysql4-install-1.1.0.php), it doesn’t check this and reload a new module when installed. If you need to reload the module and database setup then go to the database table `core_resource`.

{% codeblock %}
+-------------------------+---------+
| code                    | version |
+-------------------------+---------+
| consignment_setup       | 1.0.0   | 
| etc ...                 |   ...   | 
+-------------------------+---------+
mysql>
{% endcodeblock %}

Then clear Magento’s cache.

Always make sure to change the version of your install script before deploying. But while developing it is easier to drop the record from `core_resource`.