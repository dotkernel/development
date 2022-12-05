# Troubleshooting


## Switching PHP versions
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


## Fix common permission issues
If running your project you encounter some permission issues, follow the below steps.

**Errors:**
> PHP Fatal error:  Uncaught InvalidArgumentException: The directory "/home/_username_/projects/_example.local_/data" is not writable...

> PHP Fatal error:  Uncaught InvalidArgumentException: The directory "/home/_username_/projects/_example.local_/data/cache" is not writable...

> PHP Fatal error:  Uncaught InvalidArgumentException: The directory "/home/_username_/projects/_example.local_/data/cache/doctrine" is not writable...

**Fix:**
```
chmod -R 777 data
```


**Error:**
> PHP Fatal error:  Uncaught ErrorException: fopen(/home/_username_/projects/_example.local_/config/autoload/../../log/error-log-_yyyy-mm-dd.log_): Failed to open stream: Permission denied...

**Fix:**
```
chmod -R 777 log
```
