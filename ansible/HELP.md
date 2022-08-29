# Troubleshooting


## Switch from PHP7.4 to PHP 8.1
Using your `username` log in to your `distro` and follow the below commands.

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


## Move to your home directory
In order to move from anywhere to your `username`'s home directory, execute the following command:
```
cd ~
```

## Detect current directory
If you don't know your current location, you can output it using the below command:
```
pwd
```


## Find your home directory
By combining [Move to your home directory](#move-to-your-home-directory) and [Detect current directory](#detect-current-directory) you can find your home directory.


## Find your projects directory
[Move to your home directory](#move-to-your-home-directory) where you should see a directory called `projects`.
