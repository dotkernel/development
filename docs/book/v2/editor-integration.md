# Editor Integration

## Summary

Connect your editor of choice (VS Code or PhpStorm) directly to your **AlmaLinux 10** environment, and set up Xdebug for step debugging.

> This assumes you've already completed [Setup Packages](setup/setup-packages.md) and have at least one project running under a [virtualhost](virtualhosts/overview.md).

## VS Code (Remote - WSL)

Install the [Remote - WSL](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl) extension in VS Code on Windows.

Then, from inside **AlmaLinux 10**, move to your project's directory and run:

```shell
code .
```

This opens VS Code connected to your **AlmaLinux 10** distro - notice the green remote indicator in the bottom-left corner. Every extension, terminal, and file operation now runs using the Linux binaries inside AlmaLinux 10, not the Windows ones, which matters for anything that shells out to `php`, `composer`, or `node`.

> The first time you run `code .` from a new distro, VS Code Server is downloaded and installed automatically inside AlmaLinux 10 - this can take a minute or two.

## PhpStorm (WSL interpreter)

You install PhpStorm normally on Windows, but configure it to use the PHP interpreter inside AlmaLinux 10 instead of installing PHP on Windows.

1. Open your project in PhpStorm.
2. Go to `Settings` -> `PHP`.
3. Next to `CLI Interpreter`, click `...` and add a new interpreter.
4. Choose `From WSL`, select the `AlmaLinux-10` distro, and point it at `/usr/bin/php`.
5. Confirm and apply.

PhpStorm can now index, run, and debug using the same PHP install the playbook provisioned, instead of a separate Windows copy.

> Your project files are located inside the WSL filesystem (the default and recommended location - see [WSL Configuration](wsl-configuration.md)).
> You open the project from its `\\wsl.localhost\AlmaLinux-10\...` path in PhpStorm so file watching and indexing stay fast.

## Xdebug

Xdebug isn't installed by the playbook. To add it:

```shell
sudo dnf install php-pecl-xdebug -y
```

Then configure it (as root) by editing the Xdebug ini file:

```shell
sudo nano /etc/php.d/15-xdebug.ini
```

Add or adjust:

```text
zend_extension=xdebug.so
xdebug.mode=debug
xdebug.start_with_request=yes
xdebug.client_host=127.0.0.1
xdebug.client_port=9003
```

Restart PHP-FPM for the change to take effect:

```shell
sudo systemctl restart php-fpm
```

### VS Code

Install the `PHP Debug` extension, then add a `Listen for Xdebug` launch configuration on port `9003`. Start listening, then request your project's URL in the browser - the debugger should stop at your breakpoints.

### PhpStorm

Go to `Settings` -> `PHP` -> `Debug`, confirm the Xdebug port is `9003`, then click the "Start listening for PHP Debug connections" icon in the toolbar and request your project's URL in the browser.

## PHPUnit

Both editors can run PHPUnit using the same WSL-side PHP and Composer install:

* **VS Code**: install a PHPUnit test-runner extension once connected via Remote - WSL, and point it at your project's `vendor/bin/phpunit`.
* **PhpStorm**: go to `Settings` -> `PHP` -> `Test Frameworks`, add a PHPUnit configuration using the WSL interpreter configured above and your project's `vendor/bin/phpunit`.

## Next step

See [WSL Configuration](wsl-configuration.md) for where to put your project files and how to tune WSL's resource usage, or jump to the [FAQ](faq.md) for common troubleshooting.
