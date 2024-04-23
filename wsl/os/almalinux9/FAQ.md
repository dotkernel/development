# Frequently Asked Questions

### How do i tell Windows 11 about a new virtual hosts ?

- navigate to  "C:\Windows\System32\drivers\etc" directory.
- In the "etc" folder, you'll find a file named "hosts". Right-click on the file and choose "Open with" -> "Notepad" (or any text editor of your choice).
- add one or more lines , at the bottom of the file , like below examples:
  
   > ::1	dotkernel.local
   
   > ::1	laminas.local
  
- each of this line will correspond to a virtual host you previously created 

## How do I switch to a different version of PHP?

### Switch to PHP 8.3

    sudo dnf module switch-to php:remi-8.3 -y

### Switch to PHP 8.2

    sudo dnf module switch-to php:remi-8.2 -y

### Switch to PHP 8.1

    sudo dnf module switch-to php:remi-8.1 -y

### Switch to PHP 7.4

    sudo dnf module switch-to php:remi-7.4 -y


## How do I switch to a different version of Node.js?

### Switch to Node.js 20.x

    sudo dnf remove nodejs -y
    curl -fsSL https://rpm.nodesource.com/setup_20.x | sudo bash -
    sudo dnf install nodejs -y

### Switch to Node.js 18.x

    sudo dnf remove nodejs -y
    curl -fsSL https://rpm.nodesource.com/setup_18.x | sudo bash -
    sudo dnf install nodejs -y

### Switch to Node.js 16.x

    sudo dnf remove nodejs -y
    curl -fsSL https://rpm.nodesource.com/setup_16.x | sudo bash -
    sudo dnf install nodejs -y

### Switch to Node.js 14.x

    sudo dnf remove nodejs -y
    curl -fsSL https://rpm.nodesource.com/setup_14.x | sudo bash -
    sudo dnf install nodejs -y

### Switch to Node.js 12.x

    sudo dnf remove nodejs -y
    curl -fsSL https://rpm.nodesource.com/setup_12.x | sudo bash -
    sudo dnf install nodejs -y

**Note**: Node.js 12.x is no longer actively supported!


## How do I fix common permission issues?
If running your project you encounter some permission issues, follow the below steps.

### Errors:
> PHP Fatal error:  Uncaught InvalidArgumentException: The directory "/var/www/_example.local_/html/data" is not writable...

> PHP Fatal error:  Uncaught InvalidArgumentException: The directory "/var/www/_example.local_/html/data/cache" is not writable...

> PHP Fatal error:  Uncaught InvalidArgumentException: The directory "/var/www/_example.local_/html/data/cache/doctrine" is not writable...

**Fix:**

    chmod -R 777 data


### Error:
> PHP Fatal error:  Uncaught InvalidArgumentException: The directory "/var/www/_example.local_/html/public/uploads" is not writable...

**Fix:**

    chmod -R 777 public/uploads


### Error:
> PHP Fatal error:  Uncaught ErrorException: fopen(/var/www/_example.local_/config/autoload/../../log/error-log-_yyyy-mm-dd.log_): Failed to open stream: Permission denied...

**Fix:**

    chmod -R 777 log
