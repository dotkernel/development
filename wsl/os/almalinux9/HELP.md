# Troubleshooting


## Switching to PHP 8.2

    sudo dnf module switch-to php:remi-8.2 -y

## Switching to PHP 8.1

    sudo dnf module switch-to php:remi-8.1 -y


## Fix common permission issues
If running your project you encounter some permission issues, follow the below steps.

### Errors:
> PHP Fatal error:  Uncaught InvalidArgumentException: The directory "/var/www/_example.local_/data" is not writable...

> PHP Fatal error:  Uncaught InvalidArgumentException: The directory "/var/www/_example.local_/data/cache" is not writable...

> PHP Fatal error:  Uncaught InvalidArgumentException: The directory "/var/www/_example.local_/data/cache/doctrine" is not writable...

**Fix:**

    chmod -R 777 data


### Error:
> PHP Fatal error:  Uncaught ErrorException: fopen(/var/www/_example.local_/config/autoload/../../log/error-log-_yyyy-mm-dd.log_): Failed to open stream: Permission denied...

**Fix:**

    chmod -R 777 log
