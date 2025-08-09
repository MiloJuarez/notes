# CONFIGURAR DNS EN VHOST PARA PROYECTO LARAVEL

## 1. Create the vhost template

<small>laravel-vhost-templage.conf</small>

```
<VirtualHost *:80>
    ServerName {{SERVER_NAME}}
    DocumentRoot {{DOC_ROOT}}/public

    <Directory {{DOC_ROOT}}/public>
        Options Indexes FollowSymLinks
        AllowOverride None
        Require all granted

        <IfModule mod_rewrite.c>
            RewriteEngine On
            RewriteCond %{REQUEST_FILENAME} !-f
            RewriteCond %{REQUEST_FILENAME} !-d
            RewriteRule ^ index.php [L]
        </IfModule>
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/{{SERVER_NAME}}_error.log
    CustomLog ${APACHE_LOG_DIR}/{{SERVER_NAME}}_access.log combined
</VirtualHost>
```

## 2. Create the helper script

Save this as <b>add-vhost.sh</b> in your home folder:

```
#!/bin/bash
# Usage: sudo ./add-vhost.sh myproject /home/user/projects/myproject

if [ $# -ne 2 ]; then
    echo "Usage: sudo $0 <project-name> <document-root>"
    exit 1
fi

PROJECT_NAME=$1
DOC_ROOT=$2
SERVER_NAME="$PROJECT_NAME.local"
VHOST_FILE="/etc/apache2/sites-available/$PROJECT_NAME.conf"
TEMPLATE_PATH="$HOME/vhost-templates/laravel-vhost-template.conf"

if [ ! -f "$TEMPLATE_PATH" ]; then
    echo "Template not found: $TEMPLATE_PATH"
    exit 1
fi

# Replace placeholders in template and save to sites-available
sed "s|{{SERVER_NAME}}|$SERVER_NAME|g; s|{{DOC_ROOT}}|$DOC_ROOT|g" \
    "$TEMPLATE_PATH" > "$VHOST_FILE"

# Enable site
a2ensite "$PROJECT_NAME.conf"

# Add domain to Windows hosts file
WINDOWS_HOSTS="/mnt/c/Windows/System32/drivers/etc/hosts"
if ! grep -q "$SERVER_NAME" "$WINDOWS_HOSTS"; then
    echo "127.0.0.1 $SERVER_NAME" >> "$WINDOWS_HOSTS"
    echo "Added $SERVER_NAME to Windows hosts file."
fi

# Restart Apache
service apache2 reload

echo "Vhost for $SERVER_NAME created. Open http://$SERVER_NAME in your browser."
```

3. Make it executable

```
chmod +x add-vhost.sh
```

## 4. Usage example

```
sudo ./add-vhost.sh myproject /home/<username>/projects/myproject
```

This will:

-   Create /etc/apache2/sites-available/myproject.conf
-   Enable it in Apache
-   Add 127.0.0.1 myproject.local to Windows hosts file
-   Reload Apache
