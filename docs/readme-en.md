# ionCube Loader Installation and Configuration Guide for PHP X

This document provides a comprehensive and step-by-step guide to install, configure, and verify ionCube Loader on a server running PHP X. It assumes SSH access with root or sudo privileges.
* X means the PHP version that you want.
---

## 1. Prerequisites

- Linux-based operating system (Ubuntu/Debian/CentOS, etc.)
- PHP X installed (replace X with your PHP version, e.g., 8.1)
- SSH access to the server
- It is recommended to back up your website and server settings before making changes

---

## 2. Download and Extract ionCube Loader

```
cd /tmp
wget https://downloads.ioncube.com/loader_downloads/ioncube_loaders_lin_x86-64.tar.gz
tar xzf ioncube_loaders_lin_x86-64.tar.gz
cd ioncube
```

---

## 3. Find the `extension_dir` Path for PHP X

```
phpX -i | grep extension_dir
```

The output typically looks like `/usr/lib/php/20210902/`. This is where the ionCube loader file should be copied.

---

## 4. Copy the ionCube Loader File

```
cp ioncube_loader_lin_X.so {output of the previous step}
```

- Replace `/usr/lib/php/20210902/` with the path obtained in the previous step.
- Replace `ioncube_loader_lin_X.so` with the appropriate file for your PHP version (e.g., `ioncube_loader_lin_8.1.so`).

---

## 5. Edit the PHP Configuration File (php.ini) for PHP X

Common `php.ini` locations for PHP X:

- PHP-FPM: `/etc/php/X/fpm/php.ini`
- CLI: `/etc/php/X/cli/php.ini`
- Apache: `/etc/php/X/apache2/php.ini`

For example, to edit the PHP-FPM configuration:

```
nano /etc/php/X/fpm/php.ini
```

Add the following line near the top of the file (or modify existing ionCube loader settings):

```
zend_extension = /usr/lib/php/20210902/ioncube_loader_lin_X.so
```

- Adjust the path as per your `extension_dir`.
- Replace `X` with your PHP version number.

Save and exit the editor (In nano: Ctrl+O, Enter, Ctrl+X).

---

## 6. Restart PHP and Web Server Services

### Restart PHP-FPM:

```
systemctl restart phpX-fpm
```

### Restart Apache (if used):

```
systemctl restart apache2
```

### Restart Nginx (if used):

```
systemctl restart nginx
```

---

## 7. Verify ionCube Loader Installation

### Command line check:

```
phpX -v
```

Look for this (or similar) line in the output:

```
with the ionCube PHP Loader (enabled) ...
```

### Web check:

1. Create a file named `info.php` in your web root directory with the content:

```
<?php phpinfo(); ?>
```

2. Open it in your browser (e.g., `http://your-domain.com/info.php`).
3. Verify ionCube Loader appears in the PHP information page.

**For security, delete `info.php` after verification.**

---

## 8. Configure PHP Version per Site in Control Panels (e.g., ISPConfig)

1. Log into your control panel.
2. Navigate to the **Sites** or **Websites** section.
3. Select the desired site.
4. Under **Options** or site settings, set the PHP version to `X`.
5. Save the changes.

*Note: ionCube Loader only needs to be installed once per PHP version on the server and is then available for all sites using that PHP version.*

---

## 9. Tips and Important Notes

- For servers with multiple PHP versions, repeat these steps for each PHP version you want ionCube Loader installed.
- ionCube Loader for one PHP version is not compatible with others.
- The `php.ini` files for CLI, PHP-FPM, and Apache/Nginx may be different; edit all relevant ones as needed.
- If your site does not work after installation:
  - Check the loader path and `php.ini` for correctness.
  - Review PHP-FPM, web server, and PHP error logs.
- Always keep backups before making changes.

---

## Example Table of Common PHP Versions and Paths

| PHP Version | Loader File                   | php.ini Path                      | extension_dir                      |
|-------------|------------------------------|---------------------------------|----------------------------------|
| 7.4         | ioncube_loader_lin_7.4.so    | /etc/php/7.4/fpm/php.ini         | /usr/lib/php/20190902/            |
| 8.0         | ioncube_loader_lin_8.0.so    | /etc/php/8.0/fpm/php.ini         | /usr/lib/php/20200930/            |
| 8.1         | ioncube_loader_lin_8.1.so    | /etc/php/8.1/fpm/php.ini         | /usr/lib/php/20210902/            |
| 8.2         | ioncube_loader_lin_8.2.so    | /etc/php/8.2/fpm/php.ini         | /usr/lib/php/20220829/            |
