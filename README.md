# 🚀 Complete WordPress Installation Guide

![WordPress](https://img.shields.io/badge/WordPress-21759B?style=for-the-badge&logo=wordpress&logoColor=white)
![Ubuntu](https://img.shields.io/badge/Ubuntu-E95420?style=for-the-badge&logo=ubuntu&logoColor=white)
![Apache](https://img.shields.io/badge/Apache-D22128?style=for-the-badge&logo=apache&logoColor=white)
![MySQL](https://img.shields.io/badge/MySQL-4479A1?style=for-the-badge&logo=mysql&logoColor=white)
![Let's Encrypt](https://img.shields.io/badge/Let's%20Encrypt-003A70?style=for-the-badge&logo=letsencrypt&logoColor=white)
![License](https://img.shields.io/badge/License-MIT-green.svg?style=for-the-badge)

> A comprehensive, beginner-friendly guide to installing WordPress on Ubuntu with Apache web server and free SSL certificate from Let's Encrypt.

## 📋 Table of Contents

- [Overview](#-overview)
- [Features](#-features)
- [Prerequisites](#-prerequisites)
- [Installation Steps](#-installation-steps)
- [SSL Configuration](#-ssl-configuration)
- [Post-Installation](#-post-installation)
- [Troubleshooting](#-troubleshooting)
- [Security Best Practices](#-security-best-practices)
- [Contributing](#-contributing)
- [License](#-license)
- [Support](#-support)

## 🌟 Overview

This repository contains a complete, step-by-step tutorial for installing WordPress on an Ubuntu server with Apache, MySQL, PHP (LAMP stack), and securing it with a free SSL certificate from Let's Encrypt.

**Perfect for:**
- Bloggers starting their first website
- Developers learning WordPress deployment
- System administrators managing WordPress servers
- Anyone wanting to host WordPress on their own server

## ✨ Features

- ✅ **Complete LAMP Stack Setup** - Apache, MySQL, PHP installation
- ✅ **Latest WordPress** - Direct installation from WordPress.org
- ✅ **Free SSL Certificate** - Automatic HTTPS with Let's Encrypt
- ✅ **Security Hardening** - Best practices included
- ✅ **Error-Free Installation** - Common pitfalls addressed
- ✅ **Beginner Friendly** - Clear explanations for every step
- ✅ **Production Ready** - Optimized for real-world deployment

## 📦 Prerequisites

Before you begin, ensure you have:

| Requirement | Description |
|-------------|-------------|
| **Server** | Ubuntu 18.04, 20.04, 22.04, or 24.04 LTS |
| **Access** | Root or sudo privileges |
| **RAM** | Minimum 1GB (2GB+ recommended) |
| **Storage** | At least 10GB free disk space |
| **Domain** | A domain name (for SSL certificate) |
| **Ports** | Port 80 and 443 open on firewall |

### Supported Ubuntu Versions

- Ubuntu 24.04 LTS (Noble Numbat)
- Ubuntu 22.04 LTS (Jammy Jellyfish) ⭐ Recommended
- Ubuntu 20.04 LTS (Focal Fossa)
- Ubuntu 18.04 LTS (Bionic Beaver)

## 🔧 Installation Steps

### 1️⃣ Update System

```bash
sudo apt update
```

### 2️⃣ Install Dependencies

Install Apache, MySQL, PHP and required extensions:

```bash
sudo apt install apache2 \
                 ghostscript \
                 libapache2-mod-php \
                 mysql-server \
                 php \
                 php-bcmath \
                 php-curl \
                 php-imagick \
                 php-intl \
                 php-json \
                 php-mbstring \
                 php-mysql \
                 php-xml \
                 php-zip
```

### 3️⃣ Download WordPress

```bash
sudo mkdir -p /srv/www
sudo chown www-data: /srv/www
curl https://wordpress.org/latest.tar.gz | sudo -u www-data tar zx -C /srv/www
```

### 4️⃣ Configure Apache

Create virtual host configuration:

```bash
sudo nano /etc/apache2/sites-available/wordpress.conf
```

Add the following configuration:

```apache
<VirtualHost *:80>
    ServerName yourwebsite.com
    ServerAlias www.yourwebsite.com
    DocumentRoot /srv/www/wordpress
    
    <Directory /srv/www/wordpress>
        Options FollowSymLinks
        AllowOverride Limit Options FileInfo
        DirectoryIndex index.php
        Require all granted
    </Directory>
    
    <Directory /srv/www/wordpress/wp-content>
        Options FollowSymLinks
        Require all granted
    </Directory>
</VirtualHost>
```

Enable the site:

```bash
sudo a2ensite wordpress
sudo a2enmod rewrite
sudo a2dissite 000-default
sudo service apache2 reload
```

### 5️⃣ Setup MySQL Database

```bash
sudo mysql -u root
```

Run these commands in MySQL:

```sql
CREATE DATABASE wordpress;
CREATE USER wordpress@localhost IDENTIFIED BY 'your_secure_password';
GRANT SELECT,INSERT,UPDATE,DELETE,CREATE,DROP,ALTER ON wordpress.* TO wordpress@localhost;
FLUSH PRIVILEGES;
quit
```

### 6️⃣ Configure WordPress

```bash
sudo -u www-data cp /srv/www/wordpress/wp-config-sample.php /srv/www/wordpress/wp-config.php
sudo -u www-data sed -i 's/database_name_here/wordpress/' /srv/www/wordpress/wp-config.php
sudo -u www-data sed -i 's/username_here/wordpress/' /srv/www/wordpress/wp-config.php
sudo -u www-data sed -i 's/password_here/your_secure_password/' /srv/www/wordpress/wp-config.php
```

Add security keys:

```bash
sudo -u www-data nano /srv/www/wordpress/wp-config.php
```

Replace the salt keys with new ones from: https://api.wordpress.org/secret-key/1.1/salt/

### 7️⃣ Complete Web Installation

Visit `http://your-server-ip` in your browser and follow the WordPress installation wizard.

## 🔒 SSL Configuration

### Install Certbot

```bash
sudo apt install certbot python3-certbot-apache
```

### Get SSL Certificate

Replace `yourwebsite.com` with your actual domain:

```bash
sudo certbot --apache -d yourwebsite.com -d www.yourwebsite.com
```

Follow the prompts:
1. Enter your email address
2. Agree to terms of service
3. Choose to redirect HTTP to HTTPS (option 2)

### Test Auto-Renewal

```bash
sudo certbot renew --dry-run
```

### Update WordPress URLs

After SSL installation:
1. Go to `https://yourwebsite.com/wp-admin`
2. Navigate to **Settings → General**
3. Update both URLs to use `https://`
4. Save changes

## 🎯 Post-Installation

### Essential Tasks

- [ ] Install a security plugin (Wordfence, Sucuri)
- [ ] Install a backup plugin (UpdraftPlus, BackWPup)
- [ ] Set up caching (WP Super Cache, W3 Total Cache)
- [ ] Install Yoast SEO or Rank Math
- [ ] Configure permalinks (Settings → Permalinks → Post name)
- [ ] Create essential pages (About, Contact, Privacy Policy)
- [ ] Choose and install a theme
- [ ] Remove unused themes and plugins

### Recommended Plugins

| Plugin | Purpose | Free |
|--------|---------|------|
| Wordfence Security | Security & Firewall | ✅ |
| UpdraftPlus | Backup & Restore | ✅ |
| Yoast SEO | SEO Optimization | ✅ |
| WP Super Cache | Performance | ✅ |
| Contact Form 7 | Contact Forms | ✅ |
| Akismet | Spam Protection | ✅ |

## 🐛 Troubleshooting

### Error: "Error establishing a database connection"

**Solution:**
```bash
# Check database credentials in wp-config.php
sudo nano /srv/www/wordpress/wp-config.php

# Verify MySQL is running
sudo systemctl status mysql

# Restart MySQL
sudo systemctl restart mysql
```

### Error: "403 Forbidden"

**Solution:**
```bash
sudo chown -R www-data:www-data /srv/www/wordpress
sudo chmod -R 755 /srv/www/wordpress
```

### Error: "The uploaded file could not be moved"

**Solution:**
```bash
sudo mkdir -p /srv/www/wordpress/wp-content/uploads
sudo chown -R www-data:www-data /srv/www/wordpress/wp-content/uploads
sudo chmod -R 755 /srv/www/wordpress/wp-content/uploads
```

### SSL Certificate Issues

**Solution:**
```bash
# Check firewall
sudo ufw status
sudo ufw allow 80
sudo ufw allow 443

# Verify DNS
dig yourwebsite.com

# Check Apache configuration
sudo apache2ctl configtest
```

## 🔐 Security Best Practices

### 1. Keep Everything Updated

```bash
# Update WordPress core, plugins, and themes regularly
# Enable automatic updates in wp-config.php
```

Add to `wp-config.php`:
```php
define('WP_AUTO_UPDATE_CORE', true);
```

### 2. Disable File Editing

Add to `wp-config.php`:
```php
define('DISALLOW_FILE_EDIT', true);
```

### 3. Change Database Prefix

Edit `wp-config.php` and change:
```php
$table_prefix = 'wp_';
```

To something unique:
```php
$table_prefix = 'xyz_';
```

### 4. Limit Login Attempts

Install a plugin like:
- Limit Login Attempts Reloaded
- WPS Limit Login

### 5. Use Strong Passwords

- Minimum 12 characters
- Mix of uppercase, lowercase, numbers, symbols
- Use a password manager

### 6. Enable Two-Factor Authentication

Install plugins like:
- Two Factor Authentication
- Google Authenticator

### 7. Regular Backups

```bash
# Automate backups with UpdraftPlus or similar
# Store backups off-site (Google Drive, Dropbox, S3)
```

### 8. Monitor Security

- Install Wordfence or Sucuri
- Enable email notifications
- Review security logs weekly

## 📚 Useful Resources

- [WordPress Official Documentation](https://wordpress.org/documentation/)
- [WordPress Codex](https://codex.wordpress.org/)
- [WordPress Support Forums](https://wordpress.org/support/forums/)
- [Let's Encrypt Documentation](https://letsencrypt.org/docs/)
- [Apache Documentation](https://httpd.apache.org/docs/)

## 🤝 Contributing

Contributions are welcome! Here's how you can help:

1. **Fork the repository**
2. **Create a feature branch** (`git checkout -b feature/AmazingFeature`)
3. **Commit your changes** (`git commit -m 'Add some AmazingFeature'`)
4. **Push to the branch** (`git push origin feature/AmazingFeature`)
5. **Open a Pull Request**

### Areas Where You Can Contribute

- 📝 Improve documentation
- 🐛 Report bugs
- 💡 Suggest new features
- 🌐 Add translations
- 🎨 Improve formatting
- ✅ Add more troubleshooting tips

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 💬 Support

Need help? Here's how to get support:

- 📖 Check the [full tutorial](link-to-your-blog-post)
- 🐛 [Open an issue](https://github.com/yourusername/wordpress-installation-guide/issues)
- 💬 [Start a discussion](https://github.com/yourusername/wordpress-installation-guide/discussions)
- 📧 Email: your-email@example.com

## ⭐ Show Your Support

If this guide helped you, please consider:
- ⭐ Starring this repository
- 🐦 Sharing on social media
- 📝 Writing a blog post about your experience
- 💰 [Buy me a coffee](https://buymeacoffee.com/yourusername)

## 👨‍💻 Author

**Your Name**

- Website: [aws.codebyharris.com](https://yourwebsite.com)
- GitHub: [@Muhammadg](https://github.com/yourusername)
- Twitter: [@HARISKHANHK9](https://x.com/HARISKHANHK9)
- LinkedIn: [Muhammad Haris](https://www.linkedin.com/in/muhammad-haris-323268236/)

## 🙏 Acknowledgments

- WordPress.org for the amazing CMS
- Let's Encrypt for free SSL certificates
- Ubuntu and Apache communities
- All contributors who helped improve this guide

---

<p align="center">
  Made with ❤️ by <a href="https://github.com/yourusername">Muhammad Haris</a>
</p>

<p align="center">
  <sub>⭐ Star this repository if you found it helpful!</sub>
</p>
