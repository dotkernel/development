# Frequently Asked Questions

## How do I switch between PHP versions?

In this example we will switch from PHP version 7.4 to 8.1.

Follow the same pattern if you need to switch between other versions than the ones presented in this example.

Step 1: Disable PHP7.4:

```shell
sudo a2dismod php7.4
```

Step 2: Enable PHP8.1:

```shell
sudo a2enmod php8.1
```

Step 3: Make sure `php` points to PHP8.1:

```shell
sudo update-alternatives --set php /usr/bin/php8.1
```

Step 4: Restart HTTP server:

```shell
sudo service apache2 restart
```

## How do I switch to a different version of Node.js?

### Switch to Node.js 22.x

```shell
sudo apt remove nodejs -y
curl -fsSL https://deb.nodesource.com/setup_22.x | sudo -E bash -
sudo apt install nodejs -y
```

### Switch to Node.js 20.x

```shell
sudo apt remove nodejs -y
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt install nodejs -y
```

### Switch to Node.js 18.x

```shell
sudo apt remove nodejs -y
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt install nodejs -y
```

## How do I fix common permission issues?

If running your project you encounter permission issues, follow the below steps.

### Error

> PHP Fatal error: Uncaught InvalidArgumentException: The directory "`<path-to-project>`/data" is not writable...

> PHP Fatal error: Uncaught InvalidArgumentException: The directory "`<path-to-project>`/data/cache" is not writable...

> PHP Fatal error: Uncaught InvalidArgumentException: The directory "`<path-to-project>`/data/cache/doctrine" is not
> writable...

### Solution

```shell
chmod -R 777 data
```

### Error

> PHP Fatal error: Uncaught InvalidArgumentException: The directory "`<path-to-project>`/public/uploads" is not
> writable...

### Solution

```shell
chmod -R 777 public/uploads
```

### Error

> PHP Fatal error: Uncaught ErrorException: fopen(`<path-to-project>`/log/error-log-yyyy-mm-dd.log): Failed to open
> stream: Permission denied...

### Solution

```shell
chmod -R 777 log
```
