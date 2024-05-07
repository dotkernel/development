# Frequently Asked Questions


## How do I switch between PHP versions?
In this example we will switch from PHP version 7.4 to 8.1.

Follow the same pattern if you need to switch between other versions than the ones presented in this example.

Step 1: Disable PHP7.4:
```
sudo a2dismod php7.4
```
Step 2: Enable PHP8.1:
```
sudo a2enmod php8.1
```
Step 3: Make sure `php` points to PHP8.1:
```
sudo update-alternatives --set php /usr/bin/php8.1
```
Step 4: Restart HTTP server:
```
sudo service apache2 restart
```


## How do I switch to a different version of Node.js?

### Switch to Node.js 22.x

    sudo apt remove nodejs -y
    curl -fsSL https://deb.nodesource.com/setup_22.x | sudo -E bash -
    sudo apt install nodejs -y

### Switch to Node.js 20.x

    sudo apt remove nodejs -y
    curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
    sudo apt install nodejs -y

### Switch to Node.js 18.x

    sudo apt remove nodejs -y
    curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
    sudo apt install nodejs -y

### Switch to Node.js 16.x

    sudo apt remove nodejs -y
    curl -fsSL https://deb.nodesource.com/setup_16.x | sudo -E bash -
    sudo apt install nodejs -y

### Switch to Node.js 14.x

    sudo apt remove nodejs -y
    curl -fsSL https://deb.nodesource.com/setup_14.x | sudo -E bash -
    sudo apt install nodejs -y

### Switch to Node.js 12.x

    sudo apt remove nodejs -y
    curl -fsSL https://deb.nodesource.com/setup_12.x | sudo -E bash -
    sudo apt install nodejs -y

**Note**: Node.js 12.x is no longer actively supported!


## How do I fix common permission issues?
If running your project you encounter some permission issues, follow the below steps.

**Errors:**
> PHP Fatal error:  Uncaught InvalidArgumentException: The directory "/home/_username_/projects/_example.local_/data" is not writable...

> PHP Fatal error:  Uncaught InvalidArgumentException: The directory "/home/_username_/projects/_example.local_/data/cache" is not writable...

> PHP Fatal error:  Uncaught InvalidArgumentException: The directory "/home/_username_/projects/_example.local_/data/cache/doctrine" is not writable...

**Fix:**

    chmod -R 777 data


### Error:
> PHP Fatal error:  Uncaught InvalidArgumentException: The directory "/home/_username_/projects/_example.local_/public/uploads" is not writable...

**Fix:**

    chmod -R 777 public/uploads


**Error:**
> PHP Fatal error:  Uncaught ErrorException: fopen(/home/_username_/projects/_example.local_/config/autoload/../../log/error-log-_yyyy-mm-dd.log_): Failed to open stream: Permission denied...

**Fix:**

    chmod -R 777 log
