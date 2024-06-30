# Frequently asked questions

## How do I switch to a different version of PHP?

Execute the following command:

```shell
sudo dnf module switch-to php:remi-{major}.{minor} -y
```

where `{major}.{minor}` is one of the supported PHP versions: `8.3`, `8.2`, `8.1`, `8.0` and `7.4`.

Additionally, our setup includes predefined aliases for executing the above command.
The aliases are the following:

- `php74`: switch to PHP 7.4
- `php80`: switch to PHP 8.0
- `php81`: switch to PHP 8.1
- `php82`: switch to PHP 8.2
- `php83`: switch to PHP 8.3

After switching to a different PHP version, test with the following command:

```shell
php -v
```

Depending on the selected PHP version, the output should look similar to the below:

```text
PHP 8.3.8 (cli) (built: Jun  4 2024 14:53:17) (NTS gcc x86_64)
Copyright (c) The PHP Group
Zend Engine v4.3.8, Copyright (c) Zend Technologies
```

## How do I switch to a different version of Node.js?

Execute the following commands:

```shell
sudo dnf remove nodejs -y
curl -fsSL https://rpm.nodesource.com/setup_{major}.x | sudo bash -
sudo dnf install nodejs -y
```

where `{major}` is the Node.js version you want to switch to.

Additionally, our setup includes predefined aliases for the above commands.
The aliases are the following:

- `node22`: switch to Node.js 22
- `node20`: switch to Node.js 20
- `node18`: switch to Node.js 18

After switching to a different Node.js version, test with the following command:

```shell
node -v
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

Then, you can execute your custom alias in a terminal just as a regular command:

```shell
list_files
```
