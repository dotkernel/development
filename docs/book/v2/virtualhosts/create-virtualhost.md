# AlmaLinux 10 Create virtualhosts

> By using the `*.localhost` pattern for any new virtualhost, you do not need to modify the `hosts` file in Windows, because these are routed by default.

Move inside the directory `development/wsl`:

```shell
cd ~/development/wsl/
```

If you don't already have a `config.yml` file, duplicate `config.yml.dist` as `config.yml`.

Using your preferred text editor, open `config.yml` and, under the `virtualhosts` key, enter the virtualhosts that you want to create, each on its own line.
Already existing virtualhosts will be skipped, their contents will not be lost, no need to comment or remove them.
Save and close the file.

Create the specified virtualhosts:

```shell
ansible-playbook -i hosts create-virtualhost.yml --ask-become-pass
```

This process will ask for your **AlmaLinux 10** password, iterate over the list of configured `virtualhosts` and output a short summary with the results.
Your virtualhost should be accessible and ready to use.

You will install your project under the `html` directory of your project, for example `/var/www/example.localhost/html`.

> The virtualhost's document root is set to the `public` directory of the above location, for example `/var/www/example.localhost/html/public`.

> If you want to have the DocumentRoot directly in `html` folder, you need to modify the file `/etc/httpd/sites-available/example.localhost`.

## Good to know

* To run your installed projects, you need to start **AlmaLinux 10** first.
* If you work with virtualhosts, your projects are created under `/var/www/`.
* You can still run PHP scripts under the default Apache project directory, located at `/var/www/html/`.
* If you encounter write permission issues, see [this guide](https://docs.dotkernel.org/development/v2/faq/#how-do-i-fix-common-permission-issues).
* We install PHP 8.4 by default—if you need a different version, see [this guide](https://docs.dotkernel.org/development/v2/faq/#how-do-i-switch-to-a-different-version-of-php).
* We install Node.js 22 by default—if you need a different version, see [this guide](https://docs.dotkernel.org/development/v2/faq/#how-do-i-switch-to-a-different-version-of-nodejs).
