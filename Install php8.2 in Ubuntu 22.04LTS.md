## Install PHP 8.2 in Ubuntu 22.04LTS

# Update packages
```
sudo apt update && sudo apt upgrade -y
```

# Add PHP repository
```
sudo add-apt-repository ppa:ondrej/php
```

# Update packages again
```
sudo apt update
```

# Install PHP
```
sudo apt install php8.2 -y
```

# Confirm PHP version
```
php --version
```

# Install additional modules
```
sudo apt-get install -y php8.2-cli php8.2-common php8.2-fpm php8.2-mysql php8.2-zip php8.2-gd php8.2-mbstring php8.2-curl php8.2-xml php8.2-bcmath
```