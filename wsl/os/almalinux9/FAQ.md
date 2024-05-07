# Frequently Asked Questions

## How do I switch to a different version of PHP?

### Switch to PHP 8.3

Use alias `php83` or execute:

    sudo dnf module switch-to php:remi-8.3 -y

### Switch to PHP 8.2

Use alias `php82` or execute:

    sudo dnf module switch-to php:remi-8.2 -y

### Switch to PHP 8.1

Use alias `php81` or execute:

    sudo dnf module switch-to php:remi-8.1 -y

### Switch to PHP 7.4

Use alias `php74` or execute:

    sudo dnf module switch-to php:remi-7.4 -y

## How do I switch to a different version of Node.js?

### Switch to Node.js 22.x

Use alias `node22` or execute:

    sudo dnf remove nodejs -y
    curl -fsSL https://rpm.nodesource.com/setup_22.x | sudo bash -
    sudo dnf install nodejs -y

### Switch to Node.js 20.x

Use alias `node20` or execute:

    sudo dnf remove nodejs -y
    curl -fsSL https://rpm.nodesource.com/setup_20.x | sudo bash -
    sudo dnf install nodejs -y

### Switch to Node.js 18.x

Use alias `node18` or execute:

    sudo dnf remove nodejs -y
    curl -fsSL https://rpm.nodesource.com/setup_18.x | sudo bash -
    sudo dnf install nodejs -y


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

## How do I create command aliases?

From either your terminal or file explorer, navigate to your home directory (`/home/<your-username>/`).

Using your preferred text editor, open the file: `.bash_profile` (if it does not exist, creat it first).

Move to the end of the file and enter on a new line:

    alias command_alias="command to execute"

where:

- `command_alias` is the name by which you want to call your original command
- `command to execute`: the original command to be executed on alias call

### Example:

    alias list_files="ls -Al"

will create an alias called `list_files` that will run the command `ls -Al`.
