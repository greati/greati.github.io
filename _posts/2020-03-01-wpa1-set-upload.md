---
layout: post
title: "All-in-one WP Migration: change maximum upload size"
categories: [blog, tips]
tags: [wordpress]
---

If you have recently installed the free version of the Wordpress plugin
called "All-in-one WP Migration", used for doing backups
and migrations, you have probably came across with the maximum
upload size of 16MB, which is very restrictive when you
have to migrate a website with many media, users, posts, pages
and plugins. So, here is how you can modify this limit
without having to pay for it:

1. Enter in the root directory of your Wordpress installation
(that with the `wp-config.php` file);
2. Open the file `.htaccess` (create if it does not exist);
3. Add to it (it can be at the beginning) the following lines:
```php
php_value upload_max_filesize 512M
php_value post_max_size 512M
php_value max_execution_time 300
php_value max_input_time 300
```
4. Save the `.htaccess` file;
5. Check that your maximum upload file has increased to 512MB.

Of course, you can change any value for 512MB in the lines above,
if you need to upload larger files. 
